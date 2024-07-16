---
title: 非インタラクティブデータ収集
description: Adobe Experience Platform Edge Networkサーバー API が非インタラクティブ型のデータ収集を実行する方法を説明します。
exl-id: 1a704e8f-8900-4f56-a843-9550007088fe
source-git-commit: 3bf13c3f5ac0506ac88effc56ff68758deb5f566
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 5%

---


# 非インタラクティブデータ収集

## 概要 {#overview}

非インタラクティブイベントデータ収集エンドポイントは、Experience Platformデータセットまたは他のコンセントに複数のイベントを送信するために使用されます。

エンドユーザーイベントが短時間ローカルにキューに入っている場合（例えば、ネットワーク接続がない場合）は、イベントをバッチで送信することをお勧めします。

バッチイベントは、必ずしも同じエンドユーザーに属してはなりません。つまり、イベントは `identityMap` ースオブジェクト内で異なる ID を保持できます。

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
| `requestId` | `String` | × | 外部リクエストトレース ID を指定します。 何も指定されない場合、Edge Networkによって生成され、応答本文/ ヘッダーに返されます。 |
| `silent` | `Boolean` | × | Edge Networkが空のペイロードを持つ `204 No Content` 応答を返す必要があるかどうかを示す、オプションのブール値パラメーター。 重大なエラーは、対応する HTTP ステータスコードとペイロードを使用して報告されます。 |

### 応答 {#response}

応答が成功すると、次のいずれかのステータスと、リクエストで何も指定されなかった場合の `requestID` が返されます。

* リク `202 Accepted` ストが正常に処理された日時。
* リクエストが正常に処理され、`silent` パラメーターが `true` に設定された `204 No Content`。
* リクエストの形式が正しくなかった場合（必須のプライマリ ID が見つからなかったなど）の `400 Bad Request`。

```json
{
  "requestId": "f567a988-4b3c-45a6-9ed8-f283188a445e"
}
```
