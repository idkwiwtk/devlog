# 목적
- STC의 상태 정보 공유

# 구현
- ESP32-STC에서 일정 주기마다 MEGA상태[./211018MEGASTATE]를 읽어온다.
- MEGA에서 사용자 입력이나 시스템 이벤트 발생시 해당 이벤트 정보[./211018SystemEvent]를 보낸다.
- MQTT topic state/{stc id}로 메시지를 발행한다.

# STCWMS와 STC간 모듈 상태 정보 동기화 방법
- STCWMS에서 일정 주기마다 request/{stc_id}로 mqtt 메시지를 발행한다.
- ESP32-STC에서 request/{stc_id}를 통해 STCWMS의 상태 정보 요청 메시지를 수신한다.
- ESP32-STC에서 MEGA로 ESP32_REQUEST_STATE 시리얼 요청을 보낸다.
- MEGA에서 시리얼 통신으로 ESP32_REQUEST_STATE 메시지를 받으면 RTC 모듈과 SD CARD 모듈의 상태 정보를 MEGA_SEND_SYSTEM_STATE로 시리얼통신을 통해 해당 모듈의 정보를 보낸다.
- ESP32-STC에서 시리얼 통신으로 MEGA_SEND_SYSTEM_STATE로 모듈의 정보를 받으면 정보를 해석하고 MQTT 메시지를 생성한다.
- ESP32-STC에서 state MQTT 토픽으로 모듈 상태 메시지를 발생한다.
-  

# struct type system_info_t  
```c
// 메인 클래스가 관리함
// 시스템의 정보를 저장 하는 객체
typedef struct _system_info_t {
    // system info state
    SYSTEM_INFO_STATE systemInfoState;
    // STC id
    char stcId[101];
    // NET config
    net_config_t netConfig;
    // system time
    char time[21];
    // system sd state
    SD_STATE sdState;

    // system rtc state
    RTC_STATE rtcState;

    // system esp32 state
    ESP_STATE esp32State;
    ESP_NET_STATE esp32NetState;
    char rssi; // wifi signal level

    // system tool state n life
    TOOL_STATE toolState[17];

    // system counter state
    COUNTER_STATE counterState;

    // system counter info
    system_counter_t systemCounter;

    // 상태창의 툴, 상태 툴력 모드 변경
    bool isDisplayModeChange;
    bool isSwitchToolState;

    STC3_QUEUE queue;
} system_info_t;

```

