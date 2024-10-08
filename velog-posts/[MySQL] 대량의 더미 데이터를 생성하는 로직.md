<blockquote>
<p>새로운 프로젝트를 시작할 때마다 수작업으로 여러 건의 더미 데이터를 생성하는 작업이 번거로웠는데,
강의를 통해 대량의 더미 데이터를 쉽게 생성하는 방법을 알게 되어 정리해본다.
간단한 쿼리만 실행하면 1,000,000건의 데이터도 빠르게 생성할 수 있다!</p>
</blockquote>
<br />

<h3 id="🧑🏻💻-더미-데이터-생성-쿼리-실행">🧑🏻‍💻 더미 데이터 생성 쿼리 실행</h3>
<ul>
<li>데이터베이스 콘솔창에서 아래와 같은 형식의 쿼리를 작성 후 실행하면 된다.
<code>해당 로직은 MySQL 8.0 이상에서만 사용 가능하다.</code></li>
</ul>
<pre><code class="language-sql">-- 높은 재귀(반복) 횟수를 허용하도록 설정
-- (아래에서 생성할 더미 데이터의 개수와 맞춰서 작성하면 된다.)
SET SESSION cte_max_recursion_depth = 1000000;

-- boards 테이블에 더미 데이터 삽입
INSERT INTO boards (title, content, created_at)
WITH RECURSIVE cte (n) AS
(
    SELECT 1
    UNION ALL
    SELECT n + 1 FROM cte WHERE n &lt; 1000000 -- 생성하고 싶은 더미 데이터의 개수
)
SELECT
    CONCAT('Title', LPAD(n, 7, '0')) AS title, -- 'Title' 다음에 7자리 숫자로 구성된 제목 생성
    CONCAT('Content', LPAD(n, 7, '0')) AS content, -- 'Content' 다음에 7자리 숫자로 구성된 내용 생성
    TIMESTAMP(DATE_SUB(NOW(), INTERVAL FLOOR(RAND() * 3650 + 1) DAY) + INTERVAL FLOOR(RAND() * 86400) SECOND) AS created_at
                                                                                        -- 최근 10년 내의 임의의 날짜와 시간 생성
FROM cte;

-- 1,000,000건의 데이터 잘 생성되었는지 확인
select count(*) from boards;</code></pre>
<br />

<h3 id="💡-성공">💡 성공</h3>
<ul>
<li>쿼리 실행 완료 후, 1,000,000건의 데이터가 성공적으로 생성된 것을 확인할 수 있다!
<img alt="" src="https://velog.velcdn.com/images/ryuneng2/post/f4414a8e-04ec-455d-afa4-ca70604ccda8/image.png" /></li>
</ul>
<br />
<br />

<hr />
<p>References</p>
<ul>
<li><a href="https://www.inflearn.com/course/%EB%B9%84%EC%A0%84%EA%B3%B5%EC%9E%90-redis-%EC%9E%85%EB%AC%B8-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94/dashboard">인프런 강의 &lt;비전공자도 이해할 수 있는 Redis 입문/실전 (조회 성능 최적화편)&gt; 섹션 5</a></li>
</ul>