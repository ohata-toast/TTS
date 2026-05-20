## AI Service > Text to Speech > API 가이드

### 음성 합성 API

#### 요청

- {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인이 가능합니다.

[URI]

| 메서드 | URI |
|---|---|
| POST | https://speech.api.nhncloudservice.com/v1.0/appkeys/{appKey}/tts |

[요청 헤더]

| 이름 | 값 | 설명 |
|---|---|---|
| Authorization | {secretKey} | 콘솔에서 발급받은 보안 키 |
| Content-Type | application/json | |

[요청 본문]
```
{
    "inputText": "입력 텍스트",
    "fileType": "WAV",
    "speaker": "MALE_A",
    "speed": 1,
    "samplingRate": 22050
}
```

[필드]

| 이름           | 타입      | 필수 여부 | 기본값      | 유효 범위                      | 설명                                                                    |
|--------------|---------|-------|----------|----------------------------|-----------------------------------------------------------------------|
| inputText    | String  | 필수    |          | 최대 150자                    | 입력 텍스트                                                                |
| fileType     | String  | 선택    | MP3      | MP3/WAV/FLAC/OGG/ALAW/ULAW | 파일 형식(.mp3, .wav, .flac, .ogg, .alaw, .ulaw)                          |
| speaker      | String  | 선택    | FEMALE_A | MALE_A/FEMALE_A/FEMALE_B   | 음성 종류(남성, 여성, 여성2)                                                    |
| speed        | Float   | 선택    | 1        | 0.5~2                      | 속도                                                                    |
| samplingRate | Long    | 선택    | 22050    | 8000~44100                 | 음성 파일의 샘플링레이트(16000Hz, 22050Hz 등). alaw, ulaw 타입의 경우엔 8000으로 고정되어야합니다 |

#### 응답

[성공 응답]
* HTTP Status Code: 200
* Content-Type: audio/wav, audio/mpeg, audio/flac, audio/ogg, audio/x-alaw-basic(alaw), audio/basic(ulaw)
* Body: byte[]

[실패 응답]
* Content-Type: application/json
```
{
    "header": {
        "isSuccessful": false,
        "resultCode": 400,
        "resultMessage": "Bad Request"
    },
    "errorList": [
        {
            "resultCode": 4000001,
            "resultTitle": "Invalid parameter.  (fileType)",
            "resultMessage": "must be one of [MP3, FLAC, ULAW, WAV, OGG, ALAW]"
        },
        {
            "resultCode": 4000001,
            "resultTitle": "Invalid parameter.  (speed)",
            "resultMessage": "must be less than or equal to 2"
        }
    ]
}
```
