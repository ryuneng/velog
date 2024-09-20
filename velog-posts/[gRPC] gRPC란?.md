<blockquote>
<p><strong>RPC(Remote Procedure Calls)</strong>란?</p>
</blockquote>
<ul>
<li>다른 컴퓨터에 있는 기능을 <strong>마치 로컬에서 실행하는 것처럼</strong> 사용할 수 있게 해주는 프로토콜</li>
</ul>
<h2 id="❓-gprc-google-remote-procedure-calls">❓ gPRC (Google Remote Procedure Calls)</h2>
<ul>
<li>구글에서 만든 RPC 프레임워크</li>
<li><strong><code>Stub</code> 객체</strong>는 서버의 대리인 역할을 한다. Stub을 통해 원격 서버의 메서드를 호출할 수 있으며, 마치 로컬의 객체처럼 사용할 수 있다.
즉, 클라이언트는 서버나 통신 과정에 대해 자세히 알 필요 없이 <strong>로컬 함수처럼 원격 기능을 쉽게 호출</strong>할 수 있다.</li>
<li><strong>gRPC</strong>는 프로토콜 버퍼(Protocol Buffers)를 사용해, 정의된 사양에 따라 언어와 환경이 다른 시스템 간에도 원활하게 통신할 수 있도록 지원한다.</li>
<li><strong><code>.proto</code> 파일</strong>은 서버와 클라이언트가 <strong>주고받을 메시지 형식에 대한 명세</strong>를 담고 있으며, 이를 기반으로 양측은 메시지를 작성하고 해석한다.</li>
</ul>
<br />

<h4 id="💡-proto-파일-예시">💡 <code>.proto</code> 파일 예시)</h4>
<p><code>아래 코드는 서버에서 구현되고, 클라이언트에서 호출하는 함수가 된다.</code></p>
<pre><code>service LoanService {
  rpc CheckBookAvailability(BookRequest) returns (BookResponse) {}
}

message BookRequest {
  string book_id = 1;
  int32 quantity = 2;
}

message BookResponse {
  bool available =        1;
  string location =       2;
  float rentalPrice =     3;
  int32 rentalDays =      4;
  string additionalInfo = 5;
}</code></pre><br />

<h4 id="☑️-grpc의-장점--http2-기반-통신"><strong>☑️ gRPC의 장점 : HTTP/2 기반 통신</strong></h4>
<ul>
<li><p>RESTful API에서 주로 사용되는 <strong>HTTP/1.1</strong> 방식은 편지처럼 요청과 응답을 주고받는 구조다.
클라이언트가 편지들(요청)을 보내면 서버는 이를 하나씩 처리해 답장(응답)을 돌려주는데, 앞선 요청이 지연되면 뒤따르는 요청들도 대기하게 된다.</p>
</li>
<li><p>반면, <strong>HTTP/2</strong>는 전화 통화처럼 <strong>양방향 동시 통신</strong>이 가능하다.
여러 요청을 한 번에 보내고, 순서에 관계 없이 처리된 응답을 받을 수 있다.</p>
</li>
<li><p>따라서, gRPC를 통해 서버와 클라이언트는 <strong>더 빠르고 유연하게 소통</strong>할 수 있다.</p>
</li>
</ul>
<br />

<hr />
<p>Reference</p>
<ul>
<li><a href="https://www.youtube.com/watch?v=uwrR5e5_FH8">https://www.youtube.com/watch?v=uwrR5e5_FH8</a></li>
</ul>