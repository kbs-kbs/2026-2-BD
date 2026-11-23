## 웹크롤링의 개념
- HTML에 표현된 여러 건의 데이터를 추출하는 프로그램
- 흐름: 웹 페이지 → `urllib`로 요청 → `beautifulsoup`로 원하는 부분 추출 → 데이터
- 예: YES24에서 소설책 정보 수집
- 인터넷에 공개된 데이터를 가져와 필요한 형식으로 변환하는 것이 데이터를 수집하는 가장 빠른 방법

| 용어 | 의미 |
|---|---|
| 크롤링(Crawling) | 웹상에 공개된 내용에서 데이터를 추출하는 것 |
| 크롤러(Crawler) | 웹 크롤링 작업을 하는 프로그램 |

- 웹 스크래핑과 구분하기도 한다: 스크래핑은 특정 페이지의 데이터 추출, 크롤링은 추출할 대상 페이지를 찾아가는 개념(14주차 2차시 참고)

## HTML 기초
- HTML(HyperText Markup Language): 웹 페이지를 만들기 위한 대표적인 마크업 언어
- 부등호(`<>`) 안에 들어 있는 구문을 태그(Tag)라고 부름
- 파일 확장자는 `*.htm` 또는 `*.html`, 텍스트 파일이므로 메모장에서도 작성 가능
- 한글이 깨지지 않도록 인코딩은 UTF-8로 저장
- HTML은 대문자·소문자를 구분하지 않음
- `<html>` 태그로 시작해 `</html>`로 종료

### 기본 구조
```html
<html>
  <head>
    <!-- 화면에 표시되지 않는 정보(타이틀, 인코딩 등) -->
  </head>
  <body>
    <!-- 화면에 표시되는 본체(주로 태그) -->
  </body>
</html>
```

| 태그 | 역할 |
|---|---|
| `<html>~</html>` | 전체 코드를 감쌈 |
| `<head>~</head>` | 타이틀·인코딩 등 화면에 표시되지 않는 정보 |
| `<body>~</body>` | 화면에 표시되는 본체, 태그의 속성 표시 가능 |
| `<title>~</title>` | 브라우저 타이틀바에 표시되는 페이지 제목 |
| `<meta>` | 페이지 정보 설정, 검색엔진 요약, 언어·문자 코딩(UTF-8) 지정 |
| `<br>` | 행 바꿈 |
| `<u>`, `<b>`, `<i>` | 밑줄, 볼드, 이탤릭 |
| `<font>` | 글자 크기·색상 |
| `<hr>` | 수평선, `size`로 폭 지정 |
| `<a>~</a>` | 링크, `href`로 대상 지정, `target` 없으면 현재 페이지에서 열림 |
| `<img>` | 이미지 표시, `width`·`height` 생략 시 원본 크기 |
| `<table>`, `<tr>`, `<th>`, `<td>` | 표. 행은 `<tr>`, 열은 `<th>`(제목, 볼드)·`<td>`(일반) |
| `<div>~</div>` | Division, 레이아웃용 블록 묶음. `style` 지정 가능 |
| `<ul>`, `<li>` | 순서 없는 목록과 항목. `id`·`class`와 함께 사용 |
| `<strong>`, `<em>` | 글자 강조 |
| `<p>~</p>` | 문단 |
| `<span>~</span>` | `<div>`와 용도가 비슷하나 한 줄 단위 지정에 사용 |

### id와 class
- `id`: HTML 문서에서 유일한 값, 하나의 태그에만 지정
- `class`: 여러 태그에 지정 가능
- 크롤링에서 "특정 부분만 추출"할 때 이 두 속성이 기준이 된다

```html
<div id="myid">div에 id를 설정했어요.</div>
<div class="myclass">div에 class를 설정했어요.</div>
<div class="myclass">div에 class를 또 설정했어요.</div>
```

## beautifulsoup 설치와 기본 사용
- 설치 패키지 이름은 bs4(beautifulsoup 버전 4)
```bash
pip install bs4
```

### HTML 읽기
```python
import bs4

webPage = open('C:/CookAnalysis/HTML/Sample02.html', 'rt', encoding='utf-8').read()
bsObject = bs4.BeautifulSoup(webPage, 'html.parser')
print(bsObject)
```

### 태그 하나 찾기
```python
import bs4

webPage = open('C:/CookAnalysis/HTML/Sample02.html', 'rt', encoding='utf-8').read()
bsObject = bs4.BeautifulSoup(webPage, 'html.parser')
tag_div = bsObject.find('div')
print(tag_div)
```

### 여러 건 추출
```python
import bs4

webPage = open('C:/CookAnalysis/HTML/Sample02.html', 'rt', encoding='utf-8').read()
bsObject = bs4.BeautifulSoup(webPage, 'html.parser')

tag_ul = bsObject.find('ul')          # ul 전체
tag_li = bsObject.find('li')          # 첫 번째 li
tag_li_all = bsObject.findAll('li')   # 모든 li (리스트)
print(tag_ul)
print(tag_li)
print(tag_li_all)
```

### id·class로 특정 태그 추출
```python
bsObject.find('태그명', {'속성명': '속성값'})
bsObject.findAll('태그명', {'속성명': '속성값'})
```

```python
import bs4

webPage = open('C:/CookAnalysis/HTML/Sample03.html', 'rt', encoding='utf-8').read()
bsObject = bs4.BeautifulSoup(webPage, 'html.parser')

tag = bsObject.find('div', {'id': 'myId1'})
print(tag)
tag = bsObject.find('div', {'class': 'myClass1'})    # 첫 번째 하나
print(tag)
tag = bsObject.findAll('div', {'class': 'myClass1'}) # 전부 리스트
print(tag)
```

- `find()`는 id든 class든 1개만, `findAll()`은 해당하는 것을 모두 리스트로 반환

### ul/li 값 추출
```python
ul_value = bsObject.find('ul', {'class': 'myClass2'})
print(ul_value)
li_list = bsObject.findAll('li', {'class': 'myClass3'})
print(li_list)
```

### a 태그의 href 모두 출력
```python
a_list = bsObject.findAll('a')
for aTag in a_list:
    print(aTag['href'])
```

### 텍스트만 출력
```python
tag_li_all = bsObject.findAll('li')
for tag_li in tag_li_all:
    print(tag_li.text)
print()
for i in range(len(tag_li_all)):
    print(tag_li_all[i].text)
```

## 웹사이트 정보 추출
### 개발자 도구로 위치 찾기
1. 브라우저에서 대상 사이트 접속
2. 설정 및 기타 → 기타 → 개발자 도구
3. "검사할 페이지 요소를 선택하세요" 아이콘을 누르고 화면에서 원하는 부분 클릭
4. 오른쪽 소스에서 해당 태그·id·class를 확인

### urllib로 실제 사이트 읽기
```python
import bs4
import urllib.request

nateUrl = "https://www.nate.com"
htmlObject = urllib.request.urlopen(nateUrl)
webPage = htmlObject.read()
bsObject = bs4.BeautifulSoup(webPage, 'html.parser')

tag = bsObject.find('div', {'id': 'NateBi'})
print(tag, '\n')

a_tag = tag.find("a")
print(a_tag, '\n')

href = a_tag['href']
print(href, '\n')

text = a_tag.text
print(text)
```

- 결과: `//www.nate.com/?f=bi`, `네이트`

### 메뉴 목록 추출
```python
# Code09-10.py의 1~7행과 동일. 단 주소는 "https://news.nate.com"
tag = bsObject.find('div', {'class': 'snbArea'})
print('## 네이트 뉴스의 메뉴 목록 ##')
li_list = tag.findAll('li')
for li in li_list:
    print(li.text, end=' ')
```

- 결과: 오늘 종합 최신 뉴스 정치 경제 사회 세계 IT/과학 칼럼 포토 TV 라디오 랭킹 뉴스투데이 댓글

## 시간 간격 크롤러
- 시간에 따라 변하는 데이터(실시간 검색어, 현재 날씨, 인기 뉴스)를 주기적으로 저장
- 예: 네이트 날씨의 온도·습도·강수량·풍향을 1시간마다 CSV로 저장

```python
import csv
import time
import datetime

csvName = 'C:/CookAnalysis/CSV/datetime.csv'

with open(csvName, 'w', newline='') as csvFp:
    csvWriter = csv.writer(csvFp)
    csvWriter.writerow(['연월일', '시분초'])

count = 10
while count > 0:
    count -= 1
    now = datetime.datetime.now()
    yymmdd = now.strftime('%Y-%m-%d')
    hhmmss = now.strftime('%H:%M:%S')
    time_list = [yymmdd, hhmmss]
    print(time_list)

    with open(csvName, 'a', newline='') as csvFp:
        csvWriter = csv.writer(csvFp)
        csvWriter.writerow(time_list)

    time.sleep(3)
```

- 실행 결과: `2024-03-05,18:04:10` 처럼 3초 간격으로 행이 추가됨
- 실제 1시간 간격 운용에서는 `time.sleep(3600)`으로 바꾸고, 수집 부분을 bs4 추출 코드로 대체한다

## 정리
- 크롤링의 순서는 항상 같다: 페이지 구조 파악(개발자 도구) → 요청 → 파싱 → 원하는 태그 선택 → 텍스트/속성 추출 → 저장
- `find`는 하나, `findAll`은 여러 개이며, `id`는 유일·`class`는 중복 가능이라는 차이가 선택 기준이 된다
