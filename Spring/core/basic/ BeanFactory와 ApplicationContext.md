# BeanFactory에 대해서

- 스프링 컨테이너의 최상위 인터페이스이다.
- 스프링 빈을 관리하고 조회하는 역할을 한다.
- getBean() 제공한다.

# ApplicationContext 에 대해서

- BeanFactory 기능을 상속받아 제공한다.
- 빈을 관리하고 조회하는 기능은 물론이고(BeanFactory 를 통해서) 수 많은 부가기능을 제공한다.

## 어떤 부가기능을 제공할까?

- **메세지소스를 활용한 국제화 기능**
  - 한국에서 들어오면 한국어로, 영어권에서 들어오면 영어로 출력

- **환경 변수**
  - 로컬,개발,운영등을 구분해서 처리
  
- **애플리케이션 이벤트**
  - 이벤트를 발행하고 구독하는 모델을 편리하게 지원

- **편리한 리소스 조회**
  - 파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회

## ✔️정리
- `ApplicationContext`는 `BeanFactory`를 기능을 상속받는다.
- `ApplicationContext`는 빈 관리기능+편리한 기능을 제공한다.
- `BeanFactory`를 직접 사용할 일은 거의 없다. 부가기능이 있는 `ApplicationContext`를 사용한다.
- `BeanFactory` 나 `ApplicationContext`를 `스프링컨테이너` 라고 한다.