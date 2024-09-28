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

### 아래 2가지 파일을 작성하고 테스트하고 out 파일을 만들어야 한다.
- \[칩 이름\].hdl : 하드웨어 시뮬레이터에서 쓰는 칩 스크립트
- \[칩 이름\].tst : 테스트 스크립트. 확인해보니 이미 프로젝트 폴더 내에 파일이 완성되어 있음
### <br/>

### .cmp 파일은 정답이 써져 있는 파일이고, .out 파일을 만들어서 정답과 비교하면 된다.
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

### 다중 부정이 헷갈리게 써져 있는데, A\`\` = A로, 이중 부정(\`\`)은 제거해도 된다는 뜻이다.
### 그런데 A` = A는 아님(왜 저렇게 써져 있는지는 모르겠음)
### <br/><br/><br/>

## 논리 기호
### 논리 기호는 집합과 연관이 되어 있어 집합으로 표현이 가능하다.
### 책에서도 논리 기호를 쓰니 알아두면 좋다.
- 논리합 ∨ : Or과 같음
- 논리곱 ∧ : And와 같음
- 부정 ¬ : Not과 같음
#### https://ko.wikipedia.org/wiki/%EB%93%9C_%EB%AA%A8%EB%A5%B4%EA%B0%84%EC%9D%98_%EB%B2%95%EC%B9%99
#### ![image](https://github.com/user-attachments/assets/1580d017-837b-4699-bea7-ff98984194ff)
### <br/><br/><br/>

## 칩(게이트) 설계 시 최적화 방법
### nand 게이트로 다른 기초 논리 게이트를 다 만들 수 있다. 그런데 논리 회로를 어떻게 짜느냐에 따라 게이트 수가 달라진다. 
### 가장 좋은 방법은 최대한 nand 게이트 수가 적은 방향으로 사용하는 것이다. 
### and, or 등의 게이트로 구현하려 하면 게이트 수가 더 증가한다. 왜냐면 이러한 게이트들은 nand 게이트보다 더 고수준의 게이트들이기 때문이다(이 책에서는 nand 게이트 기반으로 여러 개 묶어서 설계).
#### * 이 챕터에 내용을 보면 성능은 이 챕터에서는 고려하지 않는다고 이야기하긴 함.
### <br/>

### 예를 들어 and를 구현하는 방법은 여러 가지가 있는데, 그 중 2개를 보자면
- not(nand(a, b)) : nand 게이트 2개
- nand(nand(a, b), nand(a, b)) : nand 게이트 3개
### 위와 같이 어떻게 칩을 설계하느냐에 따라 nand 게이트 수가 달라진다.
### <br/><br/><br/>

## 카르노맵
### 논리 함수식을 간소화할 수 있는 방법인데, 블로그, 유튜브 영상들이 많다. 몇 개 참고하자.
### 카르노맵은 변수 4개 이하에서만 유효하다.
- https://krakens.tistory.com/100
- https://twojun-space.tistory.com/54#google_vignette
- https://www.youtube.com/watch?v=IsMRUf_3m6U
### <br/>

### 카르노 맵을 그려주는 사이트가 있다.
#### https://www.boolean-algebra.com/kmap
### <br/>

### 아래 그럼 보면 AB 칸 순서가 00, 01, 11, 10으로 되어 있다. 이 순서를 반드시 지켜주어야 한다. 
### 00, 01, 10, 11로 바꿔서 해봤는데 간소화 식이 일치하지 않는다.
#### ![image](https://github.com/user-attachments/assets/94bbd634-37b3-41cb-9122-82d4009bc1d0)
### <br/>

### 예를 들어서 멀티플렉서 구현을 할 때에 오른쪽과 같이 넣어주어야 간소화가 진행 되고, 왼쪽과 같이 하면 식이 안 맞는다(테스트도 실패함).
#### ![image](https://github.com/user-attachments/assets/df6513b0-f54d-4e13-94a0-5e086a4eab2a)
### <br/><br/><br/>

## 퀸 맥클러스키(Quine-McCluskey algorithm) 방법
### 논리 함수를 구현하는 방법이 어차피 노가다이다. 
### 변수가 많아지면 어떻게 하지?
### 최소 함수를 구하는 걸 직접 개발할까 하다가... 계속 찾아보니 다양한 것들이 이미 있었다.
### <br/>

### minterm, 소항 또는 주항(prime implicants), 필수 소항(essential prime implicants), 최종 소항(final implicants)라는 개념을 알아야 한다.
### <br/>

### minterm은 간단하다. 참이 출력되는 항을 나타낸 집합이다.
### 예를 들어 멀티플렉서는 입력 A, B, S (selector)가 있을때 참이 110, 100, 011, 111 에 나타나 있다.
### 그럼 minterm은 110, 100, 011, 111이다.
#### ![image](https://github.com/user-attachments/assets/67b18f9f-5e1b-4b13-a217-6309bdfb6a35)
### <br/>

### 소항은 minterm에서 가능한 경우의 수를 나타낸다. 출력에 관련이 없을 경우 '-'이라고 표시한다.
### 예를 들어 110, 100, 011, 111의 소항은 
- -11 : A에 관계 없이 BS가 11을 출력함
- 11- : S에 관계 없이 AB가 11을 출력함
- 1-0 : B에 관계 없이 A는 1, S는 0을 출력함
### 이렇게 3개로 구해진다.
### <br/>

### 필수 소항은 `반드시 포함되어야 하는 소항(Prime Implicant)`을 의미한다.
### 소항을 구하고 나서, 각각의 소항들이 다른 소항에 의해 출력될 수 있는가를 본다.
### 예를 들어 11-은 110, 111을 출력할 수 있다. 그런데 110은 소항 1-0에서도 출력 가능하고, 111은 -11에서 출력 가능하다. 그럼 이 항은 없애도 된다.
### 이런 식으로 없앨 수 있는 건 없애고 남은 항이 필수 소항이다.
### <br/>

### 최종 소항
### 필수 소항을 구하면서 뭔가 문제점이 생각이 나지 않았나? 바로 항을 없애고 나서 필수 소항을 구하고 났을 때, 그 필수 소항들이 소항들의 출력을 모두 커버할 수 있는가다.
### 다 커버하지 못 한다. 그래서 그런 소항은 필수 소항에다가 다시 추가해줘야 한다.
### 이렇게 구성된 게 최종 소항이고, 최종적으로 간소화된 형태이다.
### <br/>

### 이러한 구성일 때, 다음과 같이 구해진다.
#### minterms = \['00000', '00001', '00011', '00111', '01111', '11111'\]
#### variables = \['A', 'B', 'C', 'D', 'E'\]
- 소항(prime implicants): {'0000-', '0-111', '00-11', '000-1', '-1111'}
- 필수 소항(essential prime implicants): {'0000-', '-1111'}
- 최종 소항(final implicants): {'00-11', '0000-', '-1111'}
- 논리식: ¬A∧¬B∧D∧E ∨ ¬A∧¬B∧¬C∧¬D ∨ B∧C∧D∧E
### <br/><br/><br/>

## 패트릭(Petrick) 방법
### 퀸 맥클러스키 방법과 함께 사용한다. 마지막에 최종 소항을 구하는 데에 쓰인다.
### 먼저 최종 소항을 가장 적은 게이트의 수로 구성하려면 비용 계산을 해야 한다.
- 소항의 수: 선택된 minterm의 총 개수.
- 리터럴의 수: 선택된 소항에서 등장하는 변수와 그 부정 형태의 총 개수. 예를 들어 011, 010과 같이 0, 1의 등장 횟수(여기서는 3). 1-0, 10-인 경우 리터럴 수는 2이다.
- 논리 게이트의 수: 실제 회로 구현 시 필요한 게이트의 수.
### <br/>

### 위 비용을 계산하기 위해 먼저 소항이 minterm을 커버하는 것을 보여주는 매트릭스를 그려야 한다.
### m이 minterm, P가 소항이라고 보면 된다.
#### ![image](https://github.com/user-attachments/assets/bc72eadc-2650-43f4-af25-4cb789f4973d)
#### ![image](https://github.com/user-attachments/assets/8487fc50-dbfd-4874-9e83-e89f41c5e9ab)
#### ![image](https://github.com/user-attachments/assets/bd0453f9-1e1e-4a98-ba3b-ab7c484c87d5)
#### ![image](https://github.com/user-attachments/assets/4c4051ef-3e65-4b45-b5c3-eb164e105d0c)
#### ![image](https://github.com/user-attachments/assets/6131e70e-cdc4-445c-be5a-44d1d6466c95)
#### ![image](https://github.com/user-attachments/assets/a8bdd34b-8e5c-4cd0-94f0-c365e29b38ac)



## 실행
### 하드웨어 시뮬레이터에서 칩과 테스트 스크립트를 로드하고 >> 버튼을 누르면 된다. 그러면 output-file Not.out 이라고 써진 부분과 같이 스크립트 폴더 내에 만들어진다.
#### ![image](https://github.com/user-attachments/assets/48c33a9f-770f-4fa6-9802-2188103ff41f)

