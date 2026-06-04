## AI Service > Text to Speech > API Guide

### Speech Synthesis API

#### Request

An AppKey or a Project Integrated Appkey is required to use the TTS API.<br/>
An AppKey is a unique authentication key issued for each individual NHN Cloud service, while a Project Integrated Appkey is a common authentication key that can be shared across multiple services within a single NHN Cloud project.<br/>
For more information on checking and using Appkeys, please refer to the [Appkey](/nhncloud/en/public-api/appkey). For more information on creating and using Project Integrated Appkeys, please refer to the [Project Integrated Appkey](/nhncloud/en/public-api/project-integrated-appkey).

[URI]

| Method  | URI                                                              |
|---------|------------------------------------------------------------------|
| POST    | https://speech.api.nhncloudservice.com/v1.0/appkeys/{appKey}/tts |

[Request Header]

| Name          | Value            | Description                          |
|---------------|------------------|--------------------------------------|
| Authorization | {secretKey}      | Security key issued from the console |
| Content-Type  | application/json |                                      |

[Request Body]
```
{
    "inputText": "input text",
    "fileType": "WAV",
    "speaker": "MALE_A",
    "speed": 1,
    "samplingRate": 22050
}
```

[Field]

| Name         | Type   | Required | Default value | Valid range                | Description                                                               |
|--------------|--------|----------|---------------|----------------------------|---------------------------------------------------------------------------|
| inputText    | String | Required |               | Up to 150 characters       | Input text                                                                |
| fileType     | String | Optional | MP3           | MP3/WAV/FLAC/OGG/ALAW/ULAW | File format(.mp3, .wav, .flac, .ogg, .alaw, .ulaw)                        |
| speaker      | String | Optional | FEMALE        | MALE_A/FEMALE_A/FEMALE_B   | Voice type(Male, Female, Female2)                                         |
| speed        | Float  | Optional | 1             | 0.5~2                      | Speed                                                                     |
| samplingRate | Long   | 선택       | 22050         | 8000~44100                 | 음성 파일의 샘플링 레이트(16000Hz, 22050Hz 등). alaw, ulaw 타입의 경우에는 8000으로 고정되어야 합니다. |

#### Response

[Success Response]
* HTTP Status Code: 200
* Content-Type: audio/wav, audio/mpeg, audio/flac, audio/ogg, audio/x-alaw-basic(alaw), audio/basic(ulaw)
* Body: byte[]

[Failure Response]
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
