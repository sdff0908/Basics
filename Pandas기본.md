# Pandas 기본

> Series와 DataFrame이라는 자료구조로 구성

* Series

  numpy + a
  내부적으로 numpy 사용 → 동일한 데이터 타입으로 구성, numpy의 dtype그대로 사용가능(np.float64, np.int32, np.object) 
  1차원 자료구조   

* DataFrame

  Series의 모음
  Table 형식으로 데이터 저장
  각각의 column과 row는 Series  

## 1. Series


```python
s = pd.Series([1, 2, 3, 4, 5], dtype = np.float64)  
print(s)                                   				#1차원 자료구조, 인덱스랑 같이 출력
print('Series 값만 출력:{}'.format(s.values))
print('Series index만 출력:{}'.format(s.index))
```

```html
0    1.0
1    2.0
2    3.0
3    4.0
4    5.0
dtype: float64
Series 값만 출력:[1. 2. 3. 4. 5.]
Series index만 출력: RangeIndex(start = 0, stop = 5, step = 1)
```

### 1) indexing

* 인덱스 지정

```python
import numpy as np
import pandas as pd

s = pd.Series([1, 6, 8, 9, 4], dtype = np.int32, index = ['a', 'b', 'c', 'd', 'e'])
print(s)
print(s['c'])
print(s[2])      									#인덱스가 변경되어도 숫자 인덱스 사용가능
```

```html
a    1
b    6
c    8
d    9
e    4
dtype: int32
8
8
```

```python
s = pd.Series([1, 6, 8, 9, 4], dtype = np.int32, index = ['a', 'b', 'c', 'd', 'e'])
print(s[0:3])
print(s['a':'d'])				#지정한 인덱스로 슬라이싱할 경우, a번째 값부터 d번째 값까지 모두 포함
print(s[['a', 'd']])				#fancy indexing
print(s[s % 2 == 0])				#boolean indexing
```

```html
a    1
b    6
c    8
dtype: int32
a    1
b    6
c    8
d    9
dtype: int32
a    1
d    9
dtype: int32
b    6
c    8
e    4
dtype: int32
```

사용자 지정 인덱스가 중복될 경우

```python
s=pd.Series([1, 6, 8, 97, 4], dtype = np.int32, index = ['a', 'b', 'a', 'd', 'e'])
print(s)
print(s['a'])
print(type(s['a']))
print(type(s['b']))
print(s['b'] + s['a'])    							#s['a']: Series, s['b']: int
```

```
a     1
b     6
a     8
d    97
e     4
dtype: int32
a    1
a    8
dtype: int32
<class 'pandas.core.series.Series'>
<class 'numpy.int32'>
a     7
a    14
dtype: int32
```

인덱스 기준으로 내림차순

```python
s = pd.Series([11, 21, 33, True, '55'], dtype = np.object, index = ['a', 'b', 'e', 's', 'h'])
print(s.sort_index(ascending = False))
```

```html
s    True
h      55
e      33
b      21
a      11
dtype: object
```

사용자 지정 인덱스 응용

```python
import numpy as np
import pandas as pd
from datetime import datetime, timedelta				       #datetime모듈: 일, 주, 월, 년 단위로 증감
start_day = datetime(2020, 1, 1)
print(start_day)

factoryA = pd.Series([int(x) for x in np.random.normal(50, 5, (10,))], 
                     index = [start_day + timedelta(days = x) for x in range(10)])
#factoryA 하루생산량은 편균 50, 표준편차 5인 정수 정규분포를 따른다
print(factoryA)

start_day = datetime(2020, 1, 5)
factoryB = pd.Series([int(x) for x in np.random.normal(70, 8, (10,))], 
                     index = [start_day + timedelta(days = x) for x in range(10)])
#factoryB 하루생산량은 편균 70, 표준편차 8인 정수 정규분포를 따른다
print(factoryB)

print(factoryA + factoryB)     							#Series 사칙연산은 같은 index끼리 가능
										#NaN = Not a number
```

```html
2020-01-01 00:00:00
2020-01-01    55
2020-01-02    50
2020-01-03    55
2020-01-04    54
2020-01-05    43
2020-01-06    46
2020-01-07    47
2020-01-08    50
2020-01-09    53
2020-01-10    53
dtype: int64
2020-01-05    72
2020-01-06    76
2020-01-07    81
2020-01-08    61
2020-01-09    60
2020-01-10    58
2020-01-11    61
2020-01-12    71
2020-01-13    62
2020-01-14    65
dtype: int64
2020-01-01      NaN
2020-01-02      NaN
2020-01-03      NaN
2020-01-04      NaN
2020-01-05    115.0
2020-01-06    122.0
2020-01-07    128.0
2020-01-08    111.0
2020-01-09    113.0
2020-01-10    111.0
2020-01-11      NaN
2020-01-12      NaN
2020-01-13      NaN
2020-01-14      NaN
dtype: float64
```

### 2) Series  만들기

* list로 Series 만들기

```python
s = pd.Series([1, 5, 7, 8])
print(s)
```

```html
0    1
1    5
2    7
3    8
dtype: int64
```

* dictionary로 Series 만들기

```python
my_dict = {'Seoul': 1000, 'Incheon': 300, 'Suwon': 500}
s = pd.Series(my_dict)
print(s)               									#key값이 index가 된다
print(s.index)         									#list는 아니지만 list와 동일하게 사용가능
s.index = ['서울', '인천', '수원']							   #인덱스 변경
s.index.name = 'Region'
s.name = '지역별 가격'
print(s)
```

```html
Seoul      1000
Incheon     300
Suwon       500
dtype: int64
Index(['Seoul', 'Incheon', 'Suwon'], dtype='object')
Region
서울    1000
인천     300
수원     500
Name: 지역별 가격, dtype: int64
```

# 2. DataFrame 

### 1) 데이터 표현방식

일반적으로 많이 사용하는 데이터 표현방식 3가지

* CSV(Comma Separated Values)

  예) 김김김, 20, 서울, 이이이, 25, 인천, 박박박, 36, 대구, ...
  장점: 데이터 용량 작음 → 많은 데이터 표현에 적합
  단점: 데이터 구성 알기 어려움, 데이터 처리 위해 프로그램을 따로 만들어야
  	 (데이터 변경 시 프로그램도 같이 변경해야 → 유지 보수 문제 발생)

* XML(Extended Markup Language)

  예) <person><name>김김김</name><age>20</age><address>서울</address></person>
  	<person><name>이이이</name><age>25</age><address>인천</address></person>
  	<person><name>박박박</name><age>36</age><address>대구</address></person>
  장점: 데이터 구성 알기 쉬움, 사용 편함, 프로그램 유지보수 쉬움
  단점: 데이터 용량 크다(실 데이터에 비해 부가적인 데이터가 더 많다)

* JSON(JavaScript Object Notation): 현재 일반적인 데이터 '표현방식'(특정 프로그래밍 언어와 상관X)

  자바 스크립트 객체 표현 방식 이용하여 데이터 표현
  예) {'name': '김김김', 'age': 20, 'address': 서울}
        {'name': '이이이', 'age': 25, 'address': 인천}
  	  {'name': '박박박', ' age': 36, 'address': 대구}
  장점: 데이터 구성 알기 쉬움, 사용 편함, 프로그램 유지보수 쉬움, XML보다 용량 작음
  단점: CSV에 비해서는 부가적인 데이터 많다

### 2) DataFrame 만들기

* dictionary사용하여 수동으로 데이터 입력하기

적은 양의 데이터 처리할 때 적합

```python
import numpy as np
import pandas as pd
#dictionary로 DataFrame만들기
my_dict = {'name': ['서서서', '차차차', '김김김', '구구구'], 
		   'year': [2012, 2015, 2014, 2017],
		   'point': [3.5, 3.9, 3.2, 4.3]}
                
df=pd.DataFrame(my_dict)
print(df)
display(df)
print(df.shape)
print(df.size)
print(df.ndim)				#DataFrame은 항상 2차원
print(df.index)
print(df.columns)
print(df.values)			#2차원 ndarray형태로 출력

df.index.name = '학생번호'
df.columns.name = '학생정보'
display(df)
```

```html
  name  year  point
0  서서서  2012    3.5
1  차차차  2015    3.9
2  김김김  2014    3.2
3  구구구  2017    4.3
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th></th>
      <th>name</th>
      <th>year</th>
      <th>point</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>서서서</td>
      <td>2012</td>
      <td>3.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>차차차</td>
      <td>2015</td>
      <td>3.9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>김김김</td>
      <td>2014</td>
      <td>3.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>구구구</td>
      <td>2017</td>
      <td>4.3</td>
    </tr>
  </tbody>
</table>
```html
(4, 3)
12
2
RangeIndex(start = 0, stop = 4, step = 1)
Index(['name', 'year', 'point'], dtype = 'object')
[['서서서' 2012 3.5]
 ['차차차' 2015 3.9]
 ['김김김' 2014 3.2]
 ['구구구' 2017 4.3]]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>학생정보</th>
      <th>name</th>
      <th>year</th>
      <th>point</th>
    </tr>
    <tr>
      <th>학생번호</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>서서서</td>
      <td>2012</td>
      <td>3.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>차차차</td>
      <td>2015</td>
      <td>3.9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>김김김</td>
      <td>2014</td>
      <td>3.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>구구구</td>
      <td>2017</td>
      <td>4.3</td>
    </tr>
  </tbody>
</table>

*  csv파일 이용하여 하여 데이터 읽기


```python
import numpy as np
import pandas as pd
df = pd.read_csv('C:/Users/Hwayeon Kim/Desktop/movies.csv')	#csv_read:','로 데이터 구분하여 읽어오기
display(df.head())						#상위 5개 행 출력
print(df.shape)
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th></th>
      <th>movieId</th>
      <th>title</th>
      <th>genres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Toy Story (1995)</td>
      <td>Adventure|Animation|Children|Comedy|Fantasy</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Jumanji (1995)</td>
      <td>Adventure|Children|Fantasy</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Grumpier Old Men (1995)</td>
      <td>Comedy|Romance</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Waiting to Exhale (1995)</td>
      <td>Comedy|Drama|Romance</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Father of the Bride Part II (1995)</td>
      <td>Comedy</td>
    </tr>
  </tbody>
</table>
```html
(9742, 3)
```

* Database 사용하여 데이터 가져오기

  Database: 데이터 집합
  DBMS: Database관리하는 프로그램, DBMS 시작해야 Database 생성, 접근 가능
  DBMS 종류: Oracle, DB2, SQL Server, PostgreSQL, MySQL, MariaDB, Infomix, CyBase 등

``` python
import pymysql
import pandas as pd
import numpy as np

#파이썬 프로그램이 DBMS에 접속 -> Database접속 -> 연결객체 생성
#Database에서  데이터 가져오기: Database에서 사용되는 언어(여기서는 SQL사용)로 query를 전달
#btitle에 {keyword}가 포함되어 있으면 bisbn, btitle, bauthor, bprice 데이터 가져오기  

conn = pymysql.connect(host = 'localhost', user = 'data', password = 'data', db = 'library', charset = 'utf8')
keyword = '물리'
sql = "SELECT bisbn, btitle, bauthor, bprice FROM book WHERE btitle LIKE '% {}%'".format(keyword)

#파이썬의 예외처리 이용하면 좋다
try:
    df = pd.read_sql(sql, con = conn)
    display(df)
except Exception as err:
    print(err)
finally:
    conn.close()  
#DataFrame 생성 완료  
```

|      |             bisbn |                                                       btitle |                          bauthor | bprice |
| ---: | ----------------: | -----------------------------------------------------------: | -------------------------------: | -----: |
|    0 | 978-89-6848-155-0 | 게임 개발자를 위한 물리(개정2판) : 차량, 스포츠, 폭발, 모바일 등 게임 물리... | 데이비드 버그, 브라이언 비발레츠 |  34000 |
|    1 | 978-89-7914-728-5 | Head First Physics : 생생한 게임 개발에 꼭 필요한 물리 이야기 |                          헤더 랭 |  36000 |
|    2 | 978-89-7914-897-8 |                                   생각하며 배우는 대학물리학 |                           이기영 |  30000 |
|    3 | 978-89-98756-03-1 |                      IT CookBook, 개념이 보이는 물리전자공학 |                           이현용 |  24000 |

SQL은 데이터베이스용 프로그래밍 언어 중 하나
SQL 기반 DBMS: Oracle, DB2, SQL Server, PostgreSQL, MySQL, SQLite 등

* Open API 사용하여 데이터 가져오기

API = Application Programming Interface: 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스
Interface: 서로 다른 두 개의 시스템, 장치 사이에서 정보나 신호를 주고받는 접점이나 경계면. 즉, 사용자가 기기를 쉽게 동작시키는데 도움을 주는 시스템

```python
#url통해 얻은 JSON 형태의 데이터 -> dictionary 형태 -> 2차원 데이터 형태 -> DataFrame

import numpy as np
import pandas as pd
import urllib
import json

#요청 url
open_api = 'http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json'

#Query String
query_str = '?key=9ab5cf9a1ca0e87ec3418c5ebb92c240&targetDt=20210114'

open_api_url = open_api + query_str
```

Open API url = 요청 url + Query String
Query String: 요청 인자를 전달하기 위한 형식
			  		   물음표('?')로 시작, key와 value의 쌍으로 구성
			 			요청 인자가 여러개일 때 '&'사용
			  		   위 예에서 요청인자는 2개: key와 targetDt

url 이용해서 데이터/서비스 호출하는 행위: request
request 결과로 웹서버에서 우리에게 전달하는 행위: response
전달 내용을 Response객체형태로 받을 수 있다

```python
page_obj = urllib.request.urlopen(open_api_url)           #Response객체, 이 객체 안에는 원하는 데이터(json형태) + a 포함 
json_page = json.loads(page_obj.read())                   #객체 안에 있는 json 읽기, json형태의 데이터를 python의 dict로 변환
print(json_page)    
print(type(json_page)) 
```

```python
{'boxOfficeResult': {'boxofficeType': '일별 박스오피스', 'showRange': '20210114~20210114', 'dailyBoxOfficeList': [{'rnum': '1', 'rank': '1', 'rankInten': '0', 'rankOldAndNew': 'NEW', 'movieCd': '20198056', 'movieNm': '아이 엠 우먼', 'openDt': '2021-01-14', 'salesAmt': '27630740', 'salesShare': '16.5', 'salesInten': '27630740' ...}]}}
```

원하는 내용을 DataFrame으로 생성

```python
my_dict = dict()
rank_ls = list()
title_ls = list()
sales_ls = list()

for tmp_dict in json_page['boxOfficeResult']['dailyBoxOfficeList']:
    rank_ls.append(tmp_dict['rank'])
    title_ls.append(tmp_dict['movieNm'])
    sales_ls.append(tmp_dict['salesAmt'])
my_dict['순위'] = rank_ls
my_dict['영화제목']  = title_ls    
my_dict['일일매출액'] = sales_ls    

df = pd.DataFrame(my_dict)
display(df)
```

|      | 순위 |                       영화제목 | 일일매출액 |
| ---: | ---: | -----------------------------: | ---------: |
|    0 |    1 |                   아이 엠 우먼 |   27630740 |
|    1 |    2 |                       블라인드 |   21737100 |
|    2 |    3 |                 원더 우먼 1984 |   21412470 |
|    3 |    4 |                    #아이엠히어 |   18253930 |
|    4 |    5 |                       화양연화 |   11963480 |
|    5 |    6 |                    늑대와 춤을 |    9415040 |
|    6 |    7 |             마이 미씽 발렌타인 |    8214190 |
|    7 |    8 | 빛의 아버지: 파이널 판타지 XIV |    4462300 |
|    8 |    9 |                       언플랜드 |    2582000 |
|    9 |   10 |                           도굴 |    4180890 |

### 3) DataFrame 전달하기

DataFrame은 자료구조
DataFrame이 갖고 있는 건 내 컴퓨터에 저장되어 있는 데이터
내가 가진 DataFrame의 내용을 다른 컴퓨터나 다른 사람에게 전달하기 위해서는 표준형태의 데이터 표현방식(CSV, XML, JSON 중 하나)으로 변환시켜서 전달해야 한다

* DataFrame → JSON

```python
import numpy as np
import pandas as pd

#C:/python_ML/data/books_orient_column.json 에 JSON파일 생성하기
#.:현재 위치(C:/python_ML)

#DataFrame의 column 기준으로 데이터 입력
with open('./data/books_orient_column.json','w',encoding = 'utf-8') as file1:
    df.to_json(file1, force_ascii = False, orient = 'columns')           

#DataFrame의 row 기준으로 데이터 입력
with open('./data/books_orient_records.json','w',encoding = 'utf-8') as file1:
    df.to_json(file1, force_ascii = False, orient = 'records')           
```

JSON 파일 번역기: https://jsonformatter.curiousconcept.com/

*  JSON → DataFrame

```python
import numpy as np
import pandas as pd
                
#JSON을 읽어서 파이썬의 dict로 변환
with open('./data/books_orient_column.json','r',encoding = 'utf-8') as file1:
    books = pd.read_json(file1) 

#JSON을 읽어서 파이썬의 list로 변환
with open('./data/books_orient_records.json','r',encoding = 'utf-8') as file1:
    books = pd.read_json(file1) 
```

### 4) column CRUD

```python
import numpy as np
import pandas as pd

data = {'이름': ['박동수', '이고은', '강수지', '김진우'], 
        '학과':['수학', '물리', '생명공학', '중어중문'], 
        '학년':[1, 4, 3, 2], 
        '학점': [3.3, 4.1, 3.8, 3.6]}
df = pd.DataFrame(data, 
                  columns = ['학과', '이름', '학년', '학점'], 
                  index = ['1st', '2nd', '3rd', '4th'])  
display(df)
```

|      |     학과 |   이름 | 학년 | 학점 |
| ---: | -------: | -----: | ---: | ---: |
|  1st |     수학 | 박동수 |    1 |  3.3 |
|  2nd |     물리 | 이고은 |    4 |  4.1 |
|  3rd | 생명공학 | 강수지 |    3 |  3.8 |
|  4th | 중어중문 | 김진우 |    2 |  3.6 |

* column 인덱싱

column명 사용

```python
print(df['이름'])                                          #Series로 출력
stu_name=df['이름']                                        #View가 나온다(View수정하면 원본 데이터 수정됨)
stu_name=df['이름'].copy()             			 #View가 아닌, 별도의 Series 생성

display(df[['이름','학년']]) 			        #Fancy indexing(여러 column출력 시)
                                                           #DataFrame 형태로 출력(View)
display(df['이름':'학년'])   			        #Slicing 불가, ERROR!
                                                           #boolean indexing은 column과 상관 X
```

* column 추가

직접 추가

```python
df = pd.DataFrame(data, 
                  columns = ['학과', '이름', '학년', '학점', '등급'], 
                  index = ['1st', '2nd', '3rd', '4th'])  

#'등급' column 추가
df['등급'] = ['A', 'B', 'C', 'B']      #리스트를 줘도 내부적으로 ndarray로 변환하여 데이터 입력
df['등급'] = np.array(['A', 'B', 'C', 'B'])
df['등급'] = ['A', 'B', 'C', np.nan]   #없는 값은 반드시 np.nan 입력해야. 아니면 오류 발생
df['등급'] = ['A', 'B', 'C', '']       #값 존재. 문자열('')있음. 문자열 안의 내용이 없을 뿐

#'나이' column 추가
df['나이'] = pd.Series([19, 20, 25, 24])  
#Series는 인덱스 기반으로 데이터 처리.인덱스 다르면 입력X
df['나이'] = pd.Series([19, 24], index = ['1st', '4th'])
#Series는 인덱스 기반 -> 개수 안 맞춰도 된다	
```

연산을 통해 새로운 column추가

```python
df = pd.DataFrame(data, 
                  columns = ['학과', '이름', '학년', '학점'], 
                  index = ['1st', '2nd', '3rd', '4th'])   

df['장학생여부'] = df['학점'] > 4.0
display(df)
```

|      |     학과 | 이름 | 학년 | 학점 | 장학생여부 |
| ---: | -------: | ---: | ---: | ---: | ---------: |
|  1st |     수학 |   홍 |    1 |  3.3 |      False |
|  2nd |     물리 |   신 |    4 |  4.1 |       True |
|  3rd | 생명공학 |   김 |    3 |  3.8 |      False |
|  4th | 중어중문 |   이 |    2 |  3.6 |      False |

* column 삭제

```python
#원본은 보존하고 삭제 처리된 복사본 생성
new_df = df.drop('학년', axis = 1, inplace = False)    #False가 default값
display(df)
display(new_df)

#원본에서 삭제하기
df.drop('학년', axis = 1, inplace = True)
display(df)
display(df.to_numpy())
```

* 경고 문구 차단

```python
import warnings 

warnings.filterwarnings(action = 'ignore')      	      #경고 문구 차단
warnings.filterwarnings(action = 'default')                   #경고 켜기
```

### 5) row CRUD

```python
import numpy as np
import pandas as pd

data = {'이름': ['박동수', '이고은', '강수지', '김진우'], 
        '학과':['수학', '물리', '생명공학', '중어중문'], 
        '학년':[1, 4, 3, 2], 
        '학점': [3.3, 4.1, 3.8, 3.6]}
df = pd.DataFrame(data, 
                  columns = ['학과', '이름', '학년', '학점'], 
                  index = ['1st', '2nd', '3rd', '4th'])  
display(df)
```

|      |     학과 |   이름 | 학년 | 학점 |
| ---: | -------: | -----: | ---: | ---: |
|  1st |     수학 | 박동수 |    1 |  3.3 |
|  2nd |     물리 | 이고은 |    4 |  4.1 |
|  3rd | 생명공학 | 강수지 |    3 |  3.8 |
|  4th | 중어중문 | 김진우 |    2 |  3.6 |

* row 인덱싱

내부 숫자 인덱스 사용

```python
#iloc[]사용 X
print(df[1])        			#단일 행 인덱싱 X, ERROR!
display(df[1:3])     			#slicing 가능 -> View
display(df[1:2])     			#단일 행 인덱싱은 안되지만 slicing 사용하면 단일 행 출력 가능
display(df[[1, 3]])  			#Fancy Indexing 불가, ERROR!

#iloc[]사용 
display(df.iloc[1])      		#단일 행 인덱싱 가능, Series 리턴
display(df.iloc[1:4])    		#Slicing -> View
display(df.iloc[[1, 3]])  		#Fancy indexing 가능
```

인덱스 명 사용

```python
#loc[]사용 X
print(df['1st'])                                         #단일 행 인덱싱 X, ERROR!
display(df['2nd':'4th'])   
display(df['2nd':])
display(df[['1st', '3rd']])                              #Fancy indexing X, ERROR!

#loc[]사용하여 행 추출
display(df.loc['2nd'])                                   #단일 행 인덱싱 가능, Series 리턴
display(df.loc['2nd':'3rd'])                             #DataFrame 형태로 리턴
display(df.loc[['1st', '4th']])                          #Fancy indexing 가능

#loc[]사용하여 셀 추출
display(df.loc['1st':'3rd']['이름'])                      #Series 리턴
display(df.loc['1st':'3rd', '이름'])                      #Series 리턴
display(['이름':'학년'])                                   #ERROR!
display(df.loc['1st':'3rd', '이름':'학년'])                #DataFrame 리턴
display(df.loc['1st':'3rd', ['이름', '학점']])             #Fancy Indexing 가능

#Boolean indexing
display(df.loc[df['학점'] > 4])                            #Boolean mask
#1)학점이 4.0을 초과하는 학생의 이름, 학점을 DataFrame으로 출력하기
display(df.loc[df['학점'] > 4, ['이름', '학점']])  
#2)학점이 1.5이상 2.5이하인 학생의 학과, 이름, 학점을 DataFrame으로 출력하기
display(df.loc[(1.5 < df['학점']) & (df['학점'] < 2.5), ['학과', '이름', '학점']])
#3)학점이 4.0이상인 학생의 등급 A로 입력
df.loc[df['학점'] > 4.0, '등급'] = 'A'
```

*loc[] 사용하면 숫자 인덱스 사용 불가*
and 연산자는 True/False 연산에서 사용, 벡터 연산에서는 & 연산자 사용
boolean indexing에서 조건이 여러 개일 때 괄호 필수!

* row 추가

```python
df.loc['5th', :]=['독어독문', '김명수', 3, 3.7]
df.loc['6th', '이름':'학점'] = ['김준수', 3, 3.7]
```

* row 삭제

```python
df.drop('3rd', axis = 0, inplace = True)           #axis = 0, inplace = False가 기본값
display(df)

df.drop(['1st', '4th'], axis = 0, inplace = True)  #Fancy indexing, df.drop은 slicing 불가
display(df)
```

### 6) 함수

* 기댓값, 편차, 분산

```python
import numpy as np
arr = np.array([4, 6, 1, 3, 8, 8], dtype = np.int32)

print(arr)
print(arr.mean())			#기댓값
print(arr.var())			#편차: 확률변수 X와 평균값차이, 데이터의 흩어짐 정도 나타냄
print(arr.std())			#분산
```

* 공분산

두 확률변수 편차의 곱에 대한 평균
확률변수 두개의 관계 보여줌

```python
import numpy as np
import pandas as pd
import pandas_datareader.data as pdr
from datetime import datetime

start = datetime(2019, 1, 1)   			          #2019-01-01:날짜객체생성
end = datetime(2019, 12, 31)

#yahoo에서 제공하는 kospi지수
df_KOSPI = pdr.DataReader('^KS11', 'yahoo', start, end)   #^KS11: 코스피 지수 코드
display(df_KOSPI['Close'])
df_SE = pdr.DataReader('005930.KS','yahoo', start, end)   #3006930.KS: 삼성전자 코스피 지수
display(df_SE['Close'])

np.cov(df_KOSPI['Close'].values, df_SE['Close'].values)   #공분산
```

```html
Date
2019-01-02    2010.000000
2019-01-03    1993.699951
                 ...     
2019-12-30    2197.669922
Name: Close, Length: 245, dtype: float64

Date
2019-01-02    38750.0
2019-01-03    37600.0
               ...   
2019-12-23    55500.0
Name: Close, Length: 245, dtype: float64

array([[6.28958682e+03, 9.46863621e+04],
       [9.46863621e+04, 1.41592089e+07]])
```

0행 0열: KOSPI와 KOSPI의 공분산
0행 1열: KOSPI와 삼성전자의 공분산
1행 0열: 삼성전자와 KOSPI의 공분산(0행 1열의 값과 같다)
1행 1열: 삼성전자와 삼성전자의 공분산
0행 1열 > 0 : 삼성전자 ↑ → KOSPI↑

*공분산은 두 확률 변수 간 양의 관계, 음의 관계 표현
각 행렬의 절대값이 관계의 강도를 의미 X! 공분산을 통해 관계의 강도를 알 수는 없다*

* 상관계수

상관관계(correlation): 두 대상이 서로 연관성 있다고 추측되는 관계
상관계수(correlation coefficient): -1과 1사이 실수, 하나의 변수가 변할 때 다른 변수가 변화하는 정도
상관계수 > 0: 양의 상관관계, 상관계수 < 0: 음의 상관관계
상관계수 절대값이 0에 가까울수록 관련성 작음, 절대값이 1에 가까울수록 관련성 크다

```python
import numpy as np
import pandas as pd
import pandas_datareader.data as pdr
from datetime import datetime

start = datetime(2018, 1, 1)   				   #2018-01-01:날짜객체생성
end = datetime(2018, 12, 31)

#yahoo에서 제공하는 종목 지수
df_KOSPI = pdr.DataReader('^KS11', 'yahoo', start, end)    #^KS11: 코스피 지수 코드
df_SE = pdr.DataReader('005930.KS','yahoo', start, end)    #3006930.KS: 삼성전자 코스피 지수
df_PUSAN = pdr.DataReader('011390.KS', 'yahoo', start, end)#남북경제협력주
df_LIG=pdr.DataReader('079550.KS', 'yahoo', start, end)    #LIG넥스원(방위)

my_dict = {
    'KOSPI': df_KOSPI['Close'],
    '삼성전자': df_SE['Close'],
    '납북경협주': df_PUSAN['Close'],
    'LIG넥스원(방위)': df_LIG['Close']
}
df=pd.DataFrame(my_dict)
display(df)

display(df.corr()) 					   #상관계수
```

|      KOSPI |    삼성전자 | 납북경협주 | LIG넥스원(방위) |
| ---------: | ----------: | ---------: | --------------: |
|       Date |             |            |                 |
| 2018-01-03 | 2486.350098 |    51620.0 |         59200.0 |
| 2018-01-04 |  2466.45996 |    51080.0 |         57800.0 |
|        ... |         ... |        ... |             ... |
| 2018-12-21 | 2061.489990 |    38650.0 |         33550.0 |

242 rows × 4 columns

|                 |     KOSPI |  삼성전자 | 납북경협주 | LIG넥스원(방위) |
| --------------: | --------: | --------: | ---------: | --------------: |
|           KOSPI |  1.000000 |  0.913574 |  -0.574980 |        0.791497 |
|        삼성전자 |  0.913574 |  1.000000 |  -0.464685 |        0.649536 |
|      납북경협주 | -0.574980 | -0.464685 |   1.000000 |       -0.708109 |
| LIG넥스원(방위) |  0.791497 |  0.649536 |  -0.708109 |        1.000000 |

* 덧셈

```python
import numpy as np
import pandas as pd

data = [[2, np.nan],
     	[7, -3],
     	[np.nan, np.nan],
     	[1, -2]]
df = pd.DataFrame(data, 
              	  columns = ['one', 'two'],
                  index = ['a', 'b', 'c', 'd'])
display(df)
display(df.sum())    			#axis = 0이 기본, DataFrame은 2차원이니까!
                     			#skipna = True(기본값) : NaN이 있으면 스킵하고 나머지 값 더하기
                     			#Series 리턴
display(df.sum(axis = 1))    
print(df['two'].sum())  		#특정 column만 덧셈
print(df.loc['b'].sum())
```

* NaN 처리하기

1) NaN이 포함된 행 지우기(전체 데이터가 몇 백, 몇 천만 건, NaN은 전체 데이터의 3~5%내일 때)
2) NaN을 제외한 값의 평균 대입
3) 해당 열 중 최댓값, 또는 최솟값 대입

```python
#'one'열에 있는 NaN을 'one'열 평균값으로 채우기
df['one'].fillna(value = df['one'].mean(), inplace = True)  #inplace = False가 기본값!
#'one'열의 평균을 모든 NaN 칸에 입력
df.fillna(value = df['one'].mean(), inplace = True)    
#NaN이 하나라도 존재하면 행 삭제
df.dropna(how = 'any', inplace = True)                        
#행 전체가 NaN인 경우 행 삭제
df.dropna(how = 'all', inplace = True)  
#모든 NaN 자리에 0 입력
df.fillna(value=0, inplace = True)
#Boolean mask로 NaN 확인
display(df.isnull())
#'two' 열에서 NaN인 행들을 찾아 해당 행의 모든 열 출력
display(df.loc[df.isnull()['two'], :])
```

```python
import numpy as np
import pandas as pd
import pandas_datareader.data as pdr
from datetime import datetime

data = [[2, np.nan],
     	[7, -3],
     	[np.nan, np.nan],
     	[1, -2]]
df = pd.DataFrame(data)
display(df)
print(df.mean(skipna = True))   
df.fillna(0, inplace = True)
print(df.mean())
```

skipna = True: NaN은 연산에서 완전히 배제, NaN = 0으로 처리하는거랑 다름!

|      |    0 |    1 |
| ---: | ---: | ---: |
|    0 |  2.0 |  NaN |
|    1 |  7.0 | -3.0 |
|    2 |  NaN |  NaN |
|    3 |  1.0 | -2.0 |

```html
0    3.333333
1   -2.500000
dtype: float64

0    2.50
1   -1.25
dtype: float64
```

* 순서섞기

```python
import numpy as np
import pandas as pd

np.random.seed(1)
df = pd.DataFrame(np.random.randint(0, 10, (6, 4)))
df.columns = ['a', 'b', 'c', 'd']
df.index = pd.date_range('20200101', periods=6)
display(df)

#인덱스 순서 섞지만 원본 변경 X
new_index = np.random.permutation(df.index)  		#np.random.shuffle(df.index): ERROR!

#인덱스와 컬럼을 원하는 순서 변경, 원본 변경X
df1 = df.reindex(index = new_index, columns = ['b', 'a', 'd', 'c']) 
display(df1)
```

* 정렬

axis 사용하여 정렬

```python
#인덱스 기준으로 정렬
df2 = df1.sort_index(axis = 1, ascending = False)  
display(df2)
#DataFrame은 인덱스 기준 정렬되면 데이터도 함께 이동, numpy는 데이터 이동X

#값을 기준으로 정렬
df3 = df1.sort_values(by = 'b') 	#b열 기준 오름차순으로 전체 정렬
display(df3)

df4 = df1.sort_values(by = ['b', 'a'])  #b열 기준으로 정렬, 같은 값이 있으면 a열 기준으로 정렬
display(df4)
```

* 중복 요소 제거

특정 열의 중복 요소 제거

```python
import numpy as np
import pandas as pd

np.random.seed(1)
df = pd.DataFrame(np.random.randint(0, 9, (6, 4)))
df.columns = ['가', '나', '다', '라']
df.index = pd.date_range('20200101', periods = 6)
df['마'] = ['AA', 'BB', 'CC', 'DD', 'BB', 'CC']

print(df['마'].unique())                  #중복요소 제거, ndarray 리턴

display(df.drop_duplicates(['마']))       #중복요소 제거, DataFrame 리턴
display(df.drop_duplicates(['마','다']))  #중복요소 제거, DataFrame 리턴
```

중복 행 제거

```python
import numpy as np
import pandas as pd

my_dict = {
    'k1' : ['one'] * 3 + ['two'] * 4,
    'k2' : [1, 1, 2, 3, 3, 4, 4]
}
df = pd.DataFrame(my_dict)

#Boolean mask로 중복 행 확인
print(df.duplicated())
#중복 행 출력
display(df.loc[df.duplicated(), :])  
# 중복행을 제거
df.drop_duplicates(inplce = True)  
```

* 각 value 값의 갯수 구하기

```python
print(df['마'].value_counts())           #Series 리턴
```

* 포함 여부 확인

```python
df['마'].isin(['AA', 'BB'])    		#Boolean mask
```

* 데이터 프레임 결합

기준이 되는 column명이 같을 때 결합하기

```python
import numpy as np
import pandas as pd

data1 = {
    '학번': [1, 2, 3, 4], 
    '이름': ['박동수', '이고은', '강수지', '김진우'],
    '학년': [1, 4, 3, 2]
}
data2 = {
    '학번': [1, 2, 4, 5], 
    '학과': ['수학', '물리', '생명공학', '중어중문'],
    '학점': [3.3, 4.1, 3.8, 3.6]
}
df1 = pd.DataFrame(data1)
df2 = pd.DataFrame(data2)          

#'학번' column 기준으로 데이터프레임 결합
display(pd.merge(df1, df2, on = '학번', how = 'inner'))       #on:column 에 대해서 쓴다
#inner: 값이 있는 셀만 결합하기(NaN은 결합에서 제외) -> default
display(pd.merge(df1, df2, on = '학번', how = 'outer'))
#outer: 전체 셀 결합하기(NaN 있는 셀도 포함)
display(pd.merge(df1, df2, on = '학번', how = 'left'))  
#왼쪽을 기준으로 붙이기
display(pd.merge(df1, df2, on = '학번', how = 'right')) 
#오른쪽을 기준으로 붙이기
```

기준이 되는 column명이 다를 때 결합하기

```python
import numpy as np
import pandas as pd

data1 = {
    '학번': [1, 2, 3, 4], 
    '이름': ['박동수', '이고은', '강수지', '김진우'],
    '학년': [1, 4, 3, 2]
}
data2 = {
    '학생학번': [1, 2, 4, 5], 
    '학과': ['수학', '물리', '생명공학', '중어중문'],
    '학점': [3.3, 4.1, 3.8, 3.6]
}
df1 = pd.DataFrame(data1)
df2 = pd.DataFrame(data2)
 
display(pd.merge(df1, df2, left_on = '학번', right_on = '학생학번', how = 'inner'))
```

index 기준으로 결합하기

```python
import numpy as np
import pandas as pd

data1 = {
    '이름': ['박동수', '이고은', '강수지', '김진우'],
    '학년': [1, 4, 3, 2]
}
data2 = { 
    '학과': ['수학', '물리', '생명공학', '중어중문'],
    '학점': [3.3, 4.1, 3.8, 3.6]
df1 = pd.DataFrame(data1, index = [1, 2, 3, 4])
df2 = pd.DataFrame(data2, index = [1, 2, 4, 5])
display(pd.merge(df1, df2, left_index = True, right_index = True, how = 'inner'))
```

서로 다른 기준(column, index)으로 결합하기

```python
import numpy as np
import pandas as pd

data1 = {
    '학번': [1, 2, 3, 4], 
    '이름': ['박동수', '이고은', '강수지', '김진우'],
    '학년': [1, 4, 3, 2]
}
data2 = { 
    '학과': ['수학', '물리', '생명공학', '중어중문'],
    '학점': [3.3, 4.1, 3.8, 3.6]
df1 = pd.DataFrame(data1)
df2 = pd.DataFrame(data2, index = [1, 2, 4, 5])   #학번이 index로 사용
 
display(pd.merge(df1, df2, left_on = '학번', right_index = True, how = 'inner'))
```

* 데이터 프레임 연결

```python
import numpy as np
import pandas as pd

df1 = pd.DataFrame(np.arange(6).reshape(3, 2), 
                   index = ['a', 'b', 'd'], 
                   columns = ['one', 'two'])
display(df1)
df2 = pd.DataFrame(np.arange(4).reshape(2,2), 
                   index=['a', 'c'], 
                   columns = ['three', 'four'])
display(df2)


result = pd.concat([df1, df2], axis = 1)
display(result)
result = pd.concat([df1, df2], axis = 1, sort = True)
display(result)
result = pd.concat([df1, df2], axis = 0)
display(result)
result = pd.concat([df1, df2], axis = 0, ignore_index = True)   #결합할 때 이전 인덱스 지우기
display(result)
```

* 데이터 변경

```python
np.random.seed(1)

df = pd.DataFrame(np.random.randint(0,10,(6,4)))
df.index = pd.date_range('20200101', periods=6)
df.columns = ['A', 'B', 'C', 'D']
display(df)

#5를 -100으로 변경
df.replace(5,-100)
```

* 그룹 별로 나누기

```python
import numpy as np
import pandas as pd

my_dict = {
    '학과' : ['컴퓨터','경영학과','컴퓨터','경영학과','컴퓨터'],
    '학년' : [1, 2, 3, 2, 3],
    '이름' : ['박동수', '이고은', '강수지', '김진우', '고우영'],
    '학점' : [3.3, 4.1, 3.8, 3.6, 4.5]
}
df = pd.DataFrame(my_dict)
```

single index 분류

```python
# '학점' column을 '학과'를 기준으로 분류
score = df['학점'].groupby(df['학과'])       #주소 출력
# 그룹안에 데이터를 확인하고 싶은 경우에는 get_group()
score.get_group('경영학과')
# 학과별 인원수
score.size()                                #Series 리턴
df.groupby(df['학과'])['이름'].count()
#학과별 평균학점
df['학점'].groupby(df['학과']).mean()
df.groupby(df['학과'])['학점'].mean()
#학과별로 데이터프레임 출력하기
for dept, group in df.groupby(df['학과']):
    print(dept)
    display(group)
```

multi index 분류

```python
#학과 및 학년 별 평균학점
score = df['학점'].groupby([df['학과'], df['학년']])
print(score.mean())                    		#Series 리턴
display(score.mean().unstack())  		#최하위 index(여기서는 '학년')를 column으로 변경
```

