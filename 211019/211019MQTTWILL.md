# MQTT client WILL topic / WILL message
MQTT 클라이언트가 동작하는 환경은 대부분 네트워크가 열악한 경우가 많다.
그렇기에 MQTT 브로커는 클라이언트의 네트워크 끊김을 감지하고 이를 다른 구독자 에게 알리는 기능을 가지고 있다.
이를 WILL topic / message라고 한다.

# 동작과정
1. 클라이언트와 브로커가 최초 연결시 클라이언트는 WILL TOPIC, MESSAGE를 브로커에게 알린다.
2. 브로커는 이를 저장하고 있다가 클라이언트로부터 연결 해체 메시지를 받지 않았는데 연결 끊김을 감지하면 WILL 메시지를 구독자에게 발행한다.
3. 이때 WILL TOPIC은 다른 구독자도 별도로 구독을 해야 한다.

# TEST
## 개발 환경
- MQTT Broker : mosquitto
- MQTT client 라이브러리 : [Arduino-MQTT](https://github.com/256dpi/arduino-mqtt)
- MQTT client 동작 기기 : ESP32-dev module

## WIll topic / message 설정 방법
- mqtt_client.setWill(MQTT_PUB_TOPIC_WILL, msg, true, 2); 호출

## 주의 사항
- 반드시 mqtt_client.begin() 함수 호출 전에 setWill 함수를 호출애햐 함
