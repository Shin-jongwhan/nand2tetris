## 241005
# Memory
### <br/><br/><br/>

# 과제
### DFF(Data Flip-Flop) 게이트는 내장형 칩에 이미 구현되어 있는 상태라고 한다. 
#### DFF에 대한 동작은 크게 설명하지 않기로 한다고 책에서 나왔고, 본질적으로 어떤 기능과 의미가 있는지만 설명하였다.
### 크게는 3가지를 만들어야 하고 각각에 따라 세부적으로 구현해야 하는 것들이 있다.
- 레지스터 : 1비트, 16비트 레지스터. 시간에 따라 상태를 기억하고 출력하는 칩.
- RAM : 8, 64, 512, 4K, 16K RAM. K는 1024이다. 주솟값(address)에 따라 레지스터를 선택하여 레지스터를 업데이트하거나 출력한다(읽기 / 쓰기를 한다).
- 카운터 : Program Counter(PC)라고 하는 기능을 만들어야 한다. 시간(클럭)에 따라 +1을 해주는 메모리 장치이다.
### <br/>

### 과제는 a, b 폴더로 나뉘어져 있다. a가 좀 더 저수준, b가 좀 더 고수준의 칩이다. 
### RAM64.hdl이 내장형 칩에 있어서 b에 있는 칩들을 구현할 때 내장형 칩을 가져올 수 있도록 하기 위해서 나누어 놓았다고 한다.
#### ![image](https://github.com/user-attachments/assets/e2e36f7c-cf5d-478d-ac06-01fd0572af22)

### <br/><br/><br/>

## bit.hdl
### 가장 먼저 1비트 레지스터를 구현해야 하는데 혹시 이렇게 하면 될까 하면... 그게 맞다. 만들어진다.
### 그런데 이렇게 해서 만들면 의문이 생긴다. 최초 실행 시에는 어떻게 Mux 다음에 있는 DFF의 output인 out0를 받아올 수 있는 걸까?
### 책에는 별도로 설명 되어 있지 않지만 추측으로는, 한 칩 내에서 같은 output name을 가져와서, 있으면 그 값을, 없으면 false로 하는 것 같다.
### 칩에는 없는 아예 다른 이름으로 해보면 에러가 난다. 다만, 칩에서 명시한 output 이름으로 하면 에러가 나지 않는다.
### 이에 대한 것은 아래에 자세히 설명한다.
```
// This file is part of www.nand2tetris.org
// and the book "The Elements of Computing Systems"
// by Nisan and Schocken, MIT Press.
// File name: projects/3/a/Bit.hdl
/**
 * 1-bit register:
 * If load is asserted, the register's value is set to in;
 * Otherwise, the register maintains its current value:
 * if (load(t)) out(t+1) = in(t), else out(t+1) = out(t)
 */
CHIP Bit {
    IN in, load;
    OUT out;

    PARTS:
    Mux(a=out0, b=in, sel=load, out=out1);
    DFF(in=out1, out=out, out=out0);
}

```
### <br/><br/>

## 조합 칩(Combinational chip) 과 순차 칩(Sequencial chip)
### 조합 칩은 시간 독립적 칩(time-independent chip)이라고도 한다.
### 순차 칩은 클록 칩(clocked chip)이라고도 한다.
### DFF는 순차 칩이다. 시간에 의존성이 있고, 시간 지연이 발생한다.
#### * 참고로 번역기에서 시간 의존성 칩의 영어 명칭이 time-independent chip인데 잘못 번역이 된 듯 하다. 시간 독립적 칩이 맞다.
### <br/>

### 부록 2에 보면 순차 칩인 DFF는 재귀적으로 어딘가 클록화된 핀이 있는지 검사하고 있으면 루프를 허용한다고 한다.
### 이를 피드백 루프라고 한다.
### 그래서 out을 별도로 사용할 수 있게 한다. 
### 이는 데이터 경쟁(data competition)을 방지하기 위한 조치이다(동시에 들어간 두 데이터 간 경쟁으로 잘못된 출력이 나오는 현상).
### <br/><br/><br/>

