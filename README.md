- JPA의 기능 소개</br>
JPA는 애플리케이션과 JDBC 라이브러리 사이에서 동작하며, JVM 내에 속한다.</br>
개발자로 하여금 JDBC의 작업을 편리하게 사용할 수 있게 해주는 것이다.</br>
또, JPA는 자바 객체 지향 프로그래밍과 관계형 데이터베이스의 패러다임 불일치도 해결해준다.</br>
</br>
예를 들어,</br>
리소스 저장</br>
JPA가 Entity를 분석하고 JDBC API를 이용해서 INSERT SQL을 생성하는 것이다.</br>
그러면 JDBC API는 JPA가 만들어준 INSERT SQL문을 DB로 쏴준다.</br>
</br>
리소스 조회</br>
조회 메서드를 호출하면 JPA가 SELECT SQL을 생성한다.</br>
JDBC API는 해당 SQL문을 DB에 쏘고, DB에서 JDBC API로 결과를 반환한다.</br>
JDBC에서는 반환 받은 결과를 ResultSet 매핑을 해서 자바 객체로 반환한다.</br>
</br>
</br>
</br>
- JPA 등장 배경</br>
현재 자바 표준 ORM은 JPA이지만 예전에는 EJB라는 것이 자바 표준 ORM이었다. EJB는 엔티티 빈이라고도 불렀다. 그러나 EJB 엔티티 빈을 실제로 사용하는 개발자는 별로 없었다. 기술이 너무 복잡하고 성능이 기대에 미치지 못해 효율적이지 못했기 때문이다. 직접 SQL 짜는 것보다 못했다.</br>
</br>
이러한 문제 상황에 대한 대응으로 한 개발자가 Hibernate라는 오픈 소스 ORM을 세상에 내놓았다.</br>
Hibernate는 많은 개발자들에게 호응을 얻었고, 다른 개발자들도 개발에 참여하여 점점 더 발전되었다.</br>
Java 개발 회사에서는 해당 개발자를 초빙하여 JPA를 개발하였고, 이것을 자바 표준 ORM으로 지정하게 되었다.</br>
JPA는 그러므로 Hibernate라는 오픈 소스에서 출발한 표준이다. </br>
표준화되면서 용어 등 정제할 것들을 많이 정제하게 되었다.</br>
</br>
JPA는 인터페이스의 모음이다. JPA 2.1 기준 표준 명세를 구현한 3가지 구현체는</br>
Hibernate, EclipseLink, DataNucleus 인데, 이 중에 Hibernate가 가장 많이 사용되는 구현체다.</br>
Hibernate라는 이름을 버리지 않고, 표준 명세를 구현하는 구현체로 그 이름을 남겨두었다.</br>
</br>
</br>
</br>
- JPA를 사용해야 하는 이유</br>
데이터와 행동을 객체로 표현하는 것과 테이블로 표현하는 것으로부터 오는 여러 가지 불일치점들이 해결된다.</br>
OOP에서는 객체로 다루고, RDB에서는 테이블로 다루게 된다.</br>
그렇게 되면 상속과 다형성, 참조와 외래키 등의 패러다임 불일치점들이 발생하는데,</br>
JPA를 사용하면 그러한 문제들이 해결되어 테이블을 객체 다루듯이 편리하게 다룰 수 있게 된다.</br>
그렇게 함으로써 생산성과 유지보수성을 높이게 된다.</br>
</br>
</br>
</br>
- JPA와 CRUD</br>
저장:	jpa.persist(member); </br>
조회:	jpa.find(memberId); </br>
수정:	member.setName("새 이름");	  (@Transactional 걸어주고 하면 자동으로 DB에 반영) </br>
삭제:	jpa.remove(member);  </br>
 </br>
 </br>
 </br>
- JPA의 패러다임 불일치 해결 </br>
* 참고: JPA가 관리하는 객체를 엔티티라고 부른다. </br>
 </br>
jpa.persist() 메서드가 상속 관계의 모든 테이블에 INSERT SQL를 날려준다. </br>
일일히 개발자가 INSERT SQL문을 직접 작성할 필요가 없어졌다. </br>
 </br>
조회할 때도 상속이 되 있는 객체의 경우에는 조회하는 SQL문이 꽤 복잡한데 그것도 JPA가 알아서 </br>
JOIN해서 SQL문을 작성해준다. </br>
 </br>
또, member.getTeam() 또는 member.getOrder().getDelivery() 등 JPA를 사용하면 연관관계에 있는 엔티티를 쉽게 조회할 수 있다. 특히, 연관관계 엔티티를 조회할 때 발생하는 지연(Lazy) 로딩을 통해 성능 문제도 최적화하여 조회한다. </br>
 </br>
또, 동일한 트랜잭션 내에서 동일한 id 값을 이용해서 조회한 엔티티가 같음이 보장된다. </br>
단, "동일한 트랜잭션 내에서" 라는 조건이 지켜져야 한다. </br>
 </br>
 </br>
 </br>
- JPA의 성능 최적화 1: 1차 캐시와 동일성(identity) 보장 </br>
JPA는 같은 트랜잭션 안에서는 같은 엔티티를 반환한다. 이는 조회 성능을 소폭 향상시키는 효과를 준다. </br>
이는 한 트랜잭션 동안 JPA가 메모리 상에 조회한 엔티티를 캐싱해놓기 때문에  </br>
동일한 조건으로 조회하면 두 번째부터는 SQL을 날려서 조회하는 것이 아니라 캐시 메모리로부터 조회한 덕분이다. </br>
다른 트랜잭션 동안에는 불가능하다. </br>
 </br>
 </br>
 </br>
- JPA의 성능 최적화 2: 트랜잭션을 지원하는 쓰기/수정 지연(INSERT, UPDATE) </br>
JPA는 트랜잭션이 커밋되기 전까지는 INSERT SQL들을 한꺼번에 모은다. </br>
모아놨다가 JDBC의 BATCH SQL 기능을 사용해서 한 번에 SQL을 전송한다. </br>
JDBC BATCH SQL도 직접 짤려면 엄청 복잡하다. </br>
이와 관련된 기능이 hibernate.jdbc.batch_size 값이다. </br>
예제에서는 persistence.xml 파일에 값이 10으로 설정되어 있다. </br>
 </br>
 </br>
* 여러 번 SQL을 전송하게 되면 네트워크 요청과 응답이 그만큼 자주 발생하여 시간이 소요된다. </br>
그렇기 때문에 SQL문을 모아놨다가 한꺼번에 전송하면 해당 시간에 대한 오버헤드가 감소한다. </br>
또, SQL을 자주 전송하면 DB의 CPU와 메모리 리소스가 더 많이 사용되지만, 모아서 보내면 리소스 사용량이 줄어든다. 이러한 이유 때문에 SQL을 한 번에 모아서 전송하는 것이 성능적으로 유리하다. </br>
 </br>
 </br>
- JPA의 성능 최적화 3: 지연 로딩과 즉시 로딩 </br>
즉시 로딩: JOIN SQL로 한 번에 연관된 객체까지 미리 조회 </br>
지연 로딩: 객체가 실제 사용될 때 로딩 </br>
 </br>
연관 관계에 있는 두 엔티티를 함께 보아야 하는 경우가 대부분일 때는 즉시 로딩이 유리하지만 </br>
연관 관계에 있더라도 동시에 조회할 일이 별로 많지 않으면 지연 로딩이 유리하다. </br>
 </br>
즉시 로딩은 JOIN문을 통해 한꺼번에 조회하지만 지연 로딩은 SELECT 문을 테이블마다 따로 날려서 조회한다. </br>
JPA는 둘 다 지원하므로 즉시 로딩을 할지 지연 로딩을 할지는 개발자가 선택해서 적용해주어야 한다. </br>
 </br>
 </br>
 </br>
- JPA 구동 과정 </br>
JPA는 Persistence라는 클래스가 있다. 이 Persistence 클래스가 META-INF/persistence.xml 의 설정 정보들을 읽어서 EntityManagerFactory라는 클래스를 만든다. 여기에서 필요할 때마다 EntityManager들을 생성하여 돌아간다. </br>
 </br>
 </br>
 </br>
- EntityManagerFactory와 EntityManager </br>
EntityManagerFactory(emf) 는 애플리케이션 로딩 시점에 하나만 만들어져야 한다. </br>
그리고 Transaction을 한 번 실행할 때마다 EntityManager 를 만들어줘야 한다. </br>
 </br>
EntityManager(em)는 쓰레드 간에도 공유하면 안 된다. 그러면 장애가 발생한다. </br>
EntityManager(em)는 사용 후 즉시 버려야 한다. </br>
 </br>
JPA 의 정석적인 사용 방식: try-catch-finally 를 통해 트랜잭션 관리를 한다. </br>
try 문 내부에는 em.persist(), em.find(), em.remove() 등의 메서드를 호출할 수 있다. </br>
수정은 member.필드세터() 만 호출하면 된다. </br>
 </br>

```
em.createQuery("JPQL문", Member.class)
    .setFirstResult(5)
    .setMaxResults(8) 
    .getResultList();
```

이렇게 하면 List<Member> 타입의 객체가 반환된다.  </br>
 </br>
클라이언트가 개별 요청을 할 대마다 EntityManagerFactory에서 EntityManager를 개별적으로 생성하고, 이 EntityManager는 내부적으로 커넥션을 사용해서 DB에 접근함으로써 응답이 이루어지게 된다. </br>
 </br>
 </br>
 </br>
- DB 방언 변경하기 </br>
persistence.xml에 현재 기준 org.hibernate.dialect.H2Dialect 으로 되어 있지만 만약 </br>
다른 방언을 선택해서 입력하면 JpaMain.java 를 실행했을 때 쿼리문이 뜨는 것을 보면 해당 DB 방언에 맞게  </br>
쿼리문이 작성되는 것을 확인할 수 있다. </br>
 </br>
 </br>
 </br>
- JPQL </br>
JPQL은 바로 JPA가 SQL을 추상화시킨 객체 지향 쿼리 언어이다. </br>
그렇기 때문에 JPA를 사용하면 엔티티 객체를 중심으로 개발을 할 수 있다. </br>
SQL이 DB 테이블을 대상으로 하는 쿼리문이라면 </br>
JPQL은 쿼리 자체가 DB 테이블을 대상으로 하는 쿼리문이 아니라 엔티티 객체를 대상으로 하는 쿼리이다. </br>
JPQL은 SQL과 문법적으로 유사하며, select, from, where, group by, having, join 등을 지원한다. </br>
또, JPQL은 SQL을 추상화한 것이기 때문에 특정 DB의 SQL에 의존하지 않는다. </br>
 </br>
 </br>
 </br>
- 영속성 컨텍스트란 무엇인가? </br>
"엔티티를 영구적으로 저장해주는 환경"을 의미한다.  </br>
실습에서는 단순히 "데이터를 DB에 저장하는 것"을 의미하는 것처럼 배웠지만 더 깊은 내용이 있다. </br>
사실 persist() 메서드가 직접 DB에 저장시켜주는 것이 아니라, "영속성 컨텍스트"라는 곳에 저장시켜주는 것이다. </br>
 </br>
이 영속성 컨텍스트는 논리적인 개념이며 EntityManager를 통해 접근할 수 있는 곳이다. </br>
즉, EntityManager는 PersistenceContext를 조작하는 데 사용되는 API이다. </br>
 </br>
J2SE 환경에서 영속성 컨텍스트는 PersistenceContext라는 객체로 구현되며, EntityManager와 1:1로 연결된다. </br>
J2EE나 스프링 프레임워크 같은 컨테이너 환경에서는 PersistenceContext와 EntityManager가 1:N 으로 연결되기 때문에 조금 달라진다. 스프링 프레임워크에서는 PersistenceContext 하나만으로 모든 EntityManager를 관리할 수 있게 된다. </br>
 </br>
 </br>
 </br>
- 엔티티의 생명주기 </br>
엔티티의 생명주기는 크게 4가지다. 이 엔티티의 생명주기는 PersistenceContext와 깊은 관계가 있다. </br>
 </br>
1) 비영속 new/transient </br>
영속성 컨텍스트와 전혀 관계가 없는 새로운 상태. 즉, JPA와 전혀 관계 없는 상태이다. </br>
자바로 치면, 생성자를 통해 만들어진 자바 객체이다. 그러나 아직 persist() 메서드를 통해 영속화되지 못한 상태. </br>

```
    Member member1 = new Member();
    member.setId(1L);
    member.setName("홍길동");
```

 </br>
2) 영속 managed </br>
영속성 컨텍스트에 의해 관리되는 상태. 즉, EntityManager를 통해 PersistenceContext에 접근해서 그 안에 해당 객체가 들어가 있는 것을 의미한다. </br>
em.persist() 메서드 안에 매개변수로 입력하면 영속 상태가 된다. </br>
    (em 생성 및 트랜잭션 관련 코드 생략) </br>
    
```
    em.persist(member1);
```

 </br>
이렇게 persist()를 호출하는 거 자체가 DB에 저장시켜주는 것은 아니다. </br>
그렇다면 해당 객체가 DB에 어떻게 저장되는 것일까? </br>
바로 tx.commit()이 호출될 때 DB에 저장되는 것이다.  </br>
 </br>
 </br>
3) 준영속 detached </br>
영속성 컨텍스트에 저장되었다가 detach() 메서드에 의해 다시 분리된 상태. </br>

```
    em.detach(member); 
```

 </br>
4) 삭제 removed </br>
삭제된 상태. remove() 메서드에 의해 실행되며, tx.commit() 까지 호출되면 실제로 DB에서 삭제된다. </br>

```
    em.remove(member);
    tx.commit();
```

 </br>
 </br>
 </br>
- 영속성 컨텍스트의 이점 </br>
1) 1차 캐시 </br>
PersistenceContext는 Map 형태이며, 내부에 1차 캐시를 갖고 있다. </br>
em.persist(member); 를 하면 key는 @Id가 붙은 필드(id)가 되고, value는 Entity 객체(member)가 된다. </br>
이렇게 em.find를 통해 조회할 때 1차 캐시를 뒤져서 여기서 바로 조회한다. </br>
그런데 만약 PersistenceContext 안의 1차 캐시에 find 해서 찾고자 하는 객체가 없다면 DB를 조회하여 1차 캐시에 가져와서 저장한다. 그렇게 한 후에 해당 객체를 반환한다. 그렇게 되면 이후에 다시 그 객체를 조회할 때는 1차 캐시에서 바로 조회해서 반환하게 된다. </br>
 </br>
물론 비즈니스 로직이 굉장히 복잡하여 캐시 저장소를 자주 사용하게 되는 상황이 아닌 이상 큰 의미는 없다. 왜냐하면 어쨌든 간에 EntityManager는 트랜잭션이 종료되면 사라져버리기 때문에 캐시의 수명도 그만큼 짧기 때문이다. 하나의 트랜잭션에만 찰나의 순간에만 존재하기 때문에 1차 캐시라고 부른다. </br>
실제로 tx.commit() 명령을 하기 전에 persist()로 저장한 데이터를 find(Member.class, 101L) 해보고 </br>
println 해보면 select 쿼리문이 날아가지도 않는데 콘솔창에 출력은 된다. </br>
 </br>
 </br>
2) 영속 엔티티의 동일성 보장 </br>

```
Member a = em.find(Member.class, "member1");
Member b = em.find(Member.class, "member1");
System.out.println(a==b);   //  true
```

1차 캐시를 이용하여 트랜잭션 격리 수준 중 "반복 가능한 읽기 등급"을 DB가 아닌 App 차원에서 제공한다. </br>
마치 자바의 Collection에서 가져온 것처럼 동일한 객체를 꺼내준다. </br>
 </br>
 </br>
3) 트랜잭션을 지원하는 쓰기 지연(transactional write-behind) </br>
tx.commit(); 을 하기 전까지는 각종 EntityManager persist(member)를 실행해도 </br>
영속성 컨텍스트에 쌓아두기만 한다. 1차 캐시에 해당 객체들을 저장해두는 것도 있는데, </br>
PersistenceContext에는 1차 캐시 저장소 외에도 '쓰기 지연 SQL 저장소' 라는 것도 있다. </br>
JPA이 만들어준 INSERT SQL문들을 이 '쓰기 지연 SQL 저장소'에 저장해두었다가 tx.commit()을 호출하면  </br>
DB로 해당 SQL문들이 flush 된다. </br>
 </br>
 </br>
4) 변경 감지(Dirty Checking) </br>
데이터에 변경할 일이 있을 경우 다시 persist()를 호출할 필요가 없다. </br>
JPA는 테이블의 데이터를 자바 객체처럼 다루게 해주기 위한 인터페이스이다. </br>
컬렉션에서 객체를 꺼내서 수정한 후에 다시 컬렉션에 담아줄 필요가 없듯이,  </br>
JPA에서도 마찬가지로 DB에서 데이터를 꺼내서 변경을 했다고 해도 다시 객체를 persist() 할 필요가 없다. </br>
실제로 데이터에 변경이 일어나면 persist()를 호출하지 않아도 알아서 UPDATE 쿼리문이 날아간다. </br>
그리고 em.update() 이런 메서드도 사실 존재하지 않는다. </br>
어떻게 이런 일이 가능한 것일까? 바로 PersistenceContext의 기능에 있다. </br>
JPA에서는 tx.commit() 하는 시점에 내부적으로 flush() 를 호출하는데, flush()를 호출하면 </br>
엔티티와 스냅샷을 비교한다. 사실 1차 캐시에는 스냅샷이라는 필드가 있다. </br>
스냅샷 필드에는 값을 읽어왔던 시점의 데이터들을 담고 있다. </br>
그래서 만약 flush를 호출했을 때 스냅샷의 데이터와 flush 호출 시점의 데이터를 비교해서 </br>
내용이 다를 경우 update 쿼리문을 날리는 것이다. 이런 식으로 JPA는 변경 감지를 수행한다. </br>
편리하다고 생각할 수도 있는데, 데이터를 바꾼 후 persist()를 해야한다고 착각할 수 있으니 주의해야 한다. </br>
 </br>
* 참고: 플러시(flush) </br>
영속성 컨텍스트에 쌓인 SQL문들을 DB에 쏘아주는 것을 flush라고 한다. </br>
스냅샷 비교를 통해 데이터 변경이 감지되면 해당 update SQL문이 '쓰기 지연 SQL 저장소'에 등록된다. </br>
flush를 하면 이 저장소에 있는 쿼리들이 DB에 전송된다(등록, 수정, 삭제 쿼리). </br>
참고로, flush() 메서드 자체도 있다. 만약 flush() 메서드만 호출하면 DB에 SQL이 전송되는데 commit은 안 된다.  </br>
 </br>
flush() 메서드는 commit() 메서드를 호출한다거나, JPQL 쿼리를 실행할 때(em.createQuery) 자동으로 함께 호출되도록 구현되어 있다. 그래서 사실 em.flush() 메서드를 직접 호출할 일은 거의 없지만 작동 원리는 이해하고 있어야 한다. 만약 flush()를 먼저 하고 commit()을 이때는 flush()가 자동 호출되지 않는다. </br>
참고로, flush()를 한다고 해서 영속성 컨텍스트를 비워버리는 것은 아니다. 단지 영속성 컨텍스트 내의 변경 내용을 DB에 동기화시켜주는 것이라 생각하면 된다. </br>
이런 것들이 다 가능한 이유는 트랜잭션이라는 메커니즘 덕분이다. 한 트랜잭션 내에서 커밋을 하기 전이라면 얼마든지 동기화 해놓고 나중에 커밋할 때 한꺼번에 적용할 수 있다. </br>
 </br>
 </br>
5) 지연 로딩(Lazy Loading) </br>
엔티티 관련 데이터가 실제로 필요할 때까지 로드되지 않게 하는 설정이다. 이를 통해 </br>
애플리케이션의 성능을 최적화하고, 불필요한 DB 접근을 줄인다. 자세한 내용은 후에 다룬다.  </br>
 </br>
 </br>
 </br>
- 준영속 상태 </br>
영속 상태란 앞에서 다루었듯이 em.persist(member) 메서드에 파라미터로 들어간다든지, </br>
조회 em.find(Member.class, 1L) 를 통한다든지 해서 "1차 캐시에 올라갔을 때 영속 상태"가 되었다고 하는 것이다. </br>
준영속 상태란 엔티티가 영속성 컨텍스트에서 분리된 상태를 말한다. </br>
준영속 상태에서는 바로 위에서 언급했던 5가지 영속성 컨텍스트 기능들이 적용되지 않는다. </br>
 </br>
어떨 때 준영속 상태가 되는가? 준영속 상태로 되는 경우는 여러 가지이다. </br>
1) em.detach(member); </br>
이렇게 하면 더이상 이 member라는 인스턴스는 JPA가 관리하지 않는다. </br>
그래서 tx.commit()을 했을 때 member 인스턴스에 대한 내용은 빠진다. </br>
다만, 이 em.detach(member) 메서드를 직접 호출할 일은 실무에서 거의 없을 것이다. </br>
 </br>
2) em.clear(); </br>
em.clear()는 매개변수를 받지 않으며, 영속성 컨텍스트에 있는 것들을 모두 지워버린다. </br>
 </br>
3) em.close(); </br>
영속성 컨텍스트를 아예 닫아버린다. em.clear()와 실질적으로 큰 차이가 없다. </br>
 </br>
 </br>
 </br>
- JPA의 엔티티 매핑 </br>
@Entity </br>
* 클래스에 @Entity를 붙여주면 해당 클래스는 JPA가 관리하게 되고, 그때부터는 '엔티티'라고 부른다. 그러므로 JPA를 사용해서 테이블로 매핑해줄 클래스에는 @Entity를 반드시 붙여주어야 한다. </br>
 </br>
* @Entity가 붙은 클래스는 기본 생성자를 필수로 갖고 있어야 한다. (@NoArgsConstructor) </br>
객체 프록싱에 필요하기 때문이라고 한다.  </br>
 </br>
* final, enum, inner 클래스나 interface에는 @Entity를 붙여줄 수 없다.  </br>
 </br>
* 엔티티의 필드에는 final 제어자를 붙이면 안 된다. </br>
 </br>
* @Entity 어노테이션의 name 속성은 JPA에서 사용할 엔티티의 이름을 지정하며, 기본값은 클래스명과 동일하다. </br>
이 속성은 JPA 내부에서 사용할 이름이며, DB 테이블 이름은 @Table 어노테이션으로 지정한다. </br>
기본값 그대로 사용하는 것이 헷갈릴 일이 없으므로 name 속성은 거의 쓸 일이 없다. </br>
 </br>
 </br>
- @Table의 속성 </br>
    name: 매핑할 테이블 이름 </br>
    catalog: DB catalog 매핑 </br>
    schema: DB 스키마 매핑 </br>
    uniqueConstraints: DDL 생성 시에 유니크 제약 조건 생성 </br>
 </br>
* 여기서 DDL이란, Data Definition Language의 약자로, DB에서 데이터 구조를 정의하고 관리하는 데 사용되는 SQL의 하위집합이다. DDL 명령어는 데이터베이스 스키마, 테이블, 인덱스, 뷰 등 데이터베이스의 객체들을 생성/수정/삭제하는 데 사용된다. DDL은 데이터베이스 '구조'와 관련된 작업을 수행하며, 실제 데이터 자체와는 직접적인 관련이 없다. </br>
주요 DDL 명령어는 다음과 같다:  </br>
    create: 새로운 데이터베이스 객체를 생성 </br>
    alter: 기존의 데이터베이스 객체를 수정 </br>
    drop: 데이터베이스 객체를 삭제 </br>
    truncate: 한 테이블의 모든 데이터를 삭제하지만 테이블의 구조는 유지 </br>
    rename: 데이터베이스 객체의 이름을 변경 </br>
 </br>
 </br>
 </br>
- JPA의 DB 스키마 자동 생성 기능: hibernate.hbm2ddl.auto </br>
JPA는 애플리케이션 로딩 시점에 DB 테이블을 생성하는 기능도 지원한다. 물론 운영 환경에서는 절대 사용하면 안 되고, 개발 및 테스트를 하는 로컬 환경에서 사용하는 용도이다.  </br>
DB 방언을 활용해서 DB에 맞는 적절한 DDL을 생성하는데, 이렇게 생성된 DDL은 운영 환경에서는 사용하지 않거나, 적절히 다듬은 후에 사용한다. 가급적 운영 환경에서는 사용하지 않는 것이 권장된다. </br>
 </br>
설정 파일에서 다음과 같은 속성으로 스키마 자동 생성에 관한 설정을 할 수 있다: </br>
hibernate.hbm2ddl.auto </br>
    create: 기존 테이블 삭제 후 다시 생성(DROP + CREATE) </br>
	애플리케이션 로딩 시점에 @Entity가 걸려 있는 클래스들을 쭉 살펴보고 기존 테이블을 지우고 </br>
     	다시 생성한다. 테이블의 구조가 다 완성되지 않은 개발 초기 단계에 자주 사용된다. </br>
 </br>
    create-drop: create와 같은데 종료 시점에 테이블 DROP을 한다는 점이 다르다. </br>
	특히 테스트 케이스를 실행할 때 마지막에 깔끔하게 테이블을 삭제하고 싶을 때 사용한다. </br>
 </br>
    update: 변경분만 반영한다. 예를 들면 테이블에 필드를 하나 추가했는데 테이블을 DROP 하고 싶지는 않을 때, </br>
	테이블을 DROP 시키지 않고 새로 생긴 필드를 추가해준다. (ALTER) </br>
	기존의 필드를 없애버리는 것은 불가능하다. </br>
 </br>
    validate: 엔티티와 테이블이 정상 매핑되었는지만 확인. Entity 클래스의 구조와 실제 DB 테이블의 구조가 </br>
	일치하지 않으면 에러를 띄운다. </br>
 </br>
    none: 스키마 자동 생성 기능을 사용하지 않음. 사실상 설정 파일에서 ddl.auto 부분을 주석처리한 것과 같다. </br>
 </br>
개발 초기 단계나 테스트 서버에서는 create나 update를 사용하며,  </br>
스테이징 및 운영 서버에서는 validate 또는 none만 사용해야 한다.  </br>
★특히, 스테이징 및 운영 서버에서 create나 update를 절대로 사용해서는 안 되고 웬만하면 그냥 none으로 한다. </br>
 </br>
 </br>
 </br>
- 필드와 컬럼 매핑 </br>
* enum 타입의 필드를 쓰고 싶을 경우에는 어떻게 해야 할까? </br>
    이럴 때는 @Enumerated 라는 어노테이션을 쓰면 된다. </br>
    @Enumerated(EnumType.STRING) </br>
    EnumType.STRING 이렇게 하면 enum의 이름을 DB에 저장한다. 문자라서 VARCHER(255)로 필드가 생성된다. </br>
     </br>
    @Enumerated(EnumType.ORDINAL) </br>
    EnumType.ORDINAL 이렇게 하면 enum 순서를 DB에 저장한다(0부터 시작). </br>
    순서라서 Integer로 필드가 생성된다. </br>
 </br>
    사실 EnumType.ORDINAL은 사용하지 않는 것이 권장된다. </br>
    만약 해당 서비스 변경 요구로 enum 클래스에 새로운 요소를 추가할 경우 </br>
    변경된 사항이 이전 데이터들에는 적용이 안 되기 때문에 데이터에 대한 오해가 발생한다. </br>
    (USER, ADMIN)  --->  (GUEST, USER, ADMIN) 예시  </br>
    그런데 안타깝게도 EnumType.ORDINAL이 @Enumerated의 기본값이기 때문에 </br>
    반드시 @Enumerated(EnumType.STRING) 이렇게 분명하게 지정해주어야 한다. </br>
 </br>
* 날짜 필드는 어떻게 매핑할까? </br>
    데이터베이스에서는 DATE, TIME, TIMESTAMP 세 가지로 구분해서 사용한다. </br>
    @Temporal(TemporalType.DATE) </br>
    @Temporal(TemporalType.TIME) </br>
    @Temporal(TemporalType.TIMESTAMP) </br>
    최신 하이버네이트에서는 @LocalDate나 @LocalDateTime을 사용하면 생략가능하다. </br>
    @LocalDate는 DB에서 date 타입이고, @LocalDateTime은 DB에서 timestamp 타입이다. </br>
    다만 예전 방식을 사용하는 기업들도 있기 때문에 알아두는 것이 좋다. </br>
      </br>
* 대량의 텍스트를 담을 필드는 어떻게 매핑할까? </br>
    @Lob </br>
    * 참고: 타입이 String인 경우 쿼리문을 보면 clob 이라고 뜬다. </br>
    Lob이라는 어노테이션은 속성이 없다. 단지 매핑하는 필드의 타입에 따라 컬럼의 타입이 결정된다. </br>
 </br>
* 데이터베이스에 컬럼으로 매핑하고 싶지 않은 필드는 어떻게 해야 할까? </br>
    @Transient </br>
    JPA가 @Transient 어노테이션이 붙은 필드는 무시하여 필드 생성에 제외시키고 ddl을 생성한다. </br>
 </br>
 </br>
 </br>
- @Column의 속성 (ddl 관점에서) </br>
    name: 해당 멤버변수의 DB 테이블 상 필드 이름. 기본값은 객체의 필드명. </br>
 </br>
    unique(DDL): 해당 필드가 다른 데이터들과 겹치지 않게 할지 제약조건을 설정한다. </br>
     </br>
    nullable(DDL): null 값 허용 여부 제약조건을 설정한다. false로 하면 DDL 생성 시 not null 제약조건이 붙는다. </br>
 </br>
    insertable, updatable: 등록, 변경 가능여부. 기본값은 true이다. 즉, 기본적으로는 추가와 변경 제약이 없다. </br>
 </br>
    columnDefinition(DDL): DB 컬럼 정보를 직접 줄 수 있다. </br>
		ex) columnDefinition = "varchar(100) default 'EMPTY'" </br>
		    columnDefinition = "TEXT" </br>
 </br>
    length(DDL): 문자 길이 제약 조건을 주며, String 타입에만 사용한다. 기본값은 255. </br>
 </br>
    precision, scale(DDL): BigDecimal 또는 BigInteger 타입에서 사용. </br>
		precision은 소수점을 포함한 전체 자릿수를, (기본값 19) </br>
		scale은 소수의 자릿수를 설정한다. (기본값 2) </br>
		참고로, double, float 타입에는 적용되지 않는다. 정밀한 소수를 다룰 때만 사용한다. </br>
 </br>
DDL 이라고 붙인 것들은 애플리케이션에 영향을 주는 것은 아니고 DB에 영향을 준다. </br>
이는 ddl 자동 생성 시에만 관련이 있으며, 그 외 JPA 실행 로직에는 영향을 주지 않는다.  </br>
 </br>
 </br>
 </br>
- 기본키 컬럼 맵핑하기 </br>
기본키 컬럼이 될 필드에 붙이는 어노테이션은 다음과 같다: </br>
@Id, @GeneratedValue </br>
 </br>
 </br>
1) 기본키를 직접 할당할 경우 @Id만 사용한다. </br>
 </br>
 </br>
2) 만약 기본키를 자동생성할 경우 @GeneratedValue를 사용한다. </br>
이 @GeneratedValue를 사용하면 데이터를 생성할 때 id 값을 주지 않아도 DB가 알아서 순차적으로 값을 부여한다.  </br>
@GeneratedValue는 strategy 속성이 있는데, 기본값은 GenerationType.AUTO이다. </br>
GenerationType.AUTO는 DB 방언에 맞추어 자동으로 생성되도록 설정해주는 값이다. </br>
MySQL이나 PostgreSQL의 경우 GenerationType.IDENTITY를 사용하고, </br>
Oracle의 경우 GenerationType.SEQUENCE를 사용한다. </br>
 </br>
 </br>
3) @GeneratedValue(strategy = GenerationType.IDENTITY) </br>
IDENTITY는 기본키 값을 데이터베이스가 자동으로 생성하고, 새 엔티티가 영속화될 때 즉시 DB의 INSERT 쿼리가 생성되어야 기본키 값이 생성된다. IDENTITY에서는 기본키 값을 DB에 전적으로 위임하기 때문에 EntityManager가 INSERT 한 후에 기본키 값을 알 수 있다. </br>
JPA는 보통 tx.commit(); 시점에 INSERT문을 실행하기 때문에 INSERT 하기 전에는 기본키 값을 확실하게 알 수가 없다. 그래서 IDENTITY로 설정하면 tx.commit() 시점이아닌, em.persist() 시점에 INSERT 문을 실행한다는 특징이 있다. 그래서 IDENTITY로 설정해 놓은 경우 JPA가 아직 commit이 되지 않은 상황에서도 DB에서 해당 PK값을 조회해서 사용이 가능하다. </br>
정리하면, INSERT SQL문들을 모아서 처리하는 것은 IDENTITY에서는 불가능하다. </br>
그래도 버퍼해서 INSERT 하지 못한다고 게 크게 문제되는 것은 아니다. </br>
 </br>
 </br>
4) @GeneratedValue(strategy = GenerationType.SEQUENCE) </br>
SEQUENCE는 기본키 값을 DB가 자동으로 생성하는 것이 아니라, SEQUENCE 객체를 사용하여 DB의 고유한 숫자 값을 생성한다. 즉, DB에 직접 위임하는 게 아니라 SEQUENCE 객체에 위임하는 것이다. </br>
SEQUENCE 방식에서는 SEQUENCE를 미리 조회하여 기본키 값을 생성할 수 있으므로 EntityManager가 기본키 값을 미리 알 수 있다. 단, 시퀀스 값을 얻기 위한 추가 쿼리가 필요하다. </br>
그리고 strategy 속성뿐만 아니라 generator 속성을 통해 SequenceGenerator를 지정해야 한다. </br>
 </br>
예) </br>

```
@Entity
@SequenceGenerator(name = "employee_seq", sequenceName = "employee_sequence", allocationSize = 1)
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "employee_seq")
    private Long id;
 
    private String name;
   
    // getters and setters
}
```

만약 Sequence를 다른 방식으로 지정하고 싶다면 @SequenceGenerator 어노테이션을  </br>
클래스 앞에 걸어준다. @SequenceGenerator의 속성은 다음과 같다: </br>
    name: 해당 SequenceGenerator의 이름을 지정 </br>
    squenceName: 매핑할 DB의 시퀀스 이름을 입력 </br>
    initialValue: sequence의 첫 번째 값 입력 (기본값은 1) </br>
    allocationSize: sequence를 한 번 호출할 때마다 증가하는 수(기본값은 50). </br>
		만약 DB 시퀀스 값이 하나씩 증가하도록 설정하려면 반드시 1로 입력해주어야 한다. </br>
		이는 성능 최적화와 관련된 내용이다. </br>
 </br>
Sequence 방식으로 하면 SQL문에 "call next value for (SEQ 이름)" 이라는 로그가 뜬다. </br>
이게 다음 기본키 값을 불러오는 것이다. 그 후에 데이터를 PersistContext에 적재된다. </br>
그런데 계속 call next 하면 결국 DB와 통신을 하는 것이기 때문에 성능 문제가 발생하지 않을까? </br>
이 문제는 allocationSize 값과 관련된다. allocationSize의 기본값이 50이라고 했는데, </br>
이는 기본키 값을 50개 미리 가져와놓고 처리하는 방법이다. </br>
allocationSize = 1 이라고 해놓으면 하나 처리할 때마다 계속 통신해야 하는데 allocationSize를 50으로 하면 </br>
50개 처리할 때마다 한 번만 DB와 통신하면 된다. 이렇게 하면 네트워크 리소스를 아낄 수 있다. </br>
대신 그 50개는 메모리에 적재해놓고 다음 호출할 때는 메모리에서 기본키 값을 가져와서 처리한다. </br>
한편, DB는 1~50번까지 애플리케이션에게 기부한 것이기 때문에 51번부터 받게 된다. </br>
그리고 create sequence (sequence 이름) start with 1 increament by 50 이런 로그가 뜬다. </br>
50개의 기본키를 DB로부터 받은 해당 서버는 1번부터 50번까지 자료를 다 받으면 그 때 커밋한다. </br>
이 방법은 DB에서 기본값을 미리 받아서 처리하는 방식이기 때문에 여러 웹 서버에서 사용해도 동시성 이슈가 발생하지 않는다. </br>
 </br>
allocationSize 값을 너무 크게 잡으면 그만큼 메모리 공간을 소비하게 되며, 중간에 예기치 않은 오류가 발생하여 웹 애플리케이션 서버가 다운되면 사용하지 못한 기본키 값들은 낭비되고, ID값이 불연속적으로 되어 버린다. 그렇기 때문에 allocationSize 값을 터무니 없이 크게 잡아놓는 것은 좋지 않다. </br>
트래픽이 높은 애플리케이션은 allocationSize 값을 적당히 크게 잡는 것이 좋고, 애플리케이션 중단 가능성이 잦을 경우 allocationSize 값을 적당히 작게 잡는 것이 좋다. </br>
기본키 값의 연속성이 요구되는 환경에서는 allocationSize 값을 작게 설정해야 한다. </br>
 </br>
 </br>
5) @GeneratedValue(strategy = GenerationType.TABLE) </br>
이 경우는 별도의 테이블을 사용해서 기본키 값을 생성한다.  </br>
일반적으로 하나의 테이블을 만들고, 그 테이블에서 여러 엔티티들의 기본키 값들을 관리한다. </br>
유연성이 높지만 성능 저하를 일으킬 수 있다. </br>
마찬가지로 generator 속성을 통해 TableGerator를 지정해주어야 한다. </br>
이 기본키 생성 전략을 사용하면 엔티티 테이블 외에 기본키 관리 테이블이 추가로 생성되는 것을 실제로 </br>
DB에서 볼 수 있다. 이 테이블을 select 문으로 조회해보면 기본키 컬럼 이름과 NEXT_VAL 이라는 컬럼이 있다. </br>
이 NEXT_VAL 이라는 컬럼은 말 그대로 해당 테이블에 데이터가 새로 생성된다면 그 기본키 값을 부여하겠다는 의미이다. 즉 NEXT_VAL 컬럼은 다음에 사용할 기본키 값을 저장하고 있는 컬럼이다. </br>
 </br>
예) </br>

```
@Entity
@TableGenerator(name = "employee_gen", table = "id_generator", pkColumnName = "gen_name", valueColumnName = "gen_value", allocationSize = 1)
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.TABLE, generator = "employee_gen")
    private Long id;

    private String name;
    
    // getters and setters
}
```

 </br>
그러나 TABLE 전략은 많이 사용되지는 않는다. </br>
 </br>
 </br>
 </br>
- 권장되는 식별자 전략 </br>
1) Not Null </br>
2) UNIQUE </br>
3) 변하면 안 된다 </br>
이 세 번째 조건이 문제다. 당장은 만족하기 쉬워보여도 먼 미래를 바라봤을 때 이 조건을 만족하는 </br>
자연키를 찾는 것은 어렵다. 그래서 미리 대리키도 생각해놓아야 한다. </br>
여기서 자연키란 주민번호나 전화번호 등 비즈니스적으로 의미있는 번호들을 의미한다. </br>
그래서 id는 Long형으로 만들고, SEQUENCE나 UUID 등의 대체키도 생각해두어야 한다. </br>
절대 비즈니스적으로 의미있는 번호를 키 값으로 사용하는 것은 권장하지 않는다. </br>
 </br>
 </br>
