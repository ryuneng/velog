<blockquote>
<p>Cache Aside 전략이란?
🔗 <a href="https://velog.io/@ryuneng2/Redis-%EC%BA%90%EC%8B%9CCache-%EC%BA%90%EC%8B%B1Caching%EC%9D%B4%EB%9E%80">https://velog.io/@ryuneng2/Redis-캐시Cache-캐싱Caching이란</a></p>
</blockquote>
<h2 id="❓-cacheable">❓ @Cacheable</h2>
<ul>
<li><code>@Cacheable</code> 어노테이션을 부착하면 <strong>Cache Aside 전략</strong>으로 캐싱이 적용된다.
즉, 해당 메서드로 요청이 들어오면 레디스를 먼저 확인한 후에 데이터가 있다면(<code>Cache Hit</code>) 레디스의 데이터를 조회해서 바로 응답한다.
만약 데이터가 없다면(<code>Cache Miss</code>) 메서드 내부의 로직을 실행시킨 뒤에 return 값으로 응답한 후, 그 return 값을 레디스에 저장한다.</li>
</ul>
<br />

<h3 id="✅-cacheable의-속성-값">✅ @Cacheable의 속성 값</h3>
<ul>
<li><strong>cacheNames</strong> : 캐시 이름 설정</li>
<li><strong>key</strong> : Redis에 저장할 Key의 이름 설정</li>
<li><strong>cacheManager</strong> : 사용할 <code>cacheManager</code>의 Bean 이름을 지정
(RedisCacheConfig에 CacheManager 타입으로 정의한 메서드명과 일치해야 함)</li>
</ul>
<br />
<br />

<hr />
<h2 id="💡-사용-예시">💡 사용 예시</h2>
<h3 id="1-buildgradle-의존성-추가">1. build.gradle 의존성 추가</h3>
<pre><code>dependencies {
    // redis 추가
    implementation 'org.springframework.boot:spring-boot-starter-data-redis'
}</code></pre><br />

<h3 id="2-applicationyml-설정">2. application.yml 설정</h3>
<pre><code class="language-yaml">spring:
  data:
    redis:
      host: ${REDIS_HOST}
      port: ${REDIS_PORT}

logging:
  level:
    org.springframework.cache: trace # redis와 관련된 정보가 로그에 출력되도록 설정</code></pre>
<br />

<h3 id="3-redisconfig-작성">3. RedisConfig 작성</h3>
<pre><code class="language-java">@Configuration
public class RedisConfig {
    @Value(&quot;${REDIS_HOST}&quot;)
    private String host;

    @Value(&quot;${REDIS_PORT}&quot;)
    private int port;

    @Bean
    public LettuceConnectionFactory redisConnectionFactory() {
        // Lettuce라는 라이브러리를 활용해 Redis 연결을 관리하는 객체를 생성하고
        // Redis 서버에 대한 정보(host, port)를 설정한다.
        return new LettuceConnectionFactory(new RedisStandaloneConfiguration(host, port));
    }
}</code></pre>
<br />

<h3 id="4-rediscacheconfig-작성">4. RedisCacheConfig 작성</h3>
<pre><code class="language-java">@Configuration
@EnableCaching // SpringBoot의 캐싱 설정을 활성화
public class RedisCacheConfig {

    @Bean
    public CacheManager boardCacheManager(RedisConnectionFactory redisConnectionFactory) {

        RedisCacheConfiguration redisCacheConfiguration = RedisCacheConfiguration
                .defaultCacheConfig()
                // Redis에 Key를 저장할 때 String으로 직렬화(변환)해서 저장
                .serializeKeysWith(
                        RedisSerializationContext.SerializationPair.fromSerializer(
                                new StringRedisSerializer()))
                // Redis에 Value를 저장할 때 Json으로 직렬화(변환)해서 저장
                .serializeValuesWith(
                        RedisSerializationContext.SerializationPair.fromSerializer(
                                new GenericJackson2JsonRedisSerializer()
                        )
                )
                // 데이터의 만료기간(TTL) 설정
                .entryTtl(Duration.ofMinutes(1L)); // 1분마다 데이터 갱신

        return RedisCacheManager
                .RedisCacheManagerBuilder
                .fromConnectionFactory(redisConnectionFactory)
                .cacheDefaults(redisCacheConfiguration)
                .build();
    }
}</code></pre>
<br />

<h3 id="5-service-클래스">5. Service 클래스</h3>
<pre><code class="language-java">@Service
public class BoardService {
    private final BoardRepository boardRepository;

    public BoardService(BoardRepository boardRepository) {
        this.boardRepository = boardRepository;
    }

    @Cacheable(cacheNames = &quot;getBoards&quot;, key = &quot;'boards:page:' + #page + ':size:' + #size&quot;, cacheManager = &quot;boardCacheManager&quot;)
    public List&lt;Board&gt; getBoards(int page, int size) {

        Pageable pageable = PageRequest.of(page - 1, size);
        Page&lt;Board&gt; pageOfBoards = boardRepository.findAllByOrderByCreatedAtDesc(pageable);
        return pageOfBoards.getContent();
    }
}</code></pre>
<h3 id="6-controller-클래스">6. Controller 클래스</h3>
<pre><code class="language-java">@RestController
@RequestMapping(&quot;boards&quot;)
public class BoardController {
    private final BoardService boardService;

    public BoardController(BoardService boardService) {
        this.boardService = boardService;
    }

    @GetMapping()
    public List&lt;Board&gt; getBoards(@RequestParam(defaultValue = &quot;1&quot;) int page,
                                 @RequestParam(defaultValue = &quot;10&quot;) int size
    ) {

        return boardService.getBoards(page, size);
    }
}</code></pre>
<p><code>이외에도 기본적으로 Board 엔티티 클래스, BoardRepository 클래스는 필요하다.</code></p>
<br />
<br />

<hr />
<h3 id="📌-요청-결과-확인">📌 요청 결과 확인</h3>
<blockquote>
<p>API를 요청하면 로그에서 Cache Aside 전략으로 캐싱이 적용되는 과정을 확인할 수 있다.</p>
</blockquote>
<h4 id="1-api-요청">1. API 요청</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/230a1016-eb6e-4878-80e5-0c1ae0e2b884/image.png" /></p>
<h4 id="2-로그-확인">2. 로그 확인</h4>
<ul>
<li>1) <strong>Cache Miss 상태</strong>
<code>No cache entry for key 'boards:page:1:size:10' in cache(s) [getBoards]</code></li>
</ul>
<ul>
<li>2) 원래 로직에서 <strong>데이터베이스 조회하는 SQL문이 실행</strong>된다.
<code>select b1_0.id,b1_0.content,b1_0.created_at,b1_0.title from boards b1_0 order by b1_0.created_at desc limit ?</code>
<code>select count(b1_0.id) from boards b1_0</code></li>
</ul>
<ul>
<li>3) 데이터베이스에서 클라이언트가 응답받은 데이터를 <strong>캐시(Redis)에 저장</strong>한다.
<code>Creating cache entry for key 'boards:page:1:size:10' in cache(s) [getBoards]</code></li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/d0f28a12-890d-4a8c-8df0-daf519be42bf/image.png" /></p>
<h4 id="3-만료시간-이내에-api-재요청-브라우저-새로고침">3. 만료시간 이내에 API 재요청 (브라우저 새로고침)</h4>
<h4 id="4-로그-확인">4. 로그 확인</h4>
<ul>
<li><code>Cache entry for key 'boards:page:1:size:10' found in cache(s) [getBoards]</code>
: <strong>캐시에 저장된 데이터를 조회해서 반환</strong>했다는 의미이다.
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/58e1acaf-83b1-41b3-912a-821550309f04/image.png" /></li>
</ul>
<br />

<h3 id="📌-저장된-데이터-확인">📌 저장된 데이터 확인</h3>
<ul>
<li>1) cmd 실행 및 <code>redis-cli</code> 명령어 입력
   혹은 Redis를 설치한 경로를 열어 <strong>redis-cli.exe를 실행</strong></li>
<li>2) <code>keys *</code> 명령어 입력 - Key가 잘 저장되었는지 확인</li>
<li>3) <code>get [key]</code> 명령어 입력 - 해당 Key에 저장된 데이터 조회</li>
<li>4) <code>ttl [key]</code> 명령어 입력 - 해당 Key의 만료시간 확인
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/460dd1d4-c2af-4fb0-93b2-553db0b0c6bc/image.png" /></li>
</ul>
<br />
<br />

<hr />
<p>Reference</p>
<ul>
<li><a href="https://www.inflearn.com/course/%EB%B9%84%EC%A0%84%EA%B3%B5%EC%9E%90-redis-%EC%9E%85%EB%AC%B8-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94/dashboard">인프런 강의 &lt;비전공자도 이해할 수 있는 Redis 입문/실전 (조회 성능 최적화편)&gt; 섹션 5</a></li>
</ul>