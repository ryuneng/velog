<h1 id="❓-캐시cache란">❓ 캐시(Cache)란?</h1>
<ul>
<li><p>원본 저장소보다 빠르게 가져올 수 있는 임시 데이터 저장소
<code>* Redis에서만 쓰이는 용어는 아니며, 전반적인 개발 분야에서 통용되는 개념이다.</code></p>
</li>
<li><p>인터넷 사용 기록을 삭제할 때, '캐시된 이미지 및 파일'이라는 항목을 볼 수 있는데, 여기서 말하는 캐시가 이 캐시(Cache)다. 이는 <strong>임시로 이미지나 파일을 저장했다</strong>는 의미이다.
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/f9663c3f-20b8-45eb-b8e3-4a48a212aa59/image.png" /></p>
</li>
</ul>
<br />

<h1 id="✔️-캐싱caching이란">✔️ 캐싱(Caching)이란?</h1>
<ul>
<li><p>캐시(Cache, 임시 데이터 저장소)에 접근해서 데이터를 빠르게 가져오는 방식</p>
</li>
<li><p>실무에서 캐싱(Caching)을 사용할 때는 보통 이런 표현을 쓴다.
: &quot;이 API 응답 속도가 너무 느린데? 이 응답 데이터는 <em>캐싱(Caching)</em> 해두고 쓰는 게 어때?&quot;
→ 이 말은, 'API 응답 결과를 <strong>원본 저장소보다 빠르게 가져올 수 있는 임시 데이터 저장소에 저장해두고, 빠르게 조회할 수 있게</strong> 만드는 게 어때?' 라는 의미로 해석할 수 있다.</p>
</li>
</ul>
<br />
<br />

<h2 id="📌-데이터를-캐싱할-때-사용하는-주요-전략-2가지">📌 데이터를 캐싱할 때 사용하는 주요 전략 2가지</h2>
<h3 id="1-cache-aside-전략">1. Cache Aside 전략</h3>
<blockquote>
<p>데이터를 조회할 때 주로 사용하는 전략.
<strong>캐시(Cache)에서 데이터를 확인하고, 없다면 DB를 통해 조회해오는 방식</strong>으로,
Look Aside 또는 Lazy Loading 전략이라고도 부른다.</p>
</blockquote>
<h4 id="--캐시에-데이터가-있을-경우-cache-hit">- 캐시에 데이터가 있을 경우 (=Cache Hit)</h4>
<ol>
<li>클라이언트가 레디스에 데이터를 요청한다.</li>
<li>레디스에서 클라이언트로 요청한 데이터를 응답한다.</li>
</ol>
<h4 id="--캐시에-데이터가-없을-경우-cache-miss">- 캐시에 데이터가 없을 경우 (=Cache Miss)</h4>
<ol>
<li>클라이언트가 레디스에 데이터를 요청한다.</li>
<li>레디스에 데이터가 없기 때문에, 클라이언트가 데이터베이스에 데이터를 요청한다.</li>
<li>데이터베이스에서 클라이언트로 요청한 데이터를 응답한다.</li>
<li>클라이언트가 응답받은 데이터를 캐시(레디스)에 저장한다.</li>
</ol>
<br />

<h3 id="2-write-around-전략">2. Write Around 전략</h3>
<blockquote>
<p><strong>쓰기 작업(저장, 수정, 삭제)을 캐시에는 반영하지 않고, DB에만 반영하는 방식</strong>
Cache Aside 전략과 같이 자주 활용되는 전략이다.</p>
</blockquote>
<p><code>매우 간단함!</code></p>
<ul>
<li>데이터를 저장할 때는 <strong>레디스에 저장하지 않고, 데이터베이스에만 저장</strong>한다.
이후 데이터를 조회할 때, 레디스에 데이터가 없으면 데이터베이스로부터 데이터를 조회해와서 레디스에 저장하는 방식이다.</li>
</ul>
<br />

<h3 id="✅-두-전략의-차이">✅ 두 전략의 차이</h3>
<ul>
<li>Cache Aside는 <strong>읽기 성능</strong>을 개선하기 위한 전략이고,
Write Around는 <strong>쓰기 성능</strong>을 고려해 캐시가 쓰기 작업에 부담을 받지 않도록 하는 전략이다.</li>
</ul>
<ul>
<li>Cache Aside는 주로 <strong>읽기 중심의 시나리오</strong>에 적합하고, Write Around는 <strong>쓰기 작업이 빈번하지 않고 읽기 작업이 많은 경우</strong>에 적합한 전략이다.</li>
</ul>
<br />
<br />

<hr />
<p>References</p>
<ul>
<li><a href="https://www.inflearn.com/course/%EB%B9%84%EC%A0%84%EA%B3%B5%EC%9E%90-redis-%EC%9E%85%EB%AC%B8-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94/dashboard">인프런 강의 &lt;비전공자도 이해할 수 있는 Redis 입문/실전 (조회 성능 최적화편)&gt; 섹션 4</a></li>
</ul>