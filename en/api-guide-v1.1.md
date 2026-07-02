## AI Service > Text to Speech > API Guide

### Speech Synthesis API

#### Request

TTS API uses User Access Key tokens for authentication and authorization. The User Access Key token is a temporary, Bearer-type access token issued from a User Access Key. For more information on issuing and using User Access Key tokens, see the [User Access Key Token](/nhncloud/en/public-api/user-access-key-token).

[URI]

| Method | URI                                                              |
|--------|------------------------------------------------------------------|
| POST   | https://api-speech.nhncloudservice.com/v1.1/appkeys/{appKey}/tts |

[Request Header]

| Name                | Value                          | Description         |
|---------------------|--------------------------------|---------------------|
| X-NHN-authorization | Bearer {User Access Key Token} | User Access Key token |
| Content-type        | application/json               |                     |

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
| inputText    | String | Required |          | Up to 150 characters       | Input text                                                                                                          |
| fileType     | String | Optional | MP3      | MP3/WAV/FLAC/OGG/ALAW/ULAW | File format (.mp3, .wav, .flac, .ogg, .alaw, .ulaw)                                                                 |
| speaker      | String | Optional | FEMALE_A | MALE_A/FEMALE_A/FEMALE_B   | Voice type (Male, female, female2)                                                                                  |
| speed        | Float  | Optional | 1        | 0.5-2                      | Speed                                                                                                               |
| samplingRate | Long   | Optional | 2,2050    | 8,000-44,100                 | Sampling rate of the audio file (e.g., 16,000 Hz, 22,050 Hz). For alaw and ulaw types, this must be fixed at 8,000. |

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
