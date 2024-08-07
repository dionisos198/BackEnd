# 프로젝트 소개 

<aside>
💡 서비스 소개서

</aside>

**펫플레이트는 견주들이 필수 영양 요소에 대해 이해도가 부족하다는 문제점을 기반하여 기획되었다. 
하지만 연구 결과에 따르면 사료는 반려견의 모든 필수 영양소를 충족하기 어렵고 영양과잉의 문제점도 있어 질병 유병률을 증가한다.
따라서 첫 째, 사용자가 반려견의 식사를 기록하면 이에 맞추어 반려견의 나이, 품종 및 체중에 따른 필수 영양소를 그램(g) 수로 시각화하여 사용자가 영양균형을 모니터링할 수 있게 한다;
둘째, 누락된 영양소를 확인한 후, 동일한 웹사이트로 더 맞춤형 된 보충제를 구매할 수 있는 마켓플레이스를 제공한다.**


# 기여한 점 

## 소셜 로그인 

<img width="410" alt="image" src="https://github.com/user-attachments/assets/fdf554bc-2296-4d7d-8776-0256a9a27dbd">

OAuth2 에 대한 개념 이해를 바탕으로<br>
**WebClient 를 이용하여 Rest api 형식으로 만든 소셜로그인과 스프링 시큐리티를 사용하여 만든 소셜 로그인** 2가지 방식으로 모두 구현해보고<br>
각 구현 방법의 내용을 정리한다.<br>

### [스프링 시큐리티를 사용한 소셜 로그인 링크](https://velog.io/@dionisos198/%EC%8A%A4%ED%94%84%EB%A7%81-oauth2-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EA%B5%AC%ED%98%84)

### [Rest api 형식의 소셜 로그인 링크](https://velog.io/@dionisos198/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%86%8C%EC%85%9C-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EA%B5%AC%ED%98%84-%EA%B8%B0%EC%A1%B4-%EB%B0%A9%EC%8B%9D%EC%9D%84-%EB%B2%84%EB%A6%AC%EB%8B%A4)



## 회원 탈퇴 및 회원 정보 조회 


<img width="410" alt="image" src="https://github.com/user-attachments/assets/60bceee8-c84c-47d5-a29f-bfd70d16fddf">

### 회원 탈퇴 

회원 탈퇴 및 소셜 로그인 연동 해제를 위하여<br>

소셜 로그인시 쉽게 변경이 될 수 있는 social login 용 accessToken 은 redis 에, 네이버 기준 1년동안 변경이 안되는 refreshToken 은 rdbms mysql 에 저장하여 관리한다 


### 회원 정보 조회 

https://github.com/dionisos198/BackEnd/blob/b08f9019e3b1e2a872f91fa2cac866e4a3d7f4f1/src/main/java/com/petplate/petplate/user/service/UserService.java#L59-L63

일부 비즈니스 로직 상 반복적으로 호출되는 ```username``` 을 통한 현재 로그인한 객체 꺼내오기 

https://github.com/dionisos198/BackEnd/blob/b08f9019e3b1e2a872f91fa2cac866e4a3d7f4f1/src/main/java/com/petplate/petplate/auth/service/AuthService.java#L84-L101

로그인할 때마다 반복적으로 호출되는 ```username``` 을 통한 회원가입 여부 판단하기<br>

에서 성능 개선이 필요하다고 판단, ```Users``` 의 칼럼 ```username``` 에 Index 를 적용하여 성능을 평균 100 ms 향상 시켰다(apache jmeter 사용,현재 soft delete 로 인한 unique 제약 조건 X) <br>

### [apache jmeter 를 활용한 index 성능 측정](https://velog.io/@dionisos198/%EC%8A%A4%ED%94%84%EB%A7%81-index-%EC%82%AC%EC%9A%A9-%ED%95%98%EC%97%AC-%EA%B2%80%EC%83%89-%EC%84%B1%EB%8A%A5-%EA%B0%9C%EC%84%A0-%EB%B0%8F-%EC%B8%A1%EC%A0%95apache-jmeter-9%EB%A7%8C%EA%B1%B4-%EB%8D%B0%EC%9D%B4%ED%84%B0)



## 부족 영양분 기반 영양제 추천 및 상세 정보 반환 


<img width="410" alt="image" src="https://github.com/user-attachments/assets/ee88c429-b747-49a5-975e-f458f42e13d2">

<img width="410" alt="image" src="https://github.com/user-attachments/assets/c06dac56-ef94-45cf-a534-115b44a5bfbc">


```enum``` 타입으로 관리되는 영양소와 다대다 연관관계를 통하여 부족 영양분에 따른 추천 영양제를 반환하고  <br>
영영제의 관련 상세 페이지를 반환한다. <br>


## Route53 과 SubDomain 을 활용한 배포 

**Vercel 로 배포하는 프론트와 상호 소틍을 통해서 도메인을 적용**하고, <br>

**비용 및 이후 원활한 쿠키 사용을 위해서 백앤드는 서브 도메인**을 적용한다. 

### [프론트에서 쿠키에 저장된 RefreshToken 에 접근하지 못하는 문제 및 실험](https://velog.io/@dionisos198/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A1%A0%ED%8A%B8-%EC%BF%A0%ED%82%A4%EA%B0%80-%EC%82%AC%EB%9D%BC%EC%A7%80%EB%8A%94-%ED%98%84%EC%83%81-%EC%A0%95%EB%A6%AC)




# 배포 링크 

https://petplate.kr/

상황에 따라 해제될 수도 있다. 


