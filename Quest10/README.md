# Quest 10. 인증의 이해

## Introduction

- 이번 퀘스트에서는 웹에서의 인증에 관해 알아보겠습니다.

## Topics

- Cookie
- Session
- JWT

## Resources

- [MDN - HTTP 쿠키](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies)
- [Cookies and Sessions](https://web.stanford.edu/~ouster/cgi-bin/cs142-fall10/lecture.php?topic=cookie)
- [JWT](https://jwt.io/)

## Checklist

- 쿠키란 무엇일까요?

> 브라우저에 저장되는 작은 크기의 문자열(최대 4KB)

> HTTP 요청 시 사용자가 요청하지 않아도 쿠키는 Header에 담겨 서버로 전송된다.

> 만료 기간을 정할 수 있다.(Expires, Max-Age ⇒ 둘 다 있다면 Expires는 무시된다.)

> 영구 쿠키(만료기간이 끝난 후 삭제), 세션 쿠키(브라우저 종료 시 삭제)

- 쿠키는 어떤 식으로 동작하나요?

> HTTP 요청 시 Headers에 담겨 자동으로 전송된다.
> 클라이언트가 페이지를 요청(request)
> 서버에서 쿠키를 생성
> HTTP 헤더에 쿠키를 포함 시켜 응답
> 브라우저가 종료되어도 쿠키 만료 기간이 있다면 클라이언트에서 보관하고 있음
> 같은 요청을 할 경우 HTTP 헤더에 쿠키를 함께 보냄
> 서버에서 쿠키를 읽어 이전 상태 정보를 변경 할 필요가 있을 때 쿠키를 업데이트 하여 변경된 쿠키를 HTTP 헤더에 포함시켜 응답

- 쿠키는 어떤 식으로 서버와 클라이언트 사이에 정보를 주고받나요?

> 클라이언트의 요청에 의해 쿠키가 생성된 응답을 받았다면,
> 클라이언트의 요청에 자동으로 쿠키가 HTTP 요청에 담겨 전송하게 되고
> 서버에서는 쿠키를 읽어 변경이 필요하면 업데이트 하여 쿠키를 다시 응답으로 보내는 방식으로 정보를 주고받는다.

- 웹 어플리케이션의 세션이란 무엇일까요?

  > 세션은 쿠키를 기반하고 있지만, 사용자 정보 파일을 브라우저에 저장하는 쿠키와 달리 세션은 서버 측에서 관리한다.
  > 서버에서는 클라이언트를 구분하기 위해 세션 ID를 부여하며 웹 브라우저가 서버에 접속해서 브라우저를 종료할 때까지 인증상태를 유지한다.
  > 물론 접속 시간에 제한을 두어 일정 시간 응답이 없다면 정보가 유지되지 않게 설정이 가능 하다.
  > 사용자에 대한 정보를 서버에 두기 때문에 쿠키보다 보안에 좋지만, 사용자가 많아질수록 서버 메모리를 많이 차지하게 된다.
  > 즉 동접자 수가 많은 웹 사이트인 경우 서버에 과부하를 주게 되므로 성능 저하의 요인이 된다.
  > 클라이언트가 Request를 보내면, 해당 서버의 엔진이 클라이언트에게 유일한 ID를 부여하는 데 이것이 세션 ID이다.

  - 세션의 ID와 내용은 각각 어디에 저장되고 어떻게 서버와 교환되나요?

  > 클라이언트가 서버에 접속 시 세션 ID를 발급 받음
  > 클라이언트는 세션 ID에 대해 쿠키를 사용해서 저장하고 가지고 있음
  > 클라리언트는 서버에 요청할 때, 이 쿠키의 세션 ID를 같이 서버에 전달해서 요청
  > 서버는 세션 ID를 전달 받아서 별다른 작업없이 세션 ID로 세션에 있는 클라언트 정보를 가져와서 사용
  > 클라이언트 정보를 가지고 서버 요청을 처리하여 클라이언트에게 응답

- JWT란 무엇인가요?

> JWT(Json Web Token): 전자 서명 된 URL-safe(URL로 이용할 수 있는 문자만 구성된) JSON
> Base64로 인코딩 또는 암호화된 데이터가 이어 붙어진 형태이다.
> JWT는 3가지 구분으로 나뉜다
> header, 헤더
> payload,페이로드
> verify signature, 서명
> JWT는 서버와 클라이언트 간 정보를 주고 받을 때 HTTP 요청 Header에 JSON 토큰을 넣어 전송하면 서버는 별도의 인증 과정없이 헤더에 포함되어 있는 JWT 정보를 통해 인증합니다.

- JWT 토큰은 어디에 저장되고 어떻게 서버와 교환되나요?

> Private Variable에 저장하는 것은 UX상 좋지 않다. ⇒ 변수는 웹페이지를 새로고침하면 사라지는 휘발성이기 때문
> 따라서 WebStorage(LocalStorage, SessionStorage) 또는 Cookie에 저장한다.
> 둘 중 정답은 없지만 장단점이 있다.

> WebStorage
> 로그인 성공 시, 서버가 토큰을 응답 정보에 넣어서 전달하고, 해당 값을 Web Storage에 저장한다.
> 그 다음부터 웹 요청 시 마다 HTTP 헤더 값에 토큰을 넣어서 요청하는 방법이다. (주로 Authorization 헤더에 Bearer 스키마를 사용해서 보낸다.)
> 장점) 구현하기 쉽고 하나의 도메인에 제한되지 않는다.

> 단점) WebStorage에 접근만 하면 바로 토큰에 접근할 수 있다.

> Cookie
> 쿠키를 정보 전송수단으로 사용하는 것일 뿐, 쿠키를 사용한다고해서 세션을 관리하는 것은 아니다.
> 이 과정에서, 서버측에서 응답을 하면서 쿠키를 설정할 때 httpOnly 값을 활성화 해주면, 네트워크 통신 상에만 토큰 정보가 들어있는 쿠키가 붙게 된다.
> 따라서, 브라우저상에서는 자바스크립트로 토큰 값에 접근하는 것이 불가능해진다.
> 장점) XSS 해킹 문제를 해결할 수 있고, 보안 수준을 높일 수 있다.

> 단점) 쿠키가 한정된 도메인에서만 사용이 되거나 CSRF 공격에 위험이 있다.

- 세션에 비해 JWT가 가지는 장점은 무엇인가요? 또 JWT에 비해 세션이 가지는 장점은 무엇인가요?

> 1. JWT 특징

> Header와 Payload를 가지고 Signature를 생성하므로 데이터 위변조를 막을 수 있다.
> 인증 정보에 대한 별도의 저장소가 필요없다.
> 클라이언트 인증 정보를 저장하는 세션과 다르게, 서버는 무상태가 됩니다.
> 확장성이 우수하다.
> 토큰 기반으로 다른 로그인 시스템에 접근 및 권한 공유가 가능하다.

> 2. 세션 특징

> 쿠키를 포함한 요청이 외부에 노출되더라도 세션 ID 자체는 유의미한 개인정보를 담고 있지 않는다.
> 그러나 해커가 이를 중간에 탈취하여 클라이언트인척 위장할 수 있다는 한계는 존재한다.
> 각 사용자마다 고유한 세션 ID가 발급되기 때문에, 요청이 들어올 때마다 회원정보를 확인할 필요가 없다.
> 서버에서 세션 저장소를 사용하므로 요청이 많아지면 서버에 부하가 심해진다.

## Quest

- 이번에는 메모장 시스템에 로그인 기능을 넣고자 합니다.
  - 사용자는 딱 세 명만 존재한다고 가정하고, 아이디와 비밀번호, 사용자의 닉네임은 하드코딩해도 무방합니다.
  - 로그인했을 때 해당 사용자가 이전에 작업했던 탭들과 마지막으로 활성화된 탭 등의 상태가 로딩 되어야 합니다.
  - 세션을 이용한 버전과, JWT를 이용한 버전 두 가지를 만들어 보세요!
    - 세션을 이용할 경우 세션은 서버의 메모리나 파일에 저장하면 됩니다.

## Advanced

- Web Authentication API(WebAuthn)은 무엇인가요?
