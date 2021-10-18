# 목적
- STC의 상태 정보 공유

# 구현
- ESP32-STC에서 일정 주기마다 MEGA상태[./211018MEGASTATE]를 읽어온다.
- MEGA에서 사용자 입력이나 시스템 이벤트 발생시 해당 이벤트 정보[./211018SystemEvent]를 보낸다.
- MQTT topic state/{stc id}로 메시지를 발행한다.
