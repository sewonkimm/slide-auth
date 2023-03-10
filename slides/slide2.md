## NOPE, 사실은 그렇게 간단하지가 않다.

비밀번호를 쌩으로 저장하면 안된다!

# 왜? 


## 보안이슈 때문!

- DB가 유출되면 어쩔?
- 관리자가 개인정보를 팔면 어쩔?


### 유저의 서비스 신뢰도가 박살난다

정보통신망법, 개인정보보호법 등에 의해서 

- 비밀번호
- 바이오정보
- 주민등록번호
- 신용카드번호
- 계좌번호
- 여권번호
- 운전면허번호
- 외국인등록번호
- ...

등을 암호화해야한다.


# 그럼 암호화를 하자!

1. 단방향 암호화
   - Hashing을 이용한 암호화
   - 역으로 복호화가 불가능
   - ex) SHA-256 알고리즘
  
2. 양방향 암호화
    - 대칭키
    - 비대칭키     


## 일반적인 암호화 방식

```jsx
Password : 123abc → HASH → Digest : fw10934gas
```

Digest가 DB에 저장되고, Digest만으로 원래 비밀번호를 알 수 없다.


<img data-src="/images/twitter.png">

운영자는 해시 테이블이 있기 때문에 사용자의 비밀번호가 필요 없다.


## 그러나 해싱만으로는 안전하지 않다.

해싱은 모든 input에 대해 output이 동일하기 때문에 

하나하나 무식하게 대입해보면 뚫린다. 

이렇게 대입해볼 수 있는 rainbow table이 존재한다.

(비밀번호를 복잡하게 만들어야하는 이유)


### 이런 약점을 보환하기 위해서 

- 해시함수를 여러번 돌리기
- 솔트(임의의 문자열) 추가
- 비밀번호 n번 틀리면 계정 잠금

등의 방법을 사용해 볼 수 있다.


## 그럼 암호화는 언제하면 좋을까?

**클라에서 서버로 보내기 전**에 암호화를 해야한다. 

중간에 해커가 가로채면 안되기 때문이다. 

보통은 Https 프로토콜을 통해 암호화하고, 

거기에 해싱까지 더해서 안전하게 전달한다.

<!-- # 예제 -->


## 그런데 클라이언트에서 암호화하는 게 안전할까?

클라이언트 단의 스크립트나 데이터는 분석이 쉽기 때문에 이런 방식의 암호화는 보통 사용되지 않는다. 대신, 

1. activex 기반의 암호화 모듈
2. https 통신

을 주로 사용한다.


<img data-src="/images/activex.jpeg">

아... 액티브X...?
