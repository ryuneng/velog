<h3 id="✅-1-env--db_name-다르게-수정">✅ 1. .env &gt; DB_NAME 다르게 수정</h3>
<p><strong>1) 인증서버</strong></p>
<pre><code class="language-env">DB_HOST=localhost
DB_PORT=3306
DB_NAME=[인증서버 DB 이름] // 상이
DB_USER=[사용자명]
DB_PASSWORD=[비번입력]</code></pre>
<p><strong>2) 자원서버</strong></p>
<pre><code class="language-env">DB_HOST=localhost
DB_PORT=3306
DB_PORT_RESOURCE=3307    // 추가
DB_NAME=[자원서버 DB 이름] // 상이
DB_USER=[사용자명]
DB_PASSWORD=[비번입력]</code></pre>
<br />
<br />

<h3 id="✅-2-docker-composeyml--ports-수정-후-각각-실행">✅ 2. docker-compose.yml &gt; ports 수정 후 각각 실행</h3>
<p><strong>1) 인증서버 <code>3306:3306</code></strong> 형식
<strong>2) 자원서버 <code>3307:3306</code></strong> 형식</p>
<ul>
<li><code>docker-compose.yml 코드 (해당 파일 또한 인증서버, 자원서버에 각각 존재)</code><pre><code class="language-yaml">version: &quot;3.9&quot;
</code></pre>
</li>
</ul>
<p>services:
  mariadb:
    image: mariadb:latest
    container_name: gold-auth-mariadb
    ports:
      - ${DB_PORT}:${DB_PORT}          # 인증서버 ver. 
      - ${DB_PORT_RESOURCE}:${DB_PORT} # 자원서버 ver. 
    restart: always
    environment:
      MARIADB_DATABASE: ${DB_NAME}
      MARIADB_ROOT_PASSWORD: ${DB_PASSWORD}
    volumes:
      - mariadb-vl:/var/lib/mariadb</p>
<p>volumes:
  mariadb-vl:
    driver: local</p>
<pre><code>
&lt;br&gt;
&lt;br&gt;

---

### ✅ 3. 각각 실행 잘 됨!!
![](https://velog.velcdn.com/images/ryuneng2/post/ab92b995-25b5-485d-b947-e9f564728372/image.png)

&lt;br&gt;
&lt;br&gt;

### ✅ 4. DB 스키마  생성 시 Database 이름, Port 번호 다르게 입력
**1) 인증서버 - Port 3306**
![](https://velog.velcdn.com/images/ryuneng2/post/1b76b2ef-e53c-48a6-954d-b5c8f426375b/image.png)


**2) 자원서버 - Port 3307**
![](https://velog.velcdn.com/images/ryuneng2/post/d387ba12-9a3e-4b15-8d5a-35a94a3df4fa/image.png)

&lt;br&gt;
&lt;br&gt;

### ✅ 5. application.yml 설정하면 서버 실행까지 정상적으로 완료~!
**1) 인증서버**
```yml
server:
  port: 8888 # 상이

spring:
  application:
    name: auth-server

  # .env import
  config:
    import: optional:file:.env[.properties]

  datasource:
    url: jdbc:mariadb://${DB_HOST}:${DB_PORT}/${DB_NAME} ### 상이
    username: ${DB_USER}
    password: ${DB_PASSWORD}
    driver-class-name: org.mariadb.jdbc.Driver

  jpa:
    open-in-view: true
    hibernate:
      ddl-auto: create # 서버 실행 시 DB의 모든 테이블 삭제 후 재생성
      naming:
        use-new-id-generator-mappings: false
    show-sql: true
    properties:
      hibernate:
        id:
          new_generator_mappings: true
        format_sql: true
      dialect: org.hibernate.dialect.MariaDBDialect
    defer-datasource-initialization: true # (2.5~) Hibernate 초기화 이후 data.sql 실행</code></pre><br />

<p><strong>2) 자원서버</strong></p>
<pre><code class="language-yml">server:
  port: 9999 # 상이

spring:
  application:
    name: resource-server

  # .env import
  config:
    import: optional:file:.env[.properties]

  datasource:
    url: jdbc:mariadb://${DB_HOST}:${DB_PORT_RESOURCE}:${DB_PORT}/${DB_NAME} ### 상이
    username: ${DB_USER}
    password: ${DB_PASSWORD}
    driver-class-name: org.mariadb.jdbc.Driver

  jpa:
    open-in-view: true
    hibernate:
      ddl-auto: create # 서버 실행 시 DB의 모든 테이블 삭제 후 재생성
      naming:
        use-new-id-generator-mappings: false
    show-sql: true
    properties:
      hibernate:
        id:
          new_generator_mappings: true
        format_sql: true
      dialect: org.hibernate.dialect.MariaDBDialect
    defer-datasource-initialization: true # (2.5~) Hibernate 초기화 이후 data.sql 실행</code></pre>