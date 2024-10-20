<h2 id="❓-부하-테스트란">❓ 부하 테스트란?</h2>
<blockquote>
<p>부하 테스트란, <strong>임계값 한계에 도달할 때까지</strong> 시스템의 부하를 지속적으로 꾸준히 증가시켜 시스템의 성능을 테스트하는 것이다.</p>
</blockquote>
<br />

<h2 id="✅-부하-테스트에서-자주-사용하는-용어">✅ 부하 테스트에서 자주 사용하는 용어</h2>
<h3 id="❔-throughput">❔ Throughput</h3>
<blockquote>
<p><strong>서비스가 1초 당 처리할 수 있는 작업량</strong>
* 단위 : <strong>TPS</strong>(Transaction Per Seconds, 1초 당 처리한 트랜잭션의 수)</p>
</blockquote>
<ul>
<li>만약 내가 만든 서비스가 1초에 최대 100개의 API 요청을 처리할 수 있다면,
이 서비스의 <strong>Throughput</strong>은 <strong>100 TPS</strong> 라고 얘기한다.</li>
</ul>
<br />

<hr />
<h2 id="⚙️-부하-테스트를-위한-환경-세팅">⚙️ 부하 테스트를 위한 환경 세팅</h2>
<h3 id="❔-k6">❔ K6</h3>
<ul>
<li>높은 정확도와 고부하를 발생시킬 수 있는 <strong>부하 테스트 툴</strong>
<code>사용자인 척 요청을 보내는 툴이다.</code></li>
</ul>
<br />

<h3 id="✔️-windows-k6-설치-방법">✔️ Windows K6 설치 방법</h3>
<h4 id="1-관리자-모드로-powershell-실행">1. 관리자 모드로 PowerShell 실행</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/3166698c-e527-4452-80fd-0a14e8080773/image.png" /></p>
<br />

<h4 id="2-chocolatey-패키지-매니저-설치">2. Chocolatey 패키지 매니저 설치</h4>
<ul>
<li>아래 명령어 입력<pre><code>Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))</code></pre><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/37d7641d-fa3d-4ffa-b538-ab2aa776fce9/image.png" /></li>
</ul>
<br />

<h4 id="3-chocolatey-패키지-매니저-설치-확인">3. Chocolatey 패키지 매니저 설치 확인</h4>
<ul>
<li><code>choco</code> 명령어 입력
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/71952524-f262-4bf3-a017-8463e5b647a3/image.png" /></li>
</ul>
<br />

<h4 id="4-k6-설치">4. k6 설치</h4>
<ul>
<li><code>choco install k6 --version 0.34.1</code> 명령어 입력
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/467f0ec2-299c-4475-841c-ba92b9cf8c40/image.png" /></li>
</ul>
<br />

<h4 id="5-k6-설치-확인">5. k6 설치 확인</h4>
<ul>
<li><code>k6</code> 명령어 입력
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/f97c67db-8787-49d1-8645-303a4d4fe02b/image.png" /></li>
</ul>
<br />

<h4 id="✔️-설치-완료-">✔️ 설치 완료 !</h4>
<br />
<br />

<hr />
<p>Reference</p>
<ul>
<li><a href="https://www.inflearn.com/course/%EB%B9%84%EC%A0%84%EA%B3%B5%EC%9E%90-redis-%EC%9E%85%EB%AC%B8-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94/dashboard">인프런 강의 &lt;비전공자도 이해할 수 있는 Redis 입문/실전 (조회 성능 최적화편)&gt; 섹션 8</a></li>
<li><a href="https://seongwon.dev/ETC/20220919-%EC%84%B1%EB%8A%A5%ED%85%8C%EC%8A%A4%ED%8A%B8-%EB%B6%80%ED%95%98%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%8A%A4%ED%8A%B8%EB%A0%88%EC%8A%A4%ED%85%8C%EC%8A%A4%ED%8A%B8%EB%9E%80/">https://seongwon.dev/ETC/20220919-성능테스트-부하테스트-스트레스테스트란</a></li>
<li><a href="https://subeen-lab.tistory.com/128">https://subeen-lab.tistory.com/128</a></li>
<li><a href="https://yscho03.tistory.com/101#google_vignette">https://yscho03.tistory.com/101#google_vignette</a></li>
</ul>