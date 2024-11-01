<h3 id="1-이미지-다운로드">1. 이미지 다운로드</h3>
<ul>
<li><strong>최신 버전(<code>latest</code>) 이미지 다운로드</strong><pre><code class="language-bash"># docker pull 이미지명
$ docker pull nginx # docker pull nginx:latest와 동일하게 작동</code></pre>
</li>
</ul>
<blockquote>
<h4 id="❓-이미지-다운-방식">❓ 이미지 다운 방식</h4>
</blockquote>
<ul>
<li><strong>Docker Hub</strong>라는 곳에서 이미지를 다운받는다.
Docker Hub는 <strong>이미지를 저장 및 다운받을 수 있는 저장소</strong>다.
다양한 코드들이 저장되어 있는 GitHub에서 pull을 받아 코드를 사용하는 것처럼, Docker Hub에서 pull을 통해 이미지를 다운받아 사용할 수 있다.</li>
<li>Docker Hub 🔗 <a href="https://hub.docker.com/">https://hub.docker.com/</a>
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/1f9fb826-8ca8-4303-ba5f-4232f8a4b202/image.png" /></li>
</ul>
<br />

<ul>
<li><strong>특정 버전 이미지 다운로드</strong><pre><code class="language-bash"># docker pull 이미지명:태그명
$ docker pull nginx:stable-perl</code></pre>
<blockquote>
<p>특정 버전을 나타내는 이름을 <code>태그명</code>이라고 한다.
<code>태그명</code>은 Docker Hub에서 확인할 수 있다.
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/c9695ce1-3643-44a7-8c04-f24c33b05433/image.png" /></p>
</blockquote>
</li>
</ul>
<br />

<h3 id="2-이미지-조회">2. 이미지 조회</h3>
<pre><code class="language-bash">$ docker image ls # ls : list의 약자</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/4332ec5c-3560-4fdc-9646-0d5434263ff7/image.png" /></p>
<ul>
<li>REPOSITORY : 이미지 이름 (이미지명)</li>
<li>TAG : 이미지 태그명 (버전)</li>
<li>IMAGE ID : 이미지 ID</li>
<li>CREATED : 이미지가 생성된 날짜 (다운받은 날짜 X)</li>
<li>SIZE : 이미지 크기</li>
</ul>
<br />

<h3 id="3-특정-이미지-삭제">3. 특정 이미지 삭제</h3>
<pre><code class="language-bash">$ docker image rm [이미지 ID 또는 이미지명] # rm : remove의 약자</code></pre>
<ul>
<li><p>이미지 ID를 입력할 때, ID의 전부를 입력하지 않고 <strong>일부만 입력</strong>해도 된다.
(단, ID의 일부만 입력했을 때, 입력한 ID의 일부를 가진 이미지가 단 1개여야 한다.)</p>
</li>
<li><p><strong>컨테이너에서 사용하고 있지 않은 이미지만 삭제 가능</strong>하다.</p>
</li>
</ul>
<br />

<h3 id="4-중지된-컨테이너에서-사용하고-있는-이미지-강제-삭제">4. 중지된 컨테이너에서 사용하고 있는 이미지 강제 삭제</h3>
<pre><code class="language-bash">$ docker image rm -f [이미지 ID 또는 이미지명]</code></pre>
<ul>
<li>실행중인 컨테이너에서 사용하고 있는 이미지는 강제로 삭제할 수 없다.</li>
</ul>
<br />

<h3 id="5-전체-이미지-삭제">5. 전체 이미지 삭제</h3>
<pre><code class="language-bash"># 컨테이너에서 사용하고 있지 않은 이미지만 전체 삭제
$ docker image rm $(docker images -q)

# 컨테이너에서 사용하고 있는 이미지를 포함해서 전체 이미지 삭제
$ docker image rm -f $(docker images -q)</code></pre>
<br />
<br />

<hr />
<p>References</p>
<ul>
<li><a href="https://www.inflearn.com/course/%EB%B9%84%EC%A0%84%EA%B3%B5%EC%9E%90-docker-%EC%9E%85%EB%AC%B8-%EC%8B%A4%EC%A0%84/dashboard">인프런 강의 &lt;비전공자도 이해할 수 있는 Docker 입문/실전&gt; 섹션 3</a></li>
</ul>