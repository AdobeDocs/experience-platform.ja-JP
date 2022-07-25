---
title: FPID による訪問者の識別
description: FPID を使用することにより、サーバー API を通じて一貫性を持って訪問者を識別する方法を説明します
seo-description: Learn how to consistently identify visitors via the Server API, by using the FPID
keywords: エッジネットワーク;ゲートウェイ;API;訪問者;識別;FPID
exl-id: c61d2e7c-7b5e-4b14-bd52-13dde34e32e3
source-git-commit: 1ab1c269fd43368e059a76f96b3eb3ac4e7b8388
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 100%

---

# FPID による訪問者の識別

[!DNL First-party IDs]（`FPIDs`）は、顧客が生成、管理、保存するデバイス ID です。これにより、ユーザーデバイスの識別を顧客が制御できるようになります。Edge Network は、`FPIDs` を送信することにより、`ECID` がリクエストに含まれていなくても新たに生成しません。

`FPID` は、`identityMap` の一部として API リクエスト本文に含むか、または、cookie として送信することができます。

`FPID` は Edge Network によって `ECID` に確定的に変換できるため、`FPID` の ID は Experience Cloud ソリューションと完全に互換性があります。特定の `FPID` から `ECID` を取得することで常に同じ結果が得られるので、ユーザーは一貫したエクスペリエンスを得ることができます。

このようにして得られた `ECID` は、`identity.fetch` クエリで取得することができます。

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

`FPID` と `ECID` の両方を含むリクエストの場合、リクエストに既に存在する `ECID` の方が、`FPID` から生成されるものよりも優先されます。つまり、Edge Network では、既に提供されている `ECID` を使用し、`FPID` は無視されます。新しい `ECID` は、 `FPID` が単独で提供された場合にのみ生成されます。

デバイス ID に関しては、`server` データストリームはデバイス ID として `FPID` を使用する必要があります。それ以外の ID（例えば `EMAIL`）もリクエスト本文内で指定できますが、Edge Network では、プライマリ ID が明示的に指定されている必要があります。プライマリ ID は、プロファイルデータが格納されるベース ID です。

>[!NOTE]
>
>ID を持たないリクエスト（リクエスト本文内で明示的に設定されたプライマリ ID を持たないリクエスト）は、失敗します。

以下の `identityMap` フィールドグループは、`server` データストリームのリクエストに対して正しく設定されています。

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

以下の `identityMap` フィールドグループは、`server` データストリームのリクエストに対して設定すると、結果としてエラー応答が返ります。

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

この場合、Edge Network から返されるエラー応答は次のようになります。

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

## `FPID` を使用した訪問者の識別

`FPID` でユーザーを識別するには、Edge Network にリクエストを行う前に、`FPID` cookie が送信されていることが必要です。`FPID` は、cookie で渡すことも、リクエスト本文の `identityMap` の一部として渡すこともできます。

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

## `identityMap` フィールドとして `FPID` が渡されたリクエスト

以下の例では、[!DNL FPID] を `identityMap` パラメーターとして渡しています。

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
