## API

### REST API (=RESTful API)

<div class="content-box">
<li> API가 시장에 주로 도입된 이후, API 어떻게 설계하고 명명하고 구조해야 하는지 규약, 규격이 없었음</li>
<li> 그 결과로 프론트엔드와 백엔드 개발자는 많은 시간을 API 문서 이해에 사용</li>
<li> REST API는 효율적인 목적으로 개발되어 널리 사용됩니다. </li>
</div>

### RESTFUL API설계시 

<ul>
  <li><strong>URI(Endpoint) 설계:</strong>
    <ul>
      <li>자원을 나타내는 URI를 명확하게 설계합니다.</li>
      <li>URI는 명사형으로 사용하고, 복수형 명사를 사용하여 컬렉션을 표현합니다.</li>
      <li>자원을 식별하기 위해 ID나 다른 고유한 식별자를 사용합니다.</li>
    </ul>
  </li>
  <li><strong>HTTP Method 활용:</strong>
    <ul>
      <li>HTTP Method를 올바르게 활용하여 각 작업을 수행합니다.</li>
      <li>GET: 데이터 읽기</li>
      <li>POST: 새로운 데이터 생성</li>
      <li>PUT: 데이터 수정</li>
      <li>DELETE: 데이터 삭제</li>
      <li>PATCH: 일부 데이터 업데이트 (선택적으로 사용)</li>
    </ul>
  </li>
  <li><strong>HTTP 상태 코드:</strong>
    <ul>
      <li>각 응답에 적절한 HTTP 상태 코드를 반환합니다.</li>
      <li>200 OK, 201 Created, 204 No Content, 400 Bad Request, 404 Not Found, 500 Internal Server Error 등을 사용합니다.</li>
    </ul>
  </li>
  <li><strong>URI 파라미터와 쿼리 스트링 활용:</strong>
    <ul>
      <li>필요한 경우 URI 파라미터 또는 쿼리 스트링을 사용하여 추가 정보를 전달합니다.</li>
    </ul>
  </li>
  <li><strong>헤더 활용:</strong>
    <ul>
      <li>HTTP 헤더를 사용하여 데이터 형식, 권한 등의 정보를 전달합니다.</li>
      <li>Content-Type, Authorization 등을 활용합니다.</li>
    </ul>
  </li>
  <li><strong>JSON 또는 XML 응답:</strong>
    <ul>
      <li>데이터 형식으로 JSON 또는 XML을 사용합니다.</li>
      <li>응답 데이터는 클라이언트가 이해하기 쉽게 구조화되어야 합니다.</li>
    </ul>
  </li>
  <li><strong>에러 처리:</strong>
    <ul>
      <li>에러가 발생할 경우 적절한 에러 응답을 반환합니다.</li>
      <li>에러 메시지와 에러 코드를 포함하여 클라이언트에게 에러 정보를 제공합니다.</li>
    </ul>
  </li>
  <li><strong>HATEOAS (Hypermedia as the Engine of Application State):</strong>
    <ul>
      <li>클라이언트에게 다음 가능한 작업에 대한 링크를 제공하여 연결된 API를 만듭니다.</li>
      <li>예를 들어, 리소스에 대한 링크가 포함된 응답을 반환하여 클라이언트가 쉽게 탐색할 수 있도록 합니다.</li>
    </ul>
  </li>
  <li><strong>보안:</strong>
    <ul>
      <li>API 보안을 위해 HTTPS를 사용하고, 권한 및 인증을 구현합니다.</li>
      <li>사용자 인증 및 권한 부여를 관리하여 데이터 보호를 보장합니다.</li>
    </ul>
  </li>
  <li><strong>테스트:</strong>
    <ul>
      <li>코드의 유단 테스트 및 통합 테스트를 수행하여 API의 안정성을 확인합니다.</li>
    </ul>
  </li>
  <li><strong>문서화:</strong>
    <ul>
      <li>API를 사용하는 방법을 상세하게 문서화하여 개발자에게 API를 이해하고 사용하는 데 도움을 줍니다.</li>
    </ul>
  </li>
  <li><strong>버전 관리:</strong>
    <ul>
      <li>API 버전 관리를 통해 변경사항을 관리하고 역호환성을 유지합니다.</li>
    </ul>
  </li>
</ul>


### 데이터베이스 정규화 (Database normalization)

<li> 엔티티 간 반복적인 연결을 피하고 중복된 원형 연결(Circular Reference)을 만들지 않아야 함.</li>
<li> URI(Uniform Resource Identifier), URL(Uniform Resource Locator), URN에 대한 차이를 구분</li>
<li> RESTful API Request 및 Response 설계에 중요한 요소</li>

### HTTP Request 구성요소 설계

<li>HTTP Method, URI(Endpoint)의 설계가 중요</li>
<li>HTTP Method (GET, POST, PUT, DELETE)의 목적과 사용법을 고려</li>
<li>URI는 자원을 표현하고 그 자원에 접근하는 방법을 설명해야 함</li>
<li>API 설계 시 CRUD 및 Functional 영역을 분리하는 것이 중요</li>
<li>다양한 URI, URL, Endpoint, Query 및 동사 사용 방법을 고려</li>

### HTTP Response 구성요소 설계

<li>HTTP Response의 구성요소는 상태 라인, 헤더, 본문으로 구성</li>
<li>상태 코드 (Status Code)를 사용하여 요청 결과를 표시</li>
<li>상태 코드는 Informational, Successful, Redirection, Client Error, Server Error 등으로 구분</li>
<li>각 상태 코드는 HTTP 버전, 상태 코드, 상태 메시지로 구성</li>
<li>헤더 (Headers)는 Response 부가 정보를 반환하고 Content-type을 사용하여 데이터 형식을 표현</li>
<li>본문 (Body)에는 실제 데이터가 포함</li>