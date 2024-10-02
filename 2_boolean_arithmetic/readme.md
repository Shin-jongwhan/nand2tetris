### 241001
## 2장 불 연산
### <br/><br/><br/>

## 과제
### 다음을 구현해야 한다.
- 반가산기(Halfadder)
- 전가산기(Fulladder)
- 16비트 가산기(Add16)
- 증분기(Incrementer)
- 산술 논리 장치((Arithmetic Logic Unit, ALU)
### <br/><br/><br/>

## 가산기 3개
### 먼저 반가산기 -> 전가산기 -> 16비트 가산기 순서로 만들어야 한다.
### 이 3개 칩은 더하는 것에 초점이 맞추어져 있다.
### <br/>

### 반가산기는 input a, b이다.
### output은 오른쪽 비트를 의미하는 sum, 왼쪽 비트를 의미하는 carry로 나뉘어지고 다음과 같이 출력 되어야 한다.
| a | b | sum | carry |
|---|---|-----|-------|
| 0 | 0 | 0   | 0     |
| 0 | 1 | 1   | 0     |
| 1 | 1 | 0   | 1     |
### <br/>

### 전가산기는 input a, b, c이다. 3개 비트를 더하는 것이다.
### output은 반가산기와 같다. 
| a | b | c   |sum    |carry|
|---|---|-----|-------|-----|
| 0 | 0 | 0   | 0     |  0  |
| 0 | 0 | 1   | 1     |  0  |
| 0 | 1 | 0   | 1     |  0  |
| 0 | 1 | 1   | 0     |  1  |
| 1 | 0 | 0   | 1     |  0  |
| 1 | 0 | 1   | 0     |  1  |
| 1 | 1 | 0   | 0     |  1  |
| 1 | 1 | 1   | 1     |  1  |
### <br/>

### Add16은 최하위 비트부터 최상위 비트까지 반가산기와 전가산기를 이용해 순차적으로 더해주면 된다.
### <br/><br/><br/>

## Inc16
### 아래와 같이 작성했다.
```
// This file is part of www.nand2tetris.org
// and the book "The Elements of Computing Systems"
// by Nisan and Schocken, MIT Press.
// File name: projects/2/Inc16.hdl
/**
 * 16-bit incrementer:
 * out = in + 1
 */
CHIP Inc16 {
    IN in[16];
    OUT out[16];

    PARTS:
    Not(in=in[0], out=out[0]);
    HalfAdder(a=in[1], b=in[0], sum=out[1], carry=carry1);
    HalfAdder(a=in[2], b=carry1, sum=out[2], carry=carry2);
    HalfAdder(a=in[3], b=carry2, sum=out[3], carry=carry3);
    HalfAdder(a=in[4], b=carry3, sum=out[4], carry=carry4);
    HalfAdder(a=in[5], b=carry4, sum=out[5], carry=carry5);
    HalfAdder(a=in[6], b=carry5, sum=out[6], carry=carry6);
    HalfAdder(a=in[7], b=carry6, sum=out[7], carry=carry7);
    HalfAdder(a=in[8], b=carry7, sum=out[8], carry=carry8);
    HalfAdder(a=in[9], b=carry8, sum=out[9], carry=carry9);
    HalfAdder(a=in[10], b=carry9, sum=out[10], carry=carry10);
    HalfAdder(a=in[11], b=carry10, sum=out[11], carry=carry11);
    HalfAdder(a=in[12], b=carry11, sum=out[12], carry=carry12);
    HalfAdder(a=in[13], b=carry12, sum=out[13], carry=carry13);
    HalfAdder(a=in[14], b=carry13, sum=out[14], carry=carry14);
    HalfAdder(a=in[15], b=carry14, sum=out[15], carry=carry15);
}
```
### <br/>

### 다른 걸 찾아보니 true, false라는 게 있었다. true는 1, false는 0을 나타낸다.
### 아래와 같이 하면 Incrementor를 별도로 작성하는 의미가 없어진다. 또한 위의 칩 로직과 비교해볼 때 성능 상 떨어진다(논리 회로가 더 많음).
```
PARTS:
    Add16(a=in,b[0]=true,b[1..15]=false,out=out);
```
### <br/>

### PARTS 부분을 한 단계 더 풀어쓰면 이렇게 된다. FullAdder는 캐리 전파 지연이 3배나 된다.
#### * 그냥 단순히 회로 개수를 비교하기 위해 풀어쓴 것으로 carry 번호는 무시한다.
```
HalfAdder(a=a[0], b=b[0], sum=out[0], carry=carry1);
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry);
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry);
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry);
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry); 
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry);
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry);
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry);
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry); 
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry);
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry);
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry);
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry); 
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry);
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry);
HalfAdder(a=a, b=b, sum=sum1, carry=carry1);
    HalfAdder(a=c, b=sum1, sum=sum, carry=carry2);
    Xor(a=carry1, b=carry2, out=carry);
}
```
### <br/><br/>
### <br/><br/><br/>

## 생각해볼 거리
### 먼저 첫 번째 방법으로 하면 코드는 길어지지만 속도는 올라간다. 
### 두 번째 방법으로 하면 효율성은 희생하지만 가독성이 증가하고 유지보수가 쉬워진다.
### <br/>

### 효율성을 좀 더 개선하기 위해 보편적으로 자리올림수 예측(carry lookahead adder, CLA)을 쓴다고 한다. 
### CLA는 16비트를 연산할 때 16비트를 4비트로 나누어 4단계로 병렬처리한다.
### 시간복잡도는 O(log16) = O(4)이다.
### <br/><br/><br/>

## ALU
### ALU는 조건을 zx, nx, zy, ny, f, no 하나씩 순서대로 풀어가야 한다.
### <br/><br/>

### `zx`
### 먼저 zx는 불 함수에서 다음과 같다. a = zx, b = x가 들어가면 된다. 그 반대로 생각하고 만들어도(NotbAnda 게이트) 상관 없다.
| a | b |out|
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |
### <br/>

### 이는 이렇게 풀 수 있다.
```
/**
 * NotaAndb gate:
 * if (a == 0 and b == 1) out = 1 else out = 0
 */
CHIP NotaAndb {
    IN a, b;
    OUT out;
    
    PARTS:
    Not(in=a, out=na);
    And(a=na, b=b, out=out);
}
```
### <br/><br/>

### zx 구현에 앞서... 
### x는 16 비트이고 zx는 1 비트인데, 어떻게 16비트 칩에 넣을 수 있는가다. 
### 방법은 2가지인데 하나는 코드는 깔끔하지 않지만 효율적인 연산을 하고, 하나는 칩 코드가 깔끔하지만 비효율적인 것이다.
### <br/>

### 첫 번째 방법은 다음과 같이 하나씩 적어주는 것이다. 이는 코드는 길어지지만 자원 소모가 최소한의 필요한 것만 쓰고 속도가 더 빠르다(효율적임).
```
/**
 * 16-bit 1 bit NotaAndb gate:
 * for i = 0, ..., 15:
 * if (a[i] == 0 and b == 1) out[i] = 1 else out[i] = 0
 */
CHIP NotaAndb16Abit {
    IN a[16], b;
    OUT out[16];

    PARTS:
    Not16(in[0]=b, in[1]=b, in[2]=b, in[3]=b, in[4]=b, 
    in[5]=b, in[6]=b, in[7]=b, in[8]=b, in[9]=b, in[10]=b, 
    in[11]=b, in[12]=b, in[13]=b, in[14]=b, in[15]=b, out=nb);
    And16(a=a, b=nb, out=out);
}
```
### <br/>

### 두 번째 방법은 Mux를 사용하는 것이다.
### 이 방식은 코드가 간결하지만, 내부적으로 더 많은 연산을 필요로 하므로 속도가 느려질 수 있다. 또한, 논리적인 자원도 더 많이 소모될 가능성이 있다.
```
/**
 * 16-bit 1 bit NotaAndb gate:
 * for i = 0, ..., 15:
 * if (a[i] == 0 and b == 1) out[i] = 1 else out[i] = 0
 */
CHIP NotaAndb16Abit {
    IN a[16], b;
    OUT out[16];

    PARTS:
    Mux16(a=x,b=false,sel=zx,out=zxout);
}
```
