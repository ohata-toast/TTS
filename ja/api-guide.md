## AI Service > Text to Speech > APIガイド

### 音声合成API

#### リクエスト

* TTS APIを使用するにはAppkeyまたはプロジェクト統合Appkeyが必要です。<br/>
  Appkeyは、NHN Cloudの各サービスごとに発行される固有の認証キーであり、プロジェクト統合Appkeyは、NHN Cloudの1つのプロジェクト内の複数のサービスに対して共通で使用できる認証キーです。<br/>
  Appkeyの確認及び使用に関する詳細は、[Appkey](/nhncloud/ja/public-api/appkey)を参照してください。プロジェクト統合Appkeyの作成及び使用に関する詳細は、[プロジェクト統合Appkey](/nhncloud/ja/public-api/project-integrated-appkey)を参照してください。


[URI]

| メソッド | URI |
|---|---|
| POST | https://speech.api.nhncloudservice.com/v1.0/appkeys/{appKey}/tts |

[リクエストヘッダ]

| 名前 | 値 | 説明 |
|---|---|---|
| Authorization | {secretKey} | コンソールで発行されたセキュリティキー |
| Content-Type | application/json | |

[リクエスト本文]
```
{
    "inputText": "入力テキスト",
    "fileType": "WAV",
    "speaker": "MALE_A",
    "speed": 1,
    "samplingRate": 22050
}
```

[フィールド]

| 名前           | タイプ     | 必須かどうか | デフォルト値   | 有効範囲                     | 説明                                                                    |
|--------------|---------|--------|----------|--------------------------|-----------------------------------------------------------------------|
| inputText    | String  | 必須     |          | 最大150文字                  | 入力テキスト                                                                |
| fileType     | String  | 任意     | MP3      | MP3/WAV                  | ファイル形式(.mp3, .wav, .flac, .ogg, .alaw, .ulaw))                        |
| speaker      | String  | 任意     | FEMALE_A | MALE_A/FEMALE_A/FEMALE_B | 音声の種類(男性、女性、女性2)                                                      |
| speed        | Float   | 任意     | 1        | 0.5~2                    | 速度                                                                    |
| samplingRate | Long    | 선택     | 22050    | 8000~44100               | 음성 파일의 샘플링레이트(16000Hz, 22050Hz 등). alaw, ulaw 타입의 경우엔 8000으로 고정되어야합니다 |

#### レスポンス

[成功レスポンス]
* HTTP Status Code: 200
* Content-Type: audio/wav, audio/mpeg, audio/flac, audio/ogg, audio/x-alaw-basic(alaw), audio/basic(ulaw)
* Body: byte[]

[失敗レスポンス]
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
