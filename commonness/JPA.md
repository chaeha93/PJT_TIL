## JPA(Java Persistence API)
- 자바 ORM 기술에 대하 표준 명세로, JAVA에서 제공하는 API. (스프링에서 제공 X)
- 자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스
	> JPA는 인터페이스이므로 특정 기능을 하는 라이브러리가 아님
	> 스프링의 PSA에 의해서 표준 인터페이스를 정해두었는데, 그중 ORM을 사용하기 위해 만든 인터페이스
	> 기존 EJB에서 제공되던 엔티티 빈을 대체하는 기술
	> ORM이기 때문에 자바 클래스와 DB 테이블을 매핑. -> SQL을 매핑하지 않는다.

## ORM(Object Relational Mapping, 객체-관계 매핑)이란?
- ORM은 DB 테이블을 자바 객체로 매핑함으로써 객체 간의 관계를 바탕으로 SQL을 자동 생성하지만 Mapper는 SQL을 명시해주어야 함
- ORM은 RDBMS을 객체에 반영하는 것이 목적이라면, Mapper는 단순히 field를 매핑시키는 것이 목적이라는 점에서 차이가 있다.

## SQL Mapper
- Object field -> mapping -> SQL
- SQL 문으로 직접 디비를 조작한다.
- MyBatis, jdbcTemplate

## ORM
- Object field -> mapping -> DB 데이터
	> 객체를 통해 간접적으로 디비 데이터를 다룬다.
- 객체와 디비의 데이터를 자동으로 매핑해준다.


