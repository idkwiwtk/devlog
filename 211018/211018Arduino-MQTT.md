# Arduino-MQTT 클라이언트 라이브러리
QoS level 2를 지원한다.

# ESP32-STC 라이브러리 변경 작업 내용
PubSubClient.h는 QoS Level2를 지원하지않아 이를 지원하는 라이브러리인 Arduino-MQTT 라이브러리로 변경하였다.
기존의 메소드와 유사한 점이 많지만 Init 함수와 callback 함수의 매개변수 타입이 달라서 이를 변경해야 했다.

## 변경 내용 상세
### mqtt client begin
```
  // mqtt_client.setServer(mqtt_server, 1883);
  // mqtt_client.setCallback(callback);
  mqtt_client.begin(mqtt_server, esp_wifi_client);
  mqtt_client.onMessage(callback);
``` 
기존의 PubSubClient의 setServer, setCallback 함수를 사용하지 않고 being, onMessage로 변경돔

### mqtt client callback
```
// static void callback(char *topic, byte *message, unsigned int length)
static void callback(String &p_topic, String &p_message)
{
    char *topic = (char *)p_topic.c_str();
    byte *message = (byte *)p_message.c_str();
    unsigned int length = strlen((const char *)message);

    char *pMsg = (char *)malloc((sizeof(char) * length) + 1);
```
String 타입의 매개변수를 char *형으로 변환하여 사용


## 추가내용
### 버그 수정 용 코드
```
void MqttMaintain()
{
    if (!mqtt_client.connected())
    {
        MqttReconnect();
    }
    mqtt_client.loop();
    delay(10); // <- fixes some issues with WiFi stability
}
```
WiFi 연결 유지를 위해 delay(10) 추가