# **Git** - fatal: Not possible to fast-forward, aborting

## **문제**

- Push/Pull 시도 시 reject 당함
- Push를 하기 위해서는 Pull을 먼저 해야하는 상황
- Pull 시도 시 요청이 중단되고 fast-forward가 불가하다는 메시지 출력
- **Error Message**<br>
  ![image](https://user-images.githubusercontent.com/60606025/147586809-52bee006-bf41-4caa-803a-8bd6f1b65b4f.png)

<br>

**문제 원인**

- Local에서 Commit 이후 Push를 안하고 Remote에서 Commit
- 결국 Local과 Remote가 분기되었기 때문에 fast-forward가 불가

---

<br>

## **해결 방법**

- 직접 fetch & merge를 하여 브랜치 정리
- `git fetch origin`
- `git status` --> fetch하여 가져온 remote branch명 (origin/main) 확인
- `git merge origin/main`

<br>

![image](https://user-images.githubusercontent.com/60606025/147588571-0a067ca4-f256-4124-bd2d-c62f493bd402.png)

<br>

**Note**
<br>
fast-forward merge 와 3-way-merge 개념을 숙지했다면 더 빠른 해결이 가능했을 것

> https://backlog.com/git-tutorial/kr/stepup/stepup1_4.html <br> https://backlog.com/git-tutorial/kr/stepup/stepup3_2.html <br> https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%B8%8C%EB%9E%9C%EC%B9%98%EC%99%80-Merge-%EC%9D%98-%EA%B8%B0%EC%B4%88
