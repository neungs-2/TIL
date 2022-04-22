# create-next-app 설치 시 Watchpack Error 이슈

<br>

## **문제**

- `npx create-next-app {my project directory}`로 Next.js 설치 시 발생
- **WSL** 환경에서 Next.js 사용하는 유저에게 발생
- localhost:3000로 렌더링된 화면은 출력 되지만 HMR (Hot Module Replacement)가 미적용
- 페이지 새로고침을 해도 수정사항 반영되지 않음
- 다시 `npm run dev`를 실행해야 수정사항 반영
- 에러 메세지는 아래와 같음

```sh
Watchpack Error (initial scan): Error: EACCES: permission denied, lstat '/mnt/c/DumpStack.log.tmp'
Watchpack Error (initial scan): Error: EACCES: permission denied, lstat '/mnt/c/hiberfil.sys'
Watchpack Error (initial scan): Error: EACCES: permission denied, lstat '/mnt/c/pagefile.sys'
Watchpack Error (initial scan): Error: EACCES: permission denied, lstat '/mnt/c/swapfile.sys'
```

---

<br><br>

## **해결방법**

- `/mnt/c` 아래의 directory에서 project를 생성할 때 해당 오류 발생
- C 드라이브에 스왑파일 같은 특수 파일이 있고 사용자 엑세스 권한이 없는 것이 원인
- 나의 directory 주소 `/mnt/c/Users/user/Git_Repository`
- 따라서 `/mnt/c` 아래의 주소가 아닌 `/home` 등의 다른 곳에 project 생성 시 정상적으로 Watchpack 작동

<br>

> **[참고]**<br> > https://github.com/webpack/watchpack/issues/187
