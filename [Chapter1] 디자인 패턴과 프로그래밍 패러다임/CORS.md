# CORS(Cross-origin Resource Sharing)

 웹 생태계에는 다른 출처로의 리소스 요청을 제한하는 것과 관련된 두 가지 정책이 존재한다. 한 가지는 CORS 이고,  다른 한 가지는 `SOP(Same-Origin Policy)`이다. 그중 CORS 가 무엇이며, 이것이 생긴 이유와 해결책 등 전반적으로 CORS에 관한 부분을 정리한다.



#### 출처(Origin)

> 출처는 **`Protocol`**과 **`Host`**, **`Port`** 의 조합을 말한다.



<figure align="center">
<img src="https://evan-moon.github.io/static/e25190005d12938c253cc72ca06777b1/21b4d/uri-structure.png" width="500">
    <figcaption style="text-align:center;color:gray">
    [출처] https://evan-moon.github.io/2020/05/21/about-cors
  </figcaption>
</figure>



**Protocol, Host, Port** 의 세 가지가 모두 같아야 **`같은 출처`** 이며, 세 가지 중 <u>하나라도</u> 다르다면 **`다른 출처`** 이다. 



## CORS 란? 

 **CORS** (Cross-Origin Resource Sharing)는 출처가 다른 자원들을 공유한다는 뜻으로, <u><b>HTTP헤더</b> 를 사용하여, 한 출처에서 실행중인 웹 애플리케이션이 <b>다른 출처(프로토콜,도메인,포트번호)의 리소스에 접근</b>할 수 있는 권한을 부여하도록 <b>브라우저</b>에 알려주는 체제</u>이다. 아래의 오류 메시지는 **CORS 정책을 위반**할 때 발생한다.



<figure align="center">
<img src="https://miro.medium.com/v2/resize:fit:1400/0*bI2yxKryqJzyUkud" width="700" >
    <figcaption style="text-align:center;color:gray">
    [출처] https://punitbatra.hashnode.dev/what-is-cors-error-and-ways-to-fix-it-1
  </figcaption>
</figure>



  CORS 오류는 한 사이트에서 '' 주소가 다른 서버 ''로 요청을 보낼 때 자주 접할 수 있으며,  웹사이트를 여는 곳. 즉, Chrome, Safari, Edge 와 같은 **`브라우저에서 다른 출처를 가지는 서버로 요청이 갈 때 발생하는 문제`** 이다.



#### SOP(Same-Origin Policy)

 CORS는 <u>SOP 제한을 우회하기 위한 매커니즘</u>으로  볼 수 있다. **SOP**는 2011년 RFC 6454 문서에서 처음 등장한 보안 정책으로, **동일한 출처 사이에서만 리소스를 공유**할 수 있다는 규칙을 가지고 있다. 이 정책은 사용자를 CSRF 나 XSS 등의 공격으로부터 보호하기 위해 시행되었다. 

 하지만, 서비스중인 어플리케이션의 프론트단에서 제 3자가 제공하는 API를 직접 호출해야하는 등  점차 웹 서비스에서 요구되는 기능들이 늘어나게 되면서, SOP는 몇 가지 **예외 조항**을 두고 이 조항에 해당하는 리소스 요청은 <u>출처가 다르더라도 허용</u>하기로 했는데, 그 중 하나가 **“CORS 정책을 지킨 리소스 요청”** 이다. 

 

## CORS 의 동작 방식

#### 기본 흐름

- 기본적으로 웹 클라이언트 어플리케이션이 다른 출처의 리소스를 요청할 때는 HTTP 프로토콜을 사용하여 요청을 보내게 되는데, 이때 브라우저는 요청 헤더의<u> `Origin`이라는 필드에 요청을 보내는 출처</u>를 함께 담아보낸다.

  ```http
  Origin: http://localhost:3000
  ```

- 이후 서버가 이 요청에 대한 응답을 할 때 응답 헤더의 `Access-Control-Allow-Origin`이라는 값에 “이 리소스를 접근하는 것이 허용된 출처”를 내려주고, 이후 응답을 받은 브라우저는 자신이 보냈던 요청의 `Origin`과 서버가 보내준 응답의 `Access-Control-Allow-Origin`을 비교해본 후 이 응답이 유효한 응답인지 아닌지를 결정한다. 

- 브라우저는 해당 응답이 유효한 응답이라면 리소스를 전달하지만, 그렇지 않은 경우 차단한다.



 CORS가 동작하는 방식은 **3가지의 시나리오에 따라 변경**된다. 따라서, 요청이 어떤 시나리오에 해당하는지 파악하는 것이 CORS 오류 해결에 있어 중요하다. CORS의 접근 제한 3가지 시나리오는 다음과 같다.



#### 1) Preflight Request

> 프리플라이트 요청

 쉽게 말해 사전 확인 작업으로, 본 요청을 보내기 전에 서버에 요청을 보내도 되는 것인지 먼저 물어보는 것이다. 따라서, Preflight 를 보내게 되면, 실질적으로 2번의 요청(preflight 사전 요청, 실제 요청)이 보내진다.

1. OPTIONS 메서드를 통해 CORS 요청이 가능한 지 확인 작업을 거친다.

2. 요청이 가능하다면 실제(Actual Request)를 보낸다.



<figure align="center">
<img src="https://velog.velcdn.com/images%2Fwiostz98kr%2Fpost%2F367d67e1-f9a6-4a0c-8a4b-0aa92e42e6da%2Fimage.png" width="600">
    <figcaption style="text-align:center;color:gray">
    [출처] https://velog.io/@wiostz98kr
  </figcaption>
</figure>




- Preflight가 필요한 이유?

  > CORS를 모르는 서버(CORS 관련된 아무런 설정이 없는 서버)를 위해서이다. 

  - 만약, CORS 에 대한 어떤한 설정도 없는 서버로 클라이언트가 본 요청을 보냈다고 하자. 
    - 이때, 서버는 응답을 보내게 되고, CORS에 대한 설정이 없으므로, Allow-origin 헤더 값 역시 존재하지 않는다. 이에 브라우저는 해당 응답을 유효한 응답이 아니라고 판단하여 차단하게 된다. (CORS 오류) 
    - 이것이 delete와 같은 요청이었다면, 서버의 데이터가 지워져 버린 후, 그제서야 CORS 오류가 터지게 된다. 
    
    

   따라서, 브라우저는 Preflight의 사전 요청을 통해(OPTIONS 메서드), 해당 서버가 CORS 설정이 있는지를 확인하여 위와 같은 상황을 방지할 수 있다. 

  

#### 2) Simple Request 

> **단순 요청(Simple Request)**은 공식 용어가 아닌, MDN에서 사용한 용어를 가져온 것이다.

 단순 요청은 예비 요청을 보내지 않고 바로 서버에게 본 요청부터 보낸 후, 서버가 이에 대한 응답의 헤더에 `Access-Control-Allow-Origin`과 같은 값을 보내주면 그때 브라우저가 CORS 정책 위반 여부를 검사하는 방식이다. 



<figure align="center">
<img src="https://velog.velcdn.com/images%2Fwiostz98kr%2Fpost%2F8291fc25-f5e7-413b-840f-51920738e020%2Fimage.png" width="600">
    <figcaption style="text-align:center;color:gray">
    [출처] https://velog.io/@wiostz98kr
  </figcaption>
</figure>



**하지만, Simple Request를 보내려면 다음의 조건을 모두 만족해야한다.**

- 요청의 메소드는 `GET`, `HEAD`, `POST` 중 하나여야 한다.
- `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, `DPR`, `Downlink`, `Save-Data`, `Viewport-Width`, `Width`를 제외한 헤더를 사용하면 안된다.
- 만약 `Content-Type`를 사용하는 경우에는 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain`만 허용된다.

 이 중 2, 3번 조건이 까다롭다. 사용자 인증에 사용되는 Authorization 헤더도 사용하면 안되고, 대부분의 HTTP API에서 사용되는 text/xml이나 application/json 컨텐츠 타입도 가지면 안된다.<b> 따라서 이 시나리오는 현실적으로 조건을 만족시키기 어렵다.</b>



#### 3) Credentialed Request

> 인증정보 포함 요청

 **인증 관련 헤더를 포함할 때**, 사용하는 요청이다. 기본적으로 브라우저가 제공하는 비동기 리소스 요청 API인 XMLHttpRequest 객체나 fetch API는 별도의 옵션 없이 브라우저의 쿠키 정보나 인증과 관련된 헤더를 함부로 요청에 담지 않는다. 따라서, 쿠키나 jwt 토큰을  담아서 클라이언트에서 서버로 보내고 싶다면, 클라이언트 측에서는 credentials 옵션의 값을 include로 설정하면 된다. 이때, 서버측에서는 Access-Control-Allow-Origin에 대한 정확한 Allow-Origin을 표기하고, Access-Control-Allow-Credentials: true 를 포함시켜야한다.



<figure align="center">
<img src="https://developer.mozilla.org/ko/docs/Web/HTTP/CORS/cred-req-updated.png" width="600">
    <figcaption style="text-align:center;color:gray">
    [출처] https://developer.mozilla.org/ko/docs/Web/HTTP/CORS
  </figcaption>
</figure>



- 클라이언트 측

​	`credentials : include`

```javascript
fetch('http://localhost:4000/api', {
  credentials: 'include', 
});
```



- 서버 측 : Access-Control-Allow-Origin에 대해 정확한 Allow-Origin(* 안됨)을 설정해야한다.

​	`Access-Control-Allow-Credentials : true`




### CORS 오류 해결 방법

<hr>

1. **프론트 프록시 서버 설정**

   - 프론트 서버에게 request를 보낸다. 브라우저 입장에서  origin과 target이 같기 때문에 해당 요청은 same origin 형식으로 보내진다.
   - 요청을 받으면 프론트 서버에서 출처를 (request를 보낼 서버와 동일하게) 수정해 (출처가 다른)서버에 request 한다.  이때, 브라우저 입장에서는 Same Origin Request 이므로, CORS 에러가 터지지 않게 된다.

   

2. <b>서버에 Access-Control-Allow-Origin 헤더 세팅</b>

   - 응답 헤더에 `Access-Control-Allow-Origin :`값을 설정한다.





### References

https://developer.mozilla.org/ko/docs/Web/HTTP/CORS

https://hudi.blog/sop-and-cors/

https://evan-moon.github.io/2020/05/21/about-cors/

https://it-eldorado.tistory.com/163

https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F

