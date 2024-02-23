## ✏️ 문자열 타입

---

### 💭 CHAR

- 테이블을 생성할 때 선언한 길이로 `고정된 길이`
- 0 ~ 255 사이의 값을 가짐

### 💭 VARCHAR

- 가변 길이 문자열
- 0 ~ 65,535 사이의 값을 가짐
- 입력한 데이터에 따라 `용량을 가변시켜 저장`
    - ex) 데이터의 길이가 10바이트라면 11바이트(10바이트 + 길이기록용 1바이트)로 가변되어 저장됨
- 최대 길이가 255를 초과하는 경우, `데이터의 길이는 2바이트`를 사용하여 저장

> 유동적인 데이터는 VARCHAR, 고정적인 데이터는 CHAR로 저장하는게 좋다

#### ☑️ VARCHAR 최대길이로 설정할 시 주의할 점

- MySQL의 행의 최대크기는 65,535바이트이다.
- MySQL은 UTF-8을 지원하는 utf8mb4 인코딩 방식이다. 이는 4바이트가 필요한 이모지와 3바이트가 필요한 한글을 모두 커버하는 인코딩방식이다.
- 괄호안에 들어가는 숫자는 바이트가 아니라 `길이제한`이다.
  - 이모지의 경우 1글자 = 4바이트 / 16,383글자 = 65,533바이트

```sql
/*
 ERROR
 바이트가 아닌 길이제한이므로 불가
 */
CREATE TABLE t3(
    C1 VARCHAR(65533) NOT NULL
);

/*
 Working
 UTF-8이어서 4바이트 이모지까지 포함하기 위해 65533/4 = 16383
 */
CREATE TABLE t4(
    c1 VARCHAR(16383) NOT NULL
);

/*
 Working
 UTF-8 방식이 아닌 ASCII방식으로 변경하면 가능
 */
CREATE TABLE t5(
    c1 VARCHAR(65533) NOT NULL
) ENGINE = InnoDB CHARACTER SET latin1;

/*
 ERROR
 title필드가 이미 최대치를 차지하고있으므로, id필드가 차지할 공간 4바이트가 더해지면 넘쳐버림
 */
CREATE TABLE book1(
    id INT NOT NULL AUTO_INCREMENT,
    title VARCHAR(65533) NOT NULL,
    PRIMARY KEY(id)
) ENGINE = InnoDB CHARACTER SET latin1;

/*
 Working
 id필드가 차지하는 공간 4바이트에 title필드의 65529바이트가 더해지면 최대 65533이므로 가능
 */
CREATE TABLE book2(
    id INT NOT NULL AUTO_INCREMENT,
    title VARCHAR(65529) NOT NULL,
    PRIMARY KEY(id)
) ENGINE = InnoDB CHARACTER SET latin1;
```

### 💭 TEXT

- 최대 65,535개 길이의 텍스트 데이터를 저장할 수 있음

#### ☑️ VARCHAR, TEXT 비교

- VARCHAR
  - 행 내부(메모리)에 데이터를 직접 저장
  - 65,535까지 MAX SIZE LIMIT을 걸 수 있음(길이기록 2바이트 제외시 65,533바이트)
  - 인덱스를 걸 수 있음
  - 중간 정도의 문자열, 메모리에서 읽으므로 읽기성능이 TEXT보다 좋음
- TEXT
  - 행 외부(디스크)에 저장되며, 행 내부에는 포인터(8바이트)가 저장됨
  - MAX SIZE LIMIT을 걸 수 없다. 무조건 최대길이인 65,535길이의 데이터 저장가능
  - 매우 큰 문자열, 디스크에서 읽기 때문에 읽기성능이 좋지않아 검색과 수정이 빈번하지 않은 데이터를 저장할때 사용
  - 인덱스를 걸 때 크기제한을 해줘야됨
  ```sql
  CREATE TABLE board(
    id INT NOT NULL AUTO_INCREMENT,
    title VARCHAR(255),
    content TEXT,
    PRIMARY KEY(id)
  );
  CREATE INDEX idx_content ON board(content(20)); <!-- 크기제한 -->
  ```

### 💭 BLOB

- 이진데이터를 저장하는데 사용하는 데이터 타입
- 주로 이미지, 오디오, 비디오 등을 저장하며 파일을 직접 데이터에 삽입해서 사용
  ```sql
  INSERT INTO board(img) VALUES (LOAD_FILE('C:/Users/dongho/Desktop/a.png'));
  ```
- 그러나 보통은 BLOB타입을 사용하지 않고, `이미지 호스팅 서비스(Amazon S3)를 이용해서 해당 서비스에 파일을 올리고 URL을 VARCHAR타입으로 저장하는게 일반적`

#### ☑️ BLOB을 안쓰는 이유
- 성능문제 : 용량이 큰 파일을 DB에 저장하는 경우, 처리나 복구 같은 시간이 오래걸려 성능저하
- 보안문제 : DB에 직접 이미지를 저장하면, 이미지에 대한 접근을 제어하고 관리하는 것이 더 복잡해짐

---

## ✏️ 열거형 타입

문자열을 열거한 타입, 지정된 문자열 이외의 값이 들어가게 되면 에러가 발생

---

### 💭 ENUM

- 열에 할당할 수 있는 값의 리스트를 정의
- 리스트 중 하나만 선택하는 `단일선택`만이 가능
- 내부적으로 숫자로 저장되지만, 사용자에게는 문자열로 표시됨
- 최대 65,535개의 요소들을 넣을 수 있음

### 💭 SET

- 하나의 열에 여러값을 저장할 수 있음
- 비트단위 연산 가능
- 한번에 `여러개의 조합`으로 선택이 가능
- 64개의 요소들을 넣을 수 있음

```sql
CREATE TABLE socks(
    id INT NOT NULL AUTO_INCREMENT,
    color_enum ENUM('red', 'green', 'blue') NOT NULL,
    color_set SET('red', 'green', 'blue') NOT NULL,
    PRIMARY KEY(id)
);

INSERT INTO socks(color_enum) VALUES('red'), ('blue'), ('green');
INSERT INTO socks(color_set) VALUES('red'), ('red, green'), ('red, green, blue');
```

- ENUM과 SET의 경우 공간적으로 이점이 있으나, 애플리케이션 수정에 따라 DB에 정의된 리스트를 매번 수정해야한다는 단점이 존재