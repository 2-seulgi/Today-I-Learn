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
