<h1 id="⚠️-오류">⚠️ 오류</h1>
<ul>
<li>gRPC 의존성을 추가한 후 서버 실행 시 여러 오류가 발생했고, <strong>실제로는 정상적으로 통과되는 테스트도 실패</strong>했다는 메시지가 출력되었다.</li>
</ul>
<ul>
<li>오류 내용
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/cc7e7061-f843-430f-8554-be9f28b5ea48/image.png" /></li>
</ul>
<pre><code>Execution failed for task ':test'.
&gt; There were failing tests. See the report at: file:///C:/~~~/reports/tests/test/index.html

* Try:
&gt; Run with --scan to get full insights.
BUILD FAILED in 33s
12 actionable tasks: 12 executed</code></pre><ul>
<li>테스트 실패 내용
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/b15c7a26-4504-46a8-ba7d-a0278b71a0aa/image.png" /></li>
</ul>
<blockquote>
<p>하지만 실패한다는 테스트는 <strong>실제로 실행해보면 성공하는 테스트</strong>였다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/b809411f-efcb-4e54-b860-2067316e43f6/image.png" /></p>
<br />
<br />

<hr />
<h1 id="❓-원인">❓ 원인</h1>
<ul>
<li><code>application.yml</code>의 <code>grpc: server: port:</code> 설정 위치가 문제였다.</li>
<li>로컬 환경과 테스트 실행 환경에서 <strong>동일한 gRPC 포트를 사용하게 되어 포트 충돌</strong>이 발생했다.
gRPC 포트 설정이 기본 프로파일(로컬과 테스트 환경에서 공통으로 사용되는 프로파일)에 있었기 때문이다.</li>
</ul>
<br />
<br />


<h1 id="💡-해결">💡 해결</h1>
<ul>
<li><p><strong>gPRC 포트 설정을 로컬 프로파일로 이동</strong>함으로써 충돌을 방지하고, 문제를 해결했다.</p>
</li>
<li><p><strong>기존 application.yml</strong></p>
<pre><code class="language-yml"># 1. 기본값 프로파일
server:
port: ${SERVER_PORT}
</code></pre>
</li>
</ul>
<p>grpc:
  server:
    port: ${GRPC_PORT} # 기존</p>
<h1 id="">~</h1>
<h1 id="2-로컬용-프로파일">2. 로컬용 프로파일</h1>
<hr />
<p>spring:
  config:
    activate:
      on-profile: local</p>
<h1 id="-1">~</h1>
<h1 id="3-테스트-실행-전용-프로파일">3. 테스트 실행 전용 프로파일</h1>
<hr />
<p>spring:
  config:
    activate:
      on-profile: test</p>
<pre><code>
&lt;br&gt;

- **수정 후 application.yml**
```yml
# 1. 기본값 프로파일
server:
  port: ${SERVER_PORT}

# ~
# 2. 로컬용 프로파일
---
spring:
  config:
    activate:
      on-profile: local

grpc:
  server:
    port: ${GRPC_PORT} # 이동

# ~
# 3. 테스트 실행 전용 프로파일
---
spring:
  config:
    activate:
      on-profile: test</code></pre><br />

<h2 id="✅-결과">✅ 결과</h2>
<ul>
<li>성공적으로 실행!
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/6468b1d7-ca73-48da-adbe-2053517dad68/image.png" /></li>
</ul>