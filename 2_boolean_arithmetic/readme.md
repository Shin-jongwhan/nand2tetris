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


# ALU
### ALU는 조건을 zx, nx, zy, ny, f, no 하나씩 순서대로 풀어가야 한다.
### <br/><br/>

## zx, zy
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
### <br/><br/><br/>

## zx 구현에 앞서... 
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

### 아니면 a는 16비트, b는 1비트로 input을 받아서 동작하게 만들어서 사용할 수도 있다. 이게 가장 좋다.
### 내장형 칩 등의 이유를 제외하면(nand2tetris 프로젝트만 해당하는 문제이고, 실제 케이스에서는 그러한 이유는 존재하지 않음을 참고) 이게 게이트 수가 가장 적고, 유지보수가 가장 쉽고 깔끔한 방법이다.
### ex)
```
Xor16Abit(a=x1, b=nx, out=x2);
```
### <br/><br/>

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
### <br/><br/><br/>


## nx, ny
### 조건은 zx == 1이면 x = !x이다. 이에 대한 진리표는 Xor과 같다.
| nx | x | out |
|----|---|-----|
| 0  | 0 | 0   |
| 0  | 1 | 1   |
| 1  | 0 | 1   |
| 1  | 1 | 0   |
### <br/>

### 그런데 문제는 내장칩에 Xor16 게이트가 없다는 것이다. 
### 다른 깃허브 찾아보면 내장칩으로 구현하기 위해 Not16, Mux16 게이트로 만들었는데 비효율적인 설계이다.
#### * 이런 것 생각해보면 추상화와 구현 패러다임 / 구현과 효율성을 생각해보면 나는 구현과 동시에 효율성을 같이 생각하고 있다.
```
Not16(in=zxout,out=notx);
Not16(in=zyout,out=noty);
Mux16(a=zxout,b=notx,sel=nx,out=nxout); 
Mux16(a=zyout,b=noty,sel=ny,out=nyout);
```
### <br/><br/><br/>


## f
### f는 Add16 또는 And16를 선택하는 문제다.
```
Add16(a=x2, b=y2, out=addxy);
And16(a=x2, b=y2, out=andxy);
Mux16(a=andxy, b=addxy, sel=f, out=outf);
```
### <br/><br/><br/>


## no, ng
### no는 nx, xy와 같은 로직이다. Xor 게이트 쓰거나 Not + Mux를 쓴다.
### ng는 맨 마지막(\[15\]) 비트가 0이면 양수, 1이면 음수이다. 그러므로 똑같이 가면 되서 같은 칩에서 output을 출력할 수 있다.
### outlow, outhigh는 내장칩에 Or16Way 있었으면 좋겠는데 없고 Or8Way만 있어서 8비트로 나눈 것이다.
```
Xor16(a[0]=no, a[1]=no, a[2]=no, a[3]=no, a[4]=no, a[5]=no, a[6]=no, a[7]=no, 
a[8]=no, a[9]=no, a[10]=no, a[11]=no, a[12]=no, a[13]=no, a[14]=no, a[15]=no, 
b=outf, out=out, out[0..7]=outlow, out[8..15]=outhigh, out[15]=ng);
```
### <br/><br/><br/>


## zr
### 모든 비트에서 1이 하나라도 있으면 0이 된다. 그러므로 Or8Way를 이용한다(Or16Way를 개발해서 사용해도 좋다).
### 모두 0이면 0이 출력되므로 Not 게이트로 반전시킨다.
```
Or8Way(in=outlow, out=outlow2);
Or8Way(in=outhigh, out=outhigh2);
Or(a=outlow2, b=outhigh2, out=nzr);
Not(in=nzr, out=zr);
```
### <br/><br/><br/>


## ALU 정리
### 일부 없는 게이트(Xor16, Xor16Abit)는 직접 만들었다. 내장형 칩으로 구현할 수 있는 것은 코드가 길어지더라도 내장형 칩을 사용하였다.

### <br/><br/>
### hdl
```
// This file is part of www.nand2tetris.org
// and the book "The Elements of Computing Systems"
// by Nisan and Schocken, MIT Press.
// File name: projects/2/ALU.hdl
/**
 * ALU (Arithmetic Logic Unit):
 * Computes out = one of the following functions:
 *                0, 1, -1,
 *                x, y, !x, !y, -x, -y,
 *                x + 1, y + 1, x - 1, y - 1,
 *                x + y, x - y, y - x,
 *                x & y, x | y
 * on the 16-bit inputs x, y,
 * according to the input bits zx, nx, zy, ny, f, no.
 * In addition, computes the two output bits:
 * if (out == 0) zr = 1, else zr = 0
 * if (out < 0)  ng = 1, else ng = 0
 */
// Implementation: Manipulates the x and y inputs
// and operates on the resulting values, as follows:
// if (zx == 1) sets x = 0        // 16-bit constant
// if (nx == 1) sets x = !x       // bitwise not
// if (zy == 1) sets y = 0        // 16-bit constant
// if (ny == 1) sets y = !y       // bitwise not
// if (f == 1)  sets out = x + y  // integer 2's complement addition
// if (f == 0)  sets out = x & y  // bitwise and
// if (no == 1) sets out = !out   // bitwise not

CHIP ALU {
    IN  
        x[16], y[16],  // 16-bit inputs        
        zx, // zero the x input?
        nx, // negate the x input?
        zy, // zero the y input?
        ny, // negate the y input?
        f,  // compute (out = x + y) or (out = x & y)?
        no; // negate the out output?
    OUT 
        out[16], // 16-bit output
        zr,      // if (out == 0) equals 1, else 0
        ng;      // if (out < 0)  equals 1, else 0

    PARTS:
        //zx, zy
        Not16(in[0]=zx, in[1]=zx, in[2]=zx, in[3]=zx, in[4]=zx, 
            in[5]=zx, in[6]=zx, in[7]=zx, in[8]=zx, in[9]=zx, in[10]=zx, 
            in[11]=zx, in[12]=zx, in[13]=zx, in[14]=zx, in[15]=zx, out=nzx);
        And16(a=x, b=nzx, out=x1);
        Not16(in[0]=zy, in[1]=zy, in[2]=zy, in[3]=zy, in[4]=zy, 
            in[5]=zy, in[6]=zy, in[7]=zy, in[8]=zy, in[9]=zy, in[10]=zy, 
            in[11]=zy, in[12]=zy, in[13]=zy, in[14]=zy, in[15]=zy, out=nzy);
        And16(a=y, b=nzy, out=y1);

        //nx, ny
        Xor16Abit(a=x1, b=nx, out=x2);
        Xor16Abit(a=y1, b=ny, out=y2);
        //Or you can implement this function using not and mux 
        //Not16(in=x1,out=nx1);
        //Not16(in=y1,out=ny1);
        //Mux16(a=x1,b=nx1,sel=nx,out=x2); 
        //Mux16(a=y1,b=ny1,sel=ny,out=y2);

        //f
        Add16(a=x2, b=y2, out=addxy);
        And16(a=x2, b=y2, out=andxy);
        Mux16(a=andxy, b=addxy, sel=f, out=outf);

        //no, ng
        Xor16Abit(a=outf, b=no, out=out, out[0..7]=outlow, out[8..15]=outhigh, out[15]=ng);
        //Or you can implement this function using not and mux
        //Not16(in=outf, out=noutf);
        //Mux16(a=outf, b=noutf, sel=no, out=out, out[0..7]=outlow, out[8..15]=outhigh, out[15]=ng);

        //zr
        Or8Way(in=outlow, out=outlow2);
        Or8Way(in=outhigh, out=outhigh2);
        Or(a=outlow2, b=outhigh2, out=nzr);
        Not(in=nzr, out=zr);
}
```
