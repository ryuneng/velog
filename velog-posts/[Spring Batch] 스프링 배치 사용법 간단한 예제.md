<h2 id="❓-스프링-배치란">❓ 스프링 배치란</h2>
<blockquote>
<p>스프링 배치(Spring Batch)는 <strong>대용량 데이터를 처리하기 위한 프레임워크</strong>로, 스프링 프레임워크 기반에서 작동한다.
일반적으로 배치 작업은 대량의 데이터를 처리하거나, <strong>주기적이고 반복적인 작업을 실행</strong>하는 데 사용되며, 스프링 배치는 <strong>이러한 작업을 효율적이고 안정적으로 처리</strong>할 수 있는 프레임워크다.</p>
</blockquote>
<h3 id="🔎-스프링-배치의-내부-구조도">🔎 스프링 배치의 내부 구조도</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/ffb4d672-fd4b-405d-bcd9-829316763874/image.png" /></p>
<h4 id="1-joblauncher">1. JobLauncher</h4>
<ul>
<li>Job을 시작하는 부분</li>
</ul>
<h4 id="2-실제-작업하는-부분-정의해야-할-부분">2. 실제 작업하는 부분 (정의해야 할 부분)</h4>
<ul>
<li><strong>Job</strong> &gt; <strong>Step</strong> &gt; <strong>ItemReader</strong> &gt; <strong>ItemProcessor</strong> &gt; <strong>ItemWriter</strong></li>
</ul>
<h4 id="3-jobrepository">3. JobRepository</h4>
<ul>
<li>메타데이터 테이블에 접근해서 해당 작업이 얼만큼 진행되는지 참조해준다.</li>
</ul>
<br />
<br />

<hr />
<blockquote>
<p>이제 스프링 배치를 <strong>구현할 준비</strong>를 해보자.</p>
</blockquote>
<h2 id="🚩-1-구현-준비">🚩 1. 구현 준비</h2>
<h3 id="1-의존성-추가">1) 의존성 추가</h3>
<pre><code>    implementation 'org.springframework.boot:spring-boot-starter-batch'
    testImplementation 'org.springframework.batch:spring-batch-test'</code></pre><br />

<h3 id="2-applicationyml">2) application.yml</h3>
<pre><code>spring:
  batch:
    job:
      enabled: false # 프로젝트 실행 시 자동으로 배치 작업이 가동되는 것을 방지</code></pre><br />
<br />

<hr />
<h2 id="⚙️-2-batchconfig-클래스-생성">⚙️ 2. BatchConfig 클래스 생성</h2>
<ul>
<li>하나의 배치 Job을 정의할 클래스를 생성하고, Job 메서드를 등록해야 한다.</li>
</ul>
<blockquote>
<p>Job으로 하나의 <strong>배치 작업을 정의</strong>하고,
  실제 배치 처리는 Job 아래에 존재하는 <strong>하나의 Step에서 수행</strong>한다.
  Step에서 <strong>&quot;읽기 &gt; 처리 &gt; 쓰기&quot;</strong> 과정을 구상해야 하며, Step을 등록하기 위한 Bean을 등록해야 한다.</p>
</blockquote>
<h4 id="0-먼저-배치-작업-시-사용할-repository-의존성들을-필드에-주입받는다">0. 먼저, 배치 작업 시 사용할 Repository 의존성들을 필드에 주입받는다.</h4>
<pre><code class="language-java">@Configuration
@EnableBatchProcessing // 스프링 배치를 작동시켜준다.
@RequiredArgsConstructor
public class BatchConfig {

    private final JobRepository jobRepository;
    private final PlatformTransactionManager platformTransactionManager;

    private final BeforeRepository beforeRepository;
    private final AfterRepository afterRepository;

}</code></pre>
<h3 id="1-job-작업-정의">1) Job (작업 정의)</h3>
<ul>
<li><p>Job 정의는 아주 간단하게 메서드로 정의하면 된다.</p>
<pre><code class="language-java">  @Bean
  public Job firstJob() {

      // 첫번째 매개변수 : 해당 Job을 지칭할 이름 선언 (&quot;firstJob&quot;)
      // 두번째 매개변수 : 해당 작업에 대한 트래킹을 진행하기 위해 jobRepository를 넣어주면
      //                스프링 배치가 자동으로 작업이 진행되는지를 메타 데이터 테이블에 기록해준다.
      return new JobBuilder(&quot;firstJob&quot;, jobRepository) 
              .start(firstStep)  // 이 작업에서 처음 시작할 Step 선언
              .next(secondStep)  // Step이 1개 이상일 때, next()로 이후의 Step을 선언
              .build();          // build()로 마무리하면 해당 작업이 정의된다.
  }</code></pre>
</li>
</ul>
<br />

<h3 id="2-step-실제-데이터-처리">2) Step (실제 데이터 처리)</h3>
<ul>
<li><p><strong>chunk</strong></p>
<ul>
<li>대량의 데이터를 끊어서 처리할 <strong>최소 단위</strong> (1회 호출 시 응답받을 데이터 수)</li>
<li>읽기 &gt; 처리 &gt; 쓰기 작업은 청크 단위로 진행된다.</li>
</ul>
</li>
<li><p><strong>PlatformTransactionManager</strong></p>
<ul>
<li><p>청크가 진행되다가 실패했을 때, 롤백을 진행한다든지 다시 처리할 수 있도록 세팅해준다.</p>
<pre><code class="language-java">@Bean
public Step firstStep() {

    return new StepBuilder(&quot;firstStep&quot;, jobRepository)
         // &lt;[Reader에서 읽어들일 데이터 타입], [Writer에서 쓸 데이터 타입]&gt;
            .&lt;BeforeEntity, AfterEntity&gt;chunk(10, platformTransactionManager)
            .reader(beforeReader)       // 읽는 메서드 자리
            .processor(middleProcessor) // 처리 메서드 자리
            .writer(afterWriter)        // 쓰기 메서드 자리
            .build();
}</code></pre>
</li>
</ul>
</li>
</ul>
<br />

<h3 id="3-repositoryitemreader-읽기">3) RepositoryItemReader (읽기)</h3>
<ul>
<li><p>BeforeEntity 테이블에서 <strong>읽어오는 작업을 수행</strong>한다.</p>
</li>
<li><p>청크 단위까지만 읽기 때문에 findAll을 하더라도 <strong>전부 읽지 않고 chunck 개수 만큼 사용</strong>하게 된다.
따라서, 자원 낭비를 방지하기 위해 Sort를 진행하고, pageSize() 단위를 설정해 <strong>findAll이 아닌 페이지 만큼 읽어올 수 있도록 설정</strong>한다.</p>
<pre><code class="language-java">  @Bean
  public RepositoryItemReader&lt;BeforeEntity&gt; beforeReader() {

      return new RepositoryItemReaderBuilder&lt;BeforeEntity&gt;()
              .name(&quot;beforeReader&quot;)
              .pageSize(10)
              .methodName(&quot;findAll&quot;) // Repository의 findAll 메서드
              .repository(beforeRepository)
              .sorts(Map.of(&quot;id&quot;, Sort.Direction.ASC))
              .build();
  }</code></pre>
</li>
</ul>
<br />

<h3 id="4-itemprocessor-중간-처리">4) ItemProcessor (중간 처리)</h3>
<ul>
<li><p>Reader에서 <strong>읽어오는 Data를 처리</strong>한다.</p>
</li>
<li><p>큰 작업을 수행하지 않을 때는 굳이 ItemProcessor를 정의하지 않고 ItemReader에서 바로 읽어서 ItemWriter로 보내도 된다.</p>
<pre><code class="language-java">  @Bean
  public ItemProcessor&lt;BeforeEntity, AfterEntity&gt; middleProcessor() {
      // &lt;[Reader에서 읽어들일 데이터 타입], [Writer에서 쓸 데이터 타입]&gt;
      return new ItemProcessor&lt;BeforeEntity, AfterEntity&gt;() {

          // process 메서드를 통해 item(BeforeEntity)에 읽어들인 데이터가 담기게 됨
          @Override
          public AfterEntity process(BeforeEntity item) throws Exception {

              AfterEntity afterEntity = new AfterEntity();
              afterEntity.setName(item.getName()); // 읽어들인 데이터를 쓸 데이터에 옮겨담음

              return afterEntity;
          }
      };
  }</code></pre>
</li>
</ul>
<br />
<br />

<h3 id="5-repositoryitemwriter-쓰기">5) RepositoryItemWriter (쓰기)</h3>
<ul>
<li><p>AfterEntity에 <strong>처리한 결과를 저장</strong>한다.</p>
<pre><code class="language-java">  @Bean // RepositoryItemWriter&lt;[저장될 엔티티]&gt;
  public RepositoryItemWriter&lt;AfterEntity&gt; afterWriter() {

      return new RepositoryItemWriterBuilder&lt;AfterEntity&gt;()
              .repository(afterRepository) // afterRepository를 통해서
              .methodName(&quot;save&quot;)          // save 쿼리 실행
              .build();
  }</code></pre>
</li>
</ul>
<br />

<hr />
<h2 id="🧑🏻💻-3-joblauncher-구현">🧑🏻‍💻 3. jobLauncher 구현</h2>
<blockquote>
<p>application.yml에서 배치 자동 실행에 대한 변수 값을 false로 설정했기 때문에
<strong>배치를 실행시키기 위한 Job 실행도구, <code>jobLauncher</code>를 구현</strong>해야 한다.<br />
❓ <strong>false로 설정한 이유</strong>
    - 서버가 실행되자마자 배치가 실행되면, 원하는 날짜에 실행할 수 없다.
원하는 특정 일자에 배치를 실행하기 위해 <strong>스케줄링 혹은 특정 API 호출을 통해 실행되도록 설정</strong>한다.</p>
</blockquote>
<h3 id="1-api-방식으로-실행시키는-방법">1. API 방식으로 실행시키는 방법</h3>
<h4 id="1-controller-생성">1) Controller 생성</h4>
<pre><code class="language-java">@Controller
@RequiredArgsConstructor
public class ApiController {

    private final JobLauncher jobLauncher;
    private final JobRegistry jobRegistry;

    @GetMapping(&quot;/first&quot;)
    public String firstApi(@RequestParam(&quot;value&quot;) String value) throws Exception {

        // 쿼리 파라미터로 받아온 value 값을 date 변수에 넣어준다.
        JobParameters jobParameters = new JobParametersBuilder()
                .addString(&quot;date&quot;, value)
                .toJobParameters();

        // &quot;firstJob&quot; : 2-1 Job(작업 정의)에서 Bean으로 정의한 Job의 이름
        jobLauncher.run(jobRegistry.getJob(&quot;firstJob&quot;), jobParameters);

        return &quot;ok&quot;;
    }
}</code></pre>
<h4 id="2-api-호출">2) API 호출</h4>
<blockquote>
<p>1개의 Job에 대해 한 번 호출한 파라미터는 재사용이 불가하다.</p>
</blockquote>
<ol>
<li><p><code>http://localhost:8080/first?value=a</code> 호출 시,
 <strong>배치 작업이 실행</strong>되어 BeforeEntity -&gt; AfterEntity로 <strong>Insert 작업이 수행</strong>된다.</p>
</li>
<li><p>한 번 더 <code>http://localhost:8080/first?value=a</code> 호출 시, 예외가 발생한다.
이미 a라는 파라미터가 들어갔기 때문에 해당 배치가 실행되지 않는다.</p>
</li>
<li><p><code>http://localhost:8080/first?value=b</code> 호출 시, 배치가 제대로 실행된다.</p>
</li>
</ol>
<br />
<br />

<h3 id="2-scheduler로-실행시키는-방법">2. Scheduler로 실행시키는 방법</h3>
<h4 id="1-메인-application-파일에-enablescheduling-어노테이션-부착">1) 메인 Application 파일에 @EnableScheduling 어노테이션 부착</h4>
<pre><code class="language-java">@SpringBootApplication
@EnableScheduling // 스케줄링 어노테이션 활성화 설정
public class MainApplication {

    public static void main(String[] args) {

        SpringApplication.run(MainApplication.class, args);
    }

}</code></pre>
<h4 id="2-scheduleconfig-클래스-생성">2) ScheduleConfig 클래스 생성</h4>
<ul>
<li><p>클래스 생성 후 <strong>서버를 실행하면 정해놓은 cron 주기마다 배치 작업이 실행</strong>된다.</p>
</li>
<li><p>JobParameters를 주는 방법은 커스텀을 해야 한다.
예시처럼 계속 new를 통해 값을 넣으면 <strong>매번 새롭게 실행</strong>되기 때문에,
년도-월-일까지만 받는 등의 방법을 통해 해당 배치가 <strong>특정 조건만 갖춰서 파라미터를 받아 실행될 수 있도록</strong> JobParameters를 잘 구성해야 한다.</p>
<pre><code class="language-java">@Configuration
@RequiredArgsConstructor
@Slf4j
public class ScheduleConfig {

  private final JobLauncher jobLauncher;
  private final JobRegistry jobRegistry;

  // 한국 시간 기준으로 10초마다 배치 작업 실행
  @Scheduled(cron = &quot;10 0 0 * * *&quot;, zone = &quot;Asia/Seoul&quot;)
  public void runFirstJob() throws Exception {

      log.info(&quot;first schedule start&quot;);

      SimpleDateFormat dateFormat = new SimpleDateFormat(&quot;yyyy-MM-dd-hh-mm-ss&quot;);
      String date = dateFormat.format(new Date());

      JobParameters jobParameters = new JobParametersBuilder()
              .addString(&quot;date&quot;, date)
              .toJobParameters();

      // &quot;firstJob&quot; : 2-1 Job(작업 정의)에서 Bean으로 정의한 Job의 이름
      jobLauncher.run(jobRegistry.getJob(&quot;firstJob&quot;), jobParameters);
  }
}</code></pre>
</li>
</ul>
<br />
<br />

<hr />
<p>References</p>
<ul>
<li><a href="https://www.youtube.com/playlist?list=PLJkjrxxiBSFCaxkvfuZaK5FzqQWJwmTfR">https://www.youtube.com/playlist?list=PLJkjrxxiBSFCaxkvfuZaK5FzqQWJwmTfR</a> (1~7)</li>
<li><a href="https://dkswnkk.tistory.com/707">https://dkswnkk.tistory.com/707</a></li>
</ul>