---
title: インタラクティブなデータ収集
description: Adobe Experience Platform Edge Networkサーバー API がインタラクティブなデータ収集を実行する方法について説明します。
exl-id: 1b06e755-b6a9-42dd-96c1-98ad67e7d222
source-git-commit: f8434746c4a023ec895d23a59e04fca4baecfc36
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 15%

---

# インタラクティブなデータ収集

## 概要 {#overview}

インタラクティブデータ収集エンドポイントは、1 つのイベントを受け取り、クライアントがAdobe Experience Platform Edge Networkサーバーから返される応答を必要とする場合に使用されます。 また、これらのエンドポイントは、データ収集を実行しながら、他のEdge Networkサービスからコンテンツを返すこともできます。

>[!IMPORTANT]
>
>この `/interact` エンドポイントは、主にExperience Platform SDK で使用するように設計されています。 このエンドポイントは追加的な変更が行われる可能性があり、その動作は予告なく進化する可能性があります。 例えば、今後、新しい項目が応答ペイロードに追加される可能性があります。

サーバー応答には、1 つ以上の応答が含まれます `Handle` オブジェクト（下図を参照）。

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
| `requestId` | `String` | × | 内部サーバーリクエストを関連付けるためのクライアントランダム ID を指定します。 何も指定されない場合、Edge Network によって生成され、応答で返されます。 |

### 応答 {#response}

応答が成功すると、HTTP ステータスが返されます `200 OK`、1 つ以上 `Handle` データストリーム設定で有効になっているリアルタイムエッジサービスに応じたオブジェクト。

```json
{
   "requestId": "c85402f5-83a1-4fb3-abdd-f4c17bf6dd49",
   "handle": []
}
```
