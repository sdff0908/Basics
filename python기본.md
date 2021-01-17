# python 기본

## 1. keyword

```python
import keyword
print(keyword.kwlist)
```

    ['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']

## 2. 숫자

```python
a = 3.14E10          
b = 1 + 3j        		 #complex
c = 3 + 3.14     		 #정수 + 실수 = 실수(범위가 더 넓은 수로 계산)
```

## 3. sequence

### 1)list []

* 역순으로 출력

```python
a = [1, 2, 3, 4, 5]
print(a[::-1])       #a.reverse() = a[::-1]
```


    [5, 4, 3, 2, 1]

* slicing: a[0:1]과 a[0]은 다르다


```python
a = [1, 2, 3, 4, 5]
a[0:1] = [7, 8, 9]      
print(a)
```

```python
[7, 8, 9, 2, 3, 4, 5]
```


```python
a = [1, 2, 3, 4, 5]
a[0] = [7, 8, 9]
print(a)
```

```python
[[7, 8, 9], 2, 3, 4, 5]
```

* 오름차순 정렬: sort() vs sorted()

```python
a = [3, 7, 32, 6, 8, 4, 82]
a.sort()     		 #a에 자동 대입
a = sorted(a)  		 #'a='이 필요
```

    [3, 4, 6, 7, 8, 32, 82]

### 2) tuple ()

```python
a = (2, 3, 4)
b = 4, 5, 6      		 #tuple은 소괄호 생략 가능
```

### 3) range

```python
a = list(range(1, 11, 2))
print(a)
3 in a
```

```python
[1, 3, 5, 7, 9]
True
```

### 4) string

* +, x 연산


```python
a = 'this is a'
b = ' same'
c = ' text'
print(a + b + c)
print('python' *3)
print('count: '+str(100))  #'+'주의!
print('count:', 100)
```

    this is a same text
    pythonpythonpython
    count: 100
    count: 100
    False
    True

* Boolean

```python
d = 'this is a sample text'
print('Sam' in d)          #대소문자 구분
print('Sam' not in d)
```

* format 함수

```python
apple_n = 4
orange_n = 2
a = 'I have {} apples.'.format(apple_n)
b = 'I have {} apples and {} oranges'.format(apple_n, orange_n)
print(a)
print(b)
```

```python
I have 4 apples.
I have 4 apples and 2 oranges
```

기타 함수

```python
a = 'silverlining'
print(len(a))
print(a.count('in'))
print(a.upper())
```

```python
12
2
SILVERLINING
```

### 5) dictionary {key: value}

key는 수정X, value 수정만 가능

```python
a = {'name': '홍길동', 'age': 30, 'address': '부산'}
a['address'] = '서울'        				
print(a)

del a['age']                             
print(a)
```

```python
{'name': '홍길동', 'age': 30, 'address': '서울'}
{'name': '홍길동', 'address': '서울'}
```

예시처럼 key 중복은 불가

```python
a = {'name': '홍길동', 'age': 20, 'age': 30}  
```

key자리에 tuple 가능, list는 불가능(변할 수 있는 자료형은 key값으로 사용 X)

```python
a = {'name': '홍길동', 'age': 30}
print(a.keys())
print(a.values())

for key in a.keys():
    print('key: {}, value: {}'.format(key, a[key]))
print('name' in a)    				#dictionary에서 name이라는 key가 있는지?
a.clear()             				#dictionary 전체 내용 삭제
print(a)
```

```python
dict_keys(['name', 'age'])
dict_values(['홍길동', 30])
key: name, value: 홍길동
key: age, value: 30
True
{}
```

### 6) set

+ 중복 요소 없음

+ 중괄호 {} 사용
+ 순서 없음(인덱싱 X)

```python
a = {1, 2, 3, 4, 5, 2, 5}
b = {2, 3, 6, 7}
print(a)

result = a & b #교집합
print(result)

result = a | b #합집합
print(result)

result = a - b #차집합
print(result)

a = {}         #<-dictionary!!!
a = set()      #공집합
```

```python
{1, 2, 3, 4, 5}
{2, 3}
{1, 2, 3, 4, 5, 6, 7}
{1, 4, 5}
```

요소 추가, 삭제

```python
a.add(2)                   #요소 하나 추가할 때
a.update([3, 5, 6, 7, 1])  #요소 여러개 추가할 때
print(a)

a.remove(5)  
print(a)
```

```python
{1, 2, 3, 5, 6, 7}
{1, 2, 3, 6, 7}
```

어떤 자료형이든 del은 되도록 쓰지 말 것

## 4. bool type(True/False)

```python
print(True and False)
print(True or False)
```

```python
False
True
```

python 에서 다음은 False로 간주

* 빈 문자열: ''
* 빈 리스트: []
* 빈 튜플: ()
* 빈 딕셔너리: {}
*  숫자 0  (나머지 숫자는 True)
*  None

## 5. 반복문

### 1) for

반복횟수를 알고 있을 때

for ___ in (range/ list/ tuple/ dict/ set):


```python
for tmp in [1, 2, 3, 4]:
    print(tmp)            #출력 시 매번 줄바꿈

for tmp in [1, 2, 3, 4]:
    print(tmp, end=' ')    #end속성 이용

print()                   #한 줄 띄우기

for tmp in [1, 2, 3, 4]:    
    print(tmp, end='_')
```

```python
1
2
3
4
1 2 3 4 
1_2_3_4_
```

### 2) while

조건에 따라서 반복할 때 

```python
idx = 0

while idx < 10:
    print(idx)
    idx += 1
```

```python
0
1
2
3
4
5
6
7
8
9
```

### 3) enumerate

반복문 사용시 인덱스 값 추출하기 위해 사용

```python
a_list=['가','나','다','라']
for idx,temp in enumerate(a_list):
    print('인덱스: {}, 값: {}'.format(idx, temp))
```

```python
인덱스: 0, 값: 가
인덱스: 1, 값: 나
인덱스: 2, 값: 다
인덱스: 3, 값: 라
```

### 4)피보나치 수열(반복문, 재귀형)

* 반복문

```python
a, b=0, 1
for i in range(3):
    a, b = b, a + b
    print('a=', a)
    print('b=', b)
```

```python
a= 1
b= 1
a= 1
b= 2
a= 2
b= 3
```

* 재귀형

```python
def solution(x):
    if x <= 1:
        return x
    else:
        return solution(x - 2) + solution(x - 1)

solution(4)
```


```python
3
```

## 6. list comprehension


```python
a = [1, 2, 3, 4, 5, 6, 7]
list1 = [tmp * 2 for tmp in a]
list2 = [tmp * 2 for tmp in a if tmp % 2 == 0]
print(list1)
print(list2)
```

```python
[2, 4, 6, 8, 10, 12, 14]
[4, 8, 12]
```

## 7. 파일 불러오기

파일 안에 있는 모든 내용(모든 줄) 출력하기

```python
my_file = open('mpg.txt', 'r')  #'r': 읽기 용

while 1:        				#파일이 몇 줄인지 모르니까 while사용
    line = my_file.readline()
    print(line)
    if not line:
        break   				#가장 인접한 loop 탈출

my_file.close() 				#사용한 resource는 반드시 해제처리 해야!
```

```python
with open('mpg.txt', 'r') as my_file:
    while 1:
        line = my_file.readline()
    	print(line)
    	if not line:
        	break
```

with 구문은 file close자동으로 처리

## 8. 함수

> 특정 기능을 수행하는 코드의 묶음

### 1) 함수 인자 개수가 가변적일 때 

`*`사용, 인자 받을 때는 `tuple` 사용

```python
def solution(*tmp):    
    print(tmp)
    result = 0
    for t in tmp:
        result += t
    return result
    
print(solution(1, 2, 4))
```

### 2) 여러개 값을 리턴하는 함수

```python
def multi_return(a, b):
    result1 = a + b
    result2 = a * b 
    return result1,result2     	#tuple은 ()생략 가능

data1,data2 = multi_return(10, 20)   
```

### 3) call-by-value & call-by-reference

* call-by-value (immutable)

  값 그대로 복사, 넘겨준 인자값 변경X 

  숫자, 문자열, tuple 등

* call-by-reference (mutable)

  주소 가져옴, 넘겨준 인자값 변경

  list, dict 등

```python
def my_func(tmp_value, tmp_list):
    tmp_value = tmp_value + 100
    tmp_list.append(100)
    
data_x = 10             							#call-by-value 
data_list = [10, 20]								#call-by-reference 
my_func(data_x, data_list)

print('data_x : {}'.format(data_x))                  
print('data_list : {}'.format(data_list))
```

```python
data_x : 10
data_list : [10, 20, 100]
```

### 4) default parameter

함수이름() ← 괄호 안에 값이 입력되지 않으면 디폴트값 자동 입력

```python
def default_param(a, b, c = False):
    if c:
        result = a + b
    else:
        result = a * b
    return result

print(default_param(10, 20, True)) 
#default_param(10, 20) = default_param(10, 20, c = False)
```

```python
30
```

### 5) local variable & global variable

  *global variable사용은 지양해야*

```python
tmp = 100        				#local variable

def my_func(x):
    tmp = tmp + x
    return tmp

print(my_func(20)) 				#----------->ERROR! 함수 내부에서 tmp 값 주어지지 않음 
```

```python
tmp = 100

def my_func(x):
    global tmp      			#global variable(외부 변수를 함수 내부로 가져오기)
    tmp = tmp + x
    return tmp

print(my_func(20))
```

### 6) lambda

함수 이름 없음, 함수는 아니지만 함수처럼 행동 → 람다식
함수처럼 별도의 로컬 스코프 갖지 않는다

```python
f = lambda a, b, c : a + b + c
```

함수로 표현하면,

```python
def my_sum(a, b, c):
    return a + b + c
```

### 7) 예외 처리

```python
def func(list_data):
    my_sum = 0
    try:
        my_sum = list_data[0] + list_data[1] + list_data[2]
    except Exception as err:		#오류 발생 시 실행
        print('오류 발생')			
        my_sum = 0
    else:           				#오류 없을 때 실행
        print('오류 없음')
    finally:        				#오류 여부에 관계없이 무조건 실행
        print('무조건 실행')

    return my_sum

my_list = [2, 3, 4]
print(func(my_list))

my_list = [2, 3]
print(func(my_list))
```

```python
오류 없음
무조건 실행
9
```

```python
오류 발생
무조건 실행
0
```

## 9. 절차 지향, 객체 지향

* 절차 지향 프로그램 : 프로그램을 '기능'으로 세분화
  * 장점: 프로그램 설계 및 구현 간단
         	 누가 설계를 하든 거의 동일한 설계 나옴
  * 단점: 프로그램 규모 커지면 유지 보수 어려움 (개발 비용 보다 유지보수 비용이 더 커짐)
              기존 코드 재사용 어려움
  * 사용: 학계 프로그램, 유지보수 필요 없는 프로그램
  * 예) C언어

인터넷 보급 → 유지보수 요구사항 급증  → 객체 지향 프로그래밍 등장

* 객체 지향 프로그램: 프로그램을 '기능'으로 세분화X 

  ​								   해결해야 하는 문제를 그대로 프로그램으로 묘사

  ​                                   프로그램을 구성하는 주체 파악, 주체들이 상호작용하는 방식을 프로그램적으로 묘사

  ​									누가 설계 하는지에 따라 설계 형태도 달라짐
  * 장점: 프로그램 유지 보수성, 재사용성이 좋음
  * 단점: 설계 및 구현 상대적으로 어려움
  * 사용: 서비스류 프로그램(유지 보수 잦기 때문)
  * 예) java, C++

## 10. class

* class: 현실 세계의 객체를 프로그램적으로 모델링하는 (프로그래밍) 수단
             method( 함수)와 property(변수)의 집합
             설명서와 같은 개념. 데이터 담고 있지 않음
* instance(객체): class 기반으로 프로그램에서 사용할 수 있는 메모리 공간
                 			데이터를 실제로 담음

객체 지향 프로그램은 현실세계의 개체를 프로그램적으로 모델링

예)자동차

자동차가 가지는 속성: 도어수, 배기량, 현재 속도, 차 가격, 차 색상 등
	⇒ 표현할 속성 선택: 현재 속도, 배기량 → 변수 → `property`

자동차가 가지는 행위: 전진, 후진, 깜빡이 켜기, 와이퍼 작동 등
	⇒ 표현할 행위 선택: 전진, 후진 → 함수 → `method`

언어마다 함수와 변수를 가리키는 단어 다름

|        |      변수       |      함수      |
| :----: | :-------------: | :------------: |
|  Java  |      field      |     method     |
|  C++   | memver variable | memer function |
| Python |    property     |     method     |

<u>함수와  method 차이?</u>   

*함수는  객체. 특정 기능을 수행하는 코드의 묶음. python에는 함수와  method 모두 존재* 

***∴ 파이썬은 완벽한 객체 지향 프로그램 X***

### 1) initializer(생성자)

* def __ init __ ( self, 변수1, 변수2, ... )

첫번째 인자는 반드시 self: 각 인스턴스의 메모리 주소 가지고 있음
속성을 __ init __안에 명시

```python
class Student(object):      	#class Student() 로 써도 작동(괄호 안에 자동으로 object 입력)
    							#python의 모든 class는 object class(최상위 class)를 상속
    def __init__(self, name, dept, num, grade): 
        self.name = name      	#self.name = property, name = 변수
        self.dept = dept
        self.num = num
        self.grade = grade

class MyClass(Student):
    pass 

class MySecondClass(Myclass):
    pass

```

### 2) instance 

* instance 만들기

class 이름 그냥 쓰면 됨
Student( )라는 객체는 한 사람에 대한 데이터를 담음, 여기서 stu1은 변수

```python
stu1 = Student('홍홍홍', '철학', '20200111', '4.1')    
stu2 = Student('신신신', '통계', '20200115', '3.6')
stu3 = Student('차차차', '컴퓨터', '20200118', '3.4')
```

또는, 

```python
students = []  			#list도 객체!
students.append(Student('홍홍홍', '철학', '20200111', '4.1'))
students.append(Student('신신신', '통계', '20200115', '3.6'))
students.append(Student('차차차', '컴공', '20200118', '3.4'))
```

* instance 사용하기

객체가 가지는 property나 method를 사용할 때는 연산자 `.`(dot operator)  사용
객체는 property나 method를 가진다

```python
print(students[0])
print(students[0].dept)       
```

여기서 dept는 property
 dept가 method라면 dept( )

```python
<__main__.Student object at 0x000002B410B8A188>
철학    
```

### 3) class variable, instance variable

* class variable: class가 갖고 있는 변수
      					  						 class내 모든 instance가 공유
                        					      instance가 아닌 class에 저장되는 데이터
* instance variable: 각각의 instance가 갖고 있는 변수     

```python
class Student(object)
    
    scholarship_rate = 3.5                			  #class variable
 
    def __init__(self, name, dept, num, grade):
        self.name = name                  			  #self.name: instance variable
        self.dept = dept                  			  #self.dept: instance variable
        self.num = num					  			  #self.num: instance variable
        self.grade = grade				  			  #self.grade: instance variable
    
    def get_stu_info(self):               
        return '이름:{}, 학과:{}'.format(self.name, self.dept)  
    #format.(name, dept) 쓰면 ERROR!!! 변수가 아닌 property를 써야
    
    def is_scholarship(self):
        if self.grade > Student.scholarship_rate:     #class variable 사용방법
            return True
        else:
            return False
```

```python
stu1 = Student('김김김', '경영학과', '20201453', '3.8')
stu2 = Student('차차차', '컴공', '20175252', '3.6')
#stu1객체는 4개의 property와 2개의 method 가지고 있음

print(stu1.get_stu_info())
stu1.name = '이이이'             	 					 #객체의 name 속성에 담긴 데이터 변경 
print(stu1.name)
stu1.age = 29
print(stu1.age)
print(stu1.get_stu_info())    						   #get_stu_info(): method
print(stu1.get_stu_info)      						   #method주소 출력
print(stu1.scholarship_rate)
```

```python
이름: 김김김, 학과:경영학과
이이이
29
이름: 이이이, 학과:경영학과
<bound method Student.get_stu_info of <__main__.Student object at 0x000002B40FEF0A88>>
3.5
```

### 4) method

* instance method:  instance variable 제어(생성, 변경, 참조)

  ​								 첫 인자는 반드시 `self`

```python
class Student(object):
    
    scholarship_rate = 3.5   							   
    
    def __init__(self, name, dept, grade):
        self.name = name           
        self.dept = dept           
        self.grade = grade
    
    def get_stu_info(self):         				#instance method        
        return '이름: {}, 학과: {}'.format(self.name, self.dept)  
    
    def is_scholarship(self):						#instance method  
        if self.grade > Student.scholarship_rate:
            return True
        else:
            return False
```

* class method: class variable 제어(생성, 변경, 참조)

```python
   #class method 만들려면 특수한 데코레이터(@) 이용해야
    @classmethod       
    def change_scholarship_rate(cls, rate):   		#class method의 첫번째 인자는 cls!
        cls.scholarship_rate = rate            		#class variable(scholarship_rate) 변경 
```

* static method: self나 cls를 인자로 받지 않음, 일반적인 함수가 class 내부에 존재하는 경우 사용

  ​						  (일반적인 함수: class내부 속성과 관련없는 '함수(method 아님)')

```python
   #static method 만들려면 특수한 데코레이터(@) 이용해야
    @staticmethod
    def print_hello():
        print('Hello')
```

* private method: class벗어나서 접근 불가, class 내 다른 method에서 호출은 가능

  ​							method 명 앞에 '__'(언더바 두개)를 붙여준다

  ​					     	*private은 method에 한해서만 존재. 모든  instance variable은 public*

* public method: 어디에서나 사용 가능, 속성과 함수를 사용할 수 있음

  ​							기본적으로 파이썬의 모든 instance method는 public method

```python
    def __change_info(self, name, dept, grade): 	#private method  
        self.name = name
        self.dept = dept
        self.region = '서울'        		
    #생성자 method 아니어도 instance variable 설정 가능
```

```python
stu1 = Student('홍홍홍', '철학', 2.8)    
stu2 = Student('김김김', '경제', 3.9)

stu1.print_hello()
Student.change_scholarship_rate(4.0)    			#class variable 변경
stu1.change_info('강강강', '영문', 3.0)       			  #<-- ERROR
```

```python
Hello
AttributeError: 'Student' object has no attribute 'change_info'
```

*객체에 담긴 데이터를 변경할 때는 method를 통해서  하는게 좋다. 직접 변경 X*

### 5) namespace

객체들의 요소를 나누어서 관리하는 메모리 공간

property나 method 이용할 때  instance namespace → class namespace → super namespace 순으로 property와 method를 찾는다

```python
class Student(object)
    
    scholarship_rate = 3.5                			
 
    def __init__(self, name, dept, num, grade):
        self.name = name                  			  
        self.dept = dept                  			 
        self.num = num					  			  
        self.grade = grade		
        
    def is_scholarship(self):
        if self.grade > Student.scholarship_rate:     
            return True
        else:
            return False
```

```python
stu1=Student('홍홍홍','철학',2.8)   
stu2=Student('김김김','경제',3.9)

print(stu1.is_scholarship())
print(stu2.is_scholarship())

stu1.scholarship_rate = 4.3     #stu1에 지정된 instance 안에 scholarship_rate라는 새로운 property 생성 
print(stu1.scholarship_rate)    #stu1의 instance namespace 출력
print(stu2.scholarship_rate)    #class namespace 출력

Student.scholarship_rate = 4.1  #class variable 직접 변경 -> 좋은 방법 X
                                #class method 사용하여 class variable 변경해야!
print(stu1.scholarship_rate)  	#instance namespace부터 access하기 때문에 4.3이 나온다
print(stu2.scholarship_rate)
```

```python
False
True
4.3
3.5
4.3
4.1
```

### 6) magic function

사용자가 직접 호출 X , 특정 상황이 되면 자동적으로 호출됨

* __ init__ ( ):  instance초기화  

```python
class Student(object):
    def __init__(self, name, dept, grade):
        print('instance created')
        self.name = name
        self.dept = dept
        self.grade = grade
```

* __ str__ ( ): instance출력했을 때 instance가 갖고 있는 데이터를 문자열로 출력 가능
                    이 method 없으면 instance 주소 출력

```python
    def __str__(self):
        return 'name: {}, dept: {}'.format(self.name, self.dept)
```

* __ del__ ( ): instance가 메모리에서 삭제될 때 호출
  				  instance가 지워질 때 부가적인 처리가 필요한 경우 사용,  *__ del__ ( )이 삭제 기능하는 것 X* 

```python
    def __del__(self):
        print('instance deleted')
```

* __ gt__ ( ): `>` 연산자에 의해 호출되는 method 

     			  인자를 반드시 2개 받는다

```python
	def __gt__(self, other):  
        if self.grade > other.grade:
            return True
        else:
            return False
```

```python
stu1=Student('최최최', '영어영문', 3.6)
stu2=Student('박박박', '경제', 3.5)
#Run할 때마다 새로운 주소에 instance 담는다. 이전에 담은 instance 주소는 자동 삭제
#데이터가 삭제되는 경우: 1)명시적으로 지울 때 2)객체 사용할 수 없을 때 자동으로
#객체가 삭제될 때 해당 객체가 사용한 resource도 해제된다

print(stu1)     	  # __str__() 없으면 instance 메모리 주소 출력
print(stu1 > stu2)      #최최최의 성적은 박박박의 성적보다 높은가?
```

```python
instance created
instance deleted
instance created
instance deleted
name: 최최최, dept: 영어영문
True
```

### 7) inheritance

상위 클래스(superclass) 특징 이어받아 확장된 하위 클래스(subclass) 생성
class간 계층관계 성립
재사용성 확보, 코드 반복 줄임

*python에서 모든 class는 상속 관계에 있다*

```python
#상위 클래스
class Unit(object):
    def __init__(self, damage, life):
        self.utype=self.__class__.__name__   #__class__:현재 객체의 class자체를 지칭 
    										 #__class__.__name__: 현재 객체의 class이름 출력
        self.damage = damage
        self.life = life

#하위 클래스
class Marine(Unit):
    def __init__(self, damage, life, offense_upgrade):
        super(Marine, self).__init__(damage, life)     #Unit class 상속!     
        self.offense_upgrade=offense_upgrade
```

```python
my_unit = Unit(100, 200)
print(my_unit.damage)
print(my_unit.utype)
```

```python
100
Unit
```

```python
marine_1 = Marine(300, 400, 2)
print(marine_1.damage)
print(marine_1.utype)
print(marine_1.offense_upgrade)
```

```python
300
Marine
2
```

## 11. Module

>  함수, 변수, 클래스를 모아놓은 파일을 지칭

* 확장자가 .py로 끝나는 python 소스코드는 무조건 모듈
* 모듈을 사용하는 이유 (= 파일을 나누어서 작성하는 이유):코드 재사용성 높이고 관리 쉽게 하기 위해
* import: 모듈 불러오는 keyword
          	  파일을 객체화 시켜서 현재 사용하는 메모리에 가져온다

