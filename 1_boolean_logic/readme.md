# 불 논리(boolean logic)
### <br/><br/><br/>

## nand2tetris 다운로드
### https://www.nand2tetris.org/software
### <br/>

### 여기에 가면 다운로드할 수 있다.
### https://drive.google.com/file/d/1IkIR8Pwq3PY49QgXpUJOkUUVht-TKIET/view
#### ![image](https://github.com/user-attachments/assets/d1c1b20a-2528-40e1-b872-36652a084870)
### <br/><br/><br/>

## 하드웨어 시뮬레이터
### zip 파일을 다운로드 한 후 tools/HardwareSimulator.bat을 열면 책에 있는 그림처럼 나온다. 
### java 기반이기 때문에 java를 설치해야 한다.
#### ![image](https://github.com/user-attachments/assets/cbd33d31-aeba-4d3b-a7fc-e967d2d78883)
### <br/><br/><br/>

## 과제
### nand2tetris/projects/1 폴더에 있는 hdl 파일을 모두 작성해야 한다.
#### ![image](https://github.com/user-attachments/assets/7d161bd9-e23f-4ed6-b8d9-1db92bc07b8a)
### <br/>

### nand 게이트로 nand 게이트를 제외한 15개의 게이트에 대해 구현해야 한다. 
### 폴더에는 빠진 게이트도 있는데, 아마 나중에 필요해지면 구현해야 하지 않을까 생각이 든다. 아니면 구현하고 진행하는 것도 나쁘지 않을 듯.
#### ![image](https://github.com/user-attachments/assets/3a14eb60-36a3-43c2-b6b5-b6d808530278)
### <br/>

### 그리고 아래 3가지 파일을 작성하고 테스트하고 cmp 파일을 만들어야 한다.
- \[칩 이름\].hdl : 하드웨어 시뮬레이터에서 쓰는 칩 스크립트
- \[칩 이름\].tst : 테스트 스크립트
- \[칩 이름\].cmp : 테스트 스크립트의 output을 기록한 파일
### <br/><br/><br/>

## 논리 회로
### 논리 회로의 기호로 표현하는 것이 많기 때문에 알아두어야 한다.
#### https://ko.wikipedia.org/wiki/%EB%85%BC%EB%A6%AC_%ED%9A%8C%EB%A1%9C
#### ![image](https://github.com/user-attachments/assets/2900eb7c-a767-469b-ae07-4a24b78fbf94)
### <br/><br/><br/>

## 부울 대수 법칙
### 부울 대수 법칙과 카르노맵을 알면 논리 함수를 간소화할 수 있다.
#### ![image](https://github.com/user-attachments/assets/3a824a6b-e293-4a2a-b097-0db569abbf2a)
### <br/>

### 다중 부정이 헷갈리게 써져 있는데, A`` = A로, 이중 부정(``)은 제거해도 된다는 뜻이다.
### 그런데 A` = A는 아님(왜 저렇게 써져 있는지는 모르겠음)
### <br/><br/><br/>

## 논리 기호
### 논리 기호는 집합과 연관이 되어 있어 집합으로 표현이 가능하다.
### 책에서도 논리 기호를 쓰니 알아두면 좋다.
- 논리합 ∨
- 논리곱 ∧
- 부정 ¬
#### https://ko.wikipedia.org/wiki/%EB%93%9C_%EB%AA%A8%EB%A5%B4%EA%B0%84%EC%9D%98_%EB%B2%95%EC%B9%99
#### ![image](https://github.com/user-attachments/assets/1580d017-837b-4699-bea7-ff98984194ff)
### <br/><br/><br/>


## 칩(게이트) 설계 시 최적화 방법
### nand 게이트로 다른 기초 논리 게이트를 다 만들 수 있다. 그런데 논리 회로를 어떻게 짜느냐에 따라 게이트 수가 달라진다. 
### 가장 좋은 방법은 최대한 nand를 사용하는 것이다. 
### and, or 등의 게이트로 구현하려 하면 게이트 수가 더 증가한다. 왜냐면 이러한 게이트들은 nand 게이트보다 더 고수준의 게이트들이기 때문이다(nand 게이트 기반으로 여러 개 묶어서 설계).
### <br/>

### 예를 들어 not을 구현하는 방법은 여러 가지가 있는데, 그 중 2개를 보자면
- not(nand(a, b)) : nand 게이트 2개
- nand(nand(a, b), nand(a, b)) : nand 게이트 3개
### 위와 같이 어떻게 칩을 설계하느냐에 따라 nand 게이트 수가 달라진다.
### <br/><br/><br/>
