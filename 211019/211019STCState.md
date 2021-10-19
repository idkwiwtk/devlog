# STC State
STC의 상태 정보를 mqtt 메시지를 통해 받아 STCWMS의 STC 상태 현황 UI에 현재 상태를 반영한다.

# STCWMS 변수 정의
```js
stcStateList = [
  {
    stcId: '',
    // STC의 모듈 상태 정보 표시
    // state code 테이블 값 참고
    errorFlag: {
      sdCard: undefined,
      RTC: undefined,
    },
    // 기본 값 : false
    // 10초 주기로 mqtt heartbeat 메시지 수신 후 true로 변경
    isConnecting: false,

    // # todo
    isNeedBackup: false,
  },
]
```

# STC UI 스타일 정의
- errorFlag.sdCard
  - true : sd error icon 
  - false : sd working icon
- errorFlag.RTC
  - true : RTC error icon 
  - false : RTC working icon
- isConnecting
  - true : WiFi error icon 
  - false : WiFi working icon

# STC State List 생성
App.vue mounted() 에서 실행함
1. stc/LoadStcList 실행
2. stc/InitStcStateList 실행

# STC State List 값 변경을 위한 함수 정의
