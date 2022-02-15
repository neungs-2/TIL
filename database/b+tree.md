# **B+Tree**

## **B+Tree 란?**

- MySQL의 DB 엔진 중 하나인 InnoDB에서 사용하는 인덱스 구조
- B-Tree의 확장된 개념
- Leaf Node가 **Linked List** 구조
  - 선형 검색 가능

**_B+Tree의 차이점 (B-Tree와 비교)_**

- **리프 노드를 제외한 노드들은 Key와 참조에 대한 정보만 지님**
  - `B-Tree`는 모든 노드가 각자 key마다 data를 지님
  - `B+Tree`는 리프노드에 모든 data를 지님
- **모든 리프노드가 연결리스트 형태**
  - `B-Tree`는 바로 옆 리프노드 검사 시 루트노드부터 다시 검사
  - B+Tree`는 리프노드끼리 참조하기 때문에 선형검사를 수행하여 시간복잡도 감소
- **리프노드의 부모 Key는 리프노드의 첫번째 key보다 작거나 같음**
  브랜치 노드와 리프 노드에 모두 Key가 존재하므로 Key 중복이 발생합니다.

<br>

**_특징_**

- 다음 특징들은 B-tree와 동일함
- 최대 *M*개의 자식을 갖는 B트리는 **M차 B트리**
  - 자식 노드의 수: **_M/2 ~ M_**
  - 노드가 포함하는 key 개수: **_[M/2] - 1 ~ M - 1_**
  - 노드의 key가 *x*개일 때 자식의 수: **_x + 1_**
  - 최소차수는 자식 수의 하한값을 의미
    - 최소차수: *t*이면 **_M = 2t - 1_**
    - if `t = 2` then `M = 3`, `key의 하한 1`

<br>

**[인덱스 구조]**
![image](https://user-images.githubusercontent.com/60606025/154111669-ed48a154-615b-439b-8038-17919cf626a7.png)

<br>

**[B+Tree 구조]**
![image](https://user-images.githubusercontent.com/60606025/154111767-6060fd54-3a06-4115-a3b0-86c8b141e423.png)
