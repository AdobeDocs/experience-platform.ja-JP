---
title: フォルダーエンドポイント
description: Adobe Experience Platform API を使用してフォルダーを作成、更新、管理、削除する方法について説明します。
role: Developer
exl-id: ee43d699-725d-4ffd-a71b-049eeb3b4d7c
source-git-commit: 78aa48701abaadea963b25e390aa96d7b31386f4
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 5%

---

# フォルダーエンドポイント

>[!IMPORTANT]
>
>このエンドポイントのセットのエンドポイント URL は `https://experience.adobe.io` です。

フォルダーは、ビジネスオブジェクトを整理して、ナビゲーションや分類を容易にする機能です。

このガイドでは、フォルダーをより深く理解するための情報を提供し、API を使用して基本的なアクションを実行するためのサンプル API 呼び出しが含まれています。

## はじめに

続行する前に、[ はじめる前に ](./getting-started.md) を参照して、必要なヘッダーやサンプル API 呼び出しの読み取り方法など、API の呼び出しを正常に実行するために必要な重要な情報を確認してください。

## フォルダーのリストの取得 {#list}

組織に属するフォルダーのリストを取得するには、`/folder` エンドポイントにGETリクエストを実行し、フォルダータイプと親フォルダー ID を指定します。

**API 形式**

```http
GET /folders/{FOLDER_TYPE}/{PARENT_FOLDER_ID}/subfolders
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FOLDER_TYPE}` | フォルダーに含まれるオブジェクトのタイプ。 サポートされている値には、`segment` と `dataset` があります。 |
| `{PARENT_FOLDER_ID}` | フォルダーのリストを取得する親フォルダーの ID。 すべての親フォルダーのリストを表示するには、フォルダー ID `root` を使用します。 |

**リクエスト**

+++すべての最上位データセットフォルダーをリストするサンプルリクエスト

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

応答に成功すると、HTTP ステータス 200 が、組織内のデータセットのすべての最上位フォルダーのリストと共に返されます。

+++組織のデータセットのすべてのトップレベルフォルダーのリストを含むサンプル応答。

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

`/folder` エンドポイントに対してPOSTリクエストを行い、フォルダーのタイプを指定することで、新しいフォルダーを作成できます。

**API 形式**

```http
POST /folders/{FOLDER_TYPE}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FOLDER_TYPE}` | フォルダーに含まれるオブジェクトのタイプ。 サポートされている値には、`segment` と `dataset` があります。 |

**リクエスト**

+++新しいフォルダーを作成するリクエストのサンプル。

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
| `parentId` | 親フォルダーの ID。 |

+++

**応答**

応答に成功すると、HTTP ステータス 200 が、新しく作成されたフォルダーの詳細と共に返されます。

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
| `id` | 新しく作成されたフォルダーの ID。 |
| `createdBy` | フォルダーを作成したユーザーの ID。 |
| `createdAt` | フォルダーが作成されたときのタイムスタンプ。 |
| `modifiedBy` | フォルダーを最後に変更したユーザーの ID。 |
| `modifiedAt` | フォルダーが最後に更新された際のタイムスタンプ。 |

+++

## 特定のフォルダーの取得 {#get}

組織に属する特定のフォルダーを取得するには、`/folder` エンドポイントにGETリクエストを実行し、フォルダータイプとフォルダーの ID を指定します。

**API 形式**

```http
GET /folders/{FOLDER_TYPE}/{FOLDER_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FOLDER_TYPE}` | フォルダーに含まれるオブジェクトのタイプ。 サポートされている値には、`segment` と `dataset` があります。 |
| `{FOLDER_ID}` | 取得するフォルダーの ID。 |

**リクエスト**

+++特定のフォルダーを取得するリクエストのサンプル

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

応答に成功すると、HTTP ステータス 200 が、リクエストされたフォルダーの詳細と共に返されます。

+++リクエストされたフォルダーの詳細を含む応答のサンプル。

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
| `id` | リクエストされたフォルダーの ID。 |
| `name` | リクエストされたフォルダーの名前。 |
| `parentId` | 親フォルダーの ID。 |
| `createdBy` | フォルダーを作成したユーザーの ID。 |
| `createdAt` | フォルダーが作成されたときのタイムスタンプ。 |
| `modifiedBy` | フォルダーを最後に更新したユーザーの ID。 |
| `modifiedAt` | フォルダーが最後に更新された際のタイムスタンプ。 |
| `status` | リクエストされたフォルダーのステータス。 サポートされる値は `IN_USE` と `ARCHIVED` です。 |

+++

## 指定したフォルダーの検証 {#validate}

`/folder/{FOLDER_TYPE}/{FOLDER_ID}/validate` エンドポイントに対してGETリクエストを行い、フォルダータイプと ID の両方を指定することで、フォルダーにオブジェクトを含める資格があるかどうかを検証できます。

**API 形式**

```http
GET /folders/{FOLDER_TYPE}/{FOLDER_ID}/validate
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FOLDER_TYPE}` | フォルダーに含まれるオブジェクトのタイプ。 サポートされている値には、`segment` と `dataset` があります。 |
| `{FOLDER_ID}` | 検証するフォルダーの ID。 |

**リクエスト**

+++特定のフォルダーを検証するリクエストのサンプル

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

ステータスが成功すると、HTTP ステータス 200 が、検証中のフォルダーの詳細と共に返されます。

+++サンプルの応答には、検証済みフォルダーの詳細が含まれます

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

`/folder` エンドポイントにPATCHリクエストを実行し、フォルダータイプとフォルダーの ID を指定することで、組織に属する特定のフォルダーの詳細を更新できます。

**API 形式**

```http
PATCH /folders/{FOLDER_TYPE}/{FOLDER_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FOLDER_TYPE}` | フォルダーに含まれるオブジェクトのタイプ。 サポートされている値には、`segment` と `dataset` があります。 |
| `{FOLDER_ID}` | 更新するフォルダーの ID。 |

**リクエスト**

+++特定フォルダーを更新するリクエストの例

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

応答に成功すると、HTTP ステータス 200 と、新しく更新されたフォルダーに関する情報が返されます。

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

組織に属する特定のフォルダーを削除するには、`/folder` にDELETEリクエストを実行し、フォルダータイプとフォルダーの ID を指定します。

***API 形式**

```http
DELETE /folders/{FOLDER_TYPE}/{FOLDER_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FOLDER_TYPE}` | フォルダーに含まれるオブジェクトのタイプ。 サポートされている値には、`segment` と `dataset` があります。 |
| `{FOLDER_ID}` | 削除するフォルダーの ID。 |

**リクエスト**

+++特定のフォルダーを削除するリクエストのサンプル

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

応答に成功すると、HTTP ステータス 200 が、フォルダーの削除を知らせるメッセージ本文と共に返されます。

```json
{
    "message": "delete request accepted successfully"
}
```

## 次の手順

このガイドを読むことで、Adobe Experience Platform API を使用してフォルダーを作成、管理、削除する方法に関する理解が深まりました。
