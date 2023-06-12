---
description: このページでは、Adobe Experience Platform Destination SDK を通じて、宛先公開リクエストに関する詳細を取得するために使用される API 呼び出しの例を示します。
title: 宛先公開リクエストの取得
source-git-commit: 9e1ae44f83b886f0b5dd5a9fc9cd9b7db6154ff0
workflow-type: tm+mt
source-wordcount: '834'
ht-degree: 100%

---


# 宛先公開リクエストの取得

>[!IMPORTANT]
>
>他の Experience Platform 顧客が使用できるように、製品化（公開）された宛先を送信している場合にのみ、この API エンドポイントを使用する必要があります。自分で使用するためにプライベート宛先を作成している場合は、公開 API を使用して正式に宛先を送信する必要はありません。

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/destinations/publish`

宛先を設定してテストしたら、レビューおよび公開用にアドビに送信できます。宛先送信プロセスの一環として実行する必要があるその他のすべての手順については、[Destination SDK で作成した宛先をレビュー用に送信](../guides/submit-destination.md)を参照してください。

以下の場合に、公開宛先 API エンドポイントを使用して公開リクエストを送信します。

* Destination SDK パートナーとして、すべての Experience Platform 顧客のすべての Experience Platform 組織にわたって、製品化された宛先を利用できるようにする場合。
* 設定に対して&#x200B;*任意の更新*&#x200B;を行います。設定の更新は、新しい公開リクエストを送信し、Experience Platform チームによって承認された後にのみ、宛先に反映されます。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## 宛先公開 API 操作の概要 {#get-started}

続行する前に、「[はじめる前に](../getting-started.md)」を参照し、API の呼び出しを正常に行うために必要となる重要な情報（必要な宛先オーサリング権限および必要なヘッダーの取得方法など）を確認してください。

## 宛先の公開リクエストのリスト {#retrieve-list}

IMS 組織のすべての宛先のリストを取得するには、`/authoring/destinations/publish` エンドポイントに GET リクエストを作成します。

**API 形式**

以下の API 形式を使用して、お使いのアカウントに関するすべての公開リクエストを取得します。

```http
GET /authoring/destinations/publish
```

以下の API 形式を使用して、`{DESTINATION_ID}` パラメーターで定義された、特定の公開リクエストを取得します。

```http
GET /authoring/destinations/publish/{DESTINATION_ID}
```

**リクエスト**

以下の 2 つのリクエストは、リクエストで `DESTINATION_ID` パラメーターを渡すかどうかに応じて、お客様の IMS 組織に対するすべての公開リクエストか、特定の公開リクエストを取得します。

以下の各タブを選択して、対応するペイロードを表示します。

>[!BEGINTABS]

>[!TAB すべての公開リクエストの取得]

+++リクエスト

以下のリクエストは、[!DNL IMS Org ID] およびサンドボックス設定に基づいて、送信した公開リクエストのリストを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destinations/publish \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

+++応答

以下の応答では、HTTP ステータス 200 が、使用した IMS 組織 ID およびサンドボックス名に基づいた、アクセス権のある公開用に送信されたすべての宛先のリストと共に返されます。1 つの `configId` は、1 つの宛先の公開リクエストに対応します。

```json
{
   "destinationId":"1230e5e4-4ab8-4655-ae1e-a6296b30f2ec",
   "publishDetailsList":[
      {
         "configId":"ab41387c0-4772-4709-a3ce-6d5fee654520",
         "allowedOrgs":[
            "716543205DB85F7F0A495E5B@AdobeOrg"
         ],
         "status":"TEST",
         "destinationType":"DEV"
      },
      {
         "configId":"cd568c67-f25e-47e4-b9a2-d79297a20b27",
         "allowedOrgs":[
            "*"
         ],
         "status":"DEPRECATED",
         "destinationType":"PUBLIC",
         "publishedDate":1630525501009
      },
      {
         "configId":"ef6f07154-09bc-4bee-8baf-828ea9c92fba",
         "allowedOrgs":[
            "*"
         ],
         "status":"PUBLISHED",
         "destinationType":"PUBLIC",
         "publishedDate":1630531586002
      }
   ]
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `destinationId` | 文字列 | 公開用に送信した宛先設定の宛先 ID。 |
| `publishDetailsList.configId` | 文字列 | 送信された宛先の宛先公開リクエストの一意の ID。 |
| `publishDetailsList.allowedOrgs` | 文字列 | 宛先を使用できる Experience Platform 組織を返します。<br> <ul><li> `"destinationType": "PUBLIC"` の場合、このパラメーターは、`"*"` を返します（つまり、宛先は、すべての Experience Platform 組織で使用できます）。</li><li> `"destinationType": "DEV"` の場合、このパラメーターは、宛先の作成およびテストに使用した組織の組織 ID を返します。</li></ul> |
| `publishDetailsList.status` | 文字列 | 宛先の公開リクエストのステータス。使用可能な値は、`TEST`、`REVIEW`、`APPROVED`、`PUBLISHED`、`DENIED`、`REVOKED`、`DEPRECATED` です。値 `PUBLISHED` を持つ宛先は、ライブで、Experience Platform 顧客が使用できます。 |
| `publishDetailsList.destinationType` | 文字列 | 宛先のタイプ。値には、`DEV` および `PUBLIC` を取ることができます。`DEV` は、Experience Platform 組織の宛先に対応します。`PUBLIC` は、公開用に送信した宛先に対応します。これらの 2 つのオプションを Git 用語で考えてみると、`DEV` バージョンは、ローカルオーサリングブランチを、`PUBLIC` バージョンは、リモートメインブランチを表します。 |
| `publishDetailsList.publishedDate` | 文字列 | 宛先が公開用に送信された日付（エポック時間）。 |

{style="table-layout:auto"}

+++

>[!TAB 特定の公開リクエストの取得]

+++リクエスト

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destinations/publish/{DESTINATION_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_ID}` | 公開ステータスを取得したい宛先の ID。 |

+++

+++応答

API 呼び出しで `DESTINATION_ID` が渡されると、応答は、HTTP ステータス 200 を、指定された宛先公開リクエストに関する詳細情報と共に返します。

```json
{
   "destinationId":"1230e5e4-4ab8-4655-ae1e-a6296b30f2ec",
   "publishDetailsList":[
      {
         "configId":"123cs780-ce29-434f-921e-4ed6ec2a6c35",
         "allowedOrgs": [
            "*"
         ],    
         "status":"PUBLISHED",
         "destinationType": "PUBLIC",
         "publishedDate":"1630617746"
      }
   ]
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `destinationId` | 文字列 | 公開用に送信した宛先設定の宛先 ID。 |
| `publishDetailsList.configId` | 文字列 | 送信された宛先の宛先公開リクエストの一意の ID。 |
| `publishDetailsList.allowedOrgs` | 文字列 | 宛先を使用できる Experience Platform 組織を返します。<br> <ul><li> `"destinationType": "PUBLIC"` の場合、このパラメーターは、`"*"` を返します（つまり、宛先は、すべての Experience Platform 組織で使用できます）。</li><li> `"destinationType": "DEV"` の場合、このパラメーターは、宛先の作成およびテストに使用した組織の組織 ID を返します。</li></ul> |
| `publishDetailsList.status` | 文字列 | 宛先の公開リクエストのステータス。使用可能な値は、`TEST`、`REVIEW`、`APPROVED`、`PUBLISHED`、`DENIED`、`REVOKED`、`DEPRECATED` です。値 `PUBLISHED` を持つ宛先は、ライブで、Experience Platform 顧客が使用できます。 |
| `publishDetailsList.destinationType` | 文字列 | 宛先のタイプ。値には、`DEV` および `PUBLIC` を取ることができます。`DEV` は、Experience Platform 組織の宛先に対応します。`PUBLIC` は、公開用に送信した宛先に対応します。これらの 2 つのオプションを Git 用語で考えてみると、`DEV` バージョンは、ローカルオーサリングブランチを、`PUBLIC` バージョンは、リモートメインブランチを表します。 |
| `publishDetailsList.publishedDate` | 文字列 | 宛先が公開用に送信された日付（エポック時間）。 |

{style="table-layout:auto"}

+++

>[!ENDTABS]

## API エラー処理

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。