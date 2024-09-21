<h2 id="⚠️-오류">⚠️ 오류</h2>
<ul>
<li>gRPC 설정 과정에서 <strong>gRPC 관련 클래스들이 생성되지 않고</strong> 오류가 발생했다.
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/f1a7a266-4937-497b-b719-65bf91f0da7c/image.png" /></li>
</ul>
<pre><code>Cwanted-all-cleargold-marketauth-serverbuildgeneratedsourceprotomaingrpccomryunenggoldauthgrpcTestServiceGrpc.java10 error cannot find symbol
@javax.annotation.Generated(
                 ^
  symbol   class Generated
  location package javax.annotation</code></pre><br />

<h2 id="❓-원인">❓ 원인</h2>
<ul>
<li>해당 오류는 <code>javax.annotation.Generated</code> 클래스를 찾을 수 없다는 의미다.
이 클래스는 <strong>Java 9부터 Java 표준 라이브러리에서 제외</strong>되었기 때문에, 현재 사용중인 Java 17버전에서 충돌이 발생했다.
<code>javax.annotation.Generated</code>는 <code>javax.annotation</code> 패키지에 속해 있기 때문에, 이 패키지를 별도로 추가하면 문제를 해결할 수 있다.</li>
</ul>
<br />

<hr />
<h2 id="💡-해결-방법">💡 해결 방법</h2>
<ul>
<li><code>build.gradle</code>에 아래 <strong>의존성을 추가</strong>한 후, Gradle을 clean하고 build를 수행했다.
<code>implementation 'javax.annotation:javax.annotation-api:1.3.2'</code></li>
</ul>
<br />

<h2 id="✅-성공">✅ 성공</h2>
<ul>
<li><code>build &gt; generated</code> 하위에 gRPC 관련 클래스들이 정상적으로 생성되었다.
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/8b49d28c-02f9-47e9-9ee5-3ab8b3bf5546/image.png" /></li>
</ul>
<br />

<hr />
<h2 id="💥-반전">💥 반전</h2>
<blockquote>
<p>하지만, 생성된 클래스들이 정상적으로 import되지 않는 2차 문제가 또 발생했다 ..</p>
</blockquote>
<ul>
<li>여러 참고 자료를 찾아 수정해봤지만, 문제는 해결되지 않았다. gRPC 관련 자료가 많지 않아 해결에 어려움이 있었다.</li>
</ul>
<br />

<h3 id="-추가">+ 추가</h3>
<ul>
<li>원인이 gRPC <strong>버전 호환성</strong> 문제임을 파악하고, 기존 의존성을 전부 제거한 후 <strong>최신 버전</strong>으로 교체하니 정상적으로 작동했다. 새로운 라이브러리를 추가할 때는 항상 버전 호환성을 잘 확인하자</li>
</ul>