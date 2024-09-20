<blockquote>
<p>이미지가 많아서 스크롤이 길지만, 진행해보면 생각보다 간단하니 잘 따라해보자!</p>
</blockquote>
<br />

<h3 id="1-github--repository-생성">1. GitHub &gt; Repository 생성</h3>
<ul>
<li><strong>Public</strong>으로 생성
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/f7b72374-1b81-4349-a0fa-447bf61207c4/image.png" />
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/1e54ab49-99ff-4a77-a12b-7ed652bfa6c5/image.png" /></li>
</ul>
<br />

<hr />
<h3 id="2-update_blogyml-파일-생성">2. update_blog.yml 파일 생성</h3>
<p><code>&gt; GitHub action 작성</code></p>
<ul>
<li>0) <strong>creating a new file</strong> 클릭
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/3acc5241-bf67-431a-a21d-d388784cf255/image.png" /></li>
</ul>
<ul>
<li>1) 파일 경로 : <strong>.github/workflows/update_blog.yml</strong>
<code>&gt; 이대로 작성하면 폴더도 자동으로 함께 생성됨</code></li>
</ul>
<ul>
<li>2) 내용<pre><code class="language-yml">name: Update Blog Posts

</code></pre>
</li>
</ul>
<p>on:
  push:
      branches:
        - [1.브랜치명]  # 또는 워크플로우를 트리거하고 싶은 브랜치 이름
  schedule:
    # 테스트를 원한다면, <code>한국 표준시 = UTC + 9</code> 참고해서 시간 수정한 후 테스트해보기
    - cron: '0 0 * * *'  # 매일 자정에 실행, UTC 협정 세계시 기준 (한국 시간 기준 오전 9시)
    - cron: '0 15 * * *' # 매일 자정에 실행, 한국 시간 기준 (UTC 15:00)</p>
<p>jobs:
  update_blog:
    runs-on: ubuntu-latest</p>
<pre><code>steps:
- name: Checkout
  uses: actions/checkout@v2

- name: Push changes
  run: |
    git config --global user.name 'github-actions[bot]'
    git config --global user.email 'github-actions[bot]@users.noreply.github.com'
    git push https://${{ secrets.GH_PAT }}@github.com/[2.본인깃허브아이디]/velog.git

- name: Set up Python
  uses: actions/setup-python@v2
  with:
    python-version: '3.x'

- name: Install dependencies
  run: |
    pip install feedparser gitpython

- name: Run script
  run: python scripts/update_blog.py</code></pre><pre><code>- 3) **Commit changes** 클릭해서 커밋


- 참고 이미지
![](https://velog.velcdn.com/images/ryuneng2/post/c7aa3e0f-2658-4c10-b51a-b291e035ceb4/image.png)

&lt;br&gt;
&lt;br&gt;

---
### 3. update_blog.py 파일 생성
`&gt; 파이썬 스크립트 작성`
- 0) velog 리포지토리 &gt; **Add file &gt; Create new file** 클릭
![](https://velog.velcdn.com/images/ryuneng2/post/9e2bbd3c-d444-4f16-b620-c54f80a2bc72/image.png)


- 1) 파일 경로 : **scripts/update_blog.py**
  `&gt; 이대로 작성하면 폴더도 자동으로 함께 생성됨`


- 2) 내용
```py
import feedparser
import git
import os

# 벨로그 RSS 피드 URL
# example : rss_url = 'https://api.velog.io/rss/@rimgosu'
rss_url = 'https://api.velog.io/rss/@[본인벨로그아이디]'

# 깃허브 레포지토리 경로
repo_path = '.'

# 'velog-posts' 폴더 경로
posts_dir = os.path.join(repo_path, 'velog-posts')

# 'velog-posts' 폴더가 없다면 생성
if not os.path.exists(posts_dir):
    os.makedirs(posts_dir)

# 레포지토리 로드
repo = git.Repo(repo_path)

# RSS 피드 파싱
feed = feedparser.parse(rss_url)

# 각 글을 파일로 저장하고 커밋
for entry in feed.entries:
    # 파일 이름에서 유효하지 않은 문자 제거 또는 대체
    file_name = entry.title
    file_name = file_name.replace('/', '-')  # 슬래시를 대시로 대체
    file_name = file_name.replace('\\', '-')  # 백슬래시를 대시로 대체
    # 필요에 따라 추가 문자 대체
    file_name += '.md'
    file_path = os.path.join(posts_dir, file_name)

    # 파일이 이미 존재하지 않으면 생성
    if not os.path.exists(file_path):
        with open(file_path, 'w', encoding='utf-8') as file:
            file.write(entry.description)  # 글 내용을 파일에 작성

        # 깃허브 커밋
        repo.git.add(file_path)
        repo.git.commit('-m', f'Add post: {entry.title}')

# 변경 사항을 깃허브에 푸시
repo.git.push()</code></pre><ul>
<li>3) <strong>Commit changes</strong> 클릭해서 커밋</li>
</ul>
<ul>
<li>참고 이미지
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/57961562-b666-40c5-a50f-38a3d25698ab/image.png" /></li>
</ul>
<br />
<br />

<hr />
<h3 id="4-pat-권한-받기">4. PAT 권한 받기</h3>
<h4 id="1-github-계정--settings-클릭">1) GitHub 계정 &gt; Settings 클릭</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/5e78ee03-82c7-4279-a142-855cc8f8dc30/image.png" /></p>
<br />

<h4 id="2-developer-settings-클릭">2) Developer settings 클릭</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/cb705929-e8f1-48ab-a0e3-02dda86a0bf1/image.png" /></p>
<br />

<h4 id="3-personal-access-tokens--tokens-classic--generate-new-token--generate-new-token-classic-클릭">3) Personal access tokens &gt; Tokens (classic) &gt; Generate new token &gt; Generate new token (classic) 클릭</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/4ab59562-5239-45c1-9f64-f057cdc4d12c/image.png" /></p>
<br />

<h4 id="4-토큰-이름-작성--repo-workflow-체크-후-generate-token-클릭">4) 토큰 이름 작성 &amp; repo, workflow 체크 후 Generate token 클릭</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/b93ed0b0-1fae-4444-8c19-443776180495/image.png" /></p>
<br />

<h4 id="5-토큰-복사">5) 토큰 복사</h4>
<ul>
<li>이 창을 벗어나면 <strong>이후에는 토큰 확인이 불가</strong>하니 *<em>꼭 복사 !! *</em>해두기
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/d8e2d41c-73d8-411c-a463-d924f0d929cd/image.png" /></li>
</ul>
<br />

<h4 id="6-velog-리포지토리--settings--secrets-and-variables--actions--new-repository-secret-클릭">6) velog 리포지토리 &gt; Settings &gt; Secrets and Variables &gt; Actions &gt; New repository secret 클릭</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/42caecdd-e7c9-4490-9450-f9f4e0b40360/image.png" /></p>
<br />

<h4 id="7-name-secret-작성-후-add-secret-클릭">7) Name, Secret 작성 후 Add secret 클릭</h4>
<ul>
<li>Name : <strong>GH_PAT</strong></li>
<li>Secret : <strong>위에서 복사한 토큰 붙여넣기</strong>
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/199ba7c6-8e30-4a74-a82e-eb9add3607f2/image.png" /></li>
</ul>
<br />
<br />

<hr />
<h3 id="5-리포지토리에-외부-권한-부여">5. 리포지토리에 외부 권한 부여</h3>
<ul>
<li>1) velog 리포지토리 &gt; <strong>Settings</strong> 클릭</li>
<li>2) <strong>Actions &gt; General</strong> 클릭</li>
<li>3) 4개 체크 및 Save (<strong>각 항목 변경할 때마다 Save</strong>도 잊지 말고 눌러주기)
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/abc62a9b-9137-4c4a-b774-3a7755862460/image.png" /></li>
</ul>
<br />

<hr />
<h3 id="💡-성공-확인">💡 성공 확인!</h3>
<blockquote>
<p>velog-posts 폴더와 함께 벨로그에 작성한 글이 커밋된 것을 확인할 수 있다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/3b668756-2f16-4c2a-bf08-3525559c05cd/image.png" /></p>
<br />
<br />

<hr />
<p>References</p>
<ul>
<li><a href="https://velog.io/@rimgosu/velog-%EA%B8%80-%EC%9E%91%EC%84%B1-%EC%8B%9C-%EC%9E%90%EB%8F%99%EC%9C%BC%EB%A1%9C-github%EC%97%90-%EC%BB%A4%EB%B0%8B%ED%95%98%EA%B8%B0?utm_source=oneoneone">https://velog.io/@rimgosu/velog</a></li>
<li><a href="https://github.com/rimgosu/velog">https://github.com/rimgosu/velog</a></li>
<li><a href="https://velog.io/@sooozi/velog%EC%99%80-github-%EC%97%B0%EB%8F%99-%EB%B2%A8%EB%A1%9C%EA%B7%B8-%EA%B8%80%EC%93%B0%EA%B3%A0-%EC%9E%94%EB%94%94%EC%8B%AC%EA%B8%B0#3-update_blogyml%ED%8C%8C%EC%9D%BC%EC%97%90-github-action-%EC%9E%91%EC%84%B1">https://velog.io/@sooozi/velog</a></li>
</ul>