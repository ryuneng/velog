<blockquote>
<p>포트폴리오를 보완하면서 팀 프로젝트의 <strong>GitHub 리포지토리를 Fork받아 코드를 수정하고 Push</strong>해야 하는 상황이 생겼다.
이 때, Local에서 기존 프로젝트에 연결된 <strong>GitHub 저장소를 Fork받은 저장소로 변경</strong>하는 방법을 정리해본다.</p>
</blockquote>
<h3 id="1-intellij에서-변경하려는-프로젝트-오픈">1. IntelliJ에서 변경하려는 프로젝트 오픈</h3>
<ul>
<li><code>* 인텔리제이 아니어도 무관</code></li>
</ul>
<br />

<h3 id="2-새-github-저장소-url-복사">2. 새 GitHub 저장소 URL 복사</h3>
<ul>
<li>GitHub &gt; 새로운 Repository &gt; 클론 URL 복사
<code>https://github.com/[username]/[repository].git</code></li>
</ul>
<br />

<h3 id="3-기존-git-원격-저장소-변경">3. 기존 Git 원격 저장소 변경</h3>
<ul>
<li>터미널 &gt; 프로젝트 경로에서 아래 명령어 입력</li>
<li>1) <code>git remote -v</code> : 현재 원격 저장소의 URL 확인</li>
<li>2) <code>git remote set-url origin [새로운 저장소 URL]</code> : 기존 원격 저장소 URL 변경</li>
</ul>
<br />

<h3 id="4-잘-변경되었는지-확인">4. 잘 변경되었는지 확인</h3>
<ul>
<li><code>git remote -v</code></li>
</ul>
<br />

<h3 id="💡-결과">💡 결과</h3>
<ul>
<li>Local 프로젝트에 연결된 GitHub 저장소가 새로운 Repository로 변경된 것을 확인할 수 있다.
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/0cb83bf3-f8eb-4272-bceb-ca12c1fc14fb/image.png" /></li>
</ul>
<br />

<h4 id="선택-최신-코드-가져오기">선택) 최신 코드 가져오기</h4>
<ul>
<li>변경된 원격 저장소에서 최신 코드를 가져오려면 아래 명령어를 입력하면 된다.
<code>git fetch origin</code></li>
</ul>