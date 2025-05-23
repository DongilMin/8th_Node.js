## 환경 변수
- 환경 변수란 OS가 프로세스에게 제공하는 키-값 쌍 형태의 데이터이다. 프로그램은 이 환경 변수를 읽어서 설정값이나 외부 조건을 파악할 수 있다.
- 주로 비밀번호, 포트 번호, API 키 값 같은 것들을 담는다.

## CORS
- Cross-Origin Resource Sharing은 다른 출처 간에 리소스를 요청할 수 있도록 허용하거나 차단하는 웹 보안 정책이다.
- 기본적으로 웹 브라우저는 보안을 위해 다른 도메인에서 오는 요청을 차단한다. CORS는 서버가 특정 외부 출처 요청을 허용하도록 HTTP 헤더를 설정하는 규칙이다.

## DB Connection, DB Connection Pool
- DB Connection은 애플리케이션(프로그램)이 데이터베이스(DB)와 통신하기 위해 맺는 하나의 연결이다. 이 연결을 통해 프로그램은 쿼리(SQL)을 보내고 결과를 받는다. DB 서버와 프로그램이 소켓을 통해 지속적으로 연결되어 있어야 명령을 주고받을 수 있다. 보통 하나의 DB Connection은 네트워크 소켓과 세션을 유지한다.
- DB Connection Pool은 데이터베이스 연결을 매번 새로 만드는 대신, 미리 여러 개의 연결을 만들어 놓고 필요할 때마다 가져다 쓰는 방법이다. 애플리케이션은 Connection Pool에서 연결을 빌려 사용하고, 다 쓰면 반환한다. 이렇게 하면 매번 연결을 새로 생성하는 비용을 줄이고, 연결을 재사용하여 성능을 높일 수 있다.

## 비동기 (async, await)
- 비동기란 시간이 오래 걸리는 작업을 기다리지 않고 다음 작업을 진행하는 방식이다.
- async는 함수가 비동기 함수임을 선언하고, await은 비동기 작업이 끝날 때까지 기다린다.
- async/await을 쓰면 비동기 코드를 동기 코드처럼 깔끔하고 읽기 쉽게 작성할 수 있다.
- async는 함수 앞에 붙여서 해당 함수가 항상 프로미스를 반환하게 만든다.
- await은 async 함수 안에서만 사용하며, 프로미스가 해결(resolve)될 때까지 기다린다.

## try / catch / finally
- try  
  try는 코드 실행 중 오류가 발생할 가능성이 있는 부분을 감싸는 블록이다.  
  try 블록 안의 코드를 실행하다가 오류가 발생하면 즉시 중단하고 catch 블록으로 이동한다.  
  오류가 없으면 catch를 거치지 않고 try 블록의 코드를 끝까지 실행한다.

- catch  
  catch는 try 블록 안에서 오류가 발생했을 때 실행되는 블록이다.  
  catch 블록은 오류 객체를 받아서 에러 메시지를 출력하거나, 에러 상황을 처리하는 데 사용된다.

- finally  
  finally는 try 또는 catch 블록의 실행이 끝난 후 무조건 실행되는 블록이다.  
  에러가 발생하든 발생하지 않든 finally 블록은 항상 실행된다.  
  주로 리소스를 정리하거나, 연결을 끊거나, 로그를 남기는 데 사용된다.
