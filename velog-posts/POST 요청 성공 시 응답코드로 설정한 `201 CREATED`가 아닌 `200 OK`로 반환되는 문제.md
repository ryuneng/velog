<h2 id="💥-문제">💥 문제</h2>
<ul>
<li><code>@PostMapping</code> 요청 성공 시 반환값으로 <code>SuccessResponse.created()</code>를 사용했는데,
실제 응답은 <strong><code>201 CREATED</code>가 아닌 <code>200 OK</code>가 반환</strong>되었다.</li>
</ul>
<ul>
<li><p>작성한 API 코드</p>
<pre><code class="language-java">@PostMapping
public SuccessResponse&lt;UserCreateResponse&gt; signup(
                                     @Valid @RequestBody UserCreateRequest request) {
  return SuccessResponse.created(&quot;회원가입이 완료되었습니다.&quot;, userService.signup(request));
}</code></pre>
</li>
<li><p>SuccessResponse 클래스</p>
<pre><code class="language-java">@Getter
public class SuccessResponse&lt;T&gt; {
  private HttpStatus success;
  private String message;
  private T data;

  public SuccessResponse(HttpStatus success, String message, T data) {
      this.success = success;
      this.message = message;
      this.data = data;
  }

  // 기대한 SuccessResponse.created() 반환값
  public static &lt;T&gt; SuccessResponse&lt;T&gt; created(String message, T data) {
      return new SuccessResponse&lt;&gt;(HttpStatus.CREATED, message, data);
  }
}</code></pre>
</li>
</ul>
<br />

<hr />
<h2 id="❓-원인">❓ 원인</h2>
<ul>
<li><code>SuccessResponse.created</code> 메서드가 <code>HttpStatus.CREATED</code>를 반환하도록 설정하더라도,
Spring은 기본적으로 <strong><code>ResponseEntity</code>를 사용하지 않으면 HTTP 상태 코드를 자동으로 설정하지 않는다.</strong></li>
</ul>
<ul>
<li>Spring MVC는 컨트롤러 메서드에서 반환된 객체를 HTTP 응답 본문으로 직렬화하지만,
상태 코드를 명시적으로 설정하지 않으면 기본값인 <code>HttpStatus.OK(200)</code>가 반환된다.</li>
</ul>
<ul>
<li>즉, <code>SuccessResponse.created()</code>가 내부적으로 <code>HttpStatus.CREATED</code>를 설정하고 있어도, 이 정보는 <strong>응답의 실제 HTTP 상태 코드에 반영되지 않는다.</strong></li>
</ul>
<br />

<hr />
<h2 id="💡-해결">💡 해결</h2>
<ul>
<li>반환값을 <strong><code>ResponseEntity&lt;&gt;</code>로 감싸서 상태 코드를 명시적으로 설정</strong>했다.
수정 후, 실제로 <code>201 CREATED</code> 상태 코드가 반환되었다.<pre><code class="language-java">@PostMapping
public ResponseEntity&lt;SuccessResponse&lt;UserCreateResponse&gt;&gt; signup(@Valid @RequestBody UserCreateRequest request) {
SuccessResponse&lt;UserCreateResponse&gt; response = SuccessResponse.created(&quot;회원가입이 완료되었습니다.&quot;, userService.signup(request));
return new ResponseEntity&lt;&gt;(response, HttpStatus.CREATED);
}</code></pre>
</li>
</ul>