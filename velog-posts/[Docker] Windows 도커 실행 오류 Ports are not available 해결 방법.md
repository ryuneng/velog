<h2 id="1-오류">1. 오류</h2>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/a3e9ed49-2c2c-4a1e-bbeb-4a4123a1bca2/image.png" /></p>
<p><code>Cannot start Docker Compose application. Reason: compose [start] exit status 1. Container tasty-track-mysql Starting Error response from daemon: Ports are not available: exposing port TCP 0.0.0.0:3306 -&gt; 0.0.0.0:0: listen tcp 0.0.0.0:3306: bind: Only one usage of each socket address (protocol/network address/port) is normally permitted.</code></p>
<br />

<h2 id="2-문제">2. 문제</h2>
<ul>
<li>3306 <strong>포트번호가 이미 사용중</strong>이어서 발생한 오류였다.</li>
</ul>
<br />

<h2 id="3-해결방법">3. 해결방법</h2>
<ol>
<li><p>PowerShell <strong>관리자 모드</strong>로 실행</p>
</li>
<li><p>명령어 입력
1) <strong>netstat -ano | findstr :3306</strong>
2) <strong>taskkill /f /pid {사용중인 번호, 예시에서는 7560}</strong> (3306 포트번호 사용하고 있는 자원 종료)
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/1fec450d-310f-4853-b9cf-27516e531ae0/image.png" /></p>
</li>
</ol>
<ol start="3">
<li>도커를  실행하는 명령어 입력<ul>
<li><strong>docker-compose -f docker-compose.yml up</strong>
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/02d858cf-8d9a-459b-accc-937c1d009d03/image.png" /></li>
</ul>
</li>
</ol>
<br />

<h2 id="4-실행-완료-">4. 실행 완료 !!</h2>
<ul>
<li>드디어 된다. 감격. 도와주신 팀원분들 감사합니다 !!!!!!
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/d3b12994-e42d-496c-961e-57bb120aa382/image.png" /></li>
</ul>
<br />

<h2 id="5-intellij에서-mysql-스키마-생성">5. IntelliJ에서 MySQL 스키마 생성</h2>
<ul>
<li>Test Connection 완료 후 OK
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/a18b1c91-9e9f-4d68-a7dc-e1440d79ed17/image.png" /></li>
</ul>
<br />

<hr />
<p>Reference</p>
<ul>
<li><a href="https://poleved.tistory.com/173">https://poleved.tistory.com/173</a></li>
</ul>