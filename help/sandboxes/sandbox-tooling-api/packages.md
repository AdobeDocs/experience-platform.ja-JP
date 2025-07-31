---
title: サンドボックスツールパッケージ API エンドポイント
description: サンドボックスツール API の/packages エンドポイントを使用すると、Adobe Experience Platformのパッケージをプログラムで管理できます。
exl-id: 46efee26-d897-4941-baf4-d5ca0b8311f0
source-git-commit: 1d8c29178927c7ee3aceb0b68f97baeaefd9f695
workflow-type: tm+mt
source-wordcount: '2933'
ht-degree: 11%

---

# パッケージエンドポイント

サンドボックスツールを使用すると、様々なアーティファクト（オブジェクトとも呼ばれます）を選択し、それらをパッケージに書き出すことができます。 パッケージは、単一のアーティファクトまたは複数のアーティファクト（データセットやスキーマなど）で構成できます。 パッケージに含まれるアーティファクトは、同じサンドボックスからのアーティファクトである必要があります。

サンドボックスツール API の `/packages` エンドポイントを使用すると、パッケージの公開やサンドボックスへのパッケージの読み込みなど、組織内のパッケージをプログラムによって管理できます。

## パッケージを作成 {#create}

パッケージの名前とパッケージタイプの値を指定したうえで `/packages` エンドポイントに対して POST リクエストを行うことで、複数のアーティファクトからなるパッケージを作成できます。

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
| `sourceSandbox` | パッケージのソースサンドボックス。 | オブジェクト | × |
| `expiry` | パッケージの有効期限を定義するタイムスタンプ。 デフォルト値は作成日から 90 日間です。 応答の有効期限フィールドはエポック UTC 時間になります。 | 文字列（UTC タイムスタンプ形式） | × |
| `artifacts` | パッケージにエクスポートされるアーティファクトのリスト。 `artifacts` が **の場合、** 値は **null** または `packageType` 空 `FULL` にする必要があります。 | 配列 | × |

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

サンドボックスツール API の `/packages` エンドポイントを使用して、パッケージを更新します。

### パッケージへのアーティファクトの追加 {#add-artifacts}

アーティファクトをパッケージに追加するには、`id` を指定し、**に** ADD`action` を含める必要があります。

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
| `expiry` | パッケージの有効期限を定義するタイムスタンプ。 ペイロードに有効期限が指定されていない場合、デフォルト値はPUT API が呼び出されてから 90 日です。 応答の有効期限フィールドはエポック UTC 時間になります。 | 文字列（UTC タイムスタンプ形式） | × |

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

パッケージからアーティファクトを削除するには、`id` を指定し、**に** DELETE`action` を含める必要があります。

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

パッケージ内のメタデータフィールドを更新するには、`id` を指定し、**の** UPDATE`action` を含める必要があります。

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
| `sourceSandbox` | Source サンドボックスは、リクエストのヘッダーで指定されたのと同じ組織に属している必要があります。 | オブジェクト | ○ |

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

パッケージを削除するには、`/packages` エンドポイントに対してDELETE リクエストを実行し、削除するパッケージの ID を指定します。

**API 形式**

```http
DELETE /packages/{PACKAGE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{PACKAGE_ID}` | 削除するパッケージの ID。 |

**リクエスト**

次のリクエストでは、ID が {PACKAGE_ID} のパッケージが削除されます。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答が成功すると、理由が返され、パッケージ ID が削除されたことが示されます。

```json
{
    "reason": "Package d30e0424a37b46ada6a5cf37f47a86ff deleted"
}
```

## パッケージの公開 {#publish}

パッケージをサンドボックスにインポートできるようにするには、パッケージを公開する必要があります。 公開するパッケージの ID を指定したうえで、`/packages` エンドポイントに対してGET リクエストを行います。

**API 形式**

```http
GET /packages/{PACKAGE_ID}/export
```

| パラメーター | 説明 |
| --- | --- |
| `{PACKAGE_ID}` | 公開するパッケージの ID。 |

**リクエスト**

次のリクエストでは、{PACKAGE_ID} という ID でパッケージが公開されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID}\export \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
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
    "correlationId": "48effe5e-1bef-4250-9c71-23b93ef5d285",
    "jobId": "18abab44e25f40c284a4bd6e8f52fd29"
}
```

## パッケージの検索 {#look-up-package}

個々のパッケージを検索するには、リクエストパスにパッケージの対応する ID を含む `/packages` エンドポイントに対してGET リクエストを実行します。

**API 形式**

```http
GET /packages/{PACKAGE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{PACKAGE_ID}` | 検索するパッケージの ID。 |

**リクエスト**

次のリクエストは、{PACKAGE_ID} の情報を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
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

`/packages` エンドポイントに対してGET リクエストを実行することで、組織内のすべてのパッケージをリストできます。

**API 形式**

```http
GET /packages/?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMS}` | 結果をフィルタリングするオプションのクエリパラメーター。 詳しくは、[ クエリパラメーター ](./appendix.md) の節を参照してください。 |

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
| `{PACKAGE_ID}` | 検索するパッケージの ID。 |

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

+++応答を表示

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

競合や置き換えを確認したら、`/packages` エンドポイントに対して POST リクエストを実行することで、パッケージの読み込みを送信できます。 結果はペイロードとして提供され、ペイロードで指定された宛先サンドボックスの読み込みジョブが開始されます。

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
    "correlationId": "48effe5e-1bef-4250-9c71-23b93ef5d285",
    "jobId": "18abab44e25f40c284a4bd6e8f52fd29"
}
```

## すべての依存オブジェクトのリスト {#dependent-objects}

パッケージの ID を指定したうえで、`/packages` エンドポイントに対して POST リクエストを実行することで、パッケージ内の書き出されたオブジェクトのすべての依存オブジェクトをリストします。

**API 形式**

```http
POST /packages/{PACKAGE_ID}/children
```

| パラメーター | 説明 |
| --- | --- |
| `{PACKAGE_ID}` | パッケージの ID。 |

**リクエスト**

次のリクエストでは、{PACKAGE_ID} のすべての依存オブジェクトをリストします。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID}/children \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
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

パッケージアーティファクトをインポートする権限があるかどうかを確認するには、パッケージの ID とターゲットのサンドボックス名を指定したうえで、`/packages` エンドポイントに対してGET リクエストを行います。

**API 形式**

```http
GET /packages/preflight/{packageId}?targetSandbox=<sandbox_name
```

| パラメーター | 説明 |
| --- | --- |
| `{PACKAGE_ID}` | インポートするパッケージの ID。 |

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

+++応答を表示

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

`/packages` エンドポイントに対してGET リクエストを実行することで、現在の書き出し/読み込みジョブをリストできます。

**API 形式**

```http
GET /packages/jobs?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMS}` | 結果をフィルタリングするオプションのクエリパラメーター。 詳しくは、[ クエリパラメーター ](./appendix.md) の節を参照してください。 |

**リクエスト**

次のリクエストでは、成功したすべてのインポートジョブをリストします。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/jobs?property=requestType==IMPORT&property=jobStatus==SUCCESS&orderby=createdDate&start=0&limit=5 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
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

## 組織間でのパッケージの共有 {#org-linking}

サンドボックスツール API の `/handshake` エンドポイントを使用すると、他の組織と提携してパッケージを共有できます。

### 共有リクエストの送信 {#send-request}

`/handshake/bulkCreate` エンドポイントに POST リクエストを実行することで、承認を共有するためのリクエストをターゲットパートナー組織に送信します。 これは、プライベートパッケージを共有する前に必要です。

**API 形式**

```http
POST /handshake/bulkCreate
```

**リクエスト**

次のリクエストでは、ターゲットパートナー組織とソース組織の間で共有承認を開始します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/exim/handshake/bulkCreate \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/json' \
  -H 'Authorization: {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -d '{
      "targetIMSOrgIds":["acme@AdobeOrg"],
      "sourceIMSDetails":{
        "id":"acme@AdobeOrg",
        "name":"acme_org"
      } 
  }' 
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `targetIMSOrgIds` | 共有リクエストを送信するターゲット組織のリスト。 | 配列 | ○ |
| `sourceIMSDetails` | ソース組織に関する詳細。 | オブジェクト | ○ |

**応答**

応答が成功すると、共有リクエストに関する詳細が返されます。

```json
{
    "successfulRequests": {
        "acme@AdobeOrg": {
            "id": "{ID}",
            "version": 0,
            "createdDate": 1724938816798,
            "modifiedDate": 1724938816798,
            "createdBy": "{CREATED_BY}",
            "modifiedBy": "{MODIFIED_BY}",
            "sourceIMSOrgId": "{ORG_ID}",
            "targetIMSOrgId": "{TARGET_ID}",
            "sourceRegion": "va6",
            "sourceIMSOrgName": "{SOURCE_NAME}",
            "status": "APPROVAL_PENDING",
            "createdByName": "{CREATED_BY}",
            "modifiedByName": "{MODIFIED_BY}",
            "modifiedByIMSOrgId": "{ORG_ID}",
            "statusHistory": "[{\"actionTakenBy\":\"acme@98ff67fa661fdf6549420b.e\",\"actionTakenByName\":\"{NAME}\",\"actionTakenByImsOrgID\":\"{ORG_ID}\",\"action\":\"INITIATED\",\"actionTimeStamp\":1724938816885}]",
            "linkingId": "{LINKING_ID}"
        }
    },
    "failedRequests": {}
}
```

### 受信した共有リクエストの承認 {#approve-requests}

`/handshake/action` エンドポイントに POST リクエストを作成して、ターゲットパートナー組織からの共有リクエストを承認します。 承認後、ソースパートナー組織はプライベートパッケージを共有できます。

**API 形式**

```http
POST /handshake/action
```

**リクエスト**

次のリクエストは、ターゲットパートナー組織からの共有リクエストを承認します。

```shell
curl -X POST  \
  https://platform.adobe.io/data/foundation/exim/handshake/action \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -d '{
      "linkingID":"{LINKING_ID}",
      "status":"APPROVED",
      "reason":"Done",
      "targetIMSOrgDetails":{
          "id":"acme@AdobeOrg",
          "name":"acme",
          "region":"va7"
      }
  }'
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `linkingID` | 応答する共有リクエストの ID。 | 文字列 | ○ |
| `status` | 共有リクエストに対して実行されるアクション。 指定できる値は、`APPROVED` または `REJECTED` です。 | 文字列 | ○ |
| `reason` | アクションが実行されている理由。 | 文字列 | ○ |
| `targetIMSOrgDetails` | ID 値がターゲット組織の **ID**、名前値がターゲット組織の **NAME**、地域値がターゲット組織の **REGION** である必要がある、ターゲット組織に関する詳細。 | オブジェクト | ○ |

**応答**

応答が成功すると、承認された共有リクエストに関する詳細が返されます。

```json
{
    "id": "{ID}",
    "version": 1,
    "createdDate": 1726737474000,
    "modifiedDate": 1726737541731,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}",
    "sourceIMSOrgId": "{ORG_ID}",
    "targetIMSOrgId": "{TARGET_ID}",
    "sourceRegion": "va7",
    "targetRegion": "va7",
    "sourceOrgName": "{SOURCE_ORG}",
    "targetOrgName": "{TARGET_ORG}",
    "status": "APPROVED",
    "createdByName": "{CREATED_BY}",
    "modifiedByIMSOrgId": "{MODIFIED_BY}",
    "statusHistory": "[{\"actionTakenBy\":\"{ACTION_BY}\",\"actionTakenByName\":\"{NAME}\",\"actionTakenByImsOrgID\":\"acme@AdobeOrg\",\"action\":\"INITIATED\",\"actionTimeStamp\":1726737474450,\"reason\":null},{\"actionTakenBy\":null,\"actionTakenByName\":null,\"actionTakenByImsOrgID\":\"745F37C35E4B776E0A49421B@AdobeOrg\",\"action\":\"APPROVED\",\"actionTimeStamp\":1726737541818,\"reason\":\"Done\"}]",
    "linkingId": "{LINKING_ID}"
}
```

### 送信/受信の共有リクエストのリスト {#outgoing-and-incoming-requests}

`handshake/list?property=status%3D%3DAPPROVED&requestType=INCOMING` エンドポイントに対してGET リクエストを実行して、送信および受信の共有リクエストをリストします。

**API 形式**

```http
GET handshake/list?property=status%3D%3DAPPROVED&requestType=INCOMING
```

| パラメーター | 許容値/デフォルト値 |
| --- | --- |
| `property` | ステータスなど、フィルタリングに使用するプロパティを指定します。 ステータスに指定できる値は、`APPROVED`、`REJECTED`、`IN_PROGRESS` です。 |
| `start` | start のデフォルト値は `0` です。 |
| `limit` | limit のデフォルト値は `20` です。 |
| `orderBy` | レコードを昇順または降順に並べ替えます。 |
| `requestType` | `INCOMING` または `OUTGOING` を受け入れます。 |

**リクエスト**

次のリクエストは、すべての送信および受信の共有リクエストのリストを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/handshake/list?property=status%3D%3DAPPROVED&requestType=INCOMING \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id:{ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
```

**応答**

応答が成功すると、送信および受信する共有リクエストとその詳細のリストが返されます。

```json
{
    "totalElements": 1,
    "currentPage": 0,
    "totalPages": 1,
    "hasPreviousPage": false,
    "hasNextPage": false,
    "data": [
        {
            "id": "{ID}",
            "version": 1,
            "createdDate": 1724929446000,
            "modifiedDate": 1724929617000,
            "modifiedBy": "{MODIFIED_BY}",
            "sourceIMSOrgId": "{ORG_ID}",
            "targetIMSOrgId": "{TARGET_ID}",
            "sourceRegion": "va7",
            "targetRegion": "va6",
             "sourceOrgName": "{SOURCE_ORG}",
            "targetOrgName": "{TARGET_ORG}",
            "status": "APPROVED",
            "createdByName": "{CREATED_BY}",
            "modifiedByName": "{MODIFIED_BY}",
            "modifiedByIMSOrgId": "{MODIFIED_BY}",
            "statusHistory": "[{\"actionTakenBy\":\"{ACTION_BY}\",\"actionTakenByName\":\"{NAME}\",\"actionTakenByImsOrgID\":\"{ORG_ID}\",\"action\":\"INITIATED\",\"actionTimeStamp\":1724929442467,\"reason\":null},{\"actionTakenBy\":null,\"actionTakenByName\":\"{NAME}\",\"actionTakenByImsOrgID\":\"{ORG_ID}\",\"action\":\"APPROVED\",\"actionTimeStamp\":1724929617531,\"reason\":\"Done\"}]",
            "linkingId": "{LINKING_ID}"
        }
    ],
    "nextPage": null,
    "pageSize": null
}
```

## 転送パッケージ

サンドボックスツール API の `/transfer` エンドポイントを使用して、新しいパッケージ共有リクエストを取得および作成します。

### 新しい共有リクエスト {#share-request}

パッケージ ID とターゲット組織 ID を指定したうえで、`/transfer` エンドポイントに対して POST リクエストを実行することで、公開済みのソース組織のパッケージを取得してターゲット組織と共有します。

**API 形式**

```http
POST /transfer
```

**リクエスト**

次のリクエストでは、ソース組織パッケージを取得し、ターゲット組織と共有します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/exim/transfer/ \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -d '{
      "packageId": "{PACKAGE_ID}",
      "targets": [
          {
              "imsOrgId": "{TARGET_IMS_ORG}"
          }
      ]
  }'
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `packageId` | 共有するパッケージの ID。 | 文字列 | ○ |
| `targets` | パッケージと共有する組織のリスト。 | 配列 | ○ |

**応答**

応答が成功すると、リクエストされたパッケージの詳細と共有ステータスが返されます。

```json
[
    {
        "id": "{ID}",
        "version": 0,
        "createdDate": 1726480559313,
        "modifiedDate": 1726480559313,
        "createdBy": "{CREATED_BY}",
        "modifiedBy": "{MODIFIED_BY}",
        "sourceIMSOrgId": "{ORG_ID}",
        "targetIMSOrgId": "{TARGET_ID}",
        "packageId": "{PACKAGE_ID}",
        "status": "PENDING",
        "initiatedBy": "acme@3ec9197a65a86f34494221.e",
        "requestType": "PRIVATE"
    }
]
```

### ID による共有リクエストの取得 {#fetch-transfer-by-id}

転送 ID を指定しながら `/transfer/{TRANSFER_ID}` エンドポイントに対してGET リクエストを実行して、共有リクエストの詳細を取得します。

**API 形式**

```http
GET /transfer/{TRANSFER_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{TRANSFER_ID}` | 取得する転送の ID。 |

**リクエスト**

次のリクエストは、{TRANSFER_ID} という ID の転送を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/transfer/0c843180a64c445ca1beece339abc04b \
  -H 'x-api-key: {API__KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}'
```

**応答**

成功の応答は、共有リクエストの詳細を返します。

```json
{
    "id": "{ID}",
    "sourceIMSOrgId": "{ORG_ID}",
    "sourceOrgName": "{SOURCE_ORG}",
    "targetIMSOrgId": "{TARGET_ID}",
    "targetOrgName": "{TARGET_ORG}",
    "packageId": "{PACKAGE_ID}",
    "packageName": "{PACKAGE_NAME}",
    "status": "COMPLETED",
    "initiatedBy": "{INITIATED_BY}",
    "createdDate": 1724442856000,
    "requestType": "PRIVATE"
}
```

### 共有リストを取得 {#transfers-list}

`/transfer/list?{QUERY_PARAMETERS}` エンドポイントに対してGET リクエストを実行し、必要に応じてクエリパラメーターを変更することで、転送リクエストのリストを取得します。

**API 形式**

```http
GET `/transfer/list?{QUERY_PARAMETERS}`
```

| パラメーター | 許容値/デフォルト値 |
| --- | --- |
| `property` | ステータスなど、フィルタリングに使用するプロパティを指定します。 ステータスに指定できる値は、`COMPLETED`、`PENDING`、`IN_PROGRESS`、`FAILED` です。 |
| `start` | start のデフォルト値は `0` です。 |
| `limit` | limit のデフォルト値は `20` です。 |
| `orderBy` | 順序には、`createdDate` フィールドのみ使用できます。 |

**リクエスト**

次のリクエストは、指定された検索パラメーターから転送リクエストのリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/transfer/list?property=status==COMPLETED&start=0&limit=2&orderBy=-createdDate \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}'
```

**応答**

応答が成功すると、指定された検索パラメーターからすべての転送リクエストのリストが返されます。

```json
{
    "totalElements": 43,
    "currentPage": 0,
    "totalPages": 22,
    "hasPreviousPage": false,
    "hasNextPage": true,
    "data": [
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_ORG}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_ORG}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "{PACKAGE_NAME}",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1726129077000,
            "createdDate": 1726129062000,
            "requestType": "PRIVATE"
        },
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_ORG}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_ORG}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "{PACKAGE_NAME}",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1726066046000,
            "createdDate": 1726065936000,
            "requestType": "PRIVATE"
        }
    ],
    "nextPage": null,
    "pageSize": null
}
```

### パッケージの可用性をプライベートからパブリックに更新 {#update-availability}

`/packages/update` エンドポイントに対してGET リクエストを実行して、パッケージをプライベートからパブリックに変更します。 デフォルトでは、パッケージは非公開で作成されます。

**API 形式**

```http
PUT `/packages/update`
```

**リクエスト**

次のリクエストは、パッケージの可用性をプライベートからパブリックに変更します。

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/exim/packages \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-type: application/json' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -d '{
      "id":"{ID}",
      "action":"UPDATE",
      "packageVisibility":"PUBLIC"
  }'
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `id` | 更新するパッケージの ID。 | 文字列 | ○ |
| `action` | 表示を公開に更新するには、アクション値を **UPDATE** にする必要があります。 | 文字列 | ○ |
| `packageVisbility` | 表示を更新するには、packageVisibility の値を **PUBLIC** にする必要があります。 | 文字列 | ○ |

**応答**

応答が成功すると、パッケージとその表示に関する詳細が返されます。

```json
{
    "id": "{ID}",
    "version": 7,
    "createdDate": 1729624618000,
    "modifiedDate": 1729658596340,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}",
    "name": "acme",
    "imsOrgId": "{ORG_ID}",
    "packageType": "PARTIAL",
    "expiry": 1737434596325,
    "status": "PUBLISH_FAILED",
    "packageVisibility": "PUBLIC",
    "artifactsList": [
        {
            "id": "{ID}",
            "type": "PROFILE_SEGMENT",
            "found": false,
            "count": 0,
            "title": "Acme Profile Segment"
        }
    ],
    "schemaMapping": {},
    "sourceSandbox": {
        "name": "acme-sandbox",
        "imsOrgId": "{ORG_ID}",
        "empty": false
    }
}
```

### 公開パッケージをインポートするリクエスト {#pull-public-package}

`/transfer/pullRequest` エンドポイントに POST リクエストを実行することで、公開されているソース組織からパッケージをインポートします。

**API 形式**

```http
POST /transfer/pullRequest
```

**リクエスト**

次のリクエストは、パッケージをインポートし、可用性をパブリックに設定します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/exim/transfer/pullRequest \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -d '{
      "imsOrgId": "{ORG_ID}",
      "packageId": "{PACKAGE_ID}"
  }'
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `imsOrgId` | パッケージのソース組織の ID。 | 文字列 | ○ |
| `packageId` | インポートするパッケージの ID。 | 文字列 | ○ |

**応答**

応答が成功すると、読み込んだパブリックパッケージの詳細が返されます。

```json
{
    "id": "{ID}",
    "version": 0,
    "createdDate": 1729658890425,
    "modifiedDate": 1729658890425,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}",
    "sourceIMSOrgId": "{ORG_ID}",
    "targetIMSOrgId": "{TARGET_ID}",
    "packageId": "{PACKAGE_ID}",
    "status": "PENDING",
    "initiatedBy": "{INITIATED_BY}",
    "pipelineMessageId": "{MESSAGE_ID}",
    "requestType": "PUBLIC"
}
```

### 公開パッケージのリスト {#list-public-packages}

`/transfer/list?{QUERY_PARAMS}` エンドポイントに対してGET リクエストを実行して、公開ビジビリティを備えたパッケージのリストを取得します。

**API 形式**

```http
GET /transfer/list?{QUERY_PARAMS}
```

| パラメーター | 許容値/デフォルト値 |
| --- | --- |
| `property` | ステータスなど、フィルタリングに使用するプロパティを指定します。 ステータスに指定できる値は、`COMPLETED` および `FAILED` です。 |
| `start` | start のデフォルト値は `0` です。 |
| `limit` | limit のデフォルト値は `20` です。 |
| `orderBy` | 順序には、`createdDate` フィールドのみ使用できます。 |
| `requestType` | `PUBLIC` または `PRIVATE` を受け入れます。 |

**リクエスト**

次のリクエストは、公開されているパッケージのリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/transfer/list?property=status%3D%3DCOMPLETED%2CFAILED&requestType=PUBLIC&orderby=-createdDate \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/json' \
  -H 'Authorization: {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
```

**応答**

応答が成功すると、公開パッケージとその詳細のリストが返されます。

+++応答を表示

```json
{
    "totalElements": 14,
    "currentPage": 0,
    "totalPages": 1,
    "hasPreviousPage": false,
    "hasNextPage": false,
    "data": [
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_ORG}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "Public package demo",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729359318000,
            "createdDate": 1729359316000,
            "requestType": "PUBLIC"
        },
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_NAME}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "Public package demo",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729359284000,
            "createdDate": 1729359283000,
            "requestType": "PUBLIC"
        },
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_NAME}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "Test Private Flow Final",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729284462000,
            "createdDate": 1729275962000,
            "requestType": "PUBLIC"
        },
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOUCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_NAME}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "Fest",
            "status": "FAILED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729284104000,
            "createdDate": 1729253854000,
            "requestType": "PUBLIC"
        },
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_NAME}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "PublicPackageSharing",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729284835000,
            "createdDate": 1729253556000,
            "requestType": "PUBLIC"
        },
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_NAME}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "PublicPackageSharing",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729284835000,
            "createdDate": 1729253556000,
            "requestType": "PUBLIC"
        },
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_NAME}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "PublicPackageSharing",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729284835000,
            "createdDate": 1729253556000,
            "requestType": "PUBLIC"
        },
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_NAME}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "Public Package Audit Test",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729284667000,
            "createdDate": 1729253421000,
            "requestType": "PUBLIC"
        },
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_NAME}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "Public Package Audit Test",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729284957000,
            "createdDate": 1729253143000,
            "requestType": "PUBLIC"
        },
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_NAME}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "Public Package Audit Test",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729284562000,
            "createdDate": 1729252975000,
            "requestType": "PUBLIC"
        },
        {
               "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_NAME}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "Private Package Test 1",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729284262000,
            "createdDate": 1729229755000,
            "requestType": "PUBLIC"
        },
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_NAME}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "Demo Package 1016",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729284784000,
            "createdDate": 1729208888000,
            "requestType": "PUBLIC"
        },
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_NAME}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "Public Package test 1",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729284934000,
            "createdDate": 1729153097000,
            "requestType": "PUBLIC"
        },
        {
            "id": "{ID}",
            "sourceIMSOrgId": "{ORG_ID}",
            "sourceOrgName": "{SOURCE_NAME}",
            "targetIMSOrgId": "{TARGET_ID}",
            "targetOrgName": "{TARGET_NAME}",
            "packageId": "{PACKAGE_ID}",
            "packageName": "Public Package test 1",
            "status": "COMPLETED",
            "initiatedBy": "{INITIATED_BY}",
            "completedTime": 1729284912000,
            "createdDate": 1729153043000,
            "requestType": "PUBLIC"
        }
    ],
    "nextPage": null,
    "pageSize": null
}
```

+++

## パッケージペイロード（#package-payload）をコピー

リクエストパスにパッケージの対応する ID を含む `/packages/payload` エンドポイントに対してGET リクエストを実行することで、公開パッケージのペイロードをコピーできます。

**API 形式**

```http
GET /packages/payload/{PACKAGE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{PACKAGE_ID}` | コピーするパッケージの ID。 |

**リクエスト**

次のリクエストでは、{PACKAGE_ID} という ID を持つパッケージのペイロードを取得しています。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/payload/{PACKAGE_ID} \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `imsOrdId` | パッケージが属する組織の ID。 | 文字列 | ○ |
| `packageId` | リクエストするペイロードのパッケージの ID。 | 文字列 | ○ |

**応答**

応答が成功すると、パッケージのペイロードが返されます。

```json
{
    "imsOrgId": "{ORG_ID}",
    "packageId": "{PACKAGE_ID}"
}
```

## オブジェクト設定の更新を移行する

サンドボックスツール API の/packages エンドポイントを使用して、オブジェクト設定の更新を移行します。

### 更新操作（#update-operations）

パッケージ ID を指定して `/packages/{packageId}/version/compare` エンドポイントに POST リクエストを実行することで、指定された、または最新のバージョンのパッケージスナップショットを、ソースサンドボックスの現在の状態、またはパッケージが読み込まれた以前に使用されたターゲットサンドボックスと比較します。

***API 形式***

```http
PATCH /packages/{packageId}/version/compare
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `packageId` | パッケージの ID。 | 文字列 | ○ |

**リクエスト**

```shell
curl -X POST \
  https://platform-stage.adobe.io/data/foundation/exim/packages/{PACKAGE_ID}/version/compare/ \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
      "triggerNew": true,
      "targetSandbox": "{SANDBOX_NAME}"
  }'
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `triggerNew` | 既にアクティブまたは完了したジョブが存在する場合でも、新しい差分計算ジョブをトリガーにするフラグ。 | ブール値 | × |
| `targetSandbox` | 差分を計算する必要があるターゲットサンドボックスの名前を表します。 指定しない場合、ソースサンドボックスがターゲットサンドボックスとして使用されます。 | 文字列 | × |

**応答**

以前に完了したジョブに対する応答が成功すると、以前に計算した差分結果を含むジョブオブジェクトが返されます。 新しく完了したジョブは、JobId を返します。

+++応答を表示（送信済みジョブ）

```json
{
    "status": "OK",
    "type": "SUCCESS",
    "ajo": false,
    "message": "Job with ID: {JOB_ID}",
    "object": {
        "id": "c4b7d07ae4c646279e2070a31c50bd5c",
        "name": "Compute Job Package: {SNAPSHOT_ID}",
        "description": null,
        "visibility": "TENANT",
        "requestType": "VERSION",
        "expiry": 0,
        "snapshotId": "{SNAPSHOT_ID}",
        "packageVersion": 0,
        "createdTimestamp": 0,
        "modifiedTimestamp": 0,
        "type": "PARTIAL",
        "jobStatus": "SUCCESS",
        "jobType": "COMPUTE",
        "counter": 0,
        "imsOrgId": "{ORG_ID}",
        "sourceSandbox": {
            "name": "prod",
            "imsOrgId": "{ORG_ID}",
            "empty": false
        },
        "destinationSandbox": {
            "name": "amanda-1",
            "imsOrgId": "{ORG_ID}",
            "empty": false
        },
        "deltaPackageVersion": {
            "packageId": "{PACKAGE_ID}",
            "currentVersion": 0,
            "validated": false,
            "rootArtifacts": [
                {
                    "id": "https://ns.adobe.com/sandboxtoolingstage/schemas/355f461cbfb662fd0d12d06aeab34e206efcfa5d913604de",
                    "type": "REGISTRY_SCHEMA",
                    "found": false,
                    "count": 0
                }
            ],
            "eximGraphDelta": {
                "vertices": [],
                "pluginDeltas": [
                    {
                        "sourceArtifact": {
                            "id": "https://ns.adobe.com/sandboxtoolingstage/mixins/9fad8b185640a2db7daf9bb1295543ee8cb5965d80a21e8d",
                            "type": "REGISTRY_MIXIN",
                            "found": false,
                            "count": 0,
                            "title": "Custom FieldGroup 2"
                        },
                        "targetArtifact": {
                            "id": "https://ns.adobe.com/sandboxtoolingstage/mixins/b7fa3024777ef11b68c5121e937d8543677093f4f0e63a5f",
                            "type": "REGISTRY_MIXIN",
                            "found": false,
                            "count": 0,
                            "title": "Custom FieldGroup 2_1738766274074"
                        },
                        "changes": [
                            {
                                "op": "replace",
                                "path": "/title",
                                "oldValue": "Custom FieldGroup 2_1738766274074",
                                "newValue": "Custom FieldGroup 2"
                            },
                            {
                                "op": "replace",
                                "path": "/description",
                                "oldValue": "Description for furnished object",
                                "newValue": ""
                            }
                        ]
                    },
                    {
                        "sourceArtifact": {
                            "id": "https://ns.adobe.com/sandboxtoolingstage/mixins/304ac900943716c8bd99e6aaf6aa840aac91995729f1987f",
                            "type": "REGISTRY_MIXIN",
                            "found": false,
                            "count": 0,
                            "title": "Custom FieldGroup 4"
                        },
                        "targetArtifact": {
                            "id": "https://ns.adobe.com/sandboxtoolingstage/mixins/34c9add91cce4a40d68a0e715c9f0a16048871734f8c8b74",
                            "type": "REGISTRY_MIXIN",
                            "found": false,
                            "count": 0,
                            "title": "Custom FieldGroup 4_1738766274074"
                        },
                        "changes": [
                            {
                                "op": "replace",
                                "path": "/title",
                                "oldValue": "Custom FieldGroup 4_1738766274074",
                                "newValue": "Custom FieldGroup 4"
                            },
                            {
                                "op": "replace",
                                "path": "/description",
                                "oldValue": "Description for furnished object",
                                "newValue": ""
                            }
                        ]
                    }
                ]
            }
        },
        "importReplacementMap": {
            "https://ns.adobe.com/sandboxtoolingstage/mixins/9fad8b185640a2db7daf9bb1295543ee8cb5965d80a21e8d": "https://ns.adobe.com/sandboxtoolingstage/mixins/b7fa3024777ef11b68c5121e937d8543677093f4f0e63a5f",
            "5a45f8cd309d5ed5797be9a0af65e89152a51d57a6c74b52": "4ae041fa182d6faf2e7c56463399170d913138a7c5712909",
            "https://ns.adobe.com/sandboxtoolingstage/schemas/b2b7705e770a35341b8bc5ec5e3644d9c7387266777fe4ba": "https://ns.adobe.com/sandboxtoolingstage/schemas/838c4e21ad81543ac14238ac1756012f7f98f0e0bec6b425",
            "https://ns.adobe.com/sandboxtoolingstage/schemas/355f461cbfb662fd0d12d06aeab34e206efcfa5d913604de": "https://ns.adobe.com/sandboxtoolingstage/schemas/9a55692d527169d0239e126137a694ed9db2406c9bcbd06a",
            "8f45c79235c91e7f0c09af676a77d170a34b5ee0ad5de72c": "65d755cc3300674c3cfcec620c59876af07f046884afd359",
            "f04b8e461396ff426f8ba8dc5544f799bf287baa8e0fa5c": "b6fa821ada8cb97cac384f0b0354bbe74209ec97fb6a83a3",
            "https://ns.adobe.com/sandboxtoolingstage/mixins/304ac900943716c8bd99e6aaf6aa840aac91995729f1987f": "https://ns.adobe.com/sandboxtoolingstage/mixins/34c9add91cce4a40d68a0e715c9f0a16048871734f8c8b74",
            "c8304f3cb7986e8c9b613cd8d832125bd867fb4a5aedf67a": "4d21e9bf89ce0042b52d7d41ff177a7697d695e2617d1fc1"
        },
        "schemaFieldMappings": null
    }
}
```

+++

+++応答を表示（新しく送信されたジョブ）

```json
{
    "status": "OK",
    "type": "SUCCESS",
    "ajo": false,
    "message": "Job with ID: {JOB_ID}",
    "object": {
        "id": "aa5cfacf35a8478c8cf44a675fab1c30 ",
        "name": "Compute Job Package: {SNAPSHOT_ID}",
        "description": null,
        "visibility": "TENANT",
        "requestType": "VERSION",
        "expiry": 0,
        "snapshotId": "{SNAPSHOT_ID}",
        "packageVersion": 0,
        "createdTimestamp": 0,
        "modifiedTimestamp": 0,
        "type": "PARTIAL",
        "jobStatus": "IN_PROGRESS",
        "jobType": "COMPUTE",
        "counter": 0,
        "imsOrgId": "{ORG_ID}",
        "sourceSandbox": {
            "name": "prod",
            "imsOrgId": "{ORG_ID}",
            "empty": false
        },
        "destinationSandbox": {
            "name": "amanda-1",
            "imsOrgId": "{ORG_ID}",
            "empty": false
        },
        "schemaFieldMappings": null
    }
}
```

+++

### パッケージバージョンを更新（#package-versioning）

`/packages/{packageId}/version/save` エンドポイントにGET リクエストを実行し、パッケージ ID を指定して、各オブジェクトのソースサンドボックスの最新のスナップショットを使用して、パッケージを新しいバージョンにアップグレードします。

***API 形式***

```http
PATCH /packages/{packageId}/version/save
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `packageId` | パッケージの ID。 | 文字列 | ○ |

**リクエスト**

```shell
curl -X POST \
  https://platform-stage.adobe.io/data/foundation/exim/packages/{PACKAGE_ID}/version/save/ \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
```

**応答**

応答が成功すると、バージョンアップグレードのジョブステータスが返されます。

```json
{
    "id": "3cec9bae662e43d9b9106fcbf7744a75",
    "name": "Version Job Package: {JOB_ID}",
    "description": null,
    "visibility": "TENANT",
    "requestType": "VERSION",
    "expiry": 0,
    "snapshotId": "{SNAPSHOT_ID}",
    "packageVersion": 2,
    "createdTimestamp": 0,
    "modifiedTimestamp": 0,
    "type": "PARTIAL",
    "jobStatus": "PENDING",
    "jobType": "UPGRADE",
    "counter": 0,
    "imsOrgId": "{ORG_ID}",
    "sourceSandbox": {
        "name": "prod",
        "imsOrgId": "{ORG_ID}",
        "empty": false
    },
    "destinationSandbox": {
        "name": "prod",
        "imsOrgId": "{ORG_ID}",
        "empty": false
    },
    "schemaFieldMappings": null
}
```

### パッケージバージョン履歴の取得（#package-version-history）

パッケージ ID を指定して `/packages/{packageId}/history` エンドポイントに対してGET リクエストを実行することで、タイムスタンプや修飾子を含む、パッケージのバージョン管理履歴を取得します。

***API 形式***

```http
PATCH /packages/{packageId}/history
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `packageId` | パッケージの ID。 | 文字列 | ○ |

**リクエスト**

```shell
curl -X POST \
  https://platform-stage.adobe.io/data/foundation/exim/packages/{PACKAGE_ID}/history/ \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
```

**応答**

応答が成功すると、パッケージのバージョン履歴が返されます。

```json
[
    {
        "id": "cb68591a1ed941e191e7f52e33637a26",
        "version": 0,
        "createdDate": 1739516784000,
        "modifiedDate": 1739516784000,
        "createdBy": "{CREATED_BY}",
        "modifiedBy": "{MODIFIED_BY}",
        "imsOrgId": "{ORG_ID}",
        "packageVersion": 3
    },
    {
        "id": "e26189e6e4df476bb66c3fc3e66a1499",
        "version": 0,
        "createdDate": 1739343268000,
        "modifiedDate": 1739343268000,
        "createdBy": "{CREATED_BY}",
        "modifiedBy": "{MODIFIED_BY}",
        "imsOrgId": "{ORG_ID}",
        "packageVersion": 2
    },
    {
        "id": "11af34c0eee449ac84ef28c66d9383e3",
        "version": 0,
        "createdDate": 1739343073000,
        "modifiedDate": 1739343073000,
        "createdBy": "{CREATED_BY}",
        "modifiedBy": "{MODIFIED_BY}",
        "imsOrgId": "{ORG_ID}",
        "packageVersion": 1
    }
]
```

### 更新ジョブの送信（#submit-update）

パッケージ ID を指定して `/packages/{packageId}/import` エンドポイントにPATCH リクエストを実行することで、ターゲットサンドボックスオブジェクトに新しい更新をプッシュします。

***API 形式***

```http
PATCH /packages/{packageId}/import
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `packageId` | パッケージの ID。 | 文字列 | ○ |

**リクエスト**

```shell
curl -X POST \
  https://platform-stage.adobe.io/data/foundation/exim/packages/{PACKAGE_ID}/import/ \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
      "id": "50fd94f8072b4f248737a2b57b41058f",
      "name": "Test Update",
      "destinationSandbox": {
        "name": "test-sandbox-sbt",
        "imsOrgId": "{ORG_ID}"
      },
      "overwriteMappings": {
        "https://ns.adobe.com/sandboxtoolingstage/schemas/327a48c83a5359f8160420a00d5a07f0ba8631a1fd466f9e" : {
            "id" : "https://ns.adobe.com/sandboxtoolingstage/schemas/e346bb2cd7b26576cb51920d214aebbd42940a9bf94a75cd",
            "type" : "REGISTRY_SCHEMA"
        }
      }
  }'
```

**応答**

応答が成功すると、更新のジョブ ID が返されます。

```json
{
    "id": "3cec9bae662e43d9b9106fcbf7744a75",
    "name": "Update Job Name",
    "description": "Update Job Description",
    "visibility": "TENANT",
    "requestType": "IMPORT",
    "expiry": 0,
    "snapshotId": "{SNAPSHOT_ID}",
    "packageVersion": 2,
    "createdTimestamp": 0,
    "modifiedTimestamp": 0,
    "type": "PARTIAL",
    "jobStatus": "PENDING",
    "jobType": "UPDATE",
    "counter": 0,
    "imsOrgId": "{ORG_ID}",
    "sourceSandbox": {
        "name": "prod",
        "imsOrgId": "{ORG_ID}",
        "empty": false
    },
    "destinationSandbox": {
        "name": "amanda-1",
        "imsOrgId": "{ORG_ID}",
        "empty": false
    },
    "schemaFieldMappings": null
}
```

### パッケージの更新と上書きを無効にする（#disable-update）

パッケージ ID を指定して `/packages/{packageId}/?{QUERY_PARAMS}` エンドポイントにGET リクエストを行うことで、パッケージがサポートされていない場合は更新と上書きを無効にします。

***API 形式***

```http
PATCH /packages/{packageId}?{QUERY_PARAMS}
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `packageId` | パッケージの ID。 | 文字列 | ○ |
| {QUERY_PARAM} | getCapabilities クエリパラメーター。 `true` または `false` に設定する必要があります | ブール値 | ○ |

**リクエスト**

```shell
curl -X POST \
  https://platform-stage.adobe.io/data/foundation/exim/packages/{PACKAGE_ID}?getCapabilities=true'/ \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
```

**応答**

応答が成功すると、パッケージの機能のリストが返されます。

```json
{
    "id": "80230dde96574a828191144709bb9b51",
    "version": 3,
    "createdDate": 1749808582000,
    "modifiedDate": 1749808648000,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}",
    "name": "Ankit_Primary_Descriptor_Test",
    "description": "RestPackage",
    "imsOrgId": "{ORG_ID}",
    "clientId": "usecasebuilder",
    "packageType": "PARTIAL",
    "expiry": 1757584598000,
    "publishDate": 1749808648000,
    "status": "PUBLISHED",
    "packageVisibility": "PRIVATE",
    "latestPackageVersion": 0,
    "packageAccessType": "TENANT",
    "artifactsList": [
        {
            "id": "https://ns.adobe.com/sandboxtoolingstage/schemas/1c767056056de64d8030380d1b9f570d26bc15501a1e0e95",
            "altId": null,
            "type": "REGISTRY_SCHEMA",
            "found": false,
            "count": 0
        }
    ],
    "schemaMapping": {},
    "sourceSandbox": {
        "name": "atul-sandbox",
        "imsOrgId": "{ORG_ID}",
        "empty": false
    },
    "packageCapabilities": {
        "capabilities": [
            "VERSIONABLE"
        ]
    }
}
```
