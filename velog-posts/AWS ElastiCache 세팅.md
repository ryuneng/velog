<blockquote>
<p>AWS 인프라 구성 과정에서 AWS ElasticCache 세팅하는 방법을 정리해본다.</p>
</blockquote>
<h3 id="1-aws--elasticcache-검색-및-진입">1. AWS &gt; ElasticCache 검색 및 진입</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/d08d84a4-de77-4522-a36e-c1b6cc021555/image.png" /></p>
<br />

<h3 id="2-지금-시작--redis-oss-클릭">2. 지금 시작 &gt; Redis OSS 클릭</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/e93b1fde-0f8d-4b1f-8a5e-1c749400ae82/image.png" /></p>
<br />

<h3 id="3-클러스터-설정">3. 클러스터 설정</h3>
<blockquote>
<p><strong>클러스터란?</strong> 여러 캐시 서버(노드)를 이루는 한 단위의 그룹</p>
</blockquote>
<ul>
<li>아래처럼 설정 후 다음 클릭
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/7f8d1558-ede4-4268-a739-2b9526425079/image.png" /></li>
</ul>
<br />

<h3 id="4-보안그룹-생성">4. 보안그룹 생성</h3>
<p>0) 기존 창 그대로 둔 상태에서</p>
<p>1) EC2 진입 (우클릭해서 새 탭 or 새 창에서 열기)
2) 보안 그룹 메뉴 클릭
3) 보안 그룹 생성 클릭
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/53549fe3-fa24-447d-879d-6c37d1fb7b7e/image.png" /></p>
<p>4) <strong>보안 그룹 정보 작성 및 생성</strong></p>
<ul>
<li>보안 그룹 이름, 설명 원하는 대로 작성</li>
<li>인바운드 규칙<ul>
<li>포트 범위 6379</li>
<li>소스 Anywhere IPv4</li>
</ul>
</li>
<li>아웃바운드 규칙은 설정하지 않고 보안 그룹 생성 클릭</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/5c4adc66-6fa7-4d0e-812e-0dc9c21f5ec7/image.png" /></p>
<p>5) 보안 그룹 생성 완료
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/2080ee9a-b781-4bf7-a085-b06cf13c37d9/image.png" /></p>
<br />

<h3 id="5-elasticcache-보안-그룹-선택">5. ElasticCache 보안 그룹 선택</h3>
<p>1) 다시 이전창 (ElasticCache 클러스터 설정 창)으로 돌아와서 <strong>보안 그룹 관리</strong> 클릭
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/eef1e877-fbab-4059-ab7d-33540853831a/image.png" /></p>
<p>2) <strong>새로고침</strong> 클릭
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/4f619a9e-a737-4cbd-a79c-0d104ac51674/image.png" /></p>
<p>3) <strong>방금 만든 보안 그룹 선택</strong>
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/c35af8bc-4b12-4a94-aa13-29d99ac67e6e/image.png" /></p>
<br />

<h3 id="6-자동-백업-해지-후-다음-클릭">6. 자동 백업 해지 후 다음 클릭</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/b776b76c-610e-4074-b7eb-eebe6361c1fc/image.png" /></p>
<br />

<h3 id="7-생성-클릭">7. 생성 클릭</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/3cfb38a6-7b93-4dca-81cd-6d2d5860e35c/image.png" /></p>
<br />

<h3 id="✅-생성-완료-">✅ 생성 완료 !</h3>
<ul>
<li>Creating 상태는 생성 중이라는 뜻이고, 생성 완료까지는 시간이 조금 걸린다.
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/22caabc8-7c50-4242-bfc4-383ac9e80e18/image.png" /></li>
<li>약 12분 후 상태가 Available로 바뀌었다. 정상적으로 ElastiCache가 생성되었다는 뜻!
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/fef966b3-12a8-49d8-af55-6c784d2e51d5/image.png" /></li>
</ul>
<br />
<br />

<hr />
<p>Reference</p>
<ul>
<li><a href="https://www.inflearn.com/course/%EB%B9%84%EC%A0%84%EA%B3%B5%EC%9E%90-redis-%EC%9E%85%EB%AC%B8-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94/dashboard">인프런 강의 &lt;비전공자도 이해할 수 있는 Redis 입문/실전 (조회 성능 최적화편)&gt; 섹션 10</a></li>
</ul>