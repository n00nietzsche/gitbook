## CORS의 정의?

`Cross-Origin Resource Sharing(CORS)`는 서버가 브라우저가 리소스 로드를 허용해야 하는 출처(Origin)이 아닌 도메인, `스키마` 혹은 포트가 다른 출처를 나타낼 수 있도록 하는 HTTP 헤더 기반 매커니즘이다. `CORS`는 또한 해당 서버가 실제 요청을 허가할지 확인하기 위해 브라우저가 `cross-origin` 리소스를 호스팅하는 서버로 `"preflight"` 요청을 하는 매커니즘에 의존한다. `preflight` 중에, 브라우저는 HTTP 메소드를 나타내는 헤더와 실제 요청에 사용될 헤더를 전송한다.

> `URL 스키마`란?
`scheme` `://` `userinfo@` `host` `:port` `/path` `?query` `#fragment`
위와 같은 URL 구조 중에 `http` `https` `ftp` `blob` 등에 해당하는 부분을 스키마라고 한다.

> `cross-origin`이란? 위 URL구조 중 앞의 3개 `scheme`, `host`, `port` 중 하나라도  다른 출처를 가지는 출처끼리 통신하는 것을 말한다.

`cross-origin` 요청의 예는 `https://domain-a.com`으로부터 제공된 프론트엔드 자바스크립트 코드가 `https://domain-b.com/data.json`에 대한 요청을 보내기 위해 [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)를 사용하는 것이다.

보안상의 이유 때문에, 브라우저는 스크립트에서 시작되는 `cross-origin` HTTP 요청을 금지한다. 이를테면 `XMLHttpRequest`와 [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)는 [동일-출처 정책(same-origin policy)](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)을 따른다. 해당 API를 사용하는 웹 애플리케이션은 다른 출처에서의 응답이 올바른 CORS 헤더를 포함하지 않는다면, 애플리케이션이 로드된 같은 출처로부터만 리소스를 요청할 수 있다.

![](https://images.velog.io/images/jakeseo_me/post/bb0d7dd3-503a-4cd0-9a34-f4dbc6d73907/image.png)

CORS 매커니즘은 안전한 `cross-origin` 요청과 브라우저와 서버사이의 데이터 전송을 지원한다. 현대 브라우저들은 `cross-origin` HTTP 요청의 위험을 완화하기 위해서 `XMLHttpRequest` 혹은 `Fetch`와 같은 API에서 CORS를 사용한다.

## 이 글을 읽어야 하는 사람은?

사실 모두 읽어야 한다.

더 구체적으로는, 웹 관리자, 서버 개발자, 프론트엔드 개발자들이다. 현대 브라우저는 헤더와 정책 시행을 포함한 `cross-origin` 쉐어링의 클라이언트 사이드를 다룬다. 하지만, CORS 표준은 서버가 새로운 요청과 응답 헤더를 다뤄야만 한다는 것을 의미한다.

## 어떤 요청이 CORS를 사용하는가?

[cross-origin sharing standard](https://fetch.spec.whatwg.org/#http-cors-protocol)가 다음에 대해 `cross-site` HTTP 요청을 가능하게 해준다.

- `XMLHttpRequest` 혹은 `Fetch APIs`의 호출
- 웹 폰트 (CSS 내부 `@font-face` cross-domain 폰트 사용을 위해)
  - [서버는 사용이 허락된 웹사이트에 의해 cross-site로 로드되고 사용되는 TrueType 폰트를 배포할 수 있다.](https://www.w3.org/TR/css-fonts-3/#font-fetching-requirements)
- [WebGL 텍스쳐](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API/Tutorial/Using_textures_in_WebGL)
- [drawImage()](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/drawImage)를 이용하여 캔버스에 그려진 이미지/비디오 프레임들
- [이미지로부터 생겨난 CSS 도형들](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Shapes/Shapes_From_Images)
