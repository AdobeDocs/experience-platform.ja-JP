---
keywords: Experience Platform；ホーム；人気の高いトピック；データ準備；apiガイド；スキーマ;
solution: Experience Platform
title: スキーマAPIエンドポイント
topic: schemas
description: 'Adobe Experience PlatformAPIの「/スキーマ」エンドポイントを使用すると、プラットフォームのMapperで使用するスキーマをプログラムによって取得、作成、更新できます。 '
translation-type: tm+mt
source-git-commit: 435d27f7187074c78209948c0e57b610b63d2055
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 9%

---



# スキーマエンドポイント

スキーマをMapperと組み合わせて使用すると、Adobe Experience Platformに取り込んだデータが取り込むデータと一致することを確認できます。 `/schemas`エンドポイントを使用すると、プラットフォームのMapperで使用するカスタムスキーマをプログラムで作成、リスト、取得できます。

>[!NOTE]
>
>このエンドポイントを使用して作成されたスキーマは、マッパーとマッピングセットでのみ使用されます。 他のプラットフォームサービスがアクセスできるスキーマを作成するには、『スキーマレジストリ開発者ガイド](../../xdm/api/schemas.md)』を読んでください。[

## すべてのスキーマの取得

`/schemas`エンドポイントにGETリクエストを行うことで、IMS組織で使用可能なすべてのマッパースキーマのリストを取得できます。

**API 形式**

`/schemas`エンドポイントは、結果のフィルタリングに役立ついくつかのクエリパラメーターをサポートしています。 これらのパラメーターのほとんどはオプションですが、高価なオーバーヘッドを削減するために、このパラメーターの使用を強くお勧めします。 ただし、リクエストの一部に`start`パラメーターと`limit`パラメーターの両方を含める必要があります。 複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

```http
GET /schemas?limit={LIMIT}&start={START}
GET /schemas?limit={LIMIT}&start={START}&name={NAME}
GET /schemas?limit={LIMIT}&start={START}&orderBy={ORDER_BY}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{LIMIT}` | **必須**. 返されるスキーマの数を指定します。 |
| `{START}` | **必須**. 結果のページのオフセットを指定します。結果の最初のページを取得するには、値を`start=0`に設定します。 |
| `{NAME}` | 名前に基づいてスキーマをフィルターします。 |
| `{ORDER_BY}` | 結果の順序を並べ替えます。 サポートされているフィールドは `modifiedDate` と `createdDate` です。プロパティの前に`+`または`-`を付けて、昇順または降順で並べ替えることができます。 |

**リクエスト**

次のリクエストは、IMS組織で作成された最後の2つのスキーマを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/conversion/schemas&start=0&limit=2 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

次の応答は、要求されたスキーマのリストと共にHTTPステータス200を返します。

>[!NOTE]
>
>次の応答は、領域のために切り捨てられました。

```json
{
    "data": [
        {
            "id": "823ac494f35e4e84bcb2f061378fcca6",
            "version": 0,
            "jsonSchema": {
                "title": "Sample schema",
                "type": "object",
                "properties": {
                    "_id": {
                        "title": "Identifier",
                        "description": "A unique identifier for the record.",
                        "type": "string",
                        "format": "uri-reference",
                        "meta:xdmField": "@id",
                        "meta:xdmType": "string"
                    },
                    "city": {
                        "title": "City",
                        "description": "The name of the city.",
                        "type": "string",
                        "meta:xdmField": "xdm:city",
                        "meta:xdmType": "string"
                    }
                }
            },
            "schemaRef": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/833b1d8a749943d49fe7e925ea19b5dc",
                "contentType": "1.0"
            }
        },
        {
            "id": "0f868d3a1b804fb0abf738306290ae79",
            "version": 0,
            "jsonSchema": {
                "title": "Sample schema 2",
                "type": "object",
                "properties": {
                    "_id": {
                        "title": "Identifier",
                        "description": "A unique identifier for the record.",
                        "type": "string",
                        "format": "uri-reference",
                        "meta:xdmField": "@id",
                        "meta:xdmType": "string"
                    },
                    "city": {
                        "title": "City",
                        "description": "The name of the city.",
                        "type": "string",
                        "meta:xdmField": "xdm:city",
                        "meta:xdmType": "string"
                    }
                }
            },
            "schemaRef": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/0f868d3a1b804fb0abf738306290ae79",
                "contentType": "1.0"
            }
        }
    ],
    "_page": {
        "count": 2,
        "limit": 2
    }
}
```

## スキーマの作成

`/schemas`エンドポイントにPOSTリクエストを行うことで、検証対象のスキーマを作成できます。 スキーマを作成する方法は3つあります。[JSONスキーマ](https://json-schema.org/)を送信、サンプルデータを使用、または既存のXDMスキーマを参照。

```http
POST /schemas
```

### JSONスキーマの使用

**リクエスト**

次のリクエストでは、[JSONスキーマ](https://json-schema.org/)を送信してスキーマを作成できます。

```shell
curl -X POST https://platform.adobe.io/data/foundation/conversion/schemas \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
{
    "jsonSchema": {
        "id": "string",
        "firstName": "string",
        "lastName": "string"
    }
}'
```

**応答** 

正常に応答すると、新しく作成したスキーマに関する情報と共にHTTPステータス200が返されます。

```json
{
    "id": "6daffabf14b1425292add3719afc8fab",
    "version": 0,
    "jsonSchema": {
        "id": "string",
        "firstName": "string",
        "lastName": "string"
    }
}
```

### サンプルデータの使用

**リクエスト**

次のリクエストでは、以前にアップロードしたサンプルデータを使用してスキーマを作成できます。

```shell
curl -X POST https://platform.adobe.io/data/foundation/conversion/schemas \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
 {
     "sampleId": "45439a93d48d47d098d26e0f0840cc02"  
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `sampleId` | スキーマ元のサンプルデータのID。 |

**応答** 

正常に応答すると、新しく作成したスキーマに関する情報と共にHTTPステータス200が返されます。

```json
{
    "id": "b2ee78bd55ac45f781a93fb8b90d99b2",
    "version": 0,
    "jsonSchema": {
        "title": "SampleSchema:45439a93d48d47d098d26e0f0840cc02",
        "type": "object",
        "properties": {
            "gender": {
                "title": "gender",
                "type": "string"
            },
            "last_name": {
                "title": "last_name",
                "type": "string"
            },
            "id": {
                "title": "id",
                "type": "string"
            },
            "ip_address": {
                "title": "ip_address",
                "type": "string"
            },
            "first_name": {
                "title": "first_name",
                "type": "string"
            },
            "email": {
                "title": "email",
                "type": "string"
            }
        }
    },
    "sampleId": "45439a93d48d47d098d26e0f0840cc02"
}
```

### XDMスキーマを参照

**リクエスト**

次のリクエストでは、既存のXDMスキーマを参照してスキーマを作成できます。

```shell
curl -X POST https://platform.adobe.io/data/foundation/conversion/schemas \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
 {
     "name": "outputSchema1",
     "schemaRef": {
         "id": "https://ns.adobe.com/{TENANT_ID}/schemas/0d555890502224d187b619f23c35c8d1",
         "contentType": "application/vnd.adobe.xed-full+json;version=1"
     }  
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | 作成するスキーマの名前。 |
| `schemaRef.id` | 参照しているスキーマのID。 |
| `schemaRef.contentType` | 参照先スキーマの応答形式を決定します。 このフィールドの詳細については、[スキーマレジストリ開発者ガイド](../../xdm/api/schemas.md#lookup)を参照してください。 |

**応答** 

正常に応答すると、新しく作成したスキーマに関する情報と共にHTTPステータス200が返されます。

>[!NOTE]
>
>次の応答は、領域のために切り捨てられました。

```json
{
    "id": "4b64daa51b774cb2ac21b61d80125ed0",
    "version": 0,
    "name": "schemaName",
    "jsonSchema": "{\"id\":null,\"schema\":null,\"_refId\":null,\"title\":\"SimpleUser\",...,\"imsOrg\":\"{IMS_ORG}\",\"$id\":\"https://ns.adobe.com/{TENANT_ID}/schemas/901c44cc5b2748488574f4e2824c5f96\"}",
    "schemaRef": {
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/901c44cc5b2748488574f4e2824c5f96",
        "contentType": "application/vnd.adobe.xed+json;version=1.0"
    }
}
```

## ファイルのアップロードを使用したスキーマの作成

変換元のJSONファイルをアップロードしてスキーマを作成できます。

**API 形式**

```http
POST /schemas/upload
```

**リクエスト**

次のリクエストでは、アップロードされたJSONファイルからスキーマを作成できます。

```shell
curl -X POST https://platform.adobe.io/data/foundation/conversion/schemas/upload \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: multipart/form-data' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -F 'file=@{PATH_TO_FILE}.json'
```

**応答** 

正常に応答すると、新しく作成したスキーマに関する情報と共にHTTPステータス200が返されます。

```json
{
    "id": "292add3716daffabf14b14259afc8fab",
    "version": 0,
    "jsonSchema": {
        "id": "string",
        "firstName": "string",
        "lastName": "string"
    }
}
```

## 特定のスキーマの取得

`/schemas`エンドポイントにGETリクエストを送信し、取得するスキーマのIDをリクエストパスに指定することで、特定のスキーマに関する情報を取得できます。

**API 形式**

```http
GET /schemas/{SCHEMA_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEMA_ID}` | 検索するスキーマのID。 |

**リクエスト**

次のリクエストは、指定されたスキーマに関する情報を取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/conversion/schemas/0f868d3a1b804fb0abf738306290ae79 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

成功した場合、指定されたスキーマに関する情報と共にHTTPステータス200が返されます。

```json
{
    "id": "0f868d3a1b804fb0abf738306290ae79",
    "version": 0,
    "jsonSchema": {
        "title": "Sample schema",
        "description": "Sample description",
        "type": "object",
        "properties": {
            "_id": {
                "title": "Identifier",
                "description": "A unique identifier for the record.",
                "type": "string",
                "format": "uri-reference",
                "meta:xdmField": "@id",
                "meta:xdmType": "string"
            },
            "personalEmail": {
                "title": "Personal Email",
                "description": "A personal email address.",
                "type": "object",
                "properties": {
                    "primary": {
                        "title": "Primary",
                        "description": "Primary email indicator.\n\nA Profile can have only one `primary` email address at a given point of time.\n",
                        "type": "boolean",
                        "meta:xdmField": "xdm:primary",
                        "meta:xdmType": "boolean"
                    },
                    "address": {
                        "title": "Address",
                        "description": "The technical address, e.g 'name@domain.com' as commonly defined in RFC2822 and subsequent standards.",
                        "type": "string",
                        "format": "email",
                        "meta:xdmField": "xdm:address",
                        "meta:xdmType": "string"
                    }
                },
                "meta:xdmField": "xdm:personalEmail",
                "meta:referencedFrom": "https://ns.adobe.com/xdm/context/emailaddress",
                "meta:xdmType": "object"
            }
        },
        "imsOrg": "6A29340459CA8D350A49413A@AdobeOrg",
        "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/833b1d8a749943d49fe7e925ea19b5dc"
    },
    "schemaRef": {
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/833b1d8a749943d49fe7e925ea19b5dc",
        "contentType": "1.0"
    }
}
```
