---
keywords: Experience Platform；ホーム；人気のトピック；query service;api ガイド；Query service;Query service アカウント；アカウント；
solution: Experience Platform
title: アカウント API エンドポイント
description: 永続のクエリサービスアカウントを作成できます。
role: Developer
exl-id: 1667f4a5-e6e5-41e9-8f9d-6d2c63c7d7d6
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 5%

---

# アカウントエンドポイント

Adobe Experience Platform クエリサービスでは、アカウントを使用して、有効期限のない資格情報を作成し、外部 SQL クライアントで使用できます。 Query Service API の `/accounts` エンドポイントを使用すると、Query Service 統合アカウント（テクニカルアカウントとも呼ばれます）をプログラムによって作成、取得、編集、削除できます。

## はじめに

このガイドで使用するエンドポイントは、Query Service API の一部です。 続行する前に、[&#x200B; はじめる前に &#x200B;](./getting-started.md) を参照して、必要なヘッダーやサンプル API 呼び出しの読み取り方法など、API の呼び出しを正常に実行するために必要な重要な情報を確認してください。

## アカウントの作成

`/accounts` エンドポイントにPOSTリクエストを行うことで、クエリサービス統合アカウントを作成できます。

**API 形式**

```http
POST /accounts
```

**リクエスト**

次のリクエストは、組織の新しいクエリサービス統合アカウントを作成します。

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
| `accountName` | **必須** クエリサービス統合アカウントの名前。 |
| `assignedToUser` | **必須** クエリサービス統合アカウントを作成する対象のAdobe ID。 |
| `credential` | *（任意）* クエリサービスの統合に使用する資格情報。 指定しない場合、システムにより自動的に資格情報が生成されます。 |
| `description` | *（オプション）* クエリサービス統合アカウントの説明。 |

**応答**

応答が成功すると、HTTP ステータス 200 が、新しく作成されたクエリサービス統合アカウントの詳細と共に返されます。 これらのアカウントの詳細を使用して、クエリサービスを外部クライアントと接続できます。

```json
{
    "technicalAccountName": "2428A037-D963-47C2-A14D-CD816EFB0AA3@TECHACCT.ADOBE.COM",
    "technicalAccountId": "E09A0DFB5FDB25D90A494012",
    "credential": "samplecredential"
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `technicalAccountName` | クエリサービス統合アカウントの名前。 |
| `technicalAccountId` | クエリサービス統合アカウントの ID。 これは、`credential` と一緒に、あなたのアカウントのパスワードを構成します。 |
| `credential` | クエリサービス統合アカウントの資格情報。 これは、`technicalAccountId` と一緒に、あなたのアカウントのパスワードを構成します。 |

## アカウントの更新

`/accounts` エンドポイントにPUTリクエストを行うことで、クエリサービス統合アカウントを更新できます。

**API 形式**

```http
POST /accounts/{ACCOUNT_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{ACCOUNT_ID}` | 更新するクエリサービス統合アカウントの ID。 |

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
| `assignedToUser` | *（任意）* クエリサービス統合アカウントがリンクされている更新済みAdobe ID。 |
| `credential` | *（オプション）* クエリサービスアカウントの更新された資格情報。 |
| `description` | *（オプション）* クエリサービス統合アカウントの更新された説明。 |

**応答**

応答が成功すると、HTTP ステータス 200 が、新しく更新されたクエリサービス統合アカウントに関する情報と共に返されます。

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

## すべてのアカウントをリスト

`/accounts` エンドポイントにGETリクエストを行うことで、すべてのクエリサービス統合アカウントのリストを取得できます。

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

応答に成功すると、HTTP ステータス 200 が、すべてのクエリサービス統合アカウントのリストと共に返されます。

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

`/accounts` エンドポイントにDELETEリクエストを行うことで、クエリサービス統合アカウントを削除できます。

**API 形式**

```http
DELETE /accounts/{ACCOUNT_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{ACCOUNT_ID}` | 削除するクエリサービス統合アカウントの ID。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/foundation/queryauth/accounts/E09A0DFB5FDB25D90A494012 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、HTTP ステータス 200 が、アカウントが正常に削除されたことを示すメッセージと共に返されます。

```json
{
    "result": "Success"
}
```
