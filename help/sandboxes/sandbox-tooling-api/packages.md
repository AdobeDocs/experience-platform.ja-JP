---
title: サンドボックスツールパッケージ API エンドポイント
description: Sandbox Tooling API の/packages エンドポイントを使用すると、Adobe Experience Platformでパッケージをプログラムで管理できます。
exl-id: 46efee26-d897-4941-baf4-d5ca0b8311f0
source-git-commit: 308d07cf0c3b4096ca934a9008a13bf425dc30b6
workflow-type: tm+mt
source-wordcount: '1553'
ht-degree: 9%

---

# パッケージエンドポイント

サンドボックスツールを使用すると、様々なアーティファクト（オブジェクトとも呼ばれます）を選択し、パッケージにエクスポートできます。 パッケージは、1 つのアーティファクトまたは複数のアーティファクト（データセットやスキーマなど）で構成できます。 パッケージに含まれるアーティファクトは、同じサンドボックスから生成される必要があります。

The `/packages` サンドボックスツール API のエンドポイントを使用すると、パッケージの公開やサンドボックスへのパッケージのインポートなど、組織内のパッケージをプログラムで管理できます。

## パッケージを作成する {#create}

マルチアーティファクトパッケージを作成するには、 `/packages` エンドポイントを使用して、パッケージの名前とパッケージタイプの値を指定します。

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
| `description` | パッケージに関する詳細情報を提供する説明。 | 文字列 | × |
| `packageType` | パッケージのタイプは次のとおりです。 **部分的** をクリックして、特定のアーティファクトをパッケージに含めることを示します。 | 文字列 | はい |
| `sourceSandbox` | パッケージのソースサンドボックス。 | 文字列 | × |
| `expiry` | パッケージの有効期限を定義するタイムスタンプ。 デフォルト値は作成日から 90 日です。 応答の有効期限フィールドは、エポック UTC 時間になります。 | 文字列（UTC タイムスタンプ形式） | × |
| `artifacts` | パッケージに書き出すアーティファクトのリスト。 The `artifacts` 値は **null** または **空**、 `packageType` 次に該当 `FULL`. | 配列 | × |

**応答**

正常な応答は、新しく作成されたパッケージを返します。 応答には、対応するパッケージ ID に加えて、そのステータス、有効期限、アーティファクトのリストに関する情報が含まれます。

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

パッケージを更新するには、 `/packages` endpoint.

### パッケージへのアーティファクトの追加 {#add-artifacts}

アーティファクトをパッケージに追加するには、 `id` およびを含む **追加** （の） `action`.

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
| `action` | アーティファクトをパッケージに追加するには、アクションの値を **追加**. このアクションは、次の場合にのみサポートされます： **部分的** パッケージタイプ。 | 文字列 | ○ |
| `artifacts` | パッケージに追加するアーティファクトのリスト。 リストが **null** または **空**. アーティファクトは、パッケージに追加される前に重複排除されます。 | 配列 | × |
| `expiry` | パッケージの有効期限を定義するタイムスタンプ。 ペイロードで expiry が指定されていない場合、デフォルト値は、PUTAPI が呼び出されてから 90 日です。 応答の有効期限フィールドは、エポック UTC 時間になります。 | 文字列（UTC タイムスタンプ形式） | × |

**応答**

正常な応答は、更新されたパッケージを返します。 応答には、対応するパッケージ ID に加えて、そのステータス、有効期限、アーティファクトのリストに関する情報が含まれます。

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

パッケージからアーティファクトを削除するには、 `id` およびを含む **DELETE** （の） `action`.


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
| `action` | パッケージからアーティファクトを削除するには、アクションの値を次のようにする必要があります。 **DELETE**. このアクションは、次の場合にのみサポートされます： **部分的** パッケージタイプ。 | 文字列 | ○ |
| `artifacts` | パッケージから削除するアーティファクトのリスト。 リストが **null** または **空**. | 配列 | × |

**応答**

正常な応答は、更新されたパッケージを返します。 応答には、対応するパッケージ ID に加えて、そのステータス、有効期限、アーティファクトのリストに関する情報が含まれます。

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
>The **更新** アクションは、パッケージのパッケージメタデータフィールドの更新に使用され、 **できません** を使用して、パッケージにアーティファクトを追加または削除します。

パッケージ内のメタデータフィールドを更新するには、 `id` およびを含む **更新** （の） `action`.

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
| `action` | パッケージ内のメタデータフィールドを更新するには、action 値を次のようにします。 **更新**. このアクションは、次の場合にのみサポートされます： **部分的** パッケージタイプ。 | 文字列 | ○ |
| `name` | 更新されたパッケージの名前。 重複するパッケージ名は使用できません。 | 配列 | ○ |
| `sourceSandbox` | ソースサンドボックスは、リクエストのヘッダーで指定されたのと同じ組織に属している必要があります。 | 文字列 | ○ |

**応答**

正常な応答は、更新されたパッケージを返します。 応答には、対応するパッケージ ID と、その説明、ステータス、有効期限、アーティファクトのリストに関する情報が含まれます。

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

パッケージを削除するには、 `/packages` エンドポイントをクリックし、削除するパッケージの ID を指定します。

**API 形式**

```http
DELETE /packages/{PACKAGE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {PACKAGE_ID} | 削除するパッケージの ID。 |

**リクエスト**

次のリクエストでは、ID がのパッケージが削除されます。 {PACKAGE_ID}.

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

正常な応答は、削除されたパッケージ ID を示す理由を返します。

```json
{
    "reason": "Package d30e0424a37b46ada6a5cf37f47a86ff deleted"
}
```

## パッケージのパブリッシュ {#publish}

パッケージのサンドボックスへのインポートを有効にするには、パブリッシュする必要があります。 に対するGETリクエスト `/packages` エンドポイントを使用して、公開するパッケージの ID を指定します。

**API 形式**

```http
GET /packages/{PACKAGE_ID}/export
```

| パラメーター | 説明 |
| --- | --- |
| {PACKAGE_ID} | 公開するパッケージの ID。 |

**リクエスト**

次のリクエストでは、という ID を持つパッケージがパブリッシュされます。 {PACKAGE_ID}.

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID}\export \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

| プロパティ | 説明 | タイプ | 必須 |
| --- | --- | --- | --- |
| `expiryPeriod` | このユーザー指定の期間は、パッケージが公開された日時からのパッケージの有効期限（日数）を定義します。 この値は負にはできません。<br> 値を指定しない場合、デフォルトは公開日から 90（日）として計算されます。 | 整数 | × |

**応答**

正常な応答は、公開されたパッケージを返します。

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

個々のパッケージを検索するには、 `/packages` リクエストパスにパッケージの対応する ID を含むエンドポイント。

**API 形式**

```http
GET /packages/{PACKAGE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {PACKAGE_ID} | 検索するパッケージの ID。 |

**リクエスト**

次のリクエストでは、 {PACKAGE_ID}.

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

正常な応答は、クエリされたパッケージ ID の詳細を返します。 応答には、名前、説明、公開日と有効期限、パッケージのソースサンドボックス、およびアーティファクトのリストが含まれます。

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

に対してGETリクエストをおこなうことで、組織内のすべてのパッケージをリストできます `/packages` endpoint.

**API 形式**

```http
GET /packages/?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| {QUERY_PARAMS} | 結果をフィルターするオプションのクエリパラメーター。詳しくは、 [クエリパラメーター](./appendix.md) を参照してください。 |

**リクエスト**

次のリクエストでは、 {QUERY_PARAMS}.

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/?property=status==DRAFT,PUBLISHED&property=createdDate>=2023-05-11T18:29:59.999Z&property=createdDate<=2023-05-16T18:29:59.999Z&start=0&orderby=-createdDate&limit=20 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、組織に属するパッケージのリスト（名前、ステータス、有効期限、アーティファクトのリストなどの詳細を含む）を返します。

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

このエンドポイントは、指定されたターゲットサンドボックス内の競合するオブジェクトを取得するために使用されます。 競合するオブジェクトは、ターゲットサンドボックスに既に存在する類似のオブジェクトを表します。

**API 形式**

```http
GET /packages/{PACKAGE_ID}/import?targetSandbox=targetSandboxName
```

| パラメーター | 説明 |
| --- | --- |
| {PACKAGE_ID} | 検索するパッケージの ID。 |

**リクエスト**

次のリクエストでは、 {PACKAGE_ID}.

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID}/import?targetSandbox=targetSandboxName \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

競合が応答で返されます。 応答には、元のパッケージと `alternatives` フラグメントをランキング順に並べ替えた配列として使用する。

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

## インポートを送信 {#submit-import}

>[!NOTE]
>
>代替アーティファクトがターゲットサンドボックスに既に存在するというのは、競合の解決に固有の機能です。

パッケージに対してPOSTリクエストを実行し、競合を確認して代替を提供したら、パッケージのインポートを送信できます。 `/packages` endpoint. 結果はペイロードとして提供され、ペイロードで指定された宛先サンドボックスのインポートジョブを開始します。

ペイロードは、インポートジョブのユーザー指定のジョブ名と説明も受け付けます。 ユーザーが指定した名前と説明が使用できない場合は、ジョブ名と説明にパッケージ名と説明が使用されます。

**API 形式**

```http
POST /packages/import
```

**リクエスト**

次のリクエストでは、 {PACKAGE_ID} 提供済み ペイロードは置換のマップで、エントリが存在する場合、キーは `artifactId` はパッケージで指定され、代わりには値が使用されます。 マップまたはペイロードが **空**、置き換えは実行されません。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/exim/packages/{PACKAGE_ID}/import?targetSandbox=targetSandboxName \
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
| `id` | パッケージの ID。 | 文字列 | ○ |
| `alternatives` | `alternatives` は、ソースサンドボックスアーティファクトの既存のターゲットサンドボックスアーティファクトへのマッピングを表します。 既に存在するので、インポートジョブでは、ターゲットサンドボックスにこれらのアーティファクトが作成されるのを防ぎます。 | 文字列 | × |

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

パッケージに対してPOSTリクエストを実行し、パッケージ内のエクスポートされたオブジェクトのすべての依存オブジェクトをリストします。 `/packages` エンドポイントを使用して、パッケージの ID を指定します。

**API 形式**

```http
POST /packages/{PACKAGE_ID}/children
```

| パラメーター | 説明 |
| --- | --- |
| {PACKAGE_ID} | パッケージの ID。 |

**リクエスト**

次のリクエストでは、 {PACKAGE_ID}.

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

正常な応答は、オブジェクトの子のリストを返します。

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

## すべてのパッケージアーティファクトをインポートするために、役割に基づく権限を確認します {#role-based-permissions}

パッケージアーティファクトを読み込む権限があるかどうかを確認するには、GETリクエストを `/packages` エンドポイントを使用して、パッケージの ID とターゲットサンドボックス名を指定します。

**API 形式**

```http
GET /packages/preflight/{packageId}?targetSandbox=<sandbox_name
```

| パラメーター | 説明 |
| --- | --- |
| {PACKAGE_ID} | インポートするパッケージの ID。 |

**リクエスト**

次のリクエストは、 {PACKAGE_ID} およびサンドボックス。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/preflight/{PACKAGE_ID}?targetSandbox=<sandbox_name> \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、必要な権限のリスト、不足している権限のリスト、アーティファクトのタイプ、作成を許可するかどうかの決定など、ターゲットサンドボックスのリソース権限を返します。

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

に対してGETリクエストを実行することで、現在の書き出し/読み込みジョブをリストできます。 `/packages` endpoint.

**API 形式**

```http
GET /packages/jobs?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| {QUERY_PARAMS} | 結果をフィルターするオプションのクエリパラメーター。詳しくは、 [クエリパラメーター](./appendix.md) を参照してください。 |

**リクエスト**

次のリクエストは、成功したすべてのインポートジョブをリストします。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/packages/jobs?property=requestType==IMPORT&property=jobStatus==SUCCESS&orderby=createdDate&start=0&limit=5 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

正常な応答は、成功したすべてのインポートジョブを返します。

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
