---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；api ガイド；クエリサービス；クエリサービス；クエリサービスアカウント；アカウント；
solution: Experience Platform
title: アカウント API エンドポイント
topic-legacy: connection parameters
description: 永続的なのクエリサービスアカウントを作成できます。
exl-id: 1667f4a5-e6e5-41e9-8f9d-6d2c63c7d7d6
source-git-commit: 391b1943f1c941188b370e62ec86216367aa747f
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 5%

---

# アカウントエンドポイント

Adobe Experience Platformクエリサービスでは、外部の SQL クライアントで使用できる、期限が切れない資格情報を作成するためにアカウントが使用されます。 クエリサービス API の `/accounts` エンドポイントを使用すると、クエリサービス統合アカウント（テクニカルアカウントとも呼ばれます）をプログラムで作成、取得、編集および削除できます。

## はじめに

このガイドで使用されるエンドポイントは、クエリサービス API の一部です。 続行する前に、[ はじめに ](./getting-started.md) を参照して、必要なヘッダーやサンプル API 呼び出しを含む API を正しく呼び出すために知っておく必要がある重要な情報を確認してください。

## アカウントの作成

`/accounts` エンドポイントにPOSTリクエストを送信して、クエリサービス統合アカウントを作成できます。

**API 形式**

```http
POST /accounts
```

**リクエスト**

次のリクエストでは、IMS 組織の新しいクエリサービス統合アカウントが作成されます。

```shell
curl -X POST https://platform.adobe.io/data/foundation/queryauth/accounts \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/json' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `accountName` | **** 必須クエリサービス統合アカウントの名前。 |
| `assignedToUser` | **** 必須：クエリサービス統合アカウントを作成するAdobe ID。 |
| `credential` | *（オプション）* クエリサービスの統合に使用する秘密鍵証明書。指定しない場合は、システムによって自動的に資格情報が生成されます。 |
| `description` | *（オプション）* クエリサービス統合アカウントの説明。 |

**応答**

正常な応答は、HTTP ステータス 200 と、新しく作成したクエリサービス統合アカウントの詳細を返します。 これらのアカウントの詳細を使用して、クエリサービスを外部クライアントに接続できます。

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
| `technicalAccountId` | クエリサービス統合アカウントの ID。 これは `credential` と共に、アカウントのパスワードを構成します。 |
| `credential` | クエリサービス統合アカウントの資格情報。 これは `technicalAccountId` と共に、アカウントのパスワードを構成します。 |

## アカウントの更新

`/accounts` エンドポイントにPUTリクエストを実行することで、クエリサービス統合アカウントを更新できます。

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
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `assignedToUser` | *（オプション）* クエリサービス統合アカウントがリンクされている更新済みのAdobe ID。 |
| `credential` | *（オプション）* クエリサービスアカウント用に更新された資格情報。 |
| `description` | *（オプション）* クエリサービス統合アカウントの説明を更新しました。 |

**応答**

正常な応答は、HTTP ステータス 200 と、新しく更新されたクエリサービス統合アカウントに関する情報を返します。

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

## すべてのアカウントのリスト

`/accounts` エンドポイントに対してGETリクエストを実行することで、すべてのクエリサービス統合アカウントのリストを取得できます。

**API 形式**

```http
GET /accounts
```

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/foundation/queryauth/accounts \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 200 と、すべてのクエリサービス統合アカウントのリストを返します。

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

`/accounts` エンドポイントにDELETEリクエストを実行すると、クエリサービス統合アカウントを削除できます。

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
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 200 と、アカウントが正常に削除されたことを示すメッセージを返します。

```json
{
    "result": "Success"
}
```
