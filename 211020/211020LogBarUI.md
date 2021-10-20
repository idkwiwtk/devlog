# Log bar 
- 시스템의 정보를 출력
- 사용자 에게 중요한 메시지등을 알림
- 시스템 이상 시 알림

# 기존 log bar의 문제점
- 공구 사용량 업 메시지가 거의 대부분을 차지해서 다른 메시지를 볼 수 없다.
- 여러 종류의 메시지가 한 군데에 몰려 있어 정확한 메시지를 파악하기 어렵다.

# 해결 방안
- 메시지를 종류별로 나누어서 표시한다. 구역을 나누고 표시
- 공구 사용량 업 메시지는 최신 메시지만 표시한다.
- 종류별 구역으로 나누고 표시한다.

# 로그바 구현 계획
1. 로그바를 불러올 아이콘 변경 (기존의 스위치 버튼 -> 알림 아이콘)
2. 로그바 영역을 가로영역 25% 까지 넓히기 (기존의 디폴트 너비 -> 가로길이 25%)
3. 영역 별 출력 내용 정리
4. 메시지 구조 정리
5. 영역 별 내용에 맞게 구역 나누기
6. 메시지 구조에 맞게 UI 작업하기