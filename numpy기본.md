# Numpy 기본

> numpy = numerical python

numpy module은 vector, matrix연산에서 편리
numpy에서 사용하는 기본적인 자료구조: `ndarray(n-dimensional array)`

0차원 ndarray: scalar
1차원 ndarray: vector
2차원 ndarray: matrix
3차원 ndarray: array

## 1. list와 ndarray 

* 1차원


```python
import numpy as np

#python의 list
a = [1, 2, 3, 4]            
print(a)
print(type(a[0]))

#numpy의 ndarray
b = np.array([1, 2, 3, 4])
print(b)											  #list와 달리 ndarray에서는 ','없음
print(type(b[0]))      					#numpy.int32: 파이썬의 int와 다른 데이터 타입

print(type(b))
print(b.dtype)         					#ndarray는 모든 원소가 같은 데이터 타입 가진다
                                #list는 모든 원소가 같은 데이터 타입 가지지 않아도 상관X

c = np.array([1, 2, 3, 4, True, 'Hello'])    
print(c)
print(c.dtype)         
#ndarray에 같은 데이터 타입을 입력하지 않으면 numpy에서 자체적으로 같은 데이터 타입으로 변경
```

```python
[1, 2, 3, 4]
<class 'int'>
[1 2 3 4]
<class 'numpy.int32'>
<class 'numpy.ndarray'>
int32
['1' '2' '3' '4' 'True' 'Hello']
<U11
```

* 2차원

```python
#python의 list
a_list = [[1, 2, 3], [4, 5, 6]]
print(a_list)

#numpy의 ndarray
a_array = np.array([[1, 2, 3], [4, 5, 6]])       	  #list와 달리 ndarray에서는 ','없음
print(a_array)
a_array[1][1]
```

```python
[[1, 2, 3], [4, 5, 6]]					 
[[1 2 3]
 [4 5 6]]
 5
```

## 2. ndarray 형태

### 1) 형태 확인

* ndim 과 shape


```python
import numpy as np

#1차원
arr = np.array([1, 2, 3, 4])
print(arr.ndim)      					  	  		  #차원의 수
print(arr.shape)     					  	  		  #(행의 수, 열의 수) 튜플로 표현

#2차원
arr = np.array([[1, 2, 3], [4, 5, 6]])
print(arr.ndim)
print(arr.shape)

arr = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(arr.ndim)
print(arr.shape)

#3차원
arr = np.array([[[1, 2], [3, 4]], [[5, 6], [7, 8]]]   #(2행 2열) * 2개 -> 3차원
print(arr)
print(arr.ndim)
print(arr.shape)
```

```python
1
(4,)
2
(2, 3)
2
(3, 3)
[[[1 2]
  [3 4]]

 [[5 6]
  [7 8]]]
3
(2, 2, 2)
```

* len( ) 과 size( )

```python
a_arr = np.array([1, 2, 3, 4])

print('shape: {}'.format(a_arr.shape))       
print('크기(len): {}'.format(len(a_arr)))     
print('크기(size): {}'.format(a_arr.size))    

b_arr = np.array([[1, 2, 3], [4, 5, 6]])      
print('shape: {}'.format(b_arr.shape))    
print('크기(len): {}'.format(len(b_arr)))   			 #가장 바깥쪽 []안에 있는 요소의 갯수
print('크기(size): {}'.format(b_arr.size))  			 #요소의 갯수
```

```python
shape: (4,)
크기(len): 4
크기(size): 4
shape: (2, 3)
크기(len): 2
크기(size): 6
```

### 2) 형태 변경

* reshape( )

이미 만들어진 ndarray의 형태(shape) 변형, 새로운 ndarray  만들지 X

```python
arr1 = np.arange(12)
print(arr1)

arr2 = arr1.reshape(3, 4) 							  #arr2은 arr1과 같은 메모리 공간 사용(->메모리 절약)
print(arr2)											          #보여주는 방식(View)만 변경
print(arr1)

arr2[0, 2] = 200									        #View 데이터 변경하면 원본 데이터도 함께 변경
print(arr2)
print(arr1)

arr3 = arr1.reshape(2, -1)      					#2행을 맞춘 뒤 나머지는 알아서 채워라(-1)
print(arr3)

arr4 = arr1.reshape(-1, 3)      					#3열을 맞춘 뒤 나머지는 알아서 채워라
print(arr4)

arr5 = arr1.reshape(2, -1, 6)    					#(n행 6열) * 2개
print(arr5)
```

```python
[ 0  1  2  3  4  5  6  7  8  9 10 11]
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
[ 0  1  2  3  4  5  6  7  8  9 10 11]
[[  0   1 200   3]
 [  4   5   6   7]
 [  8   9  10  11]]
[  0   1 200   3   4   5   6   7   8   9  10  11]
[[  0   1 200   3   4   5]
 [  6   7   8   9  10  11]]
[[  0   1 200]
 [  3   4   5]
 [  6   7   8]
 [  9  10  11]]
[[[  0   1 200   3   4   5]]

 [[  6   7   8   9  10  11]]]
```

새로운 메모리 공간 사용하는 ndarray만들려면 copy( )사용

```python
arr1 = np.arange(12)

arr2 = arr1.reshape(3, 4).copy()
print(arr2)
print(arr1)

arr2[0, 0] = 100
print(arr2)
print(arr1)
```

```python
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
[ 0  1  2  3  4  5  6  7  8  9 10 11]
[[100   1   2   3]
 [  4   5   6   7]
 [  8   9  10  11]]
[ 0  1  2  3  4  5  6  7  8  9 10 11]
```

 * resize( )

reshape( )은 View, resize( )는 `View 아님`

arr.resize( ): 새로운 결과 리턴하지 않고 원본 데이터만 바꾼다 (`View 아님`)

```python
arr1 = np.array([[1, 2, 3], [4, 5, 6]])    
print(arr1)

arr1.resize(1, 6)             		    
print(arr1)

arr1 = np.array([[1, 2, 3], [4, 5, 6]])    
arr2 = arr1.resize(1, 6)                   
print(arr2)
print(arr1)

arr1 = np.array([[1, 2, 3], [4, 5, 6]])    
arr1.resize(3, 4)       							#원본이 2행 3열 --> 3행 4열로 변경 (reshape은 불가능)
print(arr1)
  
arr1.resize(2, 2)       							#3행 4열 --> 2행 2열: 남는 데이터는 버린다
print(arr1)
```

```python
[[1 2 3]
 [4 5 6]]
[[1 2 3 4 5 6]]
None
[[1 2 3 4 5 6]]
[[1 2 3 4]
 [5 6 0 0]
 [0 0 0 0]]
[[1 2]
 [3 4]]
```

np.resize(arr, ( )): 원본은 불변, 복사본 만들어짐 (`View 아님`)

*np.resize( )=reshape( ).copy( )* 

```python
arr1 = np.array([[1, 2, 3], [4, 5, 6]])    
arr2 = np.resize(arr1, (3, 2))      	 
print(arr2)
print(arr1)
```

```python
[[1 2]
 [3 4]
 [5 6]]
[[1 2 3]
 [4 5 6]]
```

* ravel( )

ndarray를 1차원 ndarray로 변경
데이터 주소 변경X, 보여주는 방식(View)만 바꿈

```python
arr1 = np.array([[1, 2, 3], [4, 5, 6]])    
print(arr1)

arr2 = arr1.ravel()
print(arr2)
print(arr1)
```

```python
[[1 2 3]
 [4 5 6]]
[1 2 3 4 5 6]
[[1 2 3]
 [4 5 6]]
```

## 3. data type 변경

* np.astype( )

```python
arr = np.array([1.2, 3.4, 5.3, 6.8])
print(arr)
arr = arr.astype(np.int32)								#np.float64 -> np.int32		
print(arr)               				    			#반올림 X, 소수점 버림
```

```python
[1.2 3.4 5.3 6.8]
[1 3 5 6]
```

## 4. ndarray 만들기

* np.zeros( )

```python
arr = np.zeros((3, 4))                   				#기본 데이터 타입은 np.float64
print(arr)

arr = np.zeros((3, 4), dtype = np.int32)				#데이터 타입 np.int32으로 설정
print(arr)
```

```python
[[0. 0. 0. 0.]
 [0. 0. 0. 0.]
 [0. 0. 0. 0.]]
[[0 0 0 0]
 [0 0 0 0]
 [0 0 0 0]]
```

* np.ones( )

```python
arr = np.ones((3, 4))
print(arr)
```

```python
[[1. 1. 1. 1.]
 [1. 1. 1. 1.]
 [1. 1. 1. 1.]]
```

* np.emtpy( )

```python
arr = np.empty((3, 4))         			#초기화 X, 빠르게 공간만 설정(계산 속도 향상)
print(arr)
```

```python
[[1.02715328e-311 3.16202013e-322 0.00000000e+000 0.00000000e+000]
 [2.44029516e-312 5.40240015e-062 3.77255463e-061 2.12897011e-052]
 [2.05538506e+160 1.71563954e+185 2.85322954e-056 1.53514592e-051]]
```

* np.full( )

```python
arr = np.full((3, 4), 7)         		#특정 숫자로 채워진 ndarray 생성
print(arr)
arr = np.full((3, 4), 7.)
print(arr)
```

```python
[[7 7 7 7]
 [7 7 7 7]
 [7 7 7 7]]
[[7. 7. 7. 7.]
 [7. 7. 7. 7.]
 [7. 7. 7. 7.]]
```

* np.arange( )

```python
arr = np.arange(0, 10, 2)
print(arr)

#python range()
arr = range(0, 10, 2)
print(arr)
```

```python
[0 2 4 6 8]
range(0, 10, 2)
```

## 5. 난수 생성

### 1) np.random.normal(mean, std, size)

정규분포에서 실수인 난수 추출

```python
import matplotlib.pyplot as plt
import numpy as np

mean = 50
std = 2       												   #표준편차
arr = np.random.normal(mean, std, (10000,))  
print(arr)
plt.hist(arr, bins = 100)                #가로값 100개(10000개 중 겹치는 값들 있으니까)
plt.show()
```

```python
[50.81295108 51.92139751 49.61002382 ... 47.04182219 46.98872778 48.0602767 ]
```

![output_10_1](https://user-images.githubusercontent.com/72610879/104833255-a63c0c00-58da-11eb-80ac-b1c0b1bf469e.png)

### 2) np.random.rand(size)

 균등분포에서 0이상 1미만 실수인 난수 추출

```python
arr = np.random.rand(10000)       			#10000개의 데이터 1차원 형태로 추출
print(arr)
plt.hist(arr, bins = 100)
plt.show()
```

```python
[0.79920834 0.73944538 0.69220126 ... 0.42942459 0.62052106 0.11573932]
```

![output_10_3](https://user-images.githubusercontent.com/72610879/104833257-a6d4a280-58da-11eb-8674-ee4ec265cd76.png)

### 3) np.random.randn(size)

표준 정규분포에서 실수인 난수 추출  

### 4) np.random.randint(low, high, size)

균등분포에서 정수표본 추출

```python
arr = np.random.randint(10, 100, (10000,))
print(arr)
plt.hist(arr, bins = 100)
plt.show()
```

```python
[57 66 39 ... 15 16 50]
```

![image-20210117151605646](https://user-images.githubusercontent.com/72610879/104833252-a50adf00-58da-11eb-9e70-3f0c8156913c.png)

### 5) np.random.random((size))

균등분포에서 0이상 1미만 실수인 난수 추출

np.random.rand( )와 동일. ( )안에 넣는 방식만 다름

```python
arr = np.random.random((10000,))
print(arr)
plt.hist(arr, bins = 100)
plt.show()
```

```python
[0.07452357 0.62520544 0.20501572 ... 0.12246561 0.92201029 0.02132257]
```

![image-20210117151624237](https://user-images.githubusercontent.com/72610879/104833253-a5a37580-58da-11eb-96f5-184f7e4ebbc9.png)

  ### 6) np.random.seed( )

실행할 때마다 동일한 난수가 추출되도록 설정

괄호 안에는 임의의 수를 넣는다

```python
np.random.seed(3)  
arr = np.random.randint(0, 100, (10,))
print(arr)
print(arr)
```

```python
[24 3 56 72 0 21 19 74 41 10]
[24 3 56 72 0 21 19 74 41 10]
```

### 7) np.random.shuffle(ndarray)

이미 만들어진 ndarray의 데이터 순서를 random하게 바꾼다

```python
arr = np.arange(10)
print(arr)
np.random.shuffle(arr)
print(arr)
```

```python
[0 1 2 3 4 5 6 7 8 9]
[3 8 2 1 9 4 0 6 7 5]
```

## 6. slicing

* 1차원

```python
import numpy as np

arr=np.arange(0, 5)
print(arr)
print(arr[0:2])
print(arr[0:-1])
print(arr[1:4:2])                			#arr[1]부터 arr[3]까지의 값을 2씩 띄워서
```

```python
[0 1 2 3 4]
[0 1]
[0 1 2 3]
[1 3]
```

* 2차원

```python
arr=np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12], [13, 14, 15, 16]])
print(arr)
print(arr[1, 1])
print(arr[1, :]) 							        #1번째 행 전체
print(arr[1:3,])
print(arr[0])								          #0번째 행 전체
```

```python
[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]
 [13 14 15 16]]
6
[5 6 7 8]
[[ 5  6  7  8]
 [ 9 10 11 12]]
[1 2 3 4]
```

##  7. 정렬

```python
import numpy as np

np.random.seed(1)
arr = np.random.randint(0, 15, (10,))
print(arr)
```

```python
[ 5 11 12  8  9 11  5  0  0  1]
```

* np.sort( ): 원본 ndarray는 변화 없고 정렬된 복사본 리턴

```python
print(np.sort(arr))
print(arr) 
```

```python
[ 0  0  1  5  5  8  9 11 11 12]
[ 5 11 12  8  9 11  5  0  0  1]
```

* arr.sort(): 원본 ndarray 정렬, 리턴X

```python
print(arr.sort()) 
print(arr)
```

```python
None
[ 0  0  1  5  5  8  9 11 11 12]
```

* 역순정렬

```python
print(arr[::-1])
```

```python
[12 11 11  9  8  5  5  1  0  0]
```

## 8. indexing

### 1) boolean indexing

* 반복문 

```python
import numpy as np
np.random.seed(1)
arr = np.random.randint(1, 20, (10,))
print(arr)

for temp in arr:
    if temp % 2 == 0:
        print(temp, end = ' ') 
```

```python
[ 6 12 13 9 10 12 6 16 1 17 ]
6 12 10 12 6 16
```

* boolean indexing

boolean mask: 원본 ndarray의 shape과 동일, 요소값은 모두 boolean(T/F)인 ndarray

boolena indexing: boolean mask 중 True 값만 추출

```python
print(arr % 2 == 0)       					  #boolean mask
print(arr[arr % 2 == 0])  					  #boolenan indexing
```

```python
[ True  True False False  True  True  True  True False False]
[ 6 12 10 12 6 16 ]
```

### 2) fancy indexing

ndarray에 index배열(list형식)을 전달하여 배열요소 참조하는 방식

```python
arr = np.array([1, 2, 3, 4, 5])
print(arr)
print(arr[[1, 3, 4]])                 #np.array([arr[1], arr[3], arr[4]]) 

arr=np.arange(0, 12).reshape(3, 4).copy()
print(arr)
print(arr[2, 2])
print(arr[1:2, 2])    							  #1차원
print(arr[1:2, 1:2])  							  #2차원
print(arr[[0, 2], 2])  							  #1차원
print(arr[[0, 2], 2:3])  							#2차원
print(arr[np.ix_([0, 2], [0, 2])])   	#[[arr[0,0],arr[0,2]],[arr[2,0],arr[2,2]]]
```

```python
[1 2 3 4 5]
[2 4 5]
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
10
[6]
[[5]]
[ 2 10]
[[ 2]
 [10]]
[[ 0  2]
 [ 8 10]]
```

### 3) iterator

다차원 ndarray 쉽게 만들 수 있다

* 1차원 ndarray

```python
arr = np.array([1, 2, 3, 4, 5])

#for문 이용한 반복처리
for tmp in arr:
    print(tmp, end = ' ')

print()

#iterator(지시자) 이용한 반복처리 
it = np.nditer(arr, flags = ['c_index'])          	#nditer = ndarray iterator
                                              			#flags = iterator가 움직이는 방식
                                              			#c_index: c언어의 기본 인덱싱 방식
while not it.finished:
    idx = it.index    									            #idx = 0
    print(arr[idx], end = ' ')
    it.iternext()   									              #화살표 옮기기 
```

1차원 ndarray는 for문을 쓰는게 편하다

```python
1 2 3 4 5 
1 2 3 4 5 
```

* 2차원 ndarray

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

for tmp1 in range(arr.shape[0]):       				    #arr.shape=(2,3), arr.shape[0]=2, arr.shape[1] = 3
    for tmp2 in range(arr.shape[1]):
        print(arr[tmp1, tmp2], end = ' ')

print()  

#iterator(지시자) 이용한 반복처리
it = np.nditer(arr, flags = ['multi_index'])			#다차원에서는 multi_index 사용
while not it.finished:
    idx = it.multi_index
    print(arr[idx], end = ' ')
    print(idx)
    it.iternext()
```

iterator 이용하면 몇 차원이든 코드 동일!

```python
1 2 3 4 5 6 
1 (0, 0)
2 (0, 1)
3 (0, 2)
4 (1, 0)
5 (1, 1)
6 (1, 2)
```

## 9. 연산

### 1) 곱셈

2차원 행렬에서만 나타나는 연산

```python
import numpy as np

arr1 = np.array([[1, 2, 3], [4, 5, 6]])
arr2 = np.arange(10, 16).reshape(3, 2).copy()
print(arr1)
print(arr2)
print(np.matmul(arr1, arr2)) 
print(np.dot(arr1, arr2))
```

```python
[[1 2 3]
 [4 5 6]]
[[10 11]
 [12 13]
 [14 15]]
[[ 76  82]
 [184 199]]
[[ 76  82]
 [184 199]]
```

### 2) 비교

```python
import numpy as np

np.random.seed(4)
arr1 = np.random.randint(0, 10, (2, 3))
arr2 = np.random.randint(0, 10, (2, 3))
print(arr1)
print(arr2)
print(arr1 == arr2)  								        #boolean mask
print(arr1 > arr2)   								        #boolean mask
print(np.array_equal(arr1, arr2)) 					#arr1과 arr2가 같은지?
```

```python
[[7 5 1]
 [8 7 8]]
[[2 9 7]
 [7 7 9]]
[[False False False]
 [False  True False]]
[[ True False False]
 [ True False False]]
False
```

## 10. 전치행렬

1차원 벡터의 전치행렬은 구할 수 없다

```python
import numpy as np

arr=np.array([[1,2,3],[4,5,6]])
print(arr)
print(arr.T)
```

```python
[[1 2 3]
 [4 5 6]]
[[1 4]
 [2 5]
 [3 6]]
```

## 11. 함수

### 1) numpy 내장 함수

```python
import numpy as np
arr = np.arange(1, 7).reshape(2, 3).copy()
print(arr)
```

```python
[[1 2 3]
 [4 5 6]]
```

* 합

```python
print(np.sum(arr))
print(arr.sum())
#print(sum(arr)) ---> ERROR!
```

* 평균 

```python
print(np.mean(arr))
```

* 최대값

```python
print(np.max(arr))
```

* 최소값

```python
print(np.min(arr))
```

* 최대값의 인덱스 반환

```python
print(np.argmax(arr))
```

* 최소값의 인덱스 반환

```python
print(np.argmin(arr))
```

* 표준편차

```python
print(np.std(arr))
```

* 제곱근

```python
print(np.sqrt(arr))
```

### 2) axis

numpy의 집계함수는 axis를 기준으로 연산

axis를 지정하지 않으면 axis는 None으로 설정, 연산 대상은 배열 전체

axis는 숫자로 표현: 0, 1, 2, 3, 4, ...

* sum( )

1차원

![그림1-1610856166528](https://user-images.githubusercontent.com/72610879/104833261-a76d3900-58da-11eb-950d-c8fd3b47f76f.png)

```python
arr = np.array([1, 2, 3, 4, 5]) 							
print(arr.sum(axis = 0))
print(arr.sum())

#print(arr.sum(axis = 1)) --> ERROR!
```

1차원의 axis는 0밖에 없기에  axis는 큰 의미 X

```python
15
15
```

2차원

![그림2](https://user-images.githubusercontent.com/72610879/104833264-a805cf80-58da-11eb-8c0b-46d6846f20b0.png)

```python
arr=np.array([[1,2,3],[4,5,6],[7,8,9],[10,11,12]])
print(arr.sum())  						#axis 지정X --> sum() 대상은 전체 ndarray
print(arr.sum(axis=0))     
print(arr.sum(axis=1)) 
```

```python
78
[22 26 30]
[ 6 15 24 33]
```

3차원

![그림3](https://user-images.githubusercontent.com/72610879/104833265-a89e6600-58da-11eb-82e8-b43813e5d26a.png)

* argmax( )

```python
np.random.seed(2)
arr = np.random.randint(1, 12, (4, 3))
print(arr)
print(arr.argmax(axis = 0))
print(arr.argmax(axis = 1))
```

```python
[[ 9  9  7]
 [ 3  9  8]
 [ 3  2  6]
 [11  5  5]]
[3 0 1]
[0 1 2 0]
```

* concatenate( ): ndarray 연결

```python
arr1 = np.array([[1, 2, 3], [4, 5, 6]]) 			 #2차원
arr2 = np.array([7, 8, 9]) 									   #1차원
arr3 = np.array([7, 8, 9, 10])
result1 = np.concatenate((arr1, arr2.reshape(1, 3)), axis = 0) #arr2.reshape(1, 3): arr2를 2차원으로 바꿔준다
result2 = np.concatenate((arr2.reshape(1, 3), arr1), axis = 0)
result3 = np.concatenate((arr1, arr3.reshape(2, 2)), axis = 1)
print(result1)
print(result2)
print(result3)
```

```python
[[1 2 3]
 [4 5 6]
 [7 8 9]]
[[7 8 9]
 [1 2 3]
 [4 5 6]]
[[ 1  2  3  7  8]
 [ 4  5  6  9 10]]
```

* delete( )

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])
print(arr)
result = np.delete(arr, 1)					#axis명시하지 않으면 자동으로 1차 배열로 변환 후 삭제
print(result)
result = np.delete(arr, 1, axis = 0)
print(result)
result = np.delete(arr, 1, axis = 1)
print(result)
```

```python
[[1 2 3]
 [4 5 6]]
[1 3 4 5 6]
[[1 2 3]]
[[1 3]
 [4 6]]
```
