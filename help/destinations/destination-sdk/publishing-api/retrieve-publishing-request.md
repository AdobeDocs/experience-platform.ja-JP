---
description: このページでは、Adobe Experience Platform Destination SDKを通じて宛先の公開リクエストの詳細を取得するために使用される API 呼び出しの例を示します。
title: 宛先の公開リクエストの取得
source-git-commit: 9e1ae44f83b886f0b5dd5a9fc9cd9b7db6154ff0
workflow-type: tm+mt
source-wordcount: '834'
ht-degree: 36%

---


# 宛先の公開リクエストの取得

>[!IMPORTANT]
>
>この API エンドポイントを使用する必要があるのは、製品化された（公開）宛先を送信し、他のExperience Platformのお客様が使用できるようにする場合のみです。 独自の用途で非公開の宛先を作成する場合は、公開 API を使用して正式に宛先を送信する必要はありません。

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/destinations/publish`

宛先を設定およびテストしたら、レビューおよび公開用にアドビへと送信できます。読み取り [送信してレビュー用に、Destination SDKで作成した宛先を送信](../guides/submit-destination.md) 宛先の送信プロセスの一環として実行する必要があるその他のすべての手順については、を参照してください。

公開リクエストを送信するには、次の場合に Publish Destinations API エンドポイントを使用します。

* Destination SDK パートナーとして、すべての Experience Platform 組織をまたいですべての Experience Platform の顧客がすべての Experience Platform の顧客が製品化された宛先を利用できるようにする場合。
* あなたは *更新の有無* を設定に追加します。 設定の更新は、新しい公開リクエストを送信した後にのみ、宛先に反映されます。このリクエストはExperience Platformチームが承認します。

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## 宛先公開 API 操作の概要 {#get-started}

続行する前に、[はじめる前に](../getting-started.md)の重要情報を参照してください。これは、必要な宛先オーサリング権限および必要なヘッダーを取得する方法を含む、API への呼び出しに成功するために確認する必要があります。

## 宛先の公開リクエストのリスト {#retrieve-list}

IMS 組織のすべての宛先のリストを取得するには、`/authoring/destinations/publish` エンドポイントに GET リクエストを作成します。

**API 形式**

次の API 形式を使用して、アカウントのすべての公開リクエストを取得します。

```http
GET /authoring/destinations/publish
```

次の API 形式を使用して、 `{DESTINATION_ID}` パラメーター。

```http
GET /authoring/destinations/publish/{DESTINATION_ID}
```

**リクエスト**

次の 2 つのリクエストでは、 `DESTINATION_ID` パラメーターを指定します。

下の各タブを選択し、対応するペイロードを表示します。

>[!BEGINTABS]

>[!TAB すべての発行リクエストを取得]

+++リクエスト

次のリクエストは、に基づいて、送信した発行リクエストのリストを取得します [!DNL IMS Org ID] およびサンドボックス設定

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destinations/publish \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

+++応答

次の応答は、使用した IMS 組織 ID とサンドボックス名に基づいて、HTTP ステータス 200 と、アクセス権のある公開用に送信されたすべての宛先のリストを返します。 1 つの `configId` は、1 つの宛先の公開リクエストに対応します。

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
| `publishDetailsList.allowedOrgs` | 文字列 | 宛先が使用可能なExperience Platform組織を返します。 <br> <ul><li> の場合 `"destinationType": "PUBLIC"`の場合、このパラメーターは `"*"`：宛先は、すべてのExperience Platform組織で使用できます。</li><li> の場合 `"destinationType": "DEV"`の場合、このパラメーターは宛先の作成およびテストに使用した組織の組織 ID を返します。</li></ul> |
| `publishDetailsList.status` | 文字列 | 宛先の公開リクエストのステータス。使用可能な値は `TEST`、`REVIEW`、`APPROVED`、`PUBLISHED`、`DENIED`、`REVOKED`、`DEPRECATED` です。値を持つ宛先 `PUBLISHED` は実稼動環境であり、Experience Platformのお客様が使用できます。 |
| `publishDetailsList.destinationType` | 文字列 | 宛先のタイプ。 値は `DEV` および `PUBLIC`. `DEV` は、Experience Platform組織の宛先に対応します。 `PUBLIC` は、公開用に送信した宛先に対応します。 Git では、これら 2 つのオプションを考えて、 `DEV` バージョンは、ローカルのオーサリングブランチを表し、 `PUBLIC` version は、リモートメインブランチを表します。 |
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

もし `DESTINATION_ID` API 呼び出しでは、応答は HTTP ステータス 200 と、指定された宛先公開リクエストに関する詳細情報を返します。

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
| `publishDetailsList.allowedOrgs` | 文字列 | 宛先が使用可能なExperience Platform組織を返します。 <br> <ul><li> の場合 `"destinationType": "PUBLIC"`の場合、このパラメーターは `"*"`：宛先は、すべてのExperience Platform組織で使用できます。</li><li> の場合 `"destinationType": "DEV"`の場合、このパラメーターは宛先の作成およびテストに使用した組織の組織 ID を返します。</li></ul> |
| `publishDetailsList.status` | 文字列 | 宛先の公開リクエストのステータス。使用可能な値は `TEST`、`REVIEW`、`APPROVED`、`PUBLISHED`、`DENIED`、`REVOKED`、`DEPRECATED` です。値を持つ宛先 `PUBLISHED` は実稼動環境であり、Experience Platformのお客様が使用できます。 |
| `publishDetailsList.destinationType` | 文字列 | 宛先のタイプ。 値は `DEV` および `PUBLIC`. `DEV` は、Experience Platform組織の宛先に対応します。 `PUBLIC` は、公開用に送信した宛先に対応します。 Git では、これら 2 つのオプションを考えて、 `DEV` バージョンは、ローカルのオーサリングブランチを表し、 `PUBLIC` version は、リモートメインブランチを表します。 |
| `publishDetailsList.publishedDate` | 文字列 | 宛先が公開用に送信された日付（エポック時間）。 |

{style="table-layout:auto"}

+++

>[!ENDTABS]

## API エラー処理

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。