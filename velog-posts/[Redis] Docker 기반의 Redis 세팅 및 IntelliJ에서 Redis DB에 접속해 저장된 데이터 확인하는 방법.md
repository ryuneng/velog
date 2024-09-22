<h2 id="✅-redis-의존성-추가-및-관련-파일-작성">✅ Redis 의존성 추가 및 관련 파일 작성</h2>
<h3 id="0-buildgradle-의존성-추가">0. build.gradle 의존성 추가</h3>
<pre><code>    implementation 'org.springframework.boot:spring-boot-starter-data-redis'</code></pre><h3 id="1-redisconfig-작성">1. RedisConfig 작성</h3>
<pre><code class="language-java">@Configuration
public class RedisConfig {

    @Value(&quot;${REDIS_PASSWORD}&quot;)
    private String redisPassword;

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {

        RedisStandaloneConfiguration redisConfiguration = new RedisStandaloneConfiguration();
        redisConfiguration.setPassword(redisPassword);

        return new LettuceConnectionFactory(redisConfiguration);
    }

    @Bean
    public RedisTemplate&lt;String, Object&gt; redisTemplate() {

        RedisTemplate&lt;String, Object&gt; template = new RedisTemplate&lt;&gt;();
        template.setKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        template.setConnectionFactory(redisConnectionFactory());
        return template;
    }
}</code></pre>
<h3 id="2-docker-composeyml-작성-및-실행">2. docker-compose.yml 작성 및 실행</h3>
<ul>
<li>docker-compose.yml 파일에서 직접 실행하거나,
<code>docker-compose -f docker-compose.yml up</code> 명령어 입력을 통해 실행할 수 있다.<pre><code class="language-yml">services:
redis:
  image: redis:latest
  container_name: gold-auth-redis
  ports:
    - ${REDIS_PORT}:${REDIS_PORT}
  restart: always</code></pre>
</li>
</ul>
<h3 id="3-applicationyml-작성">3. application.yml 작성</h3>
<pre><code class="language-yml">spring:
  data:
    redis:
      host: ${REDIS_HOST}
      port: ${REDIS_PORT}
      timeout: 5000</code></pre>
<br />
<br />

<hr />
<h2 id="✅-redis-패스워드-설정">✅ Redis 패스워드 설정</h2>
<h4 id="0-git-bash-실행">0. Git Bash 실행</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/39745f82-03fb-432e-9ca3-25426001b90f/image.png" /></p>
<h3 id="1-실행중인-컨테이너-확인">1. 실행중인 컨테이너 확인</h3>
<ul>
<li><code>docker ps</code> 명령어 입력</li>
</ul>
<h3 id="2-connect-redis-ssh">2. Connect Redis SSH</h3>
<ul>
<li><code>docker exec -it [CONTAINER ID] redis-cli</code></li>
<li>위 명령어 실행 안될 경우, 아래 명령어 실행
 <code>winpty docker exec -it [CONTAINER ID] redis-cli</code></li>
</ul>
<h3 id="3-패스워드-확인">3. 패스워드 확인</h3>
<ul>
<li><code>config get requirepass</code></li>
</ul>
<h3 id="4-패스워드-설정">4. 패스워드 설정</h3>
<ul>
<li><code>config set requirepass [PASSWORD]</code> </li>
</ul>
<h3 id="5-다시-실행해서-패스워드-설정-잘-됐는지-확인">5. 다시 실행해서 패스워드 설정 잘 됐는지 확인</h3>
<ul>
<li><code>config get requirepass</code></li>
</ul>
<h3 id="6-확인">6. 확인</h3>
<ul>
<li>아래 3개 차례대로 입력<ul>
<li><code>exit</code> (SSH 접속 종료)</li>
<li><code>winpty docker exec -it [CONTAINER ID] redis-cli</code> (재접속)</li>
<li><code>auth [PASSWORD]</code> (Redis 비밀번호 인증 - 인증해야만 Redis에 저장된 Data에 접근 가능)</li>
</ul>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/7c3d6b89-eebd-44d5-9351-655d69336969/image.png" /></p>
<br />
<br />

<hr />
<h2 id="✅-intellij-redis-연결">✅ IntelliJ Redis 연결</h2>
<h3 id="1-redis-db-연결">1. Redis DB 연결</h3>
<ul>
<li>비밀번호만 입력하면 된다.
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/4f29f2df-f20d-4419-8e2e-2fbca3ca4c36/image.png" /></li>
</ul>
<h3 id="2-redis-접속-확인">2. Redis 접속 확인</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/08b134d2-dc8c-494b-ad9b-e9123c51b7e5/image.png" /></p>
<br />
<br />

<hr />
<h2 id="💡-redis에-저장된-데이터-확인-성공">💡 Redis에 저장된 데이터 확인 성공</h2>
<ul>
<li>Docker 기반의 Redis에 성공적으로 데이터가 저장된 것을 확인할 수 있다 !
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/41095320-a3a1-48c6-9522-b9c7024191e2/image.png" /></li>
</ul>