---
title: FPID による訪問者の識別
description: FPID を使用して、Server API を通じて訪問者を一貫して識別する方法を学ぶ
seo-description: Learn how to consistently identify visitors via the Server API, by using the FPID
keywords: エッジネットワーク；ゲートウェイ；API；訪問者；識別；FPID
exl-id: c61d2e7c-7b5e-4b14-bd52-13dde34e32e3
source-git-commit: 946959d450cddd95fc6ee3775686bd6b4156c8c2
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# FPID による訪問者の識別

## 概要

[!DNL First-party IDs] (`FPIDs`) は、顧客が生成、管理、保存するデバイス ID です。 これにより、ユーザーデバイスの識別をユーザーが制御できるようになります。 送信別 `FPIDs`Edge Network が新しい `ECID` リクエストに含まれていない。

この `FPID` は、 `identityMap` または、cookie として送信できます。

An `FPID` 決定論的に～に変換することができる `ECID` を Edge ネットワークで設定し、 `FPID` id は、Experience Cloudソリューションと完全に互換性があります。 の取得 `ECID` 特定の `FPID` は常に同じ結果を得るので、ユーザーは一貫したエクスペリエンスを得ることができます。

この `ECID` この方法は、 `identity.fetch` クエリ：

```json
{
   "query":{
      "identity":{
         "fetch":[
            "ECID"
         ]
      }
   }
}
```

リクエストに `FPID` および `ECID`、 `ECID` リクエストに既に存在するが、 `FPID`. したがって、Edge ネットワークは `ECID` 既に提供されており、指定された `FPID`.

デバイス ID の場合、 `server` データストリームは、 `FPID` をデバイス ID として設定します。 その他の ID( 例： `EMAIL`) は、リクエスト本文内でも指定できますが、Edge ネットワークでは、プライマリ ID が明示的に指定されている必要があります。 プライマリID は、プロファイルデータの格納先となるベース ID です。

>[!NOTE]
>
>ID を持たないリクエスト（リクエスト本文内で明示的に設定されたプライマリ ID を持たないリクエスト）は、失敗します。

以下 `identityMap` フィールドグループが正しく形成されている `server` データストリームリクエスト：

```json
{
   "identityMap":{
      "FPID":[
         {
            "id":"123e4567-e89b-12d3-a456-426614174000",
            "authenticatedState":"ambiguous",
            "primary":true
         }
      ],
      "EMAIL":[
         {
            "id":"email@mail.com",
            "authenticatedState":"authenticated"
         }
      ]
   }
}
```

以下 `identityMap` フィールドグループを `server` データストリームリクエスト：

```json
{
   "identityMap":{
      "FPID":[
         {
            "id":"123e4567-e89b-12d3-a456-426614174000",
            "authenticatedState":"ambiguous"
         }
      ],
      "EMAIL":[
         {
            "id":"email@mail.com",
            "authenticatedState":"authenticated"
         }
      ]
   }
}
```

この場合、Edge ネットワークから返されるエラー応答は、次のようになります。

```json
{
   "type":"https://ns.adobe.com/aep/errors/EXEG-0306-400",
   "status":400,
   "title":"No primary identity set in request (event)",
   "detail":"No primary identity found in the input event. Update the request accordingly to your schema and try again.",
   "report":{
      "requestId":"{REQUEST_ID}",
      "configId":"{CONFIG_ID}",
      "orgId":"{ORG_ID}"
   }
}
```

## を使用した訪問者の識別 `FPID`

を介してユーザーを識別するには `FPID`で、 `FPID` Edge ネットワークにリクエストを送信する前に、Cookie が送信されていること。 この `FPID` は、cookie で渡すことも、 `identityMap` をリクエストの本文に追加します。

<!--

## Request with `FPID` passed as cookie header

```shell
curl -X POST 'https://edge.adobedc.net/v2/interact?dataStreamId={Data Stream ID}' \
-H 'cookie: FPID=e98f38e6-6183-442d-8cd2-0e384f4c8aa8' \
-H 'Content-Type: application/json' \
-d '{
    "event": 
        {
            "xdm": {
                "web": {
                    "webPageDetails": {
                        "URL": "https://alloystore.dev"
                    },
                    "webReferrer": {
                        "URL": ""
                    }
                },
                "device": {
                    "screenHeight": 1440,
                    "screenWidth": 3440,
                    "screenOrientation": "landscape"
                },
                "environment": {
                    "type": "browser",
                    "browserDetails": {
                        "viewportWidth": 1907,
                        "viewportHeight": 545
                    }
                },
                "placeContext": {
                    "localTime": "2022-03-21T21:32:59.991-06:00",
                    "localTimezoneOffset": 360
                },
                "timestamp": "2022-03-22T03:32:59.992Z",
                "implementationDetails": {
                    "name": "https://ns.adobe.com/experience/alloy/reactor",
                    "version": "1.0",
                    "environment": "serverapi"
                }
            }
        },
    "query": {
        "identity": {
            "fetch": [
                "ECID"
            ]
        }
    },
    "meta":
        {
            "state":
            {
                "domain": "alloystore.dev",
                "cookiesEnabled": true
            }
        }
}'
```
-->

## 次を使用してリクエスト `FPID` に渡される `identityMap` フィールド

以下の例では、 [!DNL FPID] as a `identityMap` パラメーター。

```shell
curl -X POST "https://server.adobedc.net/v2/interact?dataStreamId={DATASTREAM_ID}"
-H "Authorization: Bearer {TOKEN}"
-H "x-gw-ims-org-id: {ORG_ID}"
-H "x-api-key: {API_KEY}"
-H "Content-Type: application/json"
-d '{
   "event": {
      "xdm": {
         "identityMap": {
            "FPID": [
               {
                  "id": "e98f38e6-6183-442d-8cd2-0e384f4c8aa8",
                  "authenticatedState": "ambiguous",
                  "primary": true
               }
            ]
         },
         "web": {
            "webPageDetails": {
               "URL": "https://alloystore.dev"
            },
            "webReferrer": {
               "URL": ""
            }
         },
         "device": {
            "screenHeight": 1440,
            "screenWidth": 3440,
            "screenOrientation": "landscape"
         },
         "environment": {
            "type": "browser",
            "browserDetails": {
               "viewportWidth": 1907,
               "viewportHeight": 545
            }
         },
         "placeContext": {
            "localTime": "2022-03-21T21:32:59.991-06:00",
            "localTimezoneOffset": 360
         },
         "timestamp": "2022-03-22T03:32:59.992Z",
         "implementationDetails": {
            "name": "https://ns.adobe.com/experience/alloy/reactor",
            "version": "1.0",
            "environment": "serverapi"
         }
      }
   },
   "query": {
      "identity": {
         "fetch": [
            "ECID"
         ]
      }
   }
}'
```
