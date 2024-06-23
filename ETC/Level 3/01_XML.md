## 📓 키워드

- XML

---

## ✏️ XML

- Extensible Markup Language
- 마크업 형태를 쓰는 `데이터 교환 형식`
- 마크업은 태그 등을 이용하여 문서나 데이터의 구조를 나타내는 방법

### 💭 구성

- 프롤로그(버전, 인코딩)
- 루트요소
- 하위요소

```xml
<?xml version="1.0" encoding="UTF-8"?>
<MUSICList>
    <MUSIC like="1">
        <name>뉴진스</name> <song>하입보이</song>
    </MUSIC>
    <MUSIC like="2">
        <name>르세라핌</name> <song>EASY</song>
    </MUSIC>
</MUSICList>
```

### 💭 HTML과 XML의 비교

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <p></p>
    <div></div>
</body>
</html>
```

- HTML의 용도는 데이터를 표시, XML은 `데이터를 저장 및 전송`
- HTML에는 미리 정의된 태그가 있지만, XML에서는 `사용자가 고유한 태그를 만들고 정의 가능`
- HTML은 대소문자를 구분하지 않고, XML은 `대소문자를 구분함`

### 💭 JSON과 XML의 비교

```json
{
  "음악리스트" : [
    {
      "name" : "뉴진스",
      "song" : "하입보이"
    },
    {
      "name" : "르세라핌",
      "song" : "EASY"
    }
  ]
}
```

- XML은 닫힌 태그가 계속해서 들어가기 때문에 JSON보다 무거움
- XML은 JavaScript 객체로 변환(역직렬화)하기 위해서 JSON보다 더 많은 노력이 필요함

#### ☑️ sitemap.xml

- XML은 대표적으로 sitemap.xml에 사용됨
- 서비스 내의 모든 페이지들을 리스트업한 데이터
- 사이트가 매우 크거나 서로 링크가 종속적으로 연결되지 않은 경우, sitemap.xml이 크롤러가 일부 페이지를 누락하는 일을 방지하고 모든 페이지들을 크롤링할 수 있도록 도와줌

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    <url>
        <loc>http://www.example.com/foo.html</loc>
        <lastmod>2024-06-17</lastmod>
    </url>
</urlset>
```