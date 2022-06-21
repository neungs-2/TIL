# Couldn't Connect MongoDB Server at the Starbucks

## **문제**
- 스타벅스에서 테스트를 위해 로컬에서 Nest 서버를 실행시켰는데 *MongoDB*와 연결이 되지 않아서 실행 실패
- *MongoDB* 측에서 접속 가능 IP를 `0.0.0.0/0`으로 모두 열어둔 상태

## **문제 원인**
- 스타벅스와 같은 공용 와이파이 사용 시 이런 오류가 종종 발생한다고 함
- 정확한 원인은 찾지 못했음...

## **문제 해결**
- DNS 설정을 `8.8.8.8`로 해주면 성공적으로 연결됨
- `8.8.8.8`은 **Google Public DNS**의 IP 주소(IPv4)