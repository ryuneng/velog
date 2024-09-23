<p><code>* 매우 쉬움 주의</code></p>
<h2 id="1-redis-설치-프로그램-다운로드">1. Redis 설치 프로그램 다운로드</h2>
<ul>
<li>아래 링크에 접속하여 <strong>msi 확장자</strong>의 최신 버전 Redis 설치 프로그램을 다운받는다.
<a href="https://github.com/microsoftarchive/redis/releases">https://github.com/microsoftarchive/redis/releases</a></li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/f85bd145-9730-4aba-b52a-b81b3d6db31f/image.png" /></p>
<br />

<hr />
<h2 id="2-설치-프로그램-실행">2. 설치 프로그램 실행</h2>
<p><code>&gt; 사실 계속 Next만 누르면 된다.</code></p>
<h4 id="1-next-클릭">1) Next 클릭</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/b271d894-aa17-4a34-b0fd-791d2625849b/image.png" /></p>
<h4 id="2-동의-체크-후-next-클릭">2) 동의 체크 후 Next 클릭</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/f88354d8-4fb0-4ccf-95ca-ff60c0d7e879/image.png" /></p>
<h4 id="3-설치할-경로-지정">3) 설치할 경로 지정</h4>
<ul>
<li>변경하지 않고 기본 경로 그대로 지정하는 것이 일반적
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/bf1325db-ce57-409c-b220-6a399a2dc168/image.png" /></li>
</ul>
<h4 id="4-포트-설정">4) 포트 설정</h4>
<ul>
<li>Redis의 기본 포트인 6379 그대로 설정하는 것이 일반적
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/379201af-2cbf-44f7-88c3-6a02f86a98ca/image.png" /></li>
</ul>
<h4 id="5-redis에-할당할-메모리-크기-지정">5) Redis에 할당할 메모리 크기 지정</h4>
<ul>
<li>기본 100MB 그대로 지정하는 것이 일반적
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/c4dfc1d9-5f52-4d28-b3e8-0ceba9be62bb/image.png" /></li>
</ul>
<h4 id="6-install-클릭하여-설치-진행">6) Install 클릭하여 설치 진행</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/00e11c91-9766-408c-bed2-8eb7d2abf315/image.png" /></p>
<h3 id="설치-완료">설치 완료!</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/80c2f874-953f-405d-99dc-0690837ca484/image.png" /></p>
<br />

<hr />
<h2 id="✅-설치-확인">✅ 설치 확인</h2>
<ul>
<li>Ctrl+Shift+Esc를 눌러 <strong>윈도우 작업 관리자</strong>를 열면, <strong>서비스 탭</strong>에서 Redis가 실행중인 것을 확인할 수 있다.
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/4b97244e-2117-4845-b79e-b878f1d07e33/image.png" /></li>
</ul>
<br />

<hr />
<h2 id="💡-redis-사용하기">💡 Redis 사용하기</h2>
<ul>
<li>1) Redis를 설치한 경로를 열어 <strong>redis-cli.exe</strong>를 실행한다.
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/3b78b299-697e-46e2-8fa2-698d9d74fdde/image.png" /></li>
</ul>
<br />

<ul>
<li>2) 터미널이 뜨면, 레디스가 잘 작동되는지 테스트해본다.</li>
<li><em><code>ping</code> 명령어를 입력*</em>하고, <code>PONG</code>이라는 응답을 받음으로써 정상적으로 실행되는 것을 확인할 수 있다!
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/d2176ecb-fc2d-4cea-9093-9209a053f386/image.png" /></li>
</ul>
<br />

<h3 id="🧐-redis-기본-명령어-알아보기">🧐 Redis 기본 명령어 알아보기</h3>
<ul>
<li><a href="https://velog.io/@ryuneng2/Redis-%EB%A0%88%EB%94%94%EC%8A%A4-%EA%B8%B0%EB%B3%B8-%EB%AA%85%EB%A0%B9%EC%96%B4">
https://velog.io/@ryuneng2/Redis-레디스-기본-명령어</a>

</li>
</ul>
<br />
<br />

<hr />
<p>References</p>
<ul>
<li><a href="https://ittrue.tistory.com/318">https://ittrue.tistory.com/318</a></li>
<li><a href="https://inpa.tistory.com/entry/REDIS-%F0%9F%93%9A-Window10-%ED%99%98%EA%B2%BD%EC%97%90-Redis-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0">https://inpa.tistory.com/entry/REDIS-%F0%9F%93%9A-Window10-%ED%99%98%EA%B2%BD%EC%97%90-Redis-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0</a></li>
</ul>