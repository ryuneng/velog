<h4 id="0-redis-실행-확인">0. Redis 실행 확인</h4>
<ul>
<li>1) Redis를 설치한 경로를 열어 <strong>redis-cli.exe</strong>를 실행한다.
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/3b78b299-697e-46e2-8fa2-698d9d74fdde/image.png" /></li>
</ul>
<br />

<ul>
<li>2) 터미널이 뜨면, <strong><code>ping</code> 명령어를 입력</strong>하고, <code>PONG</code>이라는 응답을 받음으로써 정상적으로 실행되는 것을 확인할 수 있다.
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/d2176ecb-fc2d-4cea-9093-9209a053f386/image.png" /></li>
</ul>
<br />

<hr />
<blockquote>
<p>그럼 이제 본격적으로 Redis의 주요 명령어 7가지에 대해 알아보자.</p>
</blockquote>
<h3 id="1-데이터-저장">1. 데이터 저장</h3>
<ul>
<li><code>set [key] [value]</code></li>
<li>Value에 띄어쓰기가 있는 경우 : <code>set [key] &quot;[value]&quot;</code>
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/4a18cfa3-135b-485d-bcc5-e314ba7b8955/image.png" /></li>
</ul>
<br />

<h3 id="2-저장된-value-데이터-1건-조회">2. 저장된 Value 데이터 1건 조회</h3>
<ul>
<li><code>get [key]</code>
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/b64e7b56-134b-4ef4-a7b4-dd65e4e448c7/image.png" /></li>
</ul>
<br />

<h3 id="3-저장된-모든-key-조회">3. 저장된 모든 Key 조회</h3>
<ul>
<li><code>keys *</code>
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/d70def1d-f238-42a0-aaa9-9af1c348a188/image.png" /></li>
</ul>
<br />

<h3 id="4-데이터-삭제">4. 데이터 삭제</h3>
<ul>
<li><code>del [key]</code>
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/d081fadd-66b9-4ed5-9a7d-fcb1835ceb68/image.png" /></li>
<li>삭제 확인 : <code>get [key]</code> -&gt; 데이터가 없을 시, <code>(nil)</code> 반환
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/a9a1e2df-e085-4c48-a2e7-f181aecefce6/image.png" /></li>
</ul>
<br />

<h3 id="5-만료시간ttl-설정하여-데이터-저장">5. 만료시간(TTL) 설정하여 데이터 저장</h3>
<blockquote>
<p>레디스는 RDBMS와 다르게 데이터 저장 시 <strong>만료시간(TTL, Time To Live)</strong>을 설정할 수 있다. 영구적으로 데이터를 저장하지 않고 일정 시간이 되면 데이터가 삭제되도록 세팅할 수 있는 기능이다.<br />
❓ <strong>만료시간(TTL, Time To Live)이 있는 이유</strong>
: 레디스의 특성 상 <strong>메모리 공간이 한정</strong>되어 있기 때문에 (Disk보다 Ram의 용량이 더 작음) 모든 데이터를 레디스에 저장할 수 없다. 따라서, 만료 시간(TTL)을 활용해 <strong>자주 사용하는 데이터만 저장</strong>해놓고 사용하는 식으로 활용한다.</p>
</blockquote>
<ul>
<li><code>set [key] [value] ex [seconds]</code>
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/ac25404c-d09b-4531-bc13-416803549443/image.png" /></li>
<li>❓ <code>ex</code> : expiration(만료)의 약자
❓ <code>seconds</code> : 초(시간)</li>
</ul>
<br />

<h3 id="6-만료시간-확인">6. 만료시간 확인</h3>
<ul>
<li><code>ttl [key]</code>
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/756afe9e-c322-4eba-96df-9130f161039e/image.png" /></li>
<li>❓ <code>-2</code> : key가 없는 경우 (TTL 끝난 데이터)
❓ <code>-1</code> : key값은 존재하지만, 만료시간(TTL)이 설정되어 있지 않은 경우</li>
</ul>
<br />

<h3 id="7-저장된-모든-데이터-삭제">7. 저장된 모든 데이터 삭제</h3>
<ul>
<li><code>flushall</code>
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/d4accffe-baf8-4a2b-9b29-09407ede36ec/image.png" /></li>
<li>삭제 확인 : <code>keys *</code> -&gt; 데이터가 없을 시, <code>(empty list or set)</code> 반환
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/58bb0da7-0bf7-40e5-87d2-7cee27c3b310/image.png" /></li>
</ul>
<br />

<hr />
<h3 id="💡-redis의-key-네이밍-컨벤션">💡 Redis의 Key 네이밍 컨벤션</h3>
<blockquote>
<p>Redis의 Key 이름을 잘 짓는 것은 굉장히 중요하다.
현업에서 자주 사용하는 Key 네이밍 컨벤션에 대해 알아보자.</p>
</blockquote>
<ul>
<li><strong>현업에서 자주 사용하는 네이밍 컨벤션</strong><ul>
<li><strong>콜론(<code>:</code>)</strong>을 활용해 계층적으로 의미를 구분해서 사용</li>
</ul>
</li>
</ul>
<ul>
<li><strong>예시</strong><ul>
<li><code>users:100:profile</code> - 사용자들(users) 중에서 PK가 100인 사용자(user)의 프로필(profile)</li>
<li><code>products:123:details</code> - 상품들(products) 중에서 PK가 123인 상품(product)의 세부사항(details)</li>
</ul>
</li>
</ul>
<ul>
<li>컨벤션의 장점<ol>
<li><strong>가독성</strong> : 데이터의 의미와 용도를 쉽게 파악할 수 있다.</li>
<li><strong>일관성</strong> : 컨벤션을 따름으로써 코드의 일관성이 높아지고 유지보수가 쉬워진다.</li>
<li><strong>검색 및 필터링 용이성</strong> : 패턴 매칭을 사용해 특정 유형의 Key를 쉽게 찾을 수 있다.</li>
<li><strong>확장성</strong> : 서로 다른 Key와 이름이 겹쳐 충돌할 일이 적어진다.</li>
</ol>
</li>
</ul>
<br />
<br />

<hr />
<p>Reference</p>
<ul>
<li><ul>
<li><a href="https://www.inflearn.com/course/%EB%B9%84%EC%A0%84%EA%B3%B5%EC%9E%90-redis-%EC%9E%85%EB%AC%B8-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94/dashboard">인프런 강의 &lt;비전공자도 이해할 수 있는 Redis 입문/실전 (조회 성능 최적화편)&gt; 섹션 3</a></li>
</ul>
</li>
</ul>