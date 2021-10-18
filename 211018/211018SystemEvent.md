# System Event 정의
ESP32-STC가 발행하는 mqtt topic별로 이벤트를 정의한다.

## countup
설비의 릴레이 입력으로 현재 사용 중인 모든 공구의 사용량을 1 증가 시킴
### 메시지 형태
```c
  sprintf(msg, "{\"stc-id\":\"%s\", \"com-code\":\"TEST\"}", mqtt_id);
```

## backup 
설비의 카운터 정보를 전송한다.
### 메시지 형태
```c
  sprintf(msg,
          "{\"stc-id\":\"%s\", \"com-code\":\"TEST\", \"toolList\":[%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu], \"toolLimit\":[%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu,%lu],\"workcount\":%lu}",
          mqtt_id,

          pSystemCounter->toolSet[1].toolUsaged, pSystemCounter->toolSet[2].toolUsaged, pSystemCounter->toolSet[3].toolUsaged, pSystemCounter->toolSet[4].toolUsaged, pSystemCounter->toolSet[5].toolUsaged,
          pSystemCounter->toolSet[6].toolUsaged, pSystemCounter->toolSet[7].toolUsaged, pSystemCounter->toolSet[8].toolUsaged, pSystemCounter->toolSet[9].toolUsaged, pSystemCounter->toolSet[10].toolUsaged,
          pSystemCounter->toolSet[11].toolUsaged, pSystemCounter->toolSet[12].toolUsaged, pSystemCounter->toolSet[13].toolUsaged, pSystemCounter->toolSet[14].toolUsaged, pSystemCounter->toolSet[15].toolUsaged,

          pSystemCounter->toolSet[1].toolLimit, pSystemCounter->toolSet[2].toolLimit, pSystemCounter->toolSet[3].toolLimit, pSystemCounter->toolSet[4].toolLimit, pSystemCounter->toolSet[5].toolLimit,
          pSystemCounter->toolSet[6].toolLimit, pSystemCounter->toolSet[7].toolLimit, pSystemCounter->toolSet[8].toolLimit, pSystemCounter->toolSet[9].toolLimit, pSystemCounter->toolSet[10].toolLimit,
          pSystemCounter->toolSet[11].toolLimit, pSystemCounter->toolSet[12].toolLimit, pSystemCounter->toolSet[13].toolLimit, pSystemCounter->toolSet[14].toolLimit, pSystemCounter->toolSet[15].toolLimit,

          pSystemCounter->workCount);
```

## state 
STC의 상태를 전송한다.
### 메시지 형태
```c
  switch (statecode)
  {
  case stc_workcount_init:
  case stc_countup_tool_workcount:
  case stc_countup_tool_without_workcount:
      sprintf(
          msg,
          "{\"stc-id\":\"%s\", \"com-code\":\"test\", \"eventcode\":%u}",
          mqtt_id,
          statecode);
      break;

  case tool_init:
  case workcount_reset:
  case tool_backup:
      sprintf(
          msg,
          "{\"stc-id\":\"%s\", \"com-code\":\"test\",\"eventcode\":%u}",
          mqtt_id,
          statecode);
      break;
  case tool_over_limit:
  case tool_reset_usaged:
      sprintf(
          msg,
          "{\"stc-id\":\"%s\", \"com-code\":\"test\",\"eventcode\":%u, \"eventmsg\":%u}",
          mqtt_id,
          statecode,
          *(uint8_t *)p);
      break;

  case rtc_not_find:
      sprintf(
          msg,
          "{\"stc-id\":\"%s\", \"com-code\":\"test\",\"eventcode\":%u}",
          mqtt_id,
          statecode);
      break;
  case rtc_not_running:
      sprintf(
          msg,
          "{\"stc-id\":\"%s\", \"com-code\":\"test\",\"eventcode\":%u}",
          mqtt_id,
          statecode);
      break;

  case sd_module_disconnect:
  case sd_module_connet:
  case sd_card_disconnect:
  case fail_to_load_config_file:
  case fail_to_load_saved_file:
  case fail_to_open_config_file:
  case fail_to_open_saved_file:
  case fail_to_load_net_config_file:
      sprintf(
          msg,
          "{\"stc-id\":\"%s\", \"com-code\":\"test\",\"eventcode\":%u}",
          mqtt_id,
          statecode);
      break;

  case esp32_wifi_disconnect:
  case esp32_wifi_not_working:
  case esp32_wifi_working:
  case esp32_wifi_connect:
      sprintf(
          msg,
          "{\"stc-id\":\"%s\", \"com-code\":\"test\",\"eventcode\":%u}",
          mqtt_id,
          statecode);
      break;

  default:
      return;
  }
```

## setting/finish 
STC의 공구 수명 세팅을 위한 준비 여부 알림
setting : 0 => MEGA 세팅 준비 완료
setting : 1 => MEGA 세팅 정보 수신 완료 및 초기화 시작
### 메시지 형태
```c
  if (isSetting == true)
  {
      sprintf(msg, "{\"stc-id\":\"%s\", \"com-code\":\"TEST\", \"setting\":\"%d\"}", mqtt_id, 1);
      // sprintf(msg, "{\"stc-id\":\"%s\", \"com-code\":\"TEST\", \"setting\":\"%d\"}", stcId, 1);
  }
  else if (isSetting == false)
  {
      sprintf(msg, "{\"stc-id\":\"%s\", \"com-code\":\"TEST\", \"setting\":\"%d\"}", mqtt_id, 0);
      // sprintf(msg, "{\"stc-id\":\"%s\", \"com-code\":\"TEST\", \"setting\":\"%d\"}", stcId, 0);
  }
```

## reset/tool 
사용자가 공구 사용량을 초기화 할 때 이를 알림
해당 공구의 사용량은 초기화가 된다.
### 메시지 형태
```c
  sprintf(msg, "{\"stc-id\":\"%s\", \"com-code\":\"TEST\", \"toolNo\":\"%d\"}", mqtt_id, toolNo);
```

## reset/workcount 
사용자가 작업량을 초기화 할 때 이를 알림
### 메시지 형태
```c
  sprintf(msg, "{\"stc-id\":\"%s\", \"com-code\":\"TEST\"}", mqtt_id);
```

## countup/allTool 
작업량은 올리지 않고 공구 사용량만 올릴 때 사용
### 메시지 형태
```c
  sprintf(msg, "{\"stc-id\":\"%s\", \"com-code\":\"TEST\"}", mqtt_id);
```
