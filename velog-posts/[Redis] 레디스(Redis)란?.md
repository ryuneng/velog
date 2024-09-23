<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/73c28e9f-3fa8-4ca9-83b7-eba5a8b21b85/image.png" /></p>
<h1 id="❓-redis란">❓ Redis란?</h1>
<ul>
<li>사전적 의미
: Remote Dictionary Server의 약자로, <strong>'키-값' 구조의 비정형 데이터</strong>를 저장하고 관리하기 위한 오픈 소스 기반의 <strong>비관계형</strong> 데이터베이스 관리 시스템(DBMS)</li>
</ul>
<blockquote>
<p>쉽게 말해, Redis는 <strong>데이터 처리 속도가 매우 빠른 NoSQL 데이터베이스</strong>이다.
여기서 NoSQL 데이터베이스가 <strong>Key-Value의 형태로 저장</strong>하는 데이터베이스라고 생각하면 된다.</p>
</blockquote>
<br />
<br />

<hr />
<h2 id="✅-redis의-주요-장점">✅ Redis의 주요 장점</h2>
<blockquote>
<p><strong>인메모리(in-memory)에 모든 데이터를 저장</strong>하기 때문에, 데이터의 처리 성능이 굉장히 빠르다.</p>
</blockquote>
<ul>
<li>MySQL과 같은 RDBMS의 데이터베이스는 대부분 <strong>디스크(Disk)</strong>에 데이터를 저장한다.
하지만, Redis는 <strong>메모리(RAM)</strong>에 데이터를 저장한다. 디스크(Disk)보다 메모리(RAM)에서의 데이터 처리속도가 월등하게 빠르기 때문에, Redis의 데이터 처리 속도가 RDBMS에 비해 훨씬 빠른 것이다.</li>
</ul>
<br />
<br />

<h2 id="💡-redis의-주요-사용-사례">💡 Redis의 주요 사용 사례</h2>
<ul>
<li>캐싱 (Caching)</li>
<li>세션 관리 (Session Management)</li>
<li>실시간 분석 및 통계 (Real-time Analystics)</li>
<li>메시지 큐 (Message Queue)</li>
<li>지리공간 인덱싱 (Geospatial Indexing)</li>
<li>속도 제한 (Rate Limiting)</li>
<li>실시간 채팅 및 메시징 (Real-time Chat And Messaging)</li>
</ul>
<blockquote>
<p>레디스(Redis)에 내장된 기능이 다양하다 보니 여러 용도로 사용된다.
이 중 가장 많이 사용되는 용도는 <strong>캐싱(데이터 조회 성능 향상)</strong>이라고 할 수 있다.</p>
</blockquote>
<br />

<h2 id="⚙️-redis-설치-방법">⚙️ Redis 설치 방법</h2>
<ul>
<li><a href="https://velog.io/@ryuneng2/Windows11-%EB%A0%88%EB%94%94%EC%8A%A4-Redis-%EC%84%A4%EC%B9%98%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95">
https://velog.io/@ryuneng2/Windows11-레디스-Redis-설치하는-방법</a>

</li>
</ul>
<br />
<br />

<hr />
<p>References</p>
<ul>
<li><a href="https://www.inflearn.com/course/%EB%B9%84%EC%A0%84%EA%B3%B5%EC%9E%90-redis-%EC%9E%85%EB%AC%B8-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94/dashboard">인프런 강의 &lt;비전공자도 이해할 수 있는 Redis 입문/실전 (조회 성능 최적화편)&gt; 섹션 2</a></li>
<li><a href="https://ko.wikipedia.org/wiki/%EB%A0%88%EB%94%94%EC%8A%A4">https://ko.wikipedia.org/wiki/%EB%A0%88%EB%94%94%EC%8A%A4</a></li>
</ul>