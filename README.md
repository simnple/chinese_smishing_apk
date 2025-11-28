# Chinese smishing analyze
피싱 주소: https://nhde[.]hair/

## 동작 설명
aos를 타겟으로 한 공격으로 정부 기관을 사칭해 악성 앱을 다운받게 합니다.

설치된 앱은 드로퍼로 또 다른 서버를 거쳐 악성 앱을 다운합니다.

이후 휴대전화의 SMS 관련 정보를 공격자의 서버에 전송하고 c2를 통해 피해자의 핸드폰으로부터 스팸 메시지를 전송해 2차 피해를 발생하도록 유도합니다.

## 공격자 서버
- 115.91.26[.]76
- 115.91.26[.]70
- 27.124.8[.]135 (APK 다운로드)
- 173.0.54[.]164 (닫힌 서버)

## Apk
- [envkrss.apk (dropper)](https://github.com/simnple/chinese_fishing_apk/blob/main/envkrss_apk.md)
- [SmartApp.apk (sms stealer)](https://github.com/simnple/chinese_fishing_apk/blob/main/SmartApp_apk.md)

## Reference
- [SMS Stealer 악성코드 분석 — 다락방](https://simnple.tistory.com/21)
