---
title: 統合タグエンドポイント
description: Adobe Experience Platform API を使用して、タグカテゴリとタグを作成、更新、管理、削除する方法について説明します。
role: Developer
exl-id: 6687d1da-a5e4-435a-9f99-1b0f9ac98088
source-git-commit: 717a4ea0568200c940cf9b8f26f4dd2aa9c00a3e
workflow-type: tm+mt
source-wordcount: '1860'
ht-degree: 4%

---

# 統合タグエンドポイント

>[!IMPORTANT]
>
>このエンドポイントのセットのエンドポイント URL は `https://experience.adobe.io` です。

タグは、メタデータ分類を管理してビジネスオブジェクトを分類し、検出とカテゴリ化を容易にする機能です。 その後、これらのタグをタグカテゴリに追加することで、タグをさらにグループ化できます。

このガイドでは、タグとタグカテゴリをより深く理解するのに役立つ情報を提供し、API を使用して基本的なアクションを実行するためのサンプル API 呼び出しが含まれています。

## はじめに

このガイドで使用するエンドポイントは、Adobe Experience Platform API の一部です。 続行する前に、[&#x200B; はじめる前に &#x200B;](./getting-started.md) を参照して、必要なヘッダーやサンプル API 呼び出しの読み取り方法など、API の呼び出しを正常に実行するために必要な重要な情報を確認してください

### 用語集

次の用語集では、**タグ** と **タグカテゴリ** の違いについて説明します。

- **タグ**：タグを使用すると、ビジネスオブジェクトのメタデータ分類を管理でき、これらのオブジェクトを分類して検出と分類が容易になります。
   - **未分類タグ**：未分類タグは、タグカテゴリに属さないタグです。 デフォルトでは、作成されたタグは分類されません。
- **タグカテゴリ**：タグカテゴリを使用すると、タグを意味のあるセットにグループ化し、タグの目的に関するより多くのコンテキストを提供できます。

## タグカテゴリのリストの取得 {#get-tag-categories}

`/tagCategory` エンドポイントにGETリクエストをおこなうことで、組織に属するタグカテゴリのリストを取得できます。

**API 形式**

```http
GET /tagCategory
GET /tagCategory?{QUERY_PARAMETERS}
```

タグカテゴリを取得する場合は、次のオプションのクエリパラメーターを使用できます。

| クエリーパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `start` | 結果のリストの開始位置。 これを使用して、結果のページネーションの開始インデックスを指定できます。 | `start=a` |
| `limit` | ページごとに取得するタグカテゴリの最大数。 | `limit=20` |
| `property` | タグカテゴリを取得する際にフィルタリングする属性。 次の値がサポートされています。&lt;ul≥<li>`name`：タグカテゴリの名前。</li></ul> | `property=name==category` |
| `sortBy` | タグカテゴリが並べ替えられる順序。 サポートされる値は、`name`、`createdAt`、`modifiedAt` です。 | `sortBy=name` |
| `sortOrder` | タグカテゴリが並べ替えられる方向です。 サポートされる値は `asc` と `desc` です。 | `sortOrder=asc` |

**リクエスト**

+++組織内のすべてのタグカテゴリをリストするサンプルリクエスト

```shell
curl -X GET https://experience.adobe.io/unifiedtags/tagCategory
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
```

+++

**応答**

応答に成功すると、HTTP ステータス 200 が、組織のすべてのタグカテゴリのリストと共に返されます。

+++組織内のすべてのタグカテゴリのリストを含む応答のサンプル。

```json
{
    "_page": {
        "count": 1,
        "limit": 10,
        "property": []
    },
    "tags": [
        {
            "id": "e2b7c656-067b-4413-a366-adde0401df50",
            "name": "Test Category",
            "description": "A sample description for the test tag category.",
            "org": "{ORG_ID}",
            "createdBy": "{USER_ID}",
            "createdAt": "1661752268000",
            "modifiedBy": "{USER_ID}",
            "modifiedAt": "1661752268000",
            "tagCount": 0
        }
    ]
}
```

+++

## 新しいタグカテゴリの作成 {#create-tag-category}

>[!IMPORTANT]
>
>この API 呼び出しを使用できるのは、システム管理者と製品管理者のみです。

`/tagCategory` エンドポイントに対してタグリクエストを実行することで、新しいPOSTカテゴリを作成できます。

**API 形式**

```http
POST /tagCategory
```

**リクエスト**

+++新しいタグカテゴリを作成するリクエストの例。

```shell
curl -X POST https://experience.adobe.io/unifiedtags/tagCategory
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
 -d '{
    "name": "Sample Test Category",
    "description": "Sample test category"
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | 作成するタグカテゴリの名前。 |
| `description` | 作成するタグカテゴリの説明。 |

+++

**応答**

応答サンプルを使用すると、HTTP ステータス 200 が、新しく作成されたタグカテゴリの詳細と共に返されます。

+++新しく作成したタグカテゴリの詳細を含むサンプル応答。

```json
{
    "id": "e2b7c656-067b-4413-a366-adde0401df50",
    "name": "Sample Test Category",
    "description": "Sample test category",
    "org": "{ORG_ID}",
    "createdBy": "{USER_ID}",
    "createdAt": "1661752268000",
    "modifiedBy": "{USER_ID}",
    "modifiedAt": "1661752268000",
    "tagCount": 0
}
```

+++

## 特定のタグカテゴリの取得 {#get-tag-category}

`/tagCategory` エンドポイントに対してGETリクエストを実行し、タグカテゴリの ID を指定すると、組織に属する特定のタグカテゴリを取得できます。

**API 形式**

```http
GET /tagCategory/{TAG_CATEGORY_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TAG_CATEGORY_ID}` | 取得するタグカテゴリの ID。 |

**リクエスト**

+++特定のタグカテゴリを取得するサンプルリクエスト

```shell
curl -X GET https://experience.adobe.io/unifiedtags/tagCategory/e2b7c656-067b-4413-a366-adde0401df50 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、指定されたタグカテゴリの詳細と共に返されます。

+++指定したタグカテゴリの詳細を含む応答のサンプル。

```json
{
    "id": "e2b7c656-067b-4413-a366-adde0401df50",
    "name": "Test Category",
    "description": "A sample description for the test tag category.",
    "org": "{ORG_ID}",
    "createdBy": "{USER_ID}",
    "createdAt": "1661752268000",
    "modifiedBy": "{USER_ID}",
    "modifiedAt": "1661752268000",
    "tagCount": 0
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | リクエストされたタグカテゴリの ID。 |
| `name` | リクエストされたタグカテゴリの名前。 |
| `description` | リクエストされたタグカテゴリの説明。 |
| `createdBy` | タグカテゴリを作成したユーザーの ID。 |
| `createdAt` | タグカテゴリが作成されたときのタイムスタンプ。 |
| `modifiedBy` | タグカテゴリを最後に更新したユーザーの ID。 |
| `modifiedAt` | タグカテゴリが最後に更新された際のタイムスタンプ。 |
| `tagCount` | タグカテゴリに属するタグの数。 |

+++

## 特定のタグカテゴリの更新 {#update-tag-category}

>[!IMPORTANT]
>
>この API 呼び出しを使用できるのは、システム管理者と製品管理者のみです。

`/tagCategory` エンドポイントにPATCHリクエストを実行し、タグカテゴリの ID を指定することで、組織に属する特定のタグカテゴリの詳細を更新できます。

**API 形式**

```http
PATCH /tagCategory/{TAG_CATEGORY_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TAG_CATEGORY_ID}` | 取得するタグカテゴリの ID。 |

**リクエスト**

+++特定のタグカテゴリを更新するサンプルリクエスト

```shell
curl -X PATCH https://experience.adobe.io/unifiedtags/tagCategory/e2b7c656-067b-4413-a366-adde0401df50 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
 -d '[{
    "op": "replace",
    "path": "description",
    "value": "Updated sample description",
    "from": "Sample description"
 }]'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `op` | 完了した操作。 特定のタグカテゴリを更新するには、この値を `replace` に設定します。 |
| `path` | 更新されるフィールドのパス。 サポートされる値は `name` と `description` です。 |
| `value` | 更新するフィールドの更新された値。 |
| `from` | 更新するフィールドの元の値。 |

+++

**応答**

新しく更新されたタグカテゴリに関する情報を含んだ応答が成功すると、HTTP ステータス 200 が返されます。

+++新しく更新されたタグカテゴリの詳細を含むサンプル応答。

```json
{
    "id": "e2b7c656-067b-4413-a366-adde0401df50",
    "name": "Test Category",
    "description": "Updated sample description",
    "org": "{ORG_ID}",
    "createdBy": "{USER_ID}",
    "createdAt": "1661752268000",
    "modifiedBy": "{USER_ID}",
    "modifiedAt": "1661752268000",
    "tagCount": 0
}
```

+++

## 特定のタグカテゴリの削除 {#delete-tag-category}

>[!IMPORTANT]
>
>この API 呼び出しを使用できるのは、システム管理者と製品管理者のみです。

`/tagCategory` エンドポイントに対して削除リクエストを行い、タグカテゴリの ID を指定することで、組織に属する特定のタグカテゴリをDELETEできます。

**API 形式**

```http
DELETE /tagCategory/{TAG_CATEGORY_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TAG_CATEGORY_ID}` | 取得するタグカテゴリの ID。 |

**リクエスト**

+++特定のタグカテゴリを削除するサンプルリクエスト

```shell
curl -X DELETE https://experience.adobe.io/unifiedtags/tagCategory/e2b7c656-067b-4413-a366-adde0401df50 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、空の応答と共に返されます。

## タグのリストの取得 {#get-tags}

`/tags` エンドポイントとタグカテゴリの ID に対してGETリクエストを実行することで、組織に属するタグのリストを取得できます。

**API 形式**

```http
GET /tags
GET /tags?{QUERY_PARAMETERS}
```

タグの取得時には、次のオプションのクエリパラメーターを使用できます。

| クエリーパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `start` | 結果のリストの開始位置。 これを使用して、結果のページネーションの開始インデックスを指定できます。 | `start=a` |
| `limit` | ページごとに取得するタグの最大数。 | `limit=20` |
| `property` | タグを取得する際にフィルタリングする属性。 次の値がサポートされています。<ul><li>`name`: タグの名前。</li><li>`archived`：タグがアーカイブされているかどうか、またはアーカイブ解除されているかどうか。 この値には、`true` または `false` を設定できます。</li><li>`tagCategoryId`：タグが属するタグカテゴリの ID。</li></ul> | <ul><li>`property=name==TestTag`</li><li>`property=archived==false`</li><li>`property=tagCategoryId==e2b7c656-067b-4413-a366-adde0401df50`</li> |
| `sortBy` | タグが並べ替えられる順序。 サポートされる値は、`name`、`createdAt`、`modifiedAt` です。 | `sortBy=name` |
| `sortOrder` | タグカテゴリが並べ替えられる方向です。 サポートされる値は `asc` と `desc` です。 | `sortOrder=asc` |


**リクエスト**

+++特定のタグカテゴリに属するすべてのタグを取得するリクエストのサンプル

```shell
curl -X GET https://experience.adobe.io/unifiedtags/tags?property=tagCategoryId=e2b7c656-067b-4413-a366-adde0401df50
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、そのタグカテゴリに属するタグの詳細と共に返されます。

+++ リクエストされたタグの詳細を含む応答のサンプル。

```json
{
    "_page": {
        "count": 166,
        "limit": 10,
        "next": "eyJjb21wb3NpdGVUb2tlbiI6IntcInRva2VuXCI6XCIrUklEOn52a0owQUp3WDRrVko1d0FBQUFBQUFBPT0jUlQ6MiNUUkM6MjAjUlREOnVDTmQyWlAvWjV6TGdvUGVGR1JHQk1KNVExVmR6Mnc9I0lTVjoyI0lFTzo2NTU2NyNRQ0Y6OCNGUEM6QWdFQ0J3TG1BQ1NmQnNBQ0JBb0FBQVFBQ0FBQUNJQVlnQWVBRElBTmdBWEFFTUJCUUVBQUFBQkFRQkdBSElBR2dBQ0FENEFId0FKUkRFQUNBZ2dBUUJnQUVBQUlIb0FaZ0FDQUJNQUFRVUFBUUFCQVFScUFBc0FTQUFBRUxvQU9nQWFBQmNBQVlBQUFHSUlCUUFDQU1vQUlnQWlBQk1DQUFRQUFnZ0FnQUM2QURZQTNnQWlBR1lBQWdCZUFBY0FCZ0JlQUM4QURBQUlBQWdBQVFBQ0FBRUZBQVFFQUFBRWdBQ0FBSjRCR2dBeUFCSUFPZ0F5QU13QVNRQ0FBQUVBdGdCRUFBR0FkZ0FuQUFDZ0NBQUFBQ0lCQUFDSkFnQUJBRUFDQUg0QUhnQWFBQllBVUFFQUNCQUFFQUFRQUF4QUFzUnJBQUlFQUFBYkxoQklIQVBBQUhnUUVBTEVxQUE4RkNBQVFtcUVBd0FBTWd3Y09BSFdIa1FBZ0JGT0FTNEN4QVE0QVwiLFwicmFuZ2VcIjp7XCJtaW5cIjpcIlwiLFwibWF4XCI6XCJGRlwifX0iLCJvcmRlckJ5SXRlbXMiOlt7Iml0ZW0iOjE2OTQ0ODg2MDMwMDB9XSwicmlkIjoidmtKMEFKd1g0a1hHV2dFQUFBQUFBQT09IiwiaW5jbHVzaXZlIjp0cnVlfQ==",
        "property": [
            "tagCategoryId=e2b7c656-067b-4413-a366-adde0401df50"
        ]
    },
    "tags": [
        {
            "archived": false,
            "createdAt": 1705624523000,
            "createdBy": "{USER_ID}",
            "id": "8af14b1e-f267-44ad-b94c-9ac70274e3d5",
            "modifiedAt": 1705624523000,
            "modifiedBy": "{USER_ID}",
            "name": "xql-test-1705624481530",
            "org": "{ORG_ID}",
            "tagCategoryId": "e2b7c656-067b-4413-a366-adde0401df50",
            "tagCategoryName": "Test Category"
        },
        {
            "archived": false,
            "createdAt": 1705624523000,
            "createdBy": "{USER_ID}",
            "id": "8b907a2c-0f15-4d2c-9672-bf545d5e47ab",
            "modifiedAt": 1705624523000,
            "modifiedBy": "{USER_ID}",
            "name": "xql-test-1705624489131",
            "org": "{ORG_ID}",
            "tagCategoryId": "e2b7c656-067b-4413-a366-adde0401df50",
            "tagCategoryName": "Test Category"
        },
        {
            "archived": false,
            "createdAt": 1705624523000,
            "createdBy": "{USER_ID}",
            "id": "e30bd956-afad-40a1-8f4a-7e4428855856",
            "modifiedAt": 1705624523000,
            "modifiedBy": "{USER_ID}",
            "name": "xql-test-1705624494191",
            "org": "{ORG_ID}",
            "tagCategoryId": "e2b7c656-067b-4413-a366-adde0401df50",
            "tagCategoryName": "Test Category"
        },
        {
            "archived": false,
            "createdAt": 1705451722000,
            "createdBy": "{USER_ID}",
            "id": "3bf6a6ba-0b11-4d83-8f35-db6e5b9652d8",
            "modifiedAt": 1705451722000,
            "modifiedBy": "{USER_ID}",
            "name": "xql-test-1705451701640",
            "org": "{ORG_ID}",
            "tagCategoryId": "e2b7c656-067b-4413-a366-adde0401df50",
            "tagCategoryName": "Test Category"
        },
        {
            "archived": false,
            "createdAt": 1705422929000,
            "createdBy": "{USER_ID}",
            "id": "0910dfc8-7924-473d-afc6-1aa68337b3b6",
            "modifiedAt": 1705422929000,
            "modifiedBy": "{USER_ID}",
            "name": "xql-test-1705422890399",
            "org": "{ORG_ID}",
            "tagCategoryId": "e2b7c656-067b-4413-a366-adde0401df50",
            "tagCategoryName": "Test Category"
        },
        {
            "archived": false,
            "createdAt": 1705394126000,
            "createdBy": "{USER_ID}",
            "id": "b426085e-580b-4147-9921-8ba77ffa77a9",
            "modifiedAt": 1705394126000,
            "modifiedBy": "{USER_ID}",
            "name": "xql-test-1705394104556",
            "org": "{ORG_ID}",
            "tagCategoryId": "e2b7c656-067b-4413-a366-adde0401df50",
            "tagCategoryName": "Test Category"
        },
        {
            "archived": true,
            "createdAt": 1705392795000,
            "createdBy": "{USER_ID}",
            "id": "92961035-e72b-45a0-9625-781380017585",
            "modifiedAt": 1705392832000,
            "modifiedBy": "{USER_ID}",
            "name": "xql-test-1705392794917",
            "org": "{ORG_ID}",
            "tagCategoryId": "e2b7c656-067b-4413-a366-adde0401df50",
            "tagCategoryName": "Test Category"
        },
        {
            "archived": false,
            "createdAt": 1705335274000,
            "createdBy": "{USER_ID}",
            "id": "436ce801-ef87-45fd-b34a-9ce938a447e1",
            "modifiedAt": 1705335274000,
            "modifiedBy": "{USER_ID}",
            "name": "xql-test-1705335252944",
            "org": "{ORG_ID}",
            "tagCategoryId": "e2b7c656-067b-4413-a366-adde0401df50",
            "tagCategoryName": "Test Category"
        },
        {
            "archived": false,
            "createdAt": 1694776514000,
            "createdBy": "{USER_ID}",
            "id": "1e6e9836-5e18-4340-a959-3206c9bc3a94",
            "modifiedAt": 1694776514000,
            "modifiedBy": "{USER_ID}",
            "name": "xql-test-1694776510734",
            "org": "{ORG_ID}",
            "tagCategoryId": "e2b7c656-067b-4413-a366-adde0401df50",
            "tagCategoryName": "Test Category"
        },
        {
            "archived": false,
            "createdAt": 1694488609000,
            "createdBy": "{USER_ID}",
            "id": "b8400673-2f90-48e9-b73b-cdfbba5ab361",
            "modifiedAt": 1694488609000,
            "modifiedBy": "{USER_ID}",
            "name": "xql-test-1694488608301",
            "org": "{ORG_ID}",
            "tagCategoryId": "e2b7c656-067b-4413-a366-adde0401df50",
            "tagCategoryName": "Test Category"
        }
    ]
}
```

+++

## 新しいタグの作成 {#create-tag}

>[!IMPORTANT]
>
>この API 呼び出しを使用して、指定したタグカテゴリに新しいタグを作成できるのは、システム管理者と製品管理者のみです。
>
>分類されていないタグを作成する場合は、管理者権限は必要ありません **&#x200B;**。

`/tags` エンドポイントにPOSTリクエストを行うことで、新しいタグを作成できます。

**API 形式**

```http
POST /tags
```

**リクエスト**

+++新しいタグを作成するリクエストのサンプル。

```shell
curl -X POST https://experience.adobe.io/unifiedtags/tags
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
 -d '{
    "name": "sampleTag"
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | **必須**。作成するタグの名前。 |
| `tagCategoryId` | *オプション*。タグが属するタグカテゴリの ID。 指定しない場合、タグは未分類カテゴリの一部として作成されます。 |

+++

**応答**

応答が成功すると、HTTP ステータス 201 が、新しく作成されたタグの詳細と共に返されます。

+++新しく作成したタグの詳細を含むサンプル応答。

```json
{
    "name": "sampleTag",
    "id": "2bd5ddd9-7284-4767-81d9-c75b122f2a6a",
    "org": "{ORG_ID}",
    "createdAt": "1661753717000",
    "createdBy": "{USER_ID}",
    "modifiedAt": "1661753717000",
    "modifiedBy": "{USER_ID}",
    "tagCategoryId": "Uncategorized-{ORG_ID}",
    "tagCategoryName": "Uncategorized",
    "archived": false
}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `name` | 新しく作成したタグの名前。 |
| `id` | 新しく作成されたタグの ID。 |
| `org` | タグが属する組織の ID。 |
| `createdAt` | タグが作成されたときのタイムスタンプ。 |
| `createdBy` | タグを作成したユーザーの ID。 |
| `modifiedAt` | タグが最後に更新されたときのタイムスタンプ。 |
| `modifiedBy` | タグを最後に更新したユーザーの ID。 |
| `tagCategoryId` | タグが属するタグカテゴリの ID。 |
| `tagCategoryName` | タグが属するタグカテゴリの名前。 |

+++

## 特定のタグの取得 {#get-tag}

`/tags` エンドポイントに対してGETリクエストを実行し、取得するタグの ID を指定することで、組織に属する特定のタグを取得できます。

**API 形式**

```http
GET /tags/{TAG_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TAG_ID}` | 取得するタグの ID。 |

**リクエスト**

+++特定のタグを取得するサンプルリクエスト

```shell
curl -X GET https://experience.adobe.io/unifiedtags/tags/2bd5ddd9-7284-4767-81d9-c75b122f2a6a \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、指定されたタグの詳細と共に返されます。

+++指定したタグの詳細を含む応答のサンプル。

```json
{
    "name": "sampleTag",
    "id": "2bd5ddd9-7284-4767-81d9-c75b122f2a6a",
    "org": "{ORG_ID}",
    "createdAt": "1661753717000",
    "createdBy": "{USER_ID}",
    "modifiedAt": "1661753717000",
    "modifiedBy": "{USER_ID}",
    "tagCategoryId": "Test Category-{ORG_ID}",
    "tagCategoryName": "Test Category",
    "archived": false
}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `name` | 取得したタグの名前。 |
| `id` | 取得したタグの ID。 |
| `org` | タグが属する組織の ID。 |
| `createdAt` | タグが作成されたときのタイムスタンプ。 |
| `createdBy` | タグを作成したユーザーの ID。 |
| `modifiedAt` | タグが最後に更新されたときのタイムスタンプ。 |
| `modifiedBy` | タグを最後に更新したユーザーの ID。 |
| `tagCategoryId` | タグが属するタグカテゴリの ID。 |
| `tagCategoryName` | タグが属するタグカテゴリの名前。 |
| `archived` | タグのアーカイブステータス。 `true` に設定すると、タグがアーカイブされます。 |

+++

## タグを検証 {#validate-tags}

`/tags/validate` エンドポイントに対してPOSTリクエストを行うことで、タグが存在するかどうかを検証できます。

**API 形式**

```http
POST /tags/validate
```

**リクエスト**

+++指定されたタグ ID を検証するサンプルリクエスト。

```shell
curl -X POST https://experience.adobe.io/unifiedtags/tags/validate
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
 -d '{
    "ids": [
        "2bd5ddd9-7284-4767-81d9-c75b122f2a6a","d113f40c-0097-4626-8d5f-6d5017694453", "invalid-tag"
    ],
    "entity": "{API_KEY}"
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `ids` | 検証するタグ ID のリストを含む配列。 |
| `entity` | 検証をリクエストするエンティティ。 このパラメーターには `{API_KEY}` の値を使用できます。 |

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、有効なタグと無効なタグに関する情報と共に返されます。

+++有効なタグと無効なタグを示す応答のサンプル。

```json
{
    "invalidTags": [
        {
            "id": "invalid-tag"
        }
    ],
    "validTags": [
        {
            "id": "d113f40c-0097-4626-8d5f-6d5017694453"
        },
        {
            "id": "2bd5ddd9-7284-4767-81d9-c75b122f2a6a"
        }
    ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `invalidTags` | 無効なタグ ID のリストを含む配列。 |
| `validTags` | 有効なタグ ID のリストを含む配列。 |

+++

## 特定のタグの更新 {#update-tag}

`/tags` エンドポイントにPATCHリクエストを実行し、更新するタグの ID を指定することで、指定したタグを更新できます。

**API 形式**

```http
PATCH /tags/{TAG_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TAG_ID}` | 更新するタグの ID。 |

**リクエスト**

+++特定のタグを更新するサンプルリクエスト

```shell
curl -X GET https://experience.adobe.io/unifiedtags/tags/2bd5ddd9-7284-4767-81d9-c75b122f2a6a \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
 -d '[{
    "op": "replace",
    "path": "name",
    "value": "newSampleTag",
    "from": "sampleTag"
 }]'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `op` | 実行する必要がある操作。 このユースケースでは、常に `replace` に設定されます。 |
| `path` | 更新されるフィールドのパス。 サポートされる値は、`name`、`archived`、`tagCategoryId` です。 |
| `value` | 更新するフィールドの更新された値。 |
| `from` | 更新するフィールドの元の値。 |

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、新しく更新されたタグの詳細と共に返されます。

+++更新されたタグの詳細を含む応答のサンプル。

```json
{
    "name": "newSampleTag",
    "id": "2bd5ddd9-7284-4767-81d9-c75b122f2a6a",
    "org": "{ORG_ID}",
    "createdAt": "1661753717000",
    "createdBy": "{USER_ID}",
    "modifiedAt": "1661753717000",
    "modifiedBy": "{USER_ID}",
    "tagCategoryId": "Test Category-{ORG_ID}",
    "tagCategoryName": "Test Category",
    "archived": false
}
```

+++

## 特定のタグの削除 {#delete-tag}

>[!IMPORTANT]
>
>この API 呼び出しを使用できるのは、システム管理者と製品管理者のみです。
>
>また、タグ **不可** はビジネスオブジェクトに関連付けることができ、タグを削除する前に **アーカイブする必要** があります。 [&#x200B; タグエンドポイントを更新 &#x200B;](#update-tag) を使用して、タグをアーカイブできます。

`/tags` のエンドポイントにタグタグを作成し、削除するDELETEの ID を指定することで、特定のタグを削除できます。

**API 形式**

```http
DELETE /tags/{TAG_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TAG_ID}` | 削除するタグの ID。 |

**リクエスト**

+++特定のタグを削除するサンプルリクエスト

```shell
curl -X DELETE https://experience.adobe.io/unifiedtags/tags/2bd5ddd9-7284-4767-81d9-c75b122f2a6a \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、空の応答と共に返されます。

## 次の手順

このガイドを読むことで、Adobe Experience Platform API を使用してタグやタグカテゴリを作成、管理、削除する方法に関する理解が深まりました。 UI を使用したタグの管理について詳しくは、[&#x200B; タグ管理ガイド &#x200B;](../ui/managing-tags.md) を参照してください。 UI を使用したタグカテゴリの管理について詳しくは、[&#x200B; タグカテゴリガイド &#x200B;](../ui/tags-categories.md) を参照してください。
