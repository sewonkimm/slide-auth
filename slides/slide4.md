# JWT(Json Web Token)
- 대표적인 토큰 기반 인증
- 토큰 자체에 필요한 정보가 담겨있다
<div>
    <img data-src="/images/jwt.png" width="350" />
</div>


## 클라이언트에서 AccessToken을 어떻게 관리할까?
1. 쿠키에 저장(로그인 유지)
2. local storage나 session storage에 저장(로그인 유지)
3. js 메모리에 저장(로그인 유지❌)


## 쿠키에 저장하면
XSS 공격으로부터 안전하지만 CSRF 공격에 취약하다

## Storage에 저장하면
CSRF 공격으로부터 안전하지만 XSS 공격에 취약하다


### XSS
- 웹페이지에 악성 자바스크립트를 삽입하는 공격
- input에 js코드를 쓰는 것
- **쿠키 http only 설정**을 통해 XSS를 차단할 수 있다! (그러나 완벽히 방어하는 것은 아니다;;)

```javascript
Response.Cookies.Add(new HttpCookie("쿠키명")
{
   Value = "쿠키 값",
   HttpOnly = true
});
```


### CSRF
- 해커가 사용자의 요청을 조작하는 것
- 쿠키로 사용자 정보를 탈취해서 서비스의 url로 요청
- ex) 특정 링크를 누르면 돈이 빠져나가는 피싱


# 설상가상
한번 만들어진 토큰은 서버에서 제어 불가능하기 때문에

- 만료시간을 짧게하면 로그인이 자주 풀려서 불편하고,
- 길게하면 보안에 취약해짐


# 궁여지책
## Refresh Token을 사용하자!
---
### 장시간 로그인
AccessToken을 재발급 받을 수 있는 토큰

&nbsp;

### 강제 로그아웃

게다가 RefreshToken을 서버에 저장하는 방식으로 구현하면 

토큰이 탈취되었을 때, 서버에서 강제로 토큰을 삭제할 수 있다.


## AccessToken + RefreshToken 퓨전
1. 로그인 시 유효기간이 짧은 AccessToken과 유효기간이 긴 RefreshToken을 발급
2. (서버) RefreshToken 저장
3. (클라) AccessToken을 js 변수에 저장
    - SPA에서는 새로고침 하지 않는 이상 계속 유지할 것
    - XSS, CSRF 공격에 안전
4. (클라) RefreshToken을 http only 쿠키에 저장
5. AccessToken이 만료되면 RefreshToken으로 두 토큰을 모두 재발급
6. 새로고침하여 AccessToken이 사라지면 RefreshToken으로 두 토큰을 모두 재발급

<!-- # 예제 -->


# 명확한 한계
서버에 토큰을 저장하면 결국 세션방식의 단점을 보완할 수는 없다.

그렇다고 JWT방식으로 구현하면 탈취되었을 때, 토큰을 무효화 시킬수는 없다.
