---
keywords: Experience Platform；ホーム；人気の高いトピック；データ準備；apiガイド；サンプルデータ；
solution: Experience Platform
title: サンプルデータAPIエンドポイント
topic: sample data
description: 'Adobe Experience PlatformAPIの「/samples」エンドポイントを使用すると、マッピングされたサンプルデータをプログラムによって取得、作成、更新および検証できます。 '
translation-type: tm+mt
source-git-commit: a2c966ae2401faa572cbba974ce6f572d5280a8f
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 8%

---


# サンプルデータエンドポイント

マッピングセットのスキーマを作成する場合は、サンプルデータを使用できます。 データ準備APIの`/samples`エンドポイントを使用して、サンプルデータをプログラムによって取得、作成および更新できます。

## リストサンプルデータ

`/samples`エンドポイントにGETリクエストを行うことで、IMS組織のすべてのマッピングサンプルデータのリストを取得できます。

**API 形式**

`/samples`エンドポイントは、結果のフィルタリングに役立ついくつかのクエリパラメーターをサポートしています。 現在、リクエストには、`start`パラメーターと`limit`パラメーターの両方を含める必要があります。

```http
GET /samples?limit={LIMIT}&start={START}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{LIMIT}` | **必須**. 返されるマッピングサンプルデータの数を指定します。 |
| `{START}` | **必須**. 結果のページのオフセットを指定します。結果の最初のページを取得するには、値を`start=0`に設定します。 |

**リクエスト**

次のリクエストは、IMS組織内の最後の2つのマッピングサンプルデータを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/conversion/samples?limit=2&start=0 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答が成功すると、HTTPステータス200が返され、マッピングされたサンプルデータの最後の2つのオブジェクトに関する情報が返されます。

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

`/samples`エンドポイントにPOSTリクエストを行うことで、サンプルデータを作成できます。

```http
POST /samples
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/conversion/samples \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '
  {
    "sampleData": "{\"FirstName\":\"Johnson\",\"LastName\":\"Smith\", \"id\": \"3197210762560\"}",
    "sampleType": "JSON"    
  }'
```

**応答** 

正常に応答すると、新たに作成されたサンプルデータに関する情報と共にHTTPステータス200が返されます。

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

`/samples/upload`エンドポイントにPOSTリクエストを行うことで、ファイルを使用してサンプルデータを作成できます。

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -F 'file=@{PATH_TO_FILE}.json'
```

**応答** 

正常に応答すると、新たに作成されたサンプルデータに関する情報と共にHTTPステータス200が返されます。

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

## 特定のサンプルデータオブジェクトを検索する

`/samples`エンドポイントへのGETリクエストのパスにサンプルデータの特定のオブジェクトのIDを指定すると、サンプルデータの特定のオブジェクトを検索できます。

**API 形式**

```http
GET /samples/{SAMPLE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SAMPLE_ID}` | 取得するサンプルデータオブジェクトのID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/conversion/samples/1fc0b6c20bae49d8ab33209ed126bdcd \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、取得したいサンプルデータオブジェクトの情報と共にHTTPステータス200を返します。

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

`/samples`エンドポイントへのPUTリクエストのパスにIDを指定することで、特定のサンプルデータオブジェクトを更新できます。

**API 形式**

```http
PUT /samples/{SAMPLE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SAMPLE_ID}` | 更新するサンプルデータオブジェクトのID。 |

**リクエスト**

次のリクエストは、指定されたサンプルデータを更新します。 特に、姓を「Smith」から「Lee」に更新します。

```shell
curl -X PUT https://platform.adobe.io/data/foundation/conversion/samples/1fc0b6c20bae49d8ab33209ed126bdcd \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '
  {
    "sampleData": "{\"FirstName\":\"Johnson\",\"LastName\":\"Lee\", \"id\": \"3197210762560\"}",
    "sampleType": "JSON"    
  }'
```

**応答** 

応答が成功すると、更新されたサンプルデータに関する情報と共にHTTPステータス200が返されます。

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
