---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；api ガイド；クエリサービス；クエリサービスアカウント；アカウント；
solution: Experience Platform
title: アカウント API エンドポイント
description: 永続的なクエリサービスアカウントを作成できます。
role: Developer
exl-id: 1667f4a5-e6e5-41e9-8f9d-6d2c63c7d7d6
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 5%

---

# アカウントエンドポイント

Adobe Experience Platform クエリサービスでは、アカウントを使用して、外部SQL クライアントで使用できる期限切れでない資格情報を作成します。 Query Service APIで`/accounts` エンドポイントを使用できます。これにより、Query Service統合アカウント（テクニカルアカウントとも呼ばれます）をプログラムで作成、取得、編集、削除できます。

## はじめに

このガイドで使用するエンドポイントは、Query Service APIの一部です。 続行する前に、必須ヘッダーやサンプル API呼び出しの読み取り方法など、APIへの呼び出しを正常に行うために知っておく必要がある重要な情報については、[入門ガイド ](./getting-started.md)を確認してください。

## アカウントの作成

`/accounts` エンドポイントにPOST リクエストを行うことで、Query Service統合アカウントを作成できます。

**API 形式**

```http
POST /accounts
```

**リクエスト**

次のリクエストにより、組織の新しいQuery Service統合アカウントが作成されます。

```shell
curl -X POST https://platform.adobe.io/data/foundation/queryauth/accounts \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/json' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '
{
    "accountName": "sampleName",
    "assignedToUser": "sample@example.com",
    "credential": "samplecredential",
    "description": "Sample description"
}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `accountName` | **必須** Query Service統合アカウントの名前。 |
| `assignedToUser` | **必須** Query Service統合アカウントが作成されるAdobe ID。 |
| `credential` | *（オプション）* クエリサービス統合に使用される資格情報。 指定しない場合、システムは自動的に資格情報を生成します。 |
| `description` | *（オプション）* クエリサービス統合アカウントの説明。 |

**応答**

応答が成功すると、HTTP ステータス 200が返され、新しく作成したQuery Service統合アカウントの詳細が表示されます。 これらのアカウントの詳細を使用して、クエリサービスを外部クライアントに接続できます。

```json
{
    "technicalAccountName": "2428A037-D963-47C2-A14D-CD816EFB0AA3@TECHACCT.ADOBE.COM",
    "technicalAccountId": "E09A0DFB5FDB25D90A494012",
    "credential": "samplecredential"
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `technicalAccountName` | Query Service統合アカウントの名前。 |
| `technicalAccountId` | Query Service統合アカウントのID。 これは、`credential`と共に、アカウントのパスワードを構成します。 |
| `credential` | Query Service統合アカウントの資格情報。 これは、`technicalAccountId`と共に、アカウントのパスワードを構成します。 |

## アカウントの更新

`/accounts` エンドポイントに対してPUT リクエストを行うことで、Query Service統合アカウントを更新できます。

**API 形式**

```http
POST /accounts/{ACCOUNT_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{ACCOUNT_ID}` | 更新するQuery Service統合アカウントのID。 |

**リクエスト**

```shell
curl -X PUT https://platform.adobe.io/data/foundation/queryauth/accounts/E09A0DFB5FDB25D90A494012 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
 {
     "accountName": "Updated account name",
     "assignedToUser": "sampleuser2@adobe.com",
     "credential": "UpdatedCredential",
     "description": "Updated description"
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `accountName` | *（オプション）* クエリサービス統合アカウントの更新された名前。 |
| `assignedToUser` | *（オプション）* Query Service統合アカウントがリンクされている更新されたAdobe ID。 |
| `credential` | *（オプション）* クエリサービスアカウントの更新された資格情報。 |
| `description` | *（オプション）* クエリサービス統合アカウントの更新された説明。 |

**応答**

応答が成功すると、HTTP ステータス 200が、新しく更新されたQuery Service統合アカウントに関する情報と共に返されます。

```json
{
    "accountName": "Updated account name",
    "assignedToUser": "sampleuser2@adobe.com",
    "created": "2021-06-16T16:44:42.073Z",
    "createdBy": "3BF132EF5BC636C10A49400B@AdobeID",
    "credential": "UpdatedCredential",
    "description": "Updated description",
    "lastUpdated": "2021-08-03T23:47:46.588Z",
    "lastUpdatedBy": "3BF132EF5BC636C10A49400B@AdobeID",
    "technicalAccountName": "2428A037-D963-47C2-A14D-CD816EFB0AA3@TECHACCT.ADOBE.COM",
    "technicalAccountId": "E09A0DFB5FDB25D90A494012"
}
```

## すべてのアカウントを表示

`/accounts` エンドポイントに対してGET リクエストを行うことで、すべてのQuery Service統合アカウントのリストを取得できます。

**API 形式**

```http
GET /accounts
```

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/foundation/queryauth/accounts \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、すべてのクエリサービス統合アカウントのリストを含むHTTP ステータス 200が返されます。

```json
{
    "accounts": [
        {
            "lastUpdated": "2021-06-16T18:07:49.581Z",
            "accountName": "Use for XQL testing of account creation",
            "description": "Local test",
            "assignedToUser": "platform-ui-automation@adobe.com",
            "technicalAccountName": "ADC7EB19-63B4-4B9F-A192-EBE3CD0C034E@TECHACCT.ADOBE.COM",
            "technicalAccountId": "38645A7360CA3DF30A49400F",
            "lastUpdatedBy": "3BF132EF5BC636C10A49400B@AdobeID",
            "createdBy": "3BF132EF5BC636C10A49400B@AdobeID",
            "created": "2021-06-16T18:07:49.581Z",
            "active": true
        },
        {
            "lastUpdated": "2021-06-16T16:44:42.073Z",
            "accountName": "Use for XQL testing of account creation",
            "description": " ",
            "assignedToUser": "platform-ui-automation@adobe.com",
            "technicalAccountName": "66E91FDD-4733-45E2-A312-D87580CFA55D@TECHACCT.ADOBE.COM",
            "technicalAccountId": "392202E060CA2A770A49420A",
            "lastUpdatedBy": "3BF132EF5BC636C10A49400B@AdobeID",
            "createdBy": "3BF132EF5BC636C10A49400B@AdobeID",
            "created": "2021-06-16T16:44:42.073Z",
            "active": true
        }
    ],
    "_page": {
        "count": 2,
        "next": "2021-06-16T16:44:42.073Z",
        "orderby": "-created",
        "property": "active==true",
        "start": "2021-06-16T18:07:49.581Z"
    },
    "_links": {
        "next": {
            "href": "https://platform.adobe.io/data/foundation/queryauth/accounts?orderby=-created&property=active==true&start=2021-06-16T16:44:42.073Z"
        },
        "prev": {
            "href": "https://platform.adobe.io/data/foundation/queryauth/accounts?orderby=-created&property=active==true&start=2021-06-16T18:07:49.581Z&isPrevLink=true"
        }
    },
    "version": 1
}
```

## アカウントの削除

`/accounts` エンドポイントに対してDELETE リクエストを行うことで、Query Service統合アカウントを削除できます。

**API 形式**

```http
DELETE /accounts/{ACCOUNT_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{ACCOUNT_ID}` | 削除するQuery Service統合アカウントのID。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/foundation/queryauth/accounts/E09A0DFB5FDB25D90A494012 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、HTTP ステータス 200が返され、アカウントが正常に削除されたことを示すメッセージが表示されます。

```json
{
    "result": "Success"
}
```
