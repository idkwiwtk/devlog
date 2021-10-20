# Log Content

## 콘텐츠 내용
1. 공구, 작업량 초기화
2. 공구 사용량, 작업량 증가
3. 기기별 모듈 에러 (SD Card, RTC, WiFi connection)

## 상세 내용
메인 주제에 맞게 영역을 나누고 각각의 영역에서 STC 별로 내용을 표시
### 공구 초기화
- v-list-item으로 표현함
- 별도의 리스트에 공구 초기화 시간 정보를 기록함
### 공구 초기화 시간 정보 리스트
- STC ID, 가장 최근에 초기화 된 공구를 표시
```js
[
  {
    stcId: '',
    lastResetToolOrder: '',
    lastResetToolTime: '',
  },...
]
```
### 작업량 초기화
- v-list-item으로 표현함
- 별도의 리스트에 작업량 초기화 시간 정보를 기록함
### 작업량 초기화 시간 정보 리스트
- STC ID, 가장 최근에 초기화 된 작업량 정보 표시
```js
[
  {
    stcId: '',
    lastResetWorkcount: '',
    lastResetWorkcountTime: '',
  },...
]