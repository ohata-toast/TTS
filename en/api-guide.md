## AI Service > Text to Speech > API Guide

### Speech Synthesis API

#### Request

Appkey or project integrated Appkey is required to use Text to Speech (TTS) API.<br/>
An Appkey is a unique authentication key issued for each NHN Cloud service. The project-integrated Appkey is an authentication key that can be commonly used across multiple services within a single project in NHN Cloud.<br/>
For more information on checking and using an Appkey, see [Appkey](/nhncloud/en/public-api/appkey). For more information on creating and using a project-integrated Appkey, see [Project-Integrated Appkey](/nhncloud/en/public-api/project-integrated-appkey).


[URI]

| Method | URI                                                              |
|--------|------------------------------------------------------------------|
| POST | https://api-speech.nhncloudservice.com/v1.0/appkeys/{appKey}/tts |

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

| Name         | Type   | Required | Default  | Valid range                | Description                                                                                                         |
|--------------|--------|----------|----------|----------------------------|---------------------------------------------------------------------------------------------------------------------|
| inputText    | String | Required |          | up to 150 characters       | Input text                                                                                                          |
| fileType     | String | Optional | MP3      | MP3/WAV/FLAC/OGG/ALAW/ULAW | File format (.mp3, .wav, .flac, .ogg, .alaw, .ulaw)                                                                 |
| speaker      | String | Optional | FEMALE_A | MALE_A/FEMALE_A/FEMALE_B   | Voice type (Male, female, female2)                                                                                  |
| speed        | Float  | Optional | 1        | 0.5~2                      | Speed                                                                                                               |
| samplingRate | Long   | Optional | 22050    | 8000~44100                 | Sampling rate of the audio file (e.g., 16,000 Hz, 22,050 Hz). For alaw and ulaw types, this must be fixed at 8,000. |

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
