<h1 id="❓-docker란">❓ Docker란?</h1>
<ul>
<li>Go언어로 작성된 리눅스 <strong>컨테이너 기반</strong>의 <strong>가상화 시스템</strong></li>
<li><strong>컨테이너</strong>를 사용하여 각각의 프로그램을 <strong>분리된 환경</strong>에서 실행 및 관리할 수 있는 툴</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/5f8a0e7c-689a-4b61-8f6e-72b724382ba3/image.png" /></p>
<br />

<h2 id="🧐-docker를-쓰는-이유">🧐 Docker를 쓰는 이유</h2>
<blockquote>
<ul>
<li><strong>이식성</strong> - 특정 프로그램을 다른 곳으로 쉽게 옮겨서 설치 및 실행할 수 있는 특성</li>
</ul>
</blockquote>
<ul>
<li><p>쉽게 말해 <strong>저 컴퓨터에서는 되고, 내 컴퓨터에서는 안되는</strong> 상황을 해결해준다.</p>
</li>
<li><p>매번 귀찮은 <strong>설치 과정을 일일이 거치지 않아도 된다.</strong></p>
</li>
<li><p><strong>항상 일관되게</strong> 프로그램을 설치할 수 있다. (버전, 환경 설정, 옵션, 운영 체제 등)</p>
</li>
<li><p>각 프로그램이 독립적인 환경에서 실행되기 때문에 <strong>프로그램 간에 충돌이 발생하지 않는다.</strong></p>
</li>
</ul>
<br />
<br />

<hr />
<h2 id="💡-컨테이너container란">💡 컨테이너(Container)란?</h2>
<blockquote>
<p>하나의 컴퓨터 환경 내에서 <strong>독립적인 컴퓨터 환경</strong>을 구성해서, 각 환경에 프로그램을 별도로 설치할 수 있게 만든 개념</p>
</blockquote>
<ul>
<li><strong>윈도우 환경에서 하나의 컴퓨터로 여러 사용자를 설정</strong>하여 사용할 수 있는 것과 비슷한 개념이다. 윈도우에서 각 사용자의 환경은 독립적으로 구성되어 있어서, <em>사용자마다 필요한 프로그램을 별도로 설치</em>해주어야 한다.</li>
</ul>
<br />

<h3 id="✅-컨테이너의-독립성">✅ 컨테이너의 독립성</h3>
<ul>
<li><p><strong>디스크 (저장 공간)</strong> : 각 컨테이너마다 <strong>각자의 저장 공간</strong>을 가지고 있다. 일반적으로 A 컨테이너 내부에서 B 컨테이너 내부에 있는 파일에 접근할 수 없다.</p>
</li>
<li><p><strong>네트워크 (IP, Port)</strong> : 각 컨테이너마다 <strong>고유의 네트워크</strong>를 가지고 있다. 컨테이너는 각자의 IP 주소를 가지고 있다.</p>
</li>
</ul>
<br />
<br />

<hr />
<h2 id="💡-이미지image란">💡 이미지(Image)란?</h2>
<blockquote>
<p>프로그램을 실행하는 데 필요한 설치 과정, 설정, 버전 정보 등을 포함하고 있는 것
즉, <strong>프로그램을 실행하는 데 필요한 모든 것을 포함</strong>하고 있다.</p>
</blockquote>
<ul>
<li>닌텐도 게임기에는 다양한 칩을 꽂아서 여러 게임을 즐길 수 있다.
Docker에서 <strong>닌텐도의 칩</strong>과 같은 역할을 하는 개념이 이미지(Image)이다.</li>
</ul>
<ul>
<li>예를 들어, MySQL 서버를 <code>이미지</code>로 만들면 이 이미지를 Docker로 실행시키는 순간 MySQL 서버가 <code>컨테이너</code> 환경에서 실행된다.</li>
<li><em>MySQL을 설치하지 않고도 MySQL 데이터베이스를 사용*</em>할 수 있게 된다.</li>
</ul>
<br />
<br />

<hr />
<p>References</p>
<ul>
<li><a href="https://www.inflearn.com/course/%EB%B9%84%EC%A0%84%EA%B3%B5%EC%9E%90-docker-%EC%9E%85%EB%AC%B8-%EC%8B%A4%EC%A0%84/dashboard">인프런 강의 &lt;비전공자도 이해할 수 있는 Docker 입문/실전&gt; 섹션 2</a></li>
<li><a href="https://khj93.tistory.com/entry/Docker-Docker-%EA%B0%9C%EB%85%90">https://khj93.tistory.com/entry/Docker-Docker-%EA%B0%9C%EB%85%90</a></li>
<li><a href="https://velog.io/@rivkode/Docker-%EB%9E%80">https://velog.io/@rivkode/Docker-%EB%9E%80</a></li>
</ul>