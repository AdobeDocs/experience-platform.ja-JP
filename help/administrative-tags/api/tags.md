---
title: 統合タグエンドポイント
description: Adobe Experience Platform APIを使用して、タグカテゴリとタグを作成、更新、管理、削除する方法について説明します。
role: Developer
exl-id: 6687d1da-a5e4-435a-9f99-1b0f9ac98088
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1860'
ht-degree: 4%

---

# 統合タグエンドポイント

>[!IMPORTANT]
>
>このエンドポイント セットのエンドポイント URLは`https://experience.adobe.io`です。

タグは、メタデータの分類を管理してビジネスオブジェクトを分類し、発見や分類を容易にする機能です。 その後、これらのタグをタグカテゴリに追加して、これらのタグをさらにグループに整理できます。

このガイドでは、タグとタグカテゴリをより深く理解するための情報を提供し、APIを使用して基本的なアクションを実行するためのAPI呼び出しのサンプルを含みます。

## はじめに

このガイドで使用するエンドポイントは、Adobe Experience Platform APIの一部です。 続行する前に、必須ヘッダーやサンプル API呼び出しの読み取り方法など、APIへの呼び出しを正常に行うために知っておく必要がある重要な情報については、[入門ガイド &#x200B;](./getting-started.md)を確認してください

### 用語集

次の用語集では、**タグ**&#x200B;と&#x200B;**タグカテゴリ**&#x200B;の違いを強調しています。

- **タグ**: タグを使用すると、ビジネスオブジェクトのメタデータ分類を管理でき、これらのオブジェクトを分類して簡単に検出および分類できます。
   - **未分類タグ**：未分類タグは、タグカテゴリに属しないタグです。 デフォルトでは、作成されたタグは分類されません。
- **タグ カテゴリ**: タグ カテゴリを使用すると、タグを意味のあるセットにグループ化し、タグの目的に対してより多くのコンテキストを提供できます。

## タグカテゴリのリストの取得 {#get-tag-categories}

組織に属するタグカテゴリのリストを取得するには、`/tagCategory` エンドポイントに対してGET リクエストを行います。

**API 形式**

```http
GET /tagCategory
GET /tagCategory?{QUERY_PARAMETERS}
```

タグカテゴリを取得する際には、次のオプションのクエリパラメーターを使用できます。

| クエリパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `start` | 結果のリストが始まる場所。 これを使用すると、結果のページネーションの開始インデックスを指定できます。 | `start=a` |
| `limit` | 取得する1 ページあたりのタグカテゴリの最大数。 | `limit=20` |
| `property` | タグカテゴリを取得する際にフィルタリングする属性。 サポートされる値は次のとおりです。&lt;ul≥<li>`name`: タグカテゴリの名前。</li></ul> | `property=name==category` |
| `sortBy` | タグカテゴリの並べ替え順。 サポートされている値には、`name`、`createdAt`、`modifiedAt`などがあります。 | `sortBy=name` |
| `sortOrder` | タグカテゴリを並べ替える方向。 サポートされている値には、`asc`と`desc`が含まれます。 | `sortOrder=asc` |

**リクエスト**

+++組織内のすべてのタグカテゴリを一覧表示するサンプルリクエスト

```shell
curl -X GET https://experience.adobe.io/unifiedtags/tagCategory
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200が返され、組織のすべてのタグカテゴリのリストが表示されます。

+++組織内のすべてのタグカテゴリのリストを含むサンプル応答。

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
>このAPI呼び出しを使用できるのは、システム管理者と製品管理者のみです。

`/tagCategory` エンドポイントにPOST リクエストを行うことで、新しいタグカテゴリを作成できます。

**API 形式**

```http
POST /tagCategory
```

**リクエスト**

+++新しいタグカテゴリを作成するためのサンプルリクエスト。

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

サンプル応答は、新しく作成されたタグカテゴリの詳細を含むHTTP ステータス 200を返します。

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

組織に属する特定のタグカテゴリを取得するには、`/tagCategory` エンドポイントに対してGET リクエストを実行し、タグカテゴリのIDを指定します。

**API 形式**

```http
GET /tagCategory/{TAG_CATEGORY_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TAG_CATEGORY_ID}` | 取得するタグカテゴリのID。 |

**リクエスト**

+++特定のタグカテゴリを取得するためのサンプルリクエスト

```shell
curl -X GET https://experience.adobe.io/unifiedtags/tagCategory/e2b7c656-067b-4413-a366-adde0401df50 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200が返され、指定されたタグカテゴリの詳細が表示されます。

+++指定されたタグカテゴリの詳細を含むサンプル応答。

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
| `id` | リクエストされたタグカテゴリのID。 |
| `name` | リクエストされたタグカテゴリの名前。 |
| `description` | 要求されたタグカテゴリの説明。 |
| `createdBy` | タグカテゴリを作成したユーザーのID。 |
| `createdAt` | タグカテゴリが作成されたときのタイムスタンプ。 |
| `modifiedBy` | タグ カテゴリを最後に更新したユーザーのID。 |
| `modifiedAt` | タグカテゴリが最後に更新された際のタイムスタンプ。 |
| `tagCount` | タグカテゴリに属するタグの数。 |

+++

## 特定のタグカテゴリの更新 {#update-tag-category}

>[!IMPORTANT]
>
>このAPI呼び出しを使用できるのは、システム管理者と製品管理者のみです。

組織に属する特定のタグカテゴリの詳細を更新するには、`/tagCategory` エンドポイントに対してPATCH リクエストを実行し、タグカテゴリのIDを指定します。

**API 形式**

```http
PATCH /tagCategory/{TAG_CATEGORY_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TAG_CATEGORY_ID}` | 取得するタグカテゴリのID。 |

**リクエスト**

+++特定のタグカテゴリを更新するためのサンプルリクエスト

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
| `op` | 完了した操作。 特定のタグカテゴリを更新するには、この値を`replace`に設定します。 |
| `path` | 更新されるフィールドのパス。 サポートされている値には、`name`と`description`が含まれます。 |
| `value` | 更新するフィールドの更新された値。 |
| `from` | 更新するフィールドの元の値。 |

+++

**応答**

応答が成功した場合は、HTTP ステータス 200と、新しく更新されたタグカテゴリに関する情報が表示されます。

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
>このAPI呼び出しを使用できるのは、システム管理者と製品管理者のみです。

組織に属する特定のタグカテゴリを削除するには、`/tagCategory` エンドポイントに対してDELETE リクエストを実行し、タグカテゴリのIDを指定します。

**API 形式**

```http
DELETE /tagCategory/{TAG_CATEGORY_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TAG_CATEGORY_ID}` | 取得するタグカテゴリのID。 |

**リクエスト**

+++特定のタグカテゴリを削除するためのサンプルリクエスト

```shell
curl -X DELETE https://experience.adobe.io/unifiedtags/tagCategory/e2b7c656-067b-4413-a366-adde0401df50 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200と空の応答が返されます。

## タグのリストの取得 {#get-tags}

組織に属するタグのリストを取得するには、`/tags` エンドポイントとタグカテゴリのIDに対してGET リクエストを行います。

**API 形式**

```http
GET /tags
GET /tags?{QUERY_PARAMETERS}
```

タグを取得する際には、次のオプションのクエリパラメーターを使用できます。

| クエリパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `start` | 結果のリストが始まる場所。 これを使用すると、結果のページネーションの開始インデックスを指定できます。 | `start=a` |
| `limit` | 取得する1 ページあたりのタグの最大数。 | `limit=20` |
| `property` | タグの取得時にフィルタリングする属性。 サポートされる値は次のとおりです。<ul><li>`name`: タグの名前。</li><li>`archived`: タグがアーカイブされているかどうか、またはアーカイブされていないかどうか。 この値は、`true`または`false`のいずれかに設定できます。</li><li>`tagCategoryId`: タグが属するタグカテゴリのID。</li></ul> | <ul><li>`property=name==TestTag`</li><li>`property=archived==false`</li><li>`property=tagCategoryId==e2b7c656-067b-4413-a366-adde0401df50`</li> |
| `sortBy` | タグの並べ替え順。 サポートされている値には、`name`、`createdAt`、`modifiedAt`などがあります。 | `sortBy=name` |
| `sortOrder` | タグカテゴリを並べ替える方向。 サポートされている値には、`asc`と`desc`が含まれます。 | `sortOrder=asc` |


**リクエスト**

+++特定のタグカテゴリに属するすべてのタグを取得するためのサンプルリクエスト

```shell
curl -X GET https://experience.adobe.io/unifiedtags/tags?property=tagCategoryId=e2b7c656-067b-4413-a366-adde0401df50
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200が、そのタグカテゴリに属するタグの詳細とともに返されます。

+++ リクエストされたタグの詳細を含むサンプル応答。

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

## 新しいタグを作成 {#create-tag}

>[!IMPORTANT]
>
>このAPI呼び出しを使用して、指定したタグカテゴリに新しいタグを作成できるのは、システム管理者と製品管理者のみです。
>
>分類されていないタグを作成する場合は、**not**&#x200B;に管理者権限が必要です。

`/tags` エンドポイントにPOST リクエストを行うことで、新しいタグを作成できます。

**API 形式**

```http
POST /tags
```

**リクエスト**

+++新しいタグを作成するためのサンプルリクエスト。

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
| `tagCategoryId` | *オプション*。タグが属するタグカテゴリのID。 指定しない場合、タグは未分類カテゴリの一部として作成されます。 |

+++

**応答**

応答が成功すると、HTTP ステータス 201が、新しく作成したタグの詳細とともに返されます。

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
| `id` | 新しく作成したタグのID。 |
| `org` | タグが属する組織のID。 |
| `createdAt` | タグが作成されたときのタイムスタンプ。 |
| `createdBy` | タグを作成したユーザーのID。 |
| `modifiedAt` | タグが最後に更新されたときのタイムスタンプ。 |
| `modifiedBy` | タグを最後に更新したユーザーのID。 |
| `tagCategoryId` | タグが属するタグカテゴリのID。 |
| `tagCategoryName` | タグが属するタグカテゴリの名前。 |

+++

## 特定のタグの取得 {#get-tag}

組織に属する特定のタグを取得するには、`/tags` エンドポイントに対してGET リクエストを行い、取得するタグのIDを指定します。

**API 形式**

```http
GET /tags/{TAG_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TAG_ID}` | 取得するタグのID。 |

**リクエスト**

+++特定のタグを取得するためのサンプルリクエスト

```shell
curl -X GET https://experience.adobe.io/unifiedtags/tags/2bd5ddd9-7284-4767-81d9-c75b122f2a6a \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200が、指定されたタグの詳細とともに返されます。

+++指定されたタグの詳細を含むサンプル応答。 

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
| `id` | 取得したタグのID。 |
| `org` | タグが属する組織のID。 |
| `createdAt` | タグが作成されたときのタイムスタンプ。 |
| `createdBy` | タグを作成したユーザーのID。 |
| `modifiedAt` | タグが最後に更新されたときのタイムスタンプ。 |
| `modifiedBy` | タグを最後に更新したユーザーのID。 |
| `tagCategoryId` | タグが属するタグカテゴリのID。 |
| `tagCategoryName` | タグが属するタグカテゴリの名前。 |
| `archived` | タグのアーカイブステータス。 `true`に設定されている場合、タグがアーカイブされていることを意味します。 |

+++

## タグの検証 {#validate-tags}

タグが存在するかどうかを検証するには、`/tags/validate` エンドポイントにPOST リクエストを行います。

**API 形式**

```http
POST /tags/validate
```

**リクエスト**

+++提供されたタグ IDを検証するためのサンプルリクエスト。

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
| `ids` | 検証するタグ IDのリストを含む配列。 |
| `entity` | 検証を要求しているエンティティ。 このパラメーターには`{API_KEY}`値を使用できます。 |

+++

**応答**

応答が成功すると、HTTP ステータス 200が、有効なタグと無効なタグに関する情報とともに返されます。

+++有効なタグと無効なタグを示すサンプル応答。

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
| `invalidTags` | 無効なタグ IDのリストを含む配列。 |
| `validTags` | 有効なタグ IDのリストを含む配列。 |

+++

## 特定のタグの更新 {#update-tag}

指定したタグを更新するには、`/tags` エンドポイントに対してPATCH リクエストを行い、更新するタグのIDを指定します。

**API 形式**

```http
PATCH /tags/{TAG_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TAG_ID}` | 更新するタグのID。 |

**リクエスト**

+++特定のタグを更新するためのサンプルリクエスト

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
| `op` | 行う必要がある操作。 この使用例では、常に`replace`に設定されます。 |
| `path` | 更新されるフィールドのパス。 サポートされている値には、`name`、`archived`、`tagCategoryId`などがあります。 |
| `value` | 更新するフィールドの更新された値。 |
| `from` | 更新するフィールドの元の値。 |

+++

**応答**

応答が成功すると、HTTP ステータス 200が、新しく更新されたタグの詳細とともに返されます。

+++更新されたタグの詳細を含むサンプル応答。

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
>このAPI呼び出しを使用できるのは、システム管理者と製品管理者のみです。
>
>さらに、タグ **をビジネスオブジェクトに関連付けることはできません**。タグを削除する前に、**をアーカイブする必要があります**。 タグは、[更新タグエンドポイント &#x200B;](#update-tag)を使用してアーカイブできます。

DELETE タグを`/tags` エンドポイントに作成し、削除するタグのIDを指定することで、特定のタグを削除できます。

**API 形式**

```http
DELETE /tags/{TAG_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TAG_ID}` | 削除するタグのID。 |

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

応答が成功すると、HTTP ステータス 200と空の応答が返されます。

## 次の手順

このガイドでは、Adobe Experience Platform APIを使用して、タグとタグのカテゴリを作成、管理、削除する方法について詳しく説明します。 UIを使用したタグの管理について詳しくは、[&#x200B; タグの管理ガイド &#x200B;](../ui/managing-tags.md)を参照してください。 UIを使用したタグカテゴリの管理について詳しくは、[&#x200B; タグカテゴリガイド &#x200B;](../ui/tags-categories.md)を参照してください。
