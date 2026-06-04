## AI Service > Text to Speech > API 가이드

### 음성 합성 API

#### 요청

Text to Speech(TTS) API를 사용하려면 Appkey 또는 프로젝트 통합 Appkey가 필요합니다.<br/>
Appkey는 NHN Cloud의 각 서비스별로 발급되는 고유 인증 키이며, 프로젝트 통합 Appkey는 NHN Cloud에서 하나의 프로젝트 내 여러 서비스에 대해 공통으로 사용할 수 있는 인증 키입니다.<br/>
Appkey 확인 및 사용에 대한 자세한 내용은 [Appkey](/nhncloud/ko/public-api/appkey)를 참고하세요. 프로젝트 통합 Appkey 생성 및 사용에 대한 자세한 내용은 [프로젝트 통합 Appkey](/nhncloud/ko/public-api/project-integrated-appkey)를 참고하세요.


[URI]

| 메서드  | URI                                                              |
|------|------------------------------------------------------------------|
| POST | https://speech.api.nhncloudservice.com/v1.0/appkeys/{appKey}/tts |

[요청 헤더]

| 이름            | 값                | 설명             |
|---------------|------------------|----------------|
| Authorization | {secretKey}      | 콘솔에서 발급받은 보안 키 |
| Content-Type  | application/json |                |

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
| samplingRate | Long    | 선택    | 22050    | 8000~44100                 | 음성 파일의 샘플링 레이트(16000Hz, 22050Hz 등). alaw, ulaw 타입의 경우에는 8000으로 고정되어야 합니다. |

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
