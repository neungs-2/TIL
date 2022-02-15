# **B-Tree**

## **B-Tree 란?**

- 많은 DBMS에서 인덱스 구조로 사용하는 자료구조
- 모든 리프노드들이 같은 레벨을 가질 수 있도록 균형잡힌(Balanced) 트리
  - 따라서 어떤 값에 대해서도 같은 시간에 결과를 얻을 수 있음
- 이진 트리에서 확장되어 하나의 노드가 가질 수 있는 자식 노드의 최대 개수가 2개 이상
- 내부 노드의 자식 노드 수가 미리 정해진 범위 내에서 변경 가능

<br>

- 하나의 노드에 다수의 정보 포함
- 최대 *M*개의 자식을 갖는 B트리는 **M차 B트리**
  - 자식 노드의 수: **_M/2 ~ M_**
  - 노드가 포함하는 key 개수: **_[M/2] - 1 ~ M - 1_**
  - 노드의 key가 *x*개일 때 자식의 수: **_x + 1_**
  - 최소차수는 자식 수의 하한값을 의미
    - 최소차수: *t*이면 **_M = 2t - 1_**
    - if `t = 2` then `M = 3`, `key의 하한 1`
- key들은 노드 안에서 항상 정렬
- **왼쪽 값은 key보다 작은 값**
- **우측 값은 key보다 큰 값**

![image](https://user-images.githubusercontent.com/60606025/154067146-af102c10-4e23-441f-9c83-ab623d4ca79b.png)

---

<br>

### **_Key 검색 과정_**

- 루트 노드부터 하향식으로 검색
- 검색하려는 값이 k일 때
  - 루트 노드에서 시작하여 key들을 순회하며 검사
    - `k = key`라면 검색 종료
    - k와 key의 대소관계를 비교하여 key들 사이에 k가 존재 시 해당 key들 사이의 자식노드로 내려감
  - 해당 과정을 리프노드에 도달 시까지 반복

<br>

### **_Key 삽입 과정_**

> B-Tree Insert Simulation<br>https://www.cs.usfca.edu/~galles/visualization/BTree.html

- 1. 요소 삽입에 적절한 리프 노드 검색
- 2. 필요한 경우 노드 분할

<br>

- **분할이 일어나지 않는 경우**
  - 리프노드가 가득 차지 않은 경우 오름차순으로 k를 삽입
  - 즉, 루트부터 k의 알맞은 위치를 검색하여 삽입

<br>

- **분할이 일어나는 경우**
  - 리프노드가 가득 찬 경우 노드를 분할
  - 1. 일단 k의 적절한 위치를 검색하여 삽입 --> 최대 key 개수 초과
  - 2. 중앙값을 부모 노드로 하여 노드를 분할
    - 중앙값은 상위 레벨 노드에 삽입
  - 3. 상위 레벨 노드도 같은 과정 반복

![image](https://user-images.githubusercontent.com/60606025/154073369-d71db21a-046c-423b-ba53-e9b7d3900db4.png)
![image](https://user-images.githubusercontent.com/60606025/154073524-ab9a2300-c8e9-4164-a3a8-52949741d24e.png)
![image](https://user-images.githubusercontent.com/60606025/154073635-65089b04-e7a5-4c18-99a1-fa48e484bf34.png)
![image](https://user-images.githubusercontent.com/60606025/154073700-cbd461e3-8f80-44e2-8706-2a8bbef91162.png)
![image](https://user-images.githubusercontent.com/60606025/154073734-57d20de4-7eb8-4a58-af62-1b2b96e33fd2.png)

<br>

### **_Key 삭제 과정_**

- 1. 삭제할 키가 있는 노드 검색
- 2. 키 삭제
- 3. 필요한 경우, 트리 균형 조정
  - 최소 key 개수에 유의!

<br>

- 여러가지 경우 존재
  - **삭제할 key k가 `리프`에 존재 vs `내부 노드`에 존재**
  - **노드나 자식의 키가 `최소 키 숫자`보다 많을 때 vs 최소 키 숫자 이하일 때**

<br>

**예시**

<br>

![image](https://user-images.githubusercontent.com/60606025/154081311-c3318ce3-cb65-4eb3-81c9-57079875b8f8.png)

![image](https://user-images.githubusercontent.com/60606025/154081778-2766123c-deff-409a-9934-7bb1f6d9cb47.png)

> [참고]<br>https://velog.io/@emplam27/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-B-Tree#key-%EC%82%BD%EC%9E%85%EA%B3%BC%EC%A0%95
