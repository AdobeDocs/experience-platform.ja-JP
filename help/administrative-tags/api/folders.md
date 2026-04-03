---
title: フォルダーエンドポイント
description: Adobe Experience Platform APIを使用してフォルダーを作成、更新、管理、削除する方法について説明します。
role: Developer
exl-id: ee43d699-725d-4ffd-a71b-049eeb3b4d7c
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 5%

---

# フォルダーエンドポイント

>[!IMPORTANT]
>
>このエンドポイント セットのエンドポイント URLは`https://experience.adobe.io`です。

フォルダーは、ビジネスオブジェクトをより適切に整理し、ナビゲーションや分類を容易にする機能です。

このガイドでは、フォルダーをより深く理解するための情報を提供し、APIを使用して基本的なアクションを実行するためのAPI呼び出しの例を紹介します。

## はじめに

続行する前に、必須ヘッダーやサンプル API呼び出しの読み取り方法など、APIへの呼び出しを正常に行うために知っておく必要がある重要な情報については、[入門ガイド ](./getting-started.md)を確認してください。

## フォルダーのリストの取得 {#list}

組織に属するフォルダーのリストを取得するには、`/folder` エンドポイントに対してGET リクエストを行い、フォルダーの種類と親フォルダーIDを指定します。

**API 形式**

```http
GET /folders/{FOLDER_TYPE}/{PARENT_FOLDER_ID}/subfolders
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FOLDER_TYPE}` | フォルダー内に含まれるオブジェクトのタイプ。 サポートされている値には、`segment`と`dataset`が含まれます。 |
| `{PARENT_FOLDER_ID}` | フォルダーのリストを取得する親フォルダーのID。 すべての親フォルダーのリストを表示するには、フォルダーID `root`を使用します。 |

**リクエスト**

+++すべてのトップレベルのデータセットフォルダーを一覧表示するサンプルリクエスト

```shell
curl -X GET https://experience.adobe.io/unifiedfolders/folders/dataset/root/subfolders
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200が返され、組織内のデータセットのすべてのトップレベルフォルダーのリストが表示されます。

+++組織内のデータセットのすべての最上位フォルダーのリストを含むサンプル応答。

```json
{
    "id": "c626b4f7-223b-4486-8900-00c266e31dd1",
    "name": "ParentFolder",
    "noun": "Dataset",
    "parentId": "{PARENT_ID}",
    "imsOrg": "{ORG_ID}",
    "sandboxId": "{SANDBOX_ID}",
    "sandboxName": "prod",
    "createdBy": null,
    "createdAt": "2023-01-12T03:31:00.118+00:00",
    "modifiedBy": null,
    "modifiedAt": "2023-01-13T05:47:06.718+00:00",
    "_links": null,
    "children": [
        {
            "id": "09d86b23-4819-471b-8a2a-05774ed268de",
            "name": "ChildFolder.1",
            "noun": "dataset",
            "parentId": "c626b4f7-223b-4486-8900-00c266e31dd1",
            "imsOrg": "{ORG_ID}",
            "sandboxId": "{SANDBOX_ID}",
            "sandboxName": null,
            "createdBy": "{USER_ID}",
            "createdAt": "2023-01-12T12:51:39.284+00:00",
            "modifiedBy": "{USER_ID}",
            "modifiedAt": "2023-01-12T12:51:39.284+00:00",
            "_links": null,
            "children": []
        },
        {
            "id": "fd2f6a68-ef65-470d-ab31-b02b7b2241ca",
            "name": "ChildFolder.2",
            "noun": "dataset",
            "parentId": "c626b4f7-223b-4486-8900-00c266e31dd1",
            "imsOrg": "{ORG_ID}",
            "sandboxId": "1bd86660-c5da-11e9-93d4-6d5fc3a66a8e",
            "sandboxName": null,
            "createdBy": "{USER_ID}",
            "createdAt": "2023-01-13T03:38:40.006+00:00",
            "modifiedBy": "{USER_ID}",
            "modifiedAt": "2023-01-13T03:38:40.006+00:00",
            "_links": null,
            "children": []
        }
    ]
}
```

+++

## 新しいフォルダーの作成 {#create}

`/folder` エンドポイントにPOST リクエストを行い、フォルダータイプを指定することで、新しいフォルダーを作成できます。

**API 形式**

```http
POST /folders/{FOLDER_TYPE}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FOLDER_TYPE}` | フォルダー内に含まれるオブジェクトのタイプ。 サポートされている値には、`segment`と`dataset`が含まれます。 |

**リクエスト**

+++新しいフォルダーを作成するためのサンプルリクエスト。

```shell
curl -X POST https://experience.adobe.io/unifiedfolders/folders/dataset
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
    "name": "SampleFolder",
    "parentId": "6a5e0927-1527-4abc-9993-376fd7067ca5"
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | 作成するフォルダーの名前。 |
| `parentId` | 親フォルダーのID。 |

+++

**応答**

応答が成功すると、HTTP ステータス 200が、新しく作成したフォルダーの詳細とともに返されます。

+++新しく作成したフォルダーの詳細を含むサンプル応答。

```json
{
    "id": "83f8287c-767b-4106-b271-257282fd170e",
    "name": "SampleFolder",
    "noun": "dataset",
    "parentId": "6a5e0927-1527-4abc-9993-376fd7067ca5",
    "imsOrg": "{ORG_ID}",
    "sandboxId": "{SANDBOX_ID}",
    "sandboxName": "prod",
    "createdBy": "{USER_ID}",
    "createdAt": "2023-10-01T08:47:06.192+00:00",
    "modifiedBy": "{USER_ID}",
    "modifiedAt": "2023-10-01T08:47:06.192+00:00",
    "status": "IN_USE",
    "_links": null
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 新しく作成したフォルダーのID。 |
| `createdBy` | フォルダーを作成したユーザーのID。 |
| `createdAt` | フォルダーが作成されたときのタイムスタンプ。 |
| `modifiedBy` | フォルダーを最後に変更したユーザーのID。 |
| `modifiedAt` | フォルダーが最後に更新されたときのタイムスタンプ。 |

+++

## 特定のフォルダーの取得 {#get}

組織に属する特定のフォルダーを取得するには、`/folder` エンドポイントに対してGET リクエストを行い、フォルダーの種類とフォルダーのIDを指定します。

**API 形式**

```http
GET /folders/{FOLDER_TYPE}/{FOLDER_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FOLDER_TYPE}` | フォルダー内に含まれるオブジェクトのタイプ。 サポートされている値には、`segment`と`dataset`が含まれます。 |
| `{FOLDER_ID}` | 取得するフォルダーのID。 |

**リクエスト**

+++特定のフォルダーを取得するためのサンプルリクエスト

```shell
curl -X GET https://experience.adobe.io/unifiedfolders/folders/dataset/83f8287c-767b-4106-b271-257282fd170e
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200が、リクエストされたフォルダーの詳細とともに返されます。

+++リクエストされたフォルダーの詳細を含むサンプル応答。

```json
{
    "id": "83f8287c-767b-4106-b271-257282fd170e",
    "name": "SampleFolder",
    "noun": "dataset",
    "parentId": "{PARENT_ID}",
    "imsOrg": "{ORG_ID}",
    "sandboxId": "{SANDBOX_ID}",
    "sandboxName": "prod",
    "createdBy": "{USER_ID}",
    "createdAt": "2023-10-01T08:47:06.192+00:00",
    "modifiedBy": "{USER_ID}",
    "modifiedAt": "2023-10-01T08:47:06.192+00:00",
    "status": "IN_USE",
    "_links": {
        "self": {
            "href": "/folders/dataset/83f8287c-767b-4106-b271-257282fd170e"
        }
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | リクエストされたフォルダーのID。 |
| `name` | リクエストされたフォルダーの名前。 |
| `parentId` | 親フォルダーのID。 |
| `createdBy` | フォルダーを作成したユーザーのID。 |
| `createdAt` | フォルダーが作成されたときのタイムスタンプ。 |
| `modifiedBy` | フォルダーを最後に更新したユーザーのID。 |
| `modifiedAt` | フォルダーが最後に更新されたときのタイムスタンプ。 |
| `status` | リクエストされたフォルダーのステータス。 サポートされている値には、`IN_USE`と`ARCHIVED`が含まれます。 |

+++

## 指定したフォルダーの検証 {#validate}

フォルダーにオブジェクトを含める資格があるかどうかを検証するには、`/folder/{FOLDER_TYPE}/{FOLDER_ID}/validate` エンドポイントにGET リクエストを行い、フォルダーの種類とIDの両方を指定します。

**API 形式**

```http
GET /folders/{FOLDER_TYPE}/{FOLDER_ID}/validate
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FOLDER_TYPE}` | フォルダー内に含まれるオブジェクトのタイプ。 サポートされている値には、`segment`と`dataset`が含まれます。 |
| `{FOLDER_ID}` | 検証中のフォルダーのID。 |

**リクエスト**

+++特定のフォルダーを検証するためのサンプルリクエスト

```shell
curl -X GET https://experience.adobe.io/unifiedfolders/folders/dataset/83f8287c-767b-4106-b271-257282fd170e/validate
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

ステータスが成功すると、HTTP ステータス 200が返され、検証中のフォルダーの詳細が表示されます。

+++サンプル応答には、検証済みフォルダーの詳細が含まれています

```json
{
    "id": "83f8287c-767b-4106-b271-257282fd170e",
    "name": "SampleFolder",
    "noun": "dataset",
    "parentId": "{PARENT_ID}",
    "imsOrg": "{ORG_ID}",
    "sandboxId": "{SANDBOX_ID}",
    "sandboxName": "prod",
    "createdBy": "{USER_ID}",
    "createdAt": "2023-10-01T08:47:06.192+00:00",
    "status": "IN_USE",
    "modifiedBy": "{USER_ID}",
    "modifiedAt": "2023-10-01T08:47:06.192+00:00",
    "_links": {
        "self": {
            "href": "/folders/dataset/83f8287c-767b-4106-b271-257282fd170e"
        }
    }
}
```

+++

## 特定のフォルダーの更新 {#update}

組織に属する特定のフォルダーの詳細を更新するには、`/folder` エンドポイントに対してPATCH リクエストを実行し、フォルダーの種類とフォルダーのIDを指定します。

**API 形式**

```http
PATCH /folders/{FOLDER_TYPE}/{FOLDER_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FOLDER_TYPE}` | フォルダー内に含まれるオブジェクトのタイプ。 サポートされている値には、`segment`と`dataset`が含まれます。 |
| `{FOLDER_ID}` | 更新するフォルダーのID。 |

**リクエスト**

+++特定のフォルダーを更新するためのサンプルリクエスト

```shell
curl -X GET https://experience.adobe.io/unifiedfolders/folders/dataset/83f8287c-767b-4106-b271-257282fd170e
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '[{
    "op": "replace",
    "path": "/name",
    "value": "RenamedSampleFolder"
 }]'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200が返され、新しく更新されたフォルダーに関する情報が表示されます。

```json
{
    "id": "eafab5bf-3457-4b7f-b366-3c5399bd98f1",
    "name": "RenamedSampleFolder",
    "noun": "dataset",
    "parentFolderId": null,
    "imsOrg": "{ORG_ID}",
    "sandboxId": "{SANDBOX_ID}",
    "sandboxName": "prod",
    "createdBy": "183807A65A0F5D180A494004@AdobeID",
    "createdAt": "2024-03-05T01:42:36.910+00:00",
    "modifiedBy": "183807A65A0F5D180A494004@AdobeID",
    "modifiedAt": "2024-03-05T01:45:54.740+00:00",
    "status": "IN_USE",
    "_links": {
        "self": {
            "href": "/folders/dataset/eafab5bf-3457-4b7f-b366-3c5399bd98f1"
        }
    },
    "namespace": null
}
```

## 特定のフォルダーの削除 {#delete}

組織に属する特定のフォルダーを削除するには、`/folder`にDELETE リクエストを行い、フォルダーの種類とフォルダーのIDを指定します。

***API形式**

```http
DELETE /folders/{FOLDER_TYPE}/{FOLDER_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FOLDER_TYPE}` | フォルダー内に含まれるオブジェクトのタイプ。 サポートされている値には、`segment`と`dataset`が含まれます。 |
| `{FOLDER_ID}` | 削除するフォルダーのID。 |

**リクエスト**

+++特定のフォルダーを削除するサンプルリクエスト

```shell
curl -X DELETE https://experience.adobe.io/unifiedfolders/folders/dataset/83f8287c-767b-4106-b271-257282fd170e
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200が返され、フォルダーの削除を通知するメッセージ本文が表示されます。

```json
{
    "message": "delete request accepted successfully"
}
```

## 次の手順

このガイドでは、Adobe Experience Platform APIを使用してフォルダーを作成、管理、削除する方法について詳しく説明します。
