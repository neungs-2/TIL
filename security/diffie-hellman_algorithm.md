# **Diffie-Hellman** Algorithm

<br>

## **디피-헬만 알고리즘**이란?

- 상대방의 **공개키**와 나의 **개인키**를 이용하여 **비밀키**를 구하는 방법
  - **비밀키를 사용하여 데이터 암호화**한 후 전달
- **키 교환 알고리즘**
  - **대칭키 공유하는데 사용**
- `이산대수 문제`(Descrete Logarithm Problem) 방식 이용
  - `y = g**x mod p`에서 g, p를 알 때
    - x를 알때, y는 구하기 쉬움
    - y를 알때, x는 구하기 어려움
    - p가 **소수**라면 y는 동일한 확률로 `0 ~ p`

<br>

\*tip: `G mod p`는 G를 p로 나눈 나머지

---

<br>

## **동작 원리**

<br>

![image](https://user-images.githubusercontent.com/60606025/152136423-cb3d96ac-c899-4bd9-a900-1b4fac9ada1e.png)

- *A*와 *B*가 **대칭키를 공유**하는 방법
- 공개적으로 교환할 발생기 `g`
- mod 할 값 `p` 는 **소수**로 지정
- *A*는 개인키 `a`를 이용하여 `ya = g**a mod p` 생성
- *B*는 개인키 `b`를 이용하여 `yb = g**b mod p` 생성
- *A*와 _B_ 서로에게 생성한 공개키를 보냄
- *A*와 *B*는 각자의 비밀키와 공개키를 조합하여 새로운 **대칭키(비밀키)**를 생성
  - 둘이 동일한 키를 가질 수 있음
  - `(ya)\*\*b mod p = (yb)\*\*a mod p`
- 공격자 *T*는 기껏해야 `g**(a+b) mod p` 밖에 만들 수 없음
- 숫자가 커질 수록 비밀키 `a`, `b`는 알아내기 힘듦

> 동일한 대칭키가 만들어지는 것을 잘 모르겠다면 이산수학 정수론 공부!!
> <br> https://startedfrombottom.tistory.com/8

---

<br>

## **취약점**

<br>

![image](https://user-images.githubusercontent.com/60606025/152136540-ef707946-d93e-45b2-a3aa-3cffa2c1f78c.png)

- 대칭키를 비밀스럽게 만들 수 있다는 장점
- 하지만 공격자 *T*가 값을 조작하면 **Man In the Middle attak(MIM)**에 취약
  - *T*가 중간에서 `t`라는 개인키를 이용한 값을 _A_, *B*에 전달
  - 결국 _A, B, T_ 모두 **같은 대칭키** 갖게 됨

> 따라서 디피-헬만의 키 값을 2048 bit 이상으로 생성해야 함
> <br> https://rsec.kr/?p=242

> 현대의 웹브라우저들은 기존의 디피-헬만 방식보다 **ECDHE(Elliptic-Curve Hellman)** 선호
