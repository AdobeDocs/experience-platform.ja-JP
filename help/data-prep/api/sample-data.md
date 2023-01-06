---
keywords: Experience Platform;ホーム;人気のあるトピック;データ準備;apiガイド;サンプルデータ;
solution: Experience Platform
title: サンプルデータ API エンドポイント
description: Adobe Experience Platform API で「/samples」エンドポイントを使用し、マッピングのサンプルデータをプログラムにより取得、作成、更新および検証できます。
source-git-commit: d39ae3a31405b907f330f5d54c91b95c0f999eee
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 100%

---


# サンプルデータエンドポイント

マッピングセットのスキーマを作成する際に、サンプルデータを使用できます。データ準備 API で `/samples` エンドポイントを使用し、サンプルデータをプログラムにより取得、作成、更新できます。

## サンプルデータのリスト

IMS 組織で利用可能なすべてのマッピングサンプルデータのリストを取得するには、`/samples` エンドポイントに対して GET リクエストをおこないます。

**API 形式**

`/samples` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。現在、リクエストの一部に `start` パラメーターと `limit` パラメーターの両方を含める必要があります。

```http
GET /samples?limit={LIMIT}&start={START}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{LIMIT}` | **必須**。返されるマッピングサンプルデータの数を指定します。 |
| `{START}` | **必須**。結果のページのオフセットを指定します。結果の最初のぺージを取得するには、値を `start=0` に設定します。 |

**リクエスト**

次のリクエストでは、IMS 組織内の最後の 2 つのマッピングサンプルデータを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/conversion/samples?limit=2&start=0 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答が成功すると、HTTP ステータス 200 と、マッピングサンプルデータの最後の 2 つのオブジェクトに関する情報が返されます。

```json
{
    "data": [
        {
            "id": "2a429a0057ce47109a4b3e2bc256d755",
            "version": 0,
            "createdDate": 1582250863000,
            "modifiedDate": 1582250863000,
            "createdBy": "acp_xql_gateway",
            "modifiedBy": "acp_xql_gateway",
            "sampleData": "{\"id\":\"\",\"first_name\":\"\",\"last_name\":\"\",\"email\":\"\",\"gender\":\"\",\"ip_address\":\"\"}",
            "sampleType": "JSON"
        },
        {
            "id": "0248bfb352214f908bdd6cf9c19447e1",
            "version": 0,
            "createdDate": 1582250892000,
            "modifiedDate": 1582250892000,
            "createdBy": "acp_xql_gateway",
            "modifiedBy": "acp_xql_gateway",
            "sampleData": "{\"id\":\"\",\"first_name\":\"\",\"last_name\":\"\",\"email\":\"\",\"gender\":\"\",\"ip_address\":\"\"}",
            "sampleType": "JSON"
        }
    ],
    "_page": {
        "count": 2,
        "limit": 2
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `sampleData` |  |
| `sampleType` |  |

## サンプルデータの作成

`/samples` エンドポイントに対して POST リクエストをおこなうと、サンプルデータを作成できます。

```http
POST /samples
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/conversion/samples \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '
  {
    "sampleData": "{\"FirstName\":\"Johnson\",\"LastName\":\"Smith\", \"id\": \"3197210762560\"}",
    "sampleType": "JSON"    
  }'
```

**応答**

応答が成功すると、HTTP ステータス 200 と、新しく作成されたサンプルデータに関する情報が返されます。

```json
{
    "id": "1fc0b6c20bae49d8ab33209ed126bdcd",
    "version": 0,
    "createdDate": 1615335404492,
    "modifiedDate": 1615335404492,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}",
    "sampleData": "{\"FirstName\":\"carl\",\"LastName\":\"hooper\", \"id\": \"123456\"}",
    "sampleType": "JSON"
}
```

## ファイルをアップロードしてサンプルデータを作成する

`/samples/upload` エンドポイントに POST リクエストをおこなうと、ファイルを使用してサンプルデータを作成できます。

**API 形式**

```http
POST /samples/upload
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/conversion/samples \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: multipart/form-data' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -F 'file=@{PATH_TO_FILE}.json'
```

**応答**

応答が成功すると、HTTP ステータス 200 と、新しく作成されたサンプルデータに関する情報が返されます。

```json
{
    "id": "1fb33209ed126bdcdc0b6c20bae49d8a",
    "version": 0,
    "createdDate": 1615335404492,
    "modifiedDate": 1615335404492,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}",
    "sampleData": "{\"FirstName\":\"carl\",\"LastName\":\"hooper\", \"id\": \"123456\"}",
    "sampleType": "JSON"
}
```

## 特定のサンプルデータオブジェクトの検索

`/samples` エンドポイントに対する GET リクエストのパスで ID を指定することで、サンプルデータの特定のオブジェクトを検索できます。

**API 形式**

```http
GET /samples/{SAMPLE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SAMPLE_ID}` | 取得するサンプルデータオブジェクトの ID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/conversion/samples/1fc0b6c20bae49d8ab33209ed126bdcd \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、HTTP ステータス 200 と、取得するサンプルデータオブジェクトの情報が返されます。

```json
{
    "id": "1fc0b6c20bae49d8ab33209ed126bdcd",
    "version": 0,
    "createdDate": 1615335404000,
    "modifiedDate": 1615335404000,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}",
    "sampleData": "{\"FirstName\":\"carl\",\"LastName\":\"hooper\", \"id\": \"123456\"}",
    "sampleType": "JSON"
}
```

## サンプルデータの更新

特定のサンプルデータオブジェクトを更新するには、`/samples` エンドポイントに対する PUT リクエストのパスで ID を指定します。

**API 形式**

```http
PUT /samples/{SAMPLE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SAMPLE_ID}` | 更新するサンプルデータオブジェクトの ID。 |

**リクエスト**

次のリクエストは、指定されたサンプルデータを更新します。特に、姓を「Smith」から「Lee」に更新します。

```shell
curl -X PUT https://platform.adobe.io/data/foundation/conversion/samples/1fc0b6c20bae49d8ab33209ed126bdcd \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '
  {
    "sampleData": "{\"FirstName\":\"Johnson\",\"LastName\":\"Lee\", \"id\": \"3197210762560\"}",
    "sampleType": "JSON"    
  }'
```

**応答** 

応答が成功すると、HTTP ステータス 200 と、更新されたサンプルデータに関する情報が返されます。

```json
{
    "id": "1fc0b6c20bae49d8ab33209ed126bdcd",
    "version": 1,
    "createdDate": 1615335404000,
    "modifiedDate": 1615337870375,
    "createdBy": "CAEB5DE75E6FBFAC0A494110@techacct.adobe.com",
    "modifiedBy": "CAEB5DE75E6FBFAC0A494110@techacct.adobe.com",
    "sampleData": "{\"FirstName\":\"Johnson\",\"LastName\":\"Lee\", \"id\": \"123456\"}",
    "sampleType": "JSON"
}
```
