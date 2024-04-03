# JDBC란?

JDBC(Java Database Connectivity)는 자바 언어로 데이터베이스에 접근할 수 있게 하는 프로그래밍 API다. JDBC는 데이터베이스와 통신하기 위한 자바 표준이며, 다양한 종류의 데이터베이스에 동일한 방법으로 접근할 수 있게 해 준다.

## JDBC의 주요 구성 요소

- **JDBC 드라이버**: 데이터베이스 제조사가 제공하는 드라이버로, 데이터베이스와 통신하기 위한 구현체
- **Connection**: 데이터베이스와의 연결을 나타냅니다. 연결을 통해 SQL 명령을 실행
- **Statement**: SQL 명령을 데이터베이스에 전달하는 데 사용. Statement, PreparedStatement, CallableStatement가 있음
- **ResultSet**: SQL 쿼리의 결과. 데이터를 읽어오거나 순회할 때 사용.

## JDBC 작동 방식

1. **JDBC 드라이버 로드**: 사용할 데이터베이스의 JDBC 드라이버를 로드한다.
2. **연결 생성**: DriverManager를 사용하여 데이터베이스에 연결을 생성한다.
3. **Statement 생성**: 연결을 통해 SQL을 실행할 Statement 객체를 생성한다.
4. **쿼리 실행**: Statement 객체를 사용하여 SQL 쿼리를 실행하고, 결과를 ResultSet 객체로 받는다.
5. **결과 처리**: ResultSet 객체를 순회하며 결과 데이터를 처리함
6. **자원 정리**: 사용한 ResultSet, Statement, Connection 객체를 닫아 자원을 해제.

## JDBC 사용 예제

```java
import java.sql.*;

public class JdbcExample {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/database_name"; // 데이터베이스 URL
        String user = "username"; // 데이터베이스 사용자명
        String password = "password"; // 비밀번호

        try {
            // 1. 드라이버 로드
            Class.forName("com.mysql.jdbc.Driver");

            // 2. 연결 생성
            Connection conn = DriverManager.getConnection(url, user, password);

            // 3. Statement 생성
            Statement stmt = conn.createStatement();

            // 4. 쿼리 실행
            ResultSet rs = stmt.executeQuery("SELECT * FROM table_name");

            // 5. 결과 처리
            while (rs.next()) {
                System.out.println(rs.getString("column_name"));
            }

            // 6. 자원 정리
            rs.close();
            stmt.close();
            conn.close();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## 쿼리용 XML을 사용한 컨트롤러-서비스-DAO 패턴과 직접적인 JDBC 접근 방식의 비교

회사에서 사용하는 방법은 JDBC 접근 방법이 아니라 쿼리 정의를 위해 XML과 함께 Controller-Service-DAO(Data Access Object, 때로는 Mapper라고도 함) 패턴을 사용하는 방식을 사용하고 있다. 둘 다 데이터베이스와 상호 작용을 하는데, 어떤 차이가 존재하는지 알고 싶어졌다.

### 직접적인 JDBC 접근 방식

직접적인 JDBC 접근 방식에는 데이터베이스 연결 수동 관리, SQL 문 실행, 결과 집합 처리 및 예외 관리가 포함된다. 이 방식으로 개발자는 데이터베이스 작업을 세밀하게 할 수 있지만, 다음과 같은 단점이 있다.

- **보일러플레이트 코드 :** 각 데이터베이스 상호 작용에 대해 반복적인 코드를 작성하고 유지 관리해야 한다.
- **리소스 관리:** 개발자는 데이터베이스 연결을 열고 닫을 책임이 있으며, 이를 제대로 처리하지 않으면 리소스 누출이 발생.
- **오류 처리:** 데이터베이스 작업에 대한 오류 처리를 수동으로 구현해야 함.
- **확장성:** 애플리케이션이 성장함에 따라 원시 JDBC 코드 관리는 점점 더 복잡해지고 오류가 발생하기 쉽다.

### 쿼리용 XML을 사용한 컨트롤러-서비스-DAO 패턴

대조적으로, Controller-Service-DAO 패턴은 특히 MyBatis 또는 Spring과 같은 프레임워크와 함께 사용될 때 데이터베이스 상호 작용에 필요한 상용구 코드의 대부분을 추상화한다.

- **관심사 분리:** 이 패턴은 웹 계층(컨트롤러), 비즈니스 로직 계층(서비스) 및 데이터 액세스 계층(DAO)을 명확하게 분리하여 애플리케이션을 더 쉽게 유지 관리하고 확장할 수 있다.
- **간단한 상용구:** 기본 JDBC 작업을 처리하는 프레임워크를 사용하면 개발자가 작성하는 상용구 코드가 훨씬 줄어든다. XML 매핑 파일 또는 주석은 Java 코드와 별도로 SQL 쿼리를 정의하여 가독성과 유지 관리성을 향상시킨다.
- **리소스 관리:** 프레임워크는 데이터베이스 리소스를 자동으로 관리하여 누출 위험을 줄일 수 있다.
- **오류 처리:** 일관된 오류 처리 메커니즘을 제공한다.
- **유연성:** Java 코드를 수정하지 않고도 기본 데이터베이스 또는 SQL 쿼리를 쉽게 변경할 수 있다.

### 장점과 단점

#### XML을 사용한 Controller-Service-DAO의 장점:

- **유지관리성:** 관심사를 명확하게 분리하여 코드를 유지 관리하고 구성하기가 더 쉽다.
- **유연성:** 비즈니스 로직이나 데이터베이스 쿼리를 변경하면 Java 코드를 건드리지 않고도 코드베이스의 최소한의 부분에 영향을 준다.
- **상용구 감소:** 프레임워크는 연결 열기 및 결과 세트를 개체에 매핑하는 등의 일반적인 작업을 처리한다.

#### 단점:

- **학습 곡선:** 프레임워크의 규칙과 구성을 이해해야 한다.
- **오버헤드:** 추가 레이어 및 추상화는 간단한 애플리케이션에 성능 오버헤드를 초래할 수 있다.
- **통제력 감소:** 많은 복잡성을 추상화하는 동시에 세분화된 데이터베이스 작업에 대한 개발자의 통제력을 줄인다.
