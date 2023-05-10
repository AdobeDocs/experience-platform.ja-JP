---
title: 非インタラクティブデータ収集
description: Adobe Experience Platform Edge Network Server API による非インタラクティブデータ収集の仕組みについて説明します。
exl-id: 1a704e8f-8900-4f56-a843-9550007088fe
source-git-commit: f52603f7e65ac553e00a2b632857561cd07ae441
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 5%

---

# 非インタラクティブデータ収集

## 概要 {#overview}

非インタラクティブなイベントデータ収集エンドポイントは、複数のイベントをExperience Platformデータセットや他のアウトレットに送信するために使用されます。

エンドユーザーのイベントが短時間ローカルのキューに入れられる場合（ネットワークに接続されていない場合など）は、イベントをバッチで送信することをお勧めします。

バッチイベントは、必ずしも同じエンドユーザーに属しているわけではありません。つまり、イベントは、イベント内で異なる ID を保持できます `identityMap` オブジェクト。

## 非インタラクティブ API 呼び出しの例 {#example}

### API 形式 {#api-format}

```http
POST /ee/v2/collect
```

### リクエスト {#request}

```shell
curl -X POST "https://server.adobedc.net/ee/v2/collect?dataStreamId={DATASTREAM_ID}" 
-H "Authorization: Bearer {TOKEN}" 
-H "x-gw-ims-org-id: {ORG_ID}" 
-H "x-api-key: {API_KEY}" 
-H "Content-Type: application/json" 
-d '{
   "events": [
      {
         "xdm": {
            "identityMap": {
               "FPID": [
                  {
                     "id": "79bf8e83-f708-414b-b1ed-5789ff33bf0b",
                     "primary": "true"
                  }
               ]
            },
            "eventType": "web.webpagedetails.pageViews",
            "web": {
               "webPageDetails": {
                  "URL": "https://alloystore.dev/",
                  "name": "home-demo-Home Page"
               }
            },
            "timestamp": "2021-08-09T14:09:20.859Z"
         },
         "data": {
            "prop1": "custom value"
         }
      },
      {
         "xdm": {
            "identityMap": {
               "FPID": [
                  {
                     "id": "871e8460-a329-4e96-a5b6-ff359fb0afb9",
                     "primary": "true"
                  }
               ]
            },
            "eventType": "web.webinteraction.linkClicks",
            "web": {
               "webInteraction": {
                  "linkClicks": {
                     "value": 1
                  }
               },
               "name": "My Custom Link",
               "URL": "https://myurl.com"
            },
            "timestamp": "2021-08-09T14:09:20.859Z"
         }
      }
   ]
}'
```

| パラメーター | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| `dataStreamId` | `String` | ○ | データ収集エンドポイントで使用されるデータストリームの ID。 |
| `requestId` | `String` | いいえ | 外部リクエストトレース ID を指定します。 何も指定されない場合、Edge ネットワークはユーザーに代わって 1 つを生成し、応答の本文/ヘッダーに返します。 |
| `silent` | `Boolean` | いいえ | Edge ネットワークが `204 No Content` 空のペイロードを含む応答、または含まない応答。 重大なエラーは、対応する HTTP ステータスコードとペイロードを使用して報告されます。 |


### 応答 {#response}

正常な応答は、次のステータスのいずれかと、 `requestID` リクエストで何も指定されていない場合。

* `202 Accepted` リクエストが正常に処理されたとき。
* `204 No Content` リクエストが正常に処理され、 `silent` パラメータがに設定されました `true`;
* `400 Bad Request` リクエストの形式が正しくなかった場合（必須のプライマリ id が見つからなかった場合など）。

```json
{
  "requestId": "f567a988-4b3c-45a6-9ed8-f283188a445e"
}
```
