---
title: サンドボックスツールパッケージ API エンドポイント
description: サンドボックスツール API の/packages エンドポイントを使用すると、Adobe Experience Platformのパッケージをプログラムで管理できます。
exl-id: 46efee26-d897-4941-baf4-d5ca0b8311f0
source-git-commit: f81e15ccfd89e2d0cb450f596743341264187f52
workflow-type: tm+mt
source-wordcount: '1621'
ht-degree: 10%

---

# パッケージエンドポイント

サンドボックスツールを使用すると、様々なアーティファクト（オブジェクトとも呼ばれます）を選択し、それらをパッケージに書き出すことができます。 パッケージは、単一のアーティファクトまたは複数のアーティファクト（データセットやスキーマなど）で構成できます。 パッケージに含まれるアーティファクトは、同じサンドボックスからのアーティファクトである必要があります。

サンドボックスツール API の `/packages` エンドポイントを使用すると、パッケージの公開やサンドボックスへのパッケージの読み込みなど、組織内のパッケージをプログラムによって管理できます。

## パッケージを作成 {#create}

パッケージの名前とパッケージタイプの値を指定しながら `/packages` エンドポイントに対してPOSTリクエストを行うことで、複数のアーティファクトからなるパッケージを作成できます。

**API 形式**

```http
POST /packages/
```

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/exim/packages \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d'{
      "name": "acme",
      "description": "Acme Business Group",
      "packageType": "PARTIAL",    
      "sourceSandbox": {
        "name": "acme-sandbox",
        "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg"
      },
      "expiry": "2023-05-20T20:05:10Z",
      "artifacts": [
        {
          "id": "27115daa-c92b-4f17-a077-d65ffeb0c525",
          "type": "PROFILE_SEGMENT",
          "title": "Acme Profile Segment"
        }      
      ]
  }'
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `name` | パッケージの名前。 | 文字列 | ○ |
| `description` | パッケージの詳細情報を提供する説明。 | 文字列 | × |
| `packageType` | パッケージタイプは **PARTIAL** で、特定のアーティファクトをパッケージに含めることを示します。 | 文字列 | はい |
| `sourceSandbox` | パッケージのソースサンドボックス。 | 文字列 | × |
| `expiry` | パッケージの有効期限を定義するタイムスタンプ。 デフォルト値は作成日から 90 日間です。 応答の有効期限フィールドはエポック UTC 時間になります。 | 文字列（UTC タイムスタンプ形式） | × |
| `artifacts` | パッケージにエクスポートされるアーティファクトのリスト。 `packageType` が `FULL` の場合、`artifacts` 値は **null** または **空** にする必要があります。 | 配列 | × |

**応答**

応答が成功すると、新しく作成したパッケージが返されます。 応答には、対応するパッケージ ID と、そのステータス、有効期限およびアーティファクトのリストに関する情報が含まれます。

```json
{
    "id": "209f886b00444eac9bb5836fe32e7681",
    "version": 0,
    "createdDate": 1684475012105,
    "modifiedDate": 1684475012105,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}",
    "tenantId": "c875b077162b40409c1327b16da99c1b",
    "requestId": "devxa54a6b56d04f46119d9e3cc006fcc1cb",
    "userId": "platform_exim",
    "name": "acme",
    "description": "Acme Business Group",
    "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg",     
    "sourceSandbox": {
        "name": "cjm-mr",
        "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg"
    },     
    "packageType": "PARTIAL",
    "expiry": 1684613110000,
    "status": "DRAFT",    
    "artifactsList": [
        {
            "id": "d8d8ed6d-696a-40bd-b4fe-ca053ec94e29",
            "type": "JOURNEY",
            "found": false,
            "count": 0
        }
    ]
}
```

## パッケージの更新 {#update}

`/packages` エンドポイントに対してPUTリクエストを実行することで、パッケージを更新できます。

### パッケージへのアーティファクトの追加 {#add-artifacts}

アーティファクトをパッケージに追加するには、`id` を指定し、`action` に **ADD** を含める必要があります。

**API 形式**

```http
PUT /packages/
```

**リクエスト**

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/exim/packages \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d'{
      "id": "6fa50baedd344a278129a87e68cc9dc7",
      "action": "ADD",
      "expiry": "2023-05-20T20:05:10Z", 
      "artifacts": [
        {
         "id": "d8d8ed6d-696a-40bd-b4fe-ca053ec94e29@1647559351683",
         "type": "JOURNEY"
        }      
      ]
  }'
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `id` | 更新するパッケージの ID。 | 文字列 | ○ |
| `action` | アーティファクトをパッケージに追加するには、アクション値を **ADD** にする必要があります。 このアクションは、**PARTIAL** パッケージタイプでのみサポートされます。 | 文字列 | ○ |
| `artifacts` | パッケージに追加するアーティファクトのリスト。 リストが **null** または **空** の場合、パッケージは変更されません。 アーティファクトは、パッケージに追加される前に重複排除されます。 サポートされるアーティファクトの完全なリストについては、次の表を参照してください。 | 配列 | × |
| `expiry` | パッケージの有効期限を定義するタイムスタンプ。 ペイロードに有効期限が指定されていない場合、PUTAPI が呼び出されてから 90 日のデフォルト値です。 応答の有効期限フィールドはエポック UTC 時間になります。 | 文字列（UTC タイムスタンプ形式） | × |

現在、次のアーティファクトタイプがサポートされています。

| アーティファクト | Platform | オブジェクト | 部分フロー | 完全なサンドボックス |
| --- | --- | --- | --- | --- |
| `JOURNEY` | Adobe Journey Optimizer | ジャーニー | ○ | × |
| `ID_NAMESPACE` | 顧客データプラットフォーム | ID | ○ | ○ |
| `REGISTRY_DATATYPE` | 顧客データプラットフォーム | データタイプ | ○ | ○ |
| `REGISTRY_CLASS` | 顧客データプラットフォーム | クラス | ○ | ○ |
| `REGISTRY_MIXIN` | 顧客データプラットフォーム | フィールドグループ | ○ | ○ |
| `REGISTRY_SCHEMA` | 顧客データプラットフォーム | スキーマ | ○ | ○ |
| `CATALOG_DATASET` | 顧客データプラットフォーム | データセット | ○ | ○ |
| `DULE_CONSENT_POLICY` | 顧客データプラットフォーム | 同意およびガバナンスポリシー | ○ | ○ |
| `PROFILE_SEGMENT` | 顧客データプラットフォーム | オーディエンス | ○ | ○ |
| `FLOW` | 顧客データプラットフォーム | ソースデータフロー | ○ | ○ |

**応答**

応答が成功すると、更新されたパッケージが返されます。 応答には、対応するパッケージ ID と、そのステータス、有効期限およびアーティファクトのリストに関する情報が含まれます。

```json
{
    "id": "6fa50baedd344a278129a87e68cc9dc7",
    "version": 4,
    "createdDate": 1684235842000,
    "modifiedDate": 1684475861366,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}",
    "tenantId": "c875b077162b40409c1327b16da99c1b",
    "name": "acme",
    "description": "Acme Business Group",
    "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg",     
    "sourceSandbox": {
        "name": "acme-sandbox",
        "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg"
    },     
    "packageType": "PARTIAL",
    "expiry": 1692251861352,
    "status": "DRAFT",    
    "artifactsList": [
        {
            "id": "d8d8ed6d-696a-40bd-b4fe-ca053ec94e29@1647559351683",
            "type": "JOURNEY",
            "found": false,
            "count": 0
        },
        {
            "id": "d8d8ed6d-696a-40bd-b4fe-ca053ec94e29",
            "type": "JOURNEY",
            "found": false,
            "count": 0
        }
    ]
}
```

### パッケージからのアーティファクトの削除 {#delete-artifacts}

パッケージからアーティファクトを削除するには、`id` を指定し、`action` に **DELETE** を含める必要があります。


**API 形式**

```http
PUT /packages/
```

**リクエスト**

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/exim/packages \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d'{
      "id": "6fa50baedd344a278129a87e68cc9dc7",
      "action": "DELETE",
      "artifacts": [
        {
          "id": "d8d8ed6d-696a-40bd-b4fe-ca053ec94e29@1647559351683",
          "type": "JOURNEY"
        }      
      ]
  }'
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `id` | 更新するパッケージの ID。 | 文字列 | ○ |
| `action` | パッケージからアーティファクトを削除するには、アクション値を **DELETE** にする必要があります。 このアクションは、**PARTIAL** パッケージタイプでのみサポートされます。 | 文字列 | ○ |
| `artifacts` | パッケージから削除されるアーティファクトのリスト。 リストが **null** または **空** の場合、パッケージは変更されません。 | 配列 | × |

**応答**

応答が成功すると、更新されたパッケージが返されます。 応答には、対応するパッケージ ID と、そのステータス、有効期限およびアーティファクトのリストに関する情報が含まれます。

```json
{
    "id": "6fa50baedd344a278129a87e68cc9dc7",
    "version": 5,
    "createdDate": 1684235842000,
    "modifiedDate": 1684478830416,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}",
    "tenantId": "c875b077162b40409c1327b16da99c1b",
    "name": "acme",
    "description": "Acme Business Group",
    "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg",     
    "sourceSandbox": {
        "name": "acme-sandbox",
        "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg"
    },      
    "packageType": "PARTIAL",
    "expiry": 1692254830403,
    "status": "DRAFT",    
    "artifactsList": [
        {
            "id": "d8d8ed6d-696a-40bd-b4fe-ca053ec94e29",
            "type": "JOURNEY",
            "found": false,
            "count": 0
        }
    ]
}
```

### パッケージ内のメタデータフィールドの更新 {#update-metadata}

>[!NOTE]
>
>**UPDATE** アクションは、パッケージのパッケージメタデータフィールドを更新するために使用され、アーティファクトをパッケージに追加または削除するために使用 **できません**。

パッケージ内のメタデータフィールドを更新するには、`id` を指定し、`action` の **UPDATE** を含める必要があります。

**API 形式**

```http
PUT /packages/
```

**リクエスト**

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/exim/packages \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d'{
      "id": "6fa50baedd344a278129a87e68cc9dc7",
      "action": "UPDATE",
      "name": "acme",
      "description": "Acme Business Group",  
      "sourceSandbox": {
        "name": "acme-sandbox",
        "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg"
      }
  }'
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `id` | 更新するパッケージの ID。 | 文字列 | ○ |
| `action` | パッケージのメタデータフィールドを更新するには、アクション値を **UPDATE** にする必要があります。 このアクションは、**PARTIAL** パッケージタイプでのみサポートされます。 | 文字列 | ○ |
| `name` | パッケージの更新された名前。 パッケージ名の重複は許可されていません。 | 配列 | ○ |
| `sourceSandbox` | Source サンドボックスは、リクエストのヘッダーで指定されたのと同じ組織に属している必要があります。 | 文字列 | ○ |

**応答**

応答が成功すると、更新されたパッケージが返されます。 応答には、対応するパッケージ ID に加え、説明、ステータス、有効期限およびアーティファクトのリストに関する情報が含まれます。

```json
{
    "id": "6fa50baedd344a278129a87e68cc9dc7",
    "version": 6,
    "createdDate": 1684235842000,
    "modifiedDate": 1684479094129,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}",
    "tenantId": "c875b077162b40409c1327b16da99c1b",
    "name": "acme",
    "description": "Acme Business Group",
    "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg",    
    "sourceSandbox": {
        "name": "acme-sandbox",
        "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg"
    },
    "packageType": "PARTIAL",
    "expiry": 1692255094127,
    "status": "DRAFT",    
    "artifactsList": [
        {
            "id": "d8d8ed6d-696a-40bd-b4fe-ca053ec94e29",
            "type": "JOURNEY",
            "found": false,
            "count": 0
        }
    ]
}
```

## パッケージの削除 {#delete}

パッケージを削除するには、`/packages` エンドポイントに対してDELETEリクエストを実行し、削除するパッケージの ID を指定します。

**API 形式**

```http
DELETE /packages/{PACKAGE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {PACKAGE_ID} | 削除するパッケージの ID。 |

**リクエスト**

次のリクエストでは、ID が {PACKAGE_ID} のパッケージが削除されます。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

応答が成功すると、理由が返され、パッケージ ID が削除されたことが示されます。

```json
{
    "reason": "Package d30e0424a37b46ada6a5cf37f47a86ff deleted"
}
```

## パッケージのPublish {#publish}

パッケージをサンドボックスにインポートできるようにするには、パッケージを公開する必要があります。 公開するパッケージの ID を指定したうえで、`/packages` エンドポイントに対してGETリクエストを行います。

**API 形式**

```http
GET /packages/{PACKAGE_ID}/export
```

| パラメーター | 説明 |
| --- | --- |
| {PACKAGE_ID} | 公開するパッケージの ID。 |

**リクエスト**

次のリクエストでは、{PACKAGE_ID} という ID でパッケージが公開されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID}\export \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `expiryPeriod` | このユーザー指定の期間は、パッケージが公開された時点からのパッケージの有効期限（日数）を定義します。 この値は負の値にはできません。<br> 値を指定しない場合、デフォルトは公開日から 90 （日）として計算されます。 | 整数 | × |

**応答**

応答が成功すると、公開されたパッケージが返されます。

```json
{
    "name": "acme",
    "description": "Acme Business Group",
    "visibility": "TENANT",
    "sourceSandbox":
    {
        "name": "acme-sandbox",
        "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg"
    },
    "type": "PARTIAL",
    "correlationId": "48effe5e-1bef-4250-9c71-23b93ef5d285"
}
```

## パッケージの検索 {#look-up-package}

個々のパッケージを検索するには、リクエストパスにパッケージの対応する ID を含む `/packages` エンドポイントに対してGETリクエストを実行します。

**API 形式**

```http
GET /packages/{PACKAGE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {PACKAGE_ID} | 検索するパッケージの ID。 |

**リクエスト**

次のリクエストは、{PACKAGE_ID} の情報を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

応答が成功すると、クエリされたパッケージ ID の詳細が返されます。 応答には、名前、説明、公開日と有効期限、パッケージのソースサンドボックス、アーティファクトのリストが含まれます。

```json
{
    "id": "8f585fad94d042cd82dbcba594108a41",
    "version": 2,
    "createdDate": 1685597784000,
    "modifiedDate": 1685597810000,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}",
    "tenantId": "c875b077162b40409c1327b16da99c1b",
    "name": "acme",
    "description": "Acme Business Group",
    "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg",
    "packageType": "PARTIAL",
    "expiry": 1693373810000,
    "publishDate": 1685597810000,
    "status": "PUBLISHED",
    "artifactsList": [
        {
            "id": "f4f57771-2bd2-469a-9c13-8d803eeb6515",
            "type": "JOURNEY",
            "found": false,
            "count": 0
        },
        {
            "id": "7f4caca7-a477-400d-a41e-c4735f8e780d",
            "type": "JOURNEY",
            "found": false,
            "count": 0
        }
    ],
    "sourceSandbox": {
        "name": "acme-sandbox",
        "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg"
    }
}
```

## パッケージのリスト {#list-packages}

`/packages` エンドポイントに対してGETリクエストをおこなうと、組織内のすべてのパッケージをリストできます。

**API 形式**

```http
GET /packages/?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| {QUERY_PARAMS} | 結果をフィルタリングするオプションのクエリパラメーター。 詳しくは、[ クエリパラメーター ](./appendix.md) の節を参照してください。 |

**リクエスト**

次のリクエストは、{QUERY_PARAMS} に基づいてパッケージの情報を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/?property=status==DRAFT,PUBLISHED&property=createdDate>=2023-05-11T18:29:59.999Z&property=createdDate<=2023-05-16T18:29:59.999Z&start=0&orderby=-createdDate&limit=20 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答が成功すると、名前、ステータス、有効期限、アーティファクトリストなどの詳細を含む、組織に属するパッケージのリストが返されます。

```json
{
    "totalElements": 109,
    "currentPage": 0,
    "totalPages": 6,
    "hasPreviousPage": false,
    "hasNextPage": true,
    "data": [
        {
            "id": "8f585fad94d042cd82dbcba594108a41",
            "version": 2,
            "createdDate": 1685597784000,
            "modifiedDate": 1685597810000,
            "createdBy": "{CREATED_BY}",
            "modifiedBy": "{MODIFIED_BY}",
            "tenantId": "c875b077162b40409c1327b16da99c1b",
            "name": "acme",
            "description": "Acme Business Group",
            "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg",
            "packageType": "PARTIAL",
            "expiry": 1693373810000,
            "publishDate": 1685597810000,
            "status": "PUBLISHED",
            "artifactsList": [
                {
                    "id": "f4f57771-2bd2-469a-9c13-8d803eeb6515",
                    "type": "JOURNEY",
                    "found": false,
                    "count": 0
                },
                {
                    "id": "7f4caca7-a477-400d-a41e-c4735f8e780d",
                    "type": "JOURNEY",
                    "found": false,
                    "count": 0
                }
            ],
            "sourceSandbox": {
                "name": "acme-sandbox",
                "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg"
            }
        },
        {
            "id": "0d7e427ce4cb4dc1b78e30ef61b125c1",
            "version": 2,
            "createdDate": 1685555213000,
            "modifiedDate": 1685555275000,
            "createdBy": "{CREATED_BY}",
            "modifiedBy": "{MODIFIED_BY}",
            "tenantId": "7d7d8bbe3c7c4a8ea701cc5e42c57aeb",
            "name": "acme",
            "description": "Acme Business Group",
            "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg",
            "packageType": "PARTIAL",
            "expiry": 1693331275000,
            "publishDate": 1685555275000,
            "status": "PUBLISHED",
            "artifactsList": [
                {
                    "id": "626a9669a9f5b818db270e95",
                    "type": "CATALOG_DATASET",
                    "found": false,
                    "count": 0
                }
            ],
            "sourceSandbox": {
                "name": "acme-sandbox",
                "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg"
            }
        }
    ]
}
```

## パッケージのインポート {#import}

このエンドポイントは、指定されたターゲットサンドボックスで競合するオブジェクトを取得するために使用されます。 競合するオブジェクトは、ターゲットサンドボックスに既に存在する類似のオブジェクトを表します。

**API 形式**

```http
GET /packages/{PACKAGE_ID}/import?targetSandbox=targetSandboxName
```

| パラメーター | 説明 |
| --- | --- |
| {PACKAGE_ID} | 検索するパッケージの ID。 |

**リクエスト**

次のリクエストでは、{PACKAGE_ID} を読み込みます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID}/import?targetSandbox=targetSandboxName \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

競合は、応答で返されます。 この応答では、元のパッケージと、ランキング順の配列として `alternatives` フラグメントが表示されます。

応答を表示+++

```json
[
    {
        "artifact": {
            "id": "https://ns.adobe.com/cjmstage/schemas/20121c2110bb2c6a585baabe5f82994577da1f7d0628234c",
            "type": "REGISTRY_SCHEMA",
            "found": false,
            "count": 0,
            "messages": [
                {
                    "status": "FOUND",
                    "attempt": 1,
                    "message": "Found object with ID: https://ns.adobe.com/cjmstage/schemas/20121c2110bb2c6a585baabe5f82994577da1f7d0628234c"
                }
            ]
        },
        "suggestionList": [
            {
                "id": "https://ns.adobe.com/cjmstage/schemas/176f33f6a8ff6542de1256f8dc01cce4be1b3a68fd5f5bc5",
                "type": "REGISTRY_SCHEMA",
                "found": false,
                "count": 0,
                "title": "Dean Dataset 1 - adhoc schema - 1618950408870_1686403052050"
            },
            {
                "id": "https://ns.adobe.com/cjmstage/schemas/1b37c3403e4e12c7aa46ea9dfe380a9f2b72d4da9db62b46",
                "type": "REGISTRY_SCHEMA",
                "found": false,
                "count": 0,
                "title": "Dean Dataset 1 - adhoc schema - 1618950408870_1686218766627"
            }
        ],
        "parentID": "745F37C35E4B776E0A49421B@AdobeOrg::prod::REGISTRY_SCHEMA::https://ns.adobe.com/cjmstage/schemas/20121c2110bb2c6a585baabe5f82994577da1f7d0628234c"
    },
    {
        "artifact": {
            "id": "https://ns.adobe.com/cjmstage/classes/24c1525f4f06fae2d203c6b78e26ae479ec4541c2c0d6b26",
            "type": "REGISTRY_CLASS",
            "found": false,
            "count": 0,
            "messages": [
                {
                    "status": "FOUND",
                    "attempt": 1,
                    "message": "Found object with ID: https://ns.adobe.com/cjmstage/classes/24c1525f4f06fae2d203c6b78e26ae479ec4541c2c0d6b26"
                }
            ]
        },
        "suggestionList": [
            {
                "id": "https://ns.adobe.com/cjmstage/classes/1dd81d61cdaa89a89382d0a424db77494475bd1db3105feb",
                "type": "REGISTRY_CLASS",
                "found": false,
                "count": 0,
                "title": "Dean Dataset 1 - Adhoc class - 1618950408870_1686564843736"
            },
            {
                "id": "https://ns.adobe.com/cjmstage/classes/2511fb5396a630b2cd3d5d9e9b69d42ce66a4289db8ac917",
                "type": "REGISTRY_CLASS",
                "found": false,
                "count": 0,
                "title": "Dean Dataset 1 - Adhoc class - 1618950408870_1686408152916"
            }
        ],
        "parentID": "745F37C35E4B776E0A49421B@AdobeOrg::prod::REGISTRY_CLASS::https://ns.adobe.com/cjmstage/classes/24c1525f4f06fae2d203c6b78e26ae479ec4541c2c0d6b26"
    },
    {
        "artifact": {
            "id": "4d4c874ec3344d64bf8b3160e60ac78b",
            "type": "MAPPING_SET",
            "found": false,
            "count": 0,
            "messages": [
                {
                    "status": "FOUND",
                    "attempt": 1,
                    "message": "Found object with ID: 4d4c874ec3344d64bf8b3160e60ac78b"
                }
            ]
        },
        "suggestionList": [
            {
                "id": "6b49c959d77c48e9904f7616fe2e7848",
                "type": "MAPPING_SET",
                "found": false,
                "count": 0,
                "title": ""
            },
            {
                "id": "7b363da9e6704afb9716f57193d5246f",
                "type": "MAPPING_SET",
                "found": false,
                "count": 0,
                "title": ""
            },
            {
                "id": "93ca0b4f437c4eaf9f1986408679e965",
                "type": "MAPPING_SET",
                "found": false,
                "count": 0,
                "title": ""
            }
        ],
        "parentID": "745F37C35E4B776E0A49421B@AdobeOrg::prod::MAPPING_SET::4d4c874ec3344d64bf8b3160e60ac78b"
    }
]
```

+++

## 読み込みを送信 {#submit-import}

>[!NOTE]
>
>代替アーティファクトがターゲットサンドボックスに既に存在することは、競合解決に固有の処理です。

競合や置き換えを確認した後、`/packages` エンドポイントに対してPOSTリクエストを行うことで、パッケージの読み込みを送信できます。 結果はペイロードとして提供され、ペイロードで指定された宛先サンドボックスの読み込みジョブが開始されます。

ペイロードは、インポートジョブのユーザー指定ジョブ名と説明も受け付けます。 ユーザーが指定した名前と説明を使用できない場合は、パッケージ名と説明がジョブの名前と説明に使用されます。

**API 形式**

```http
POST /packages/import
```

**リクエスト**

次のリクエストでは、読み込むパッケージを取得します。 ペイロードは置き換えのマップで、エントリが存在する場合、キーはパッケージが提供する `artifactId`、代替値はの値です。 マップまたはペイロードが **空** の場合、代用は実行されません。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/exim/packages/import/ \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d'{
       "id": "09484a599f5f4a5faa43986643964615",
       "name": "acme",
       "description": "Acme Business Group",
       "destinationSandbox": {
         "name": "cjm-mr",
         "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg"
     },
     "alternatives": {
       "https://ns.adobe.com/cjmstage/schemas/ac33bbd22eb4ad6656e1c7e12e9f520261fb39fd28a902a9": {
         "id": "https://ns.adobe.com/cjmstage/schemas/a3b935344685afad4e52c753161cf673ec23d4fb1b3e9ce",
         "type": "REGISTRY_SCHEMA"
       }
     }
  }'
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `alternatives` | ソースサンドボックスアーティファクトから既存のターゲットサンドボックスアーティファクトへのマッピングを表すことがで `alternatives` ます。 読み込みジョブでは、これらのアーティファクトは既にターゲットサンドボックスに作成されているので、作成されません。 | 文字列 | × |

**応答**

```json
{
    "name": "acme",
    "description": "Acme Business Group",
    "visibility": "TENANT",
    "sourceSandbox":
    {
        "name": "acme-sandbox",
        "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg"
    },
    "destinationSandbox":
    {
        "name": "acme-sandbox",
        "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg"
    },
    "type": "PARTIAL",
    "correlationId": "48effe5e-1bef-4250-9c71-23b93ef5d285"
}
```

## すべての依存オブジェクトのリスト {#dependent-objects}

パッケージの ID を指定して `/packages` エンドポイントにPOSTリクエストを行い、パッケージ内の書き出されたオブジェクトのすべての依存オブジェクトをリストします。

**API 形式**

```http
POST /packages/{PACKAGE_ID}/children
```

| パラメーター | 説明 |
| --- | --- |
| {PACKAGE_ID} | パッケージの ID。 |

**リクエスト**

次のリクエストでは、{PACKAGE_ID} のすべての依存オブジェクトをリストします。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID}/import?targetSandbox=targetSandboxName \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -d'[
      {
        "id": "4d4c874ec3344d64bf8b3160e60ac78b",
        "type": "MAPPING_SET"
      },
      {
        "id": "https://ns.adobe.com/cjmstage/schemas/20121c2110bb2c6a585baabe5f82994577da1f7d0628234c",
        "type": "REGISTRY_SCHEMA"
      },
      {
        "id": "https://ns.adobe.com/cjmstage/classes/24c1525f4f06fae2d203c6b78e26ae479ec4541c2c0d6b26",
        "type": "REGISTRY_CLASS"
      }
  ]'
```

**応答**

応答が成功すると、オブジェクトの子のリストが返されます。

```json
[
    {
        "id": "4d4c874ec3344d64bf8b3160e60ac78b",
        "title": "4d4c874ec3344d64bf8b3160e60ac78b",
        "type": "MAPPING_SET",
        "children": [
            {
                "id": "https://ns.adobe.com/cjmstage/schemas/20121c2110bb2c6a585baabe5f82994577da1f7d0628234c",
                "title": "Dean Dataset 1 - adhoc schema - 1618950408870",
                "type": "REGISTRY_SCHEMA"
            }
        ]
    },
    {
        "id": "https://ns.adobe.com/cjmstage/schemas/20121c2110bb2c6a585baabe5f82994577da1f7d0628234c",
        "title": "Dean Dataset 1 - adhoc schema - 1618950408870",
        "type": "REGISTRY_SCHEMA",
        "children": [
            {
                "id": "https://ns.adobe.com/cjmstage/classes/24c1525f4f06fae2d203c6b78e26ae479ec4541c2c0d6b26",
                "title": "Dean Dataset 1 - Adhoc class - 1618950408870",
                "type": "REGISTRY_CLASS"
            }
        ]
    },
    {
        "id": "https://ns.adobe.com/cjmstage/classes/24c1525f4f06fae2d203c6b78e26ae479ec4541c2c0d6b26",
        "title": "Dean Dataset 1 - Adhoc class - 1618950408870",
        "type": "REGISTRY_CLASS",
        "children": []
    }
]
```

## 役割ベースの権限を確認してすべてのパッケージアーティファクトをインポートする {#role-based-permissions}

パッケージアーティファクトを読み込む権限があるかどうかを確認するには、パッケージの ID とターゲットのサンドボックス名を指定したうえで `/packages` エンドポイントに対してGETリクエストを行います。

**API 形式**

```http
GET /packages/preflight/{packageId}?targetSandbox=<sandbox_name
```

| パラメーター | 説明 |
| --- | --- |
| {PACKAGE_ID} | インポートするパッケージの ID。 |

**リクエスト**

次のリクエストでは、{PACKAGE_ID} とサンドボックスの権限を確認しています。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/preflight/{PACKAGE_ID}?targetSandbox=<sandbox_name> \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答が成功すると、ターゲットサンドボックスのリソース権限が返されます。この権限には、必要な権限のリスト、権限がない場合、アーティファクトのタイプ、作成が許可されているかどうかの決定が含まれます。

応答を表示+++

```json
{
  "packageID": "b7bda68eb1214c86824f1d7204616e51",
  "targetSandboxName": "acme-sandbox",
  "permissionResponse": [
    {
      "artifactID": "4aca57b6-8b83-450b-a460-e8a07ca79a44",
      "requiredPermissions": {
        "resources": [
          {
            "palmResourceType": "Sandbox",
            "resourcePermissions": [
              "view"
            ]
          },
          {
            "palmResourceType": "Schema",
            "resourcePermissions": [
              "read"
            ]
          },
          {
            "palmResourceType": "ProfileConfig",
            "resourcePermissions": [
              "read"
            ]
          },
          {
            "palmResourceType": "Segment",
            "resourcePermissions": [
              "read",
              "write",
              "delete"
            ]
          },
          {
            "palmResourceType": "Composition",
            "resourcePermissions": [
              "read",
              "write",
              "delete"
            ]
          },
          {
            "palmResourceType": "Query",
            "resourcePermissions": [
              "write"
            ]
          },
          {
            "palmResourceType": "SegmentDashboard",
            "resourcePermissions": [
              "read"
            ]
          }
        ]
      },
      "missingPermissions": {
        "resources": []
      },
      "artifactType": "PROFILE_SEGMENT",
      "creationAllowed": true
    },
    {
      "artifactID": "36bb82bc-a00d-4d6b-a598-227d43e027c1",
      "requiredPermissions": {
        "resources": [
          {
            "palmResourceType": "Sandbox",
            "resourcePermissions": [
              "view"
            ]
          },
          {
            "palmResourceType": "ProfileConfig",
            "resourcePermissions": [
              "read",
              "write",
              "delete"
            ]
          },
          {
            "palmResourceType": "Dataset",
            "resourcePermissions": [
              "read"
            ]
          }
        ]
      },
      "missingPermissions": {
        "resources": [
          {
            "palmResourceType": "ProfileConfig",
            "resourcePermissions": [
              "write",
              "delete"
            ]
          },
          {
            "palmResourceType": "Dataset",
            "resourcePermissions": [
              "read"
            ]
          }
        ]
      },
      "artifactType": "PROFILE_MERGE",
      "creationAllowed": false
    }
  ]
}
```

+++

## 書き出し/読み込みジョブのリスト {#list-jobs}

`/packages` エンドポイントに対してGETリクエストを実行することで、現在の書き出し/読み込みジョブをリストできます。

**API 形式**

```http
GET /packages/jobs?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| {QUERY_PARAMS} | 結果をフィルタリングするオプションのクエリパラメーター。 詳しくは、[ クエリパラメーター ](./appendix.md) の節を参照してください。 |

**リクエスト**

次のリクエストでは、成功したすべてのインポートジョブをリストします。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/jobs?property=requestType==IMPORT&property=jobStatus==SUCCESS&orderby=createdDate&start=0&limit=5 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

応答が成功すると、成功したすべてのインポートジョブが返されます。

```json
{
    "totalElements": 42,
    "currentPage": 0,
    "totalPages": 9,
    "hasPreviousPage": false,
    "hasNextPage": true,
    "data": [
        {
            "id": "3c1b92cf47a246d7bfbe6fd507c5d543",
            "name": "acme",
            "updated": 1685973675401,
            "created": 1685973675401,
            "jobType": "NEW",
            "packageType": "PARTIAL",
            "description": "Acme Business Group",
            "jobStatus": "SUCCESS",
            "visibility": "TENANT",
            "sourceSandBox": "acme-sandbox",
            "targetSandbox": "poc",
            "createdBy": "{CREATED_BY}"
        },
        {
            "id": "ead59d21405f4184a94dd786a1bf040d",
            "name": "acme1",
            "updated": 1685986367198,
            "created": 1685986367198,
            "jobType": "NEW",
            "packageType": "PARTIAL",
            "description": "Acme Business Group",
            "jobStatus": "SUCCESS",
            "visibility": "TENANT",
            "sourceSandBox": "acme-sandbox",
            "targetSandbox": "poc",
            "createdBy": "{CREATED_BY}"
        },
        {
            "id": "85ddaa3c2f6c475088167cde7a9d4326",
            "name": "acme2",
            "updated": 1686147692568,
            "created": 1686147692568,
            "jobType": "NEW",
            "packageType": "PARTIAL",
            "description": "Acme Business Group",
            "jobStatus": "SUCCESS",
            "visibility": "TENANT",
            "sourceSandBox": "acme-sandbox",
            "targetSandbox": "poc",
            "createdBy": "{CREATED_BY}"
        },
        {
            "id": "c49a4fcb31954cbd828ece1da096c8f5",
            "name": "acme3",
            "updated": 1686148007586,
            "created": 1686148007586,
            "jobType": "NEW",
            "packageType": "PARTIAL",
            "description": "Acme Business Group",
            "jobStatus": "SUCCESS",
            "visibility": "TENANT",
            "sourceSandBox": "acme-sandbox",
            "targetSandbox": "poc",
            "createdBy": "{CREATED_BY}"
        },
        {
            "id": "a3669315baed4cf2af49bf9ce90b8158",
            "name": "acme4",
            "updated": 1686148651910,
            "created": 1686148651910,
            "jobType": "NEW",
            "packageType": "PARTIAL",
            "description": "Acme Business Group",
            "jobStatus": "SUCCESS",
            "visibility": "TENANT",
            "sourceSandBox": "acme-sandbox",
            "targetSandbox": "poc",
            "createdBy": "{CREATED_BY}"
        }
    ]
}
```
