---
title: インタラクティブデータ収集
description: Adobe Experience Platform Edge Network Server API によるインタラクティブなデータ収集の仕組みについて説明します。
exl-id: 1b06e755-b6a9-42dd-96c1-98ad67e7d222
source-git-commit: 3272db15283d427eb4741708dffeb8141f61d5ff
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 19%

---

# インタラクティブデータ収集

## 概要 {#overview}

インタラクティブデータ収集エンドポイントは、単一のイベントを受け取り、クライアントがAdobe Experience Platform Edge Network サーバーによって応答が返されると想定する場合に使用されます。 また、これらのエンドポイントは、データ収集を実行中に、他の Edge ネットワークサービスからコンテンツを返すこともできます。

サーバー応答に 1 つ以上の `Handle` オブジェクトを作成します。

## API 呼び出しの例

### API 形式 {#format}

```http
POST /ee/v2/interact
```

### リクエスト {#request}

```shell
curl -X POST "https://server.adobedc.net/ee/v2/interact?dataStreamId={DATASTREAM_ID}" 
-H "Authorization: Bearer {TOKEN}" 
-H "x-gw-ims-org-id: {ORG_ID}" 
-H "x-api-key: {API_KEY}" 
-H "Content-Type: application/json" 
-d '{
   "event": {
      "xdm": {
         "identityMap": {
            "Email_LC_SHA256": [
               {
                  "id": "0c7e6a405862e402eb76a70f8a26fc732d07c32931e9fae9ab1582911d2e8a3b",
                  "primary": true
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
   }
}'
```

| パラメーター | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| `dataStreamId` | `String` | はい。 | データストリーム ID。 |
| `requestId` | `String` | × | 内部サーバーリクエストを関連付けるためのクライアントのランダム ID を指定します。 何も指定されない場合、Edge Network によって生成され、応答で返されます。 |

### 応答 {#response}

正常な応答は HTTP ステータスを返します `200 OK`（1 つ以上） `Handle` データストリーム設定で有効なリアルタイムエッジサービスに応じたオブジェクト。

```json
{
   "requestId": "c85402f5-83a1-4fb3-abdd-f4c17bf6dd49",
   "handle": []
}
```
