<h2 id="⚠️-오류">⚠️ 오류</h2>
<ul>
<li><strong>Table 'xxx.BATCH_JOB_INSTANCE' doesn't exist</strong>
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/c6fad0d9-95d2-413d-8d7a-7afe2339a8bb/image.png" /></li>
</ul>
<br />
<br />

<h2 id="🔍-원인">🔍 원인</h2>
<blockquote>
<p>BATCH를 실행시키기 위해서는 <strong>Spring Batch 정보를 저장하는 몇가지 테이블이 필요</strong>하다.
  원래는 application.yml에 <code>spring.batch.jdbc.initalize-schema=always</code> 설정을 추가하면
  자동으로 BATCH 테이블이 생성되어야 하는데, <strong>Spring Boot 3.X 버전부터는 자동으로 생성해주지 않는다</strong>고 한다.</p>
</blockquote>
<br />
<br />

<hr />
<h2 id="💡-해결-방법">💡 해결 방법</h2>
<ul>
<li>수동으로 SpringBatch 라이브러리 내에 있는 <strong>schema-[데이터 타입].sql</strong>을 실행한다.</li>
<li>External Libraries &gt; <strong>org.springframework.batch.core</strong> 모듈 하위에 있는
<strong>schema-[프로젝트에서 사용하는 DB]-sql</strong> 파일을 열어서 실행해주면 된다!
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/fd64e5fe-e0be-4aaf-8d18-16b38aa80b68/image.png" /></li>
<li>나는 MySQL 사용중이라 <code>schema-mysql.sql</code> 파일 열어서 실행했다.
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/646706e4-72e3-4361-a784-8e32e8f22b9e/image.png" /></li>
</ul>
<br />
<br />

<hr />
<h2 id="✅-결과">✅ 결과</h2>
<ul>
<li>매우 매우 잘 실행되고 <strong>DB에 Insert까지 정상적으로 완료</strong>된 것을 확인할 수 있다.
<code>&gt; TMI : 배치 처음 작업해본 건데 안되는 줄 알고 긴장하다가 감격.. 정말 감격</code>
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/b0777b60-b366-43b5-91b1-b5a41dffed3a/image.png" />
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/bfa33e99-3a27-4a29-92b3-5b9ead46aac3/image.png" /></li>
</ul>
<br />
<br />

<hr />
<p>References</p>
<ul>
<li><a href="https://jaeseo0519.tistory.com/395">https://jaeseo0519.tistory.com/395</a></li>
<li><a href="https://4h-developer.tistory.com/28">https://4h-developer.tistory.com/28</a></li>
<li><a href="https://wylee-developer.tistory.com/68">https://wylee-developer.tistory.com/68</a></li>
</ul>