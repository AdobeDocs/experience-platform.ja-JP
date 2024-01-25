---
title: フローサービス API を使用して、OracleNetSuite アクティビティのソース接続とデータフローを作成します。
description: フローサービス API を使用して、ソース接続とデータフローを作成し、OracleNetSuite イベントデータをExperience Platformに取り込む方法を説明します。
hide: true
hidefromtoc: true
badge: ベータ版
source-git-commit: 053cf0af327b39830f025686e0f8f67c27f1c45c
workflow-type: tm+mt
source-wordcount: '1961'
ht-degree: 54%

---

# のソース接続とデータフローの作成 [!DNL Oracle NetSuite Activities] フローサービス API の使用

>[!NOTE]
>
>[!DNL Oracle NetSuite Activities] ソースはベータ版です。詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

次のチュートリアルを読んで、 [!DNL Oracle NetSuite Activities] を使用してAdobe Experience Platformにアカウント [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL Oracle NetSuite Activities] の使用 [!DNL Flow Service] API.

### 認証

詳しくは、 [[!DNL Oracle NetSuite] 概要](../../../../connectors/marketing-automation/oracle-netsuite.md) 認証資格情報の取得方法の詳細。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)のガイドを参照してください。

## 接続 [!DNL Oracle NetSuite Activities] を使用して Platform に [!DNL Flow Service] API

以下のガイドに従って、 [!DNL Oracle NetSuite Activities] ソース、ソース接続を作成し、イベントデータをExperience Platformに取り込むためのデータフローを作成します。

### ベース接続の作成 {#base-connection}

ベース接続では、ソースとExperience Platform間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID など）が保持されます。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Oracle NetSuite Activities] 認証資格情報をリクエスト本文の一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Oracle NetSuite Activities] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Oracle NeSuite Activities base connection",
      "description": "Authenticated base connection for Oracle NetSuite Activities",
      "connectionSpec": {
          "id": "fdf850b4-5a8d-4a5a-9ce8-4caef9abb2a8",
          "version": "1.0"
      },
      "auth": {
          "specName": "OAuth2 Client Credential",
          "params": {
              "clientId": "{CLIENT_ID}",
              "clientSecret": "{CLIENT_SECRET}"
              "accessTokenUrl": "{ACCESS_TOKEN_URL}",
              "accessToken": "{ACCESS_TOKEN_URL}"
          }
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ベース接続の名前。ベース接続の情報を検索する際に使用できるので、ベース接続の名前はわかりやすいものにしてください。 |
| `description` | ベース接続に関する詳細情報を提供するために含めることができるオプションの値です。 |
| `connectionSpec.id` | ソースの接続仕様 ID。この ID は、ソースが登録および承認された後に、[!DNL Flow Service] API から取得することができます。 |
| `auth.specName` | Platform へのソースの認証に使用する認証タイプ。 |
| `auth.params.clientId` | 統合レコードを作成する際のクライアント ID 値。 統合レコードを作成するプロセスが見つかります [ここ](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_157771733782.html#procedure_157838925981). 値は、次のような 64 文字の文字列です。 `7fce.....b42f`. |
| `auth.params.clientSecret` | 統合レコードを作成する際のクライアント ID 値。 統合レコードを作成するプロセスが見つかります [ここ](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_157771733782.html#procedure_157838925981). 値は、次のような 64 文字の文字列です。 `5c98.....1b46`. |
| `auth.params.accessTokenUrl` | The [!DNL NetSuite] アクセストークン URL（に類似） `https://{ACCOUNT_ID}.suitetalk.api.netsuite.com/services/rest/auth/oauth2/v1/token` ここで、 ACCOUNT_ID を [!DNL NetSuite] アカウント ID。 |
| `auth.params.accessToken` | アクセストークンの値は、 [手順 2](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158081952044.html#Step-Two-POST-Request-to-the-Token-Endpoint) の [OAuth 2.0 認証コード付与フロー](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158074210415.html#OAuth-2.0-Authorization-Code-Grant-Flow) チュートリアル アクセストークンの有効期限は 60 分間のみ有効です。 値は、次のような 1024 文字の文字列で、JSON Web トークン (JWT) 形式で書式設定されます。 `eyJr......f4V0`. |

**応答**

リクエストが成功した場合は、一意の接続識別子（`id`）を含む、新しく作成されたベース接続が返されます。この ID は、次の手順でソースのファイル構造と内容を調べるために必要です。

```json
{
    "id": "60c81023-99b4-4aae-9c31-472397576dd2",
    "etag": "\"fa003785-0000-0200-0000-6555c5310000\""
}
```

### ソースを参照 {#explore}

ベース接続 ID を取得したら、 `/connections` エンドポイントを使用して、ベース接続 ID をクエリパラメーターとして指定する必要があります。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=rest&object={OBJECT}&fileType={FILE_TYPE}&preview={PREVIEW}&sourceParams={SOURCE_PARAMS}
```

**リクエスト**

ソースのファイル構造とコンテンツを調べるために GET リクエストを実行する場合、次の表に示すクエリのパラメーターを含める必要があります。

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | 前の手順で生成したベース接続 ID。 |
| `objectType=rest` | 参照するオブジェクトのタイプ。 現在、この値は常にに設定されています。 `rest`. |
| `{OBJECT}` | このパラメーターは、特定のディレクトリを表示する場合にのみ必要です。 その値は、参照するディレクトリのパスを表します。 このソースの場合、値は次のようになります。 `json`. |
| `fileType=json` | Platform に取り込むファイルのファイルタイプ。 現在、 `json` は、サポートされている唯一のファイルタイプです。 |
| `{PREVIEW}` | 接続のコンテンツがプレビューをサポートするかどうかを定義するブール値です。 |
| `{SOURCE_PARAMS}` | Platform に取り込むソースファイルのパラメーターを定義します。 の受け入れ可能な format-type を取得するには `{SOURCE_PARAMS}`の場合は、base64 で文字列全体をエンコードする必要があります。 <br> の値は空です [!DNL Oracle NetSuite Activities].</li></ul> |

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/f5421911-6f6c-41c7-aafa-5d9d2ce51535/explore?objectType=rest&object=json&fileType=json&preview=true&sourceParams=e30%3D' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、次のような JSON 構造を返します。

+++「 」を選択して JSON ペイロードを表示します。

```json
{
    "format": "hierarchical",
    "schema": {
        "type": "object",
        "properties": {
            "totalResults": {
                "type": "integer",
                "minimum": -9007199254740992,
                "maximum": 9007199254740991
            },
            "offset": {
                "type": "integer",
                "minimum": -9007199254740992,
                "maximum": 9007199254740991
            },
            "count": {
                "type": "integer",
                "minimum": -9007199254740992,
                "maximum": 9007199254740991
            },
            "hasMore": {
                "type": "boolean"
            },
            "links": {
                "type": "array",
                "items": {
                    "type": "object",
                    "properties": {
                        "rel": {
                            "type": "string"
                        },
                        "href": {
                            "type": "string"
                        }
                    }
                }
            },
            "items": {
                "type": "object",
                "properties": {
                    "owner": {
                        "type": "string"
                    },
                    "createddate": {
                        "type": "string"
                    },
                    "lastmodifieddate": {
                        "type": "string"
                    },
                    "accesslevel": {
                        "type": "string"
                    },
                    "alldayevent": {
                        "type": "string"
                    },
                    "message": {
                        "type": "string"
                    },
                    "startdate": {
                        "type": "string"
                    },
                    "title": {
                        "type": "string"
                    },
                    "organizer": {
                        "type": "string"
                    },
                    "response": {
                        "type": "string"
                    },
                    "links": {
                        "type": "array",
                        "items": {
                            "type": "object",
                            "properties": {}
                        }
                    },
                    "location": {
                        "type": "string"
                    },
                    "timedevent": {
                        "type": "string"
                    },
                    "id": {
                        "type": "string"
                    },
                    "status": {
                        "type": "string"
                    }
                }
            }
        }
    },
    "data": [
        {
            "totalResults": 13,
            "offset": 0,
            "count": 13,
            "hasMore": false,
            "links": [
                {
                    "rel": "self",
                    "href": "https://{ACCOUNT_ID}.suitetalk.api.netsuite.com/services/rest/query/v1/suiteql"
                }
            ],
            "items": {
                "accesslevel": "BUSY",
                "alldayevent": "F",
                "createddate": "2022-12-15",
                "id": "5",
                "lastmodifieddate": "2022-12-15",
                "location": "Caffe West",
                "message": "Bring Monthly Results",
                "organizer": "-5",
                "owner": "-5",
                "response": "ACCEPTED",
                "startdate": "2022-12-15",
                "status": "CONFIRMED",
                "timedevent": "T",
                "title": "Meeting with Tom"
            }
        },
        {
            "totalResults": 13,
            "offset": 0,
            "count": 13,
            "hasMore": false,
            "links": [
                {
                    "rel": "self",
                    "href": "https://{ACCOUNT_ID}.suitetalk.api.netsuite.com/services/rest/query/v1/suiteql"
                }
            ],
            "items": {
                "accesslevel": "BUSY",
                "alldayevent": "F",
                "createddate": "2023-02-01",
                "id": "103",
                "lastmodifieddate": "2023-02-01",
                "location": "Caffe West",
                "message": "Bring Monthly Results",
                "organizer": "-5",
                "owner": "-5",
                "response": "ACCEPTED",
                "startdate": "2022-12-15",
                "status": "CONFIRMED",
                "timedevent": "T",
                "title": "Meeting with Tom"
            }
        },
        {
            "totalResults": 13,
            "offset": 0,
            "count": 13,
            "hasMore": false,
            "links": [
                {
                    "rel": "self",
                    "href": "https://{ACCOUNT_ID}.suitetalk.api.netsuite.com/services/rest/query/v1/suiteql"
                }
            ],
            "items": {
                "accesslevel": "BUSY",
                "alldayevent": "F",
                "createddate": "2023-11-09",
                "id": "204",
                "lastmodifieddate": "2023-11-09",
                "location": "Caffe West",
                "message": "Bring Monthly Results",
                "organizer": "-5",
                "owner": "-5",
                "response": "ACCEPTED",
                "startdate": "2023-12-15",
                "status": "CONFIRMED",
                "timedevent": "T",
                "title": "Meeting with Tom"
            }
        },
    ]
}
```

+++

### ソース接続の作成 {#source-connection}

ソース接続を作成するには、 `/sourceConnections` エンドポイント [!DNL Flow Service] API. ソース接続は、接続 ID、ソースデータファイルへのパス、接続仕様 ID から構成されます。

**API 形式**

```https
POST /sourceConnections
```

**リクエスト**

次のリクエストは、 [!DNL Oracle NetSuite Activities].

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Oracle NetSuite Activities Source Connection",
      "description": "Oracle NetSuite Activities Source Connection",
      "baseConnectionId": "60c81023-99b4-4aae-9c31-472397576dd2",
      "connectionSpec": {
          "id": "fdf850b4-5a8d-4a5a-9ce8-4caef9abb2a8",
          "version": "1.0"
      },
      "data": {
          "format": "json"
      },
      "params": {
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前はわかりやすいものにしてください。 |
| `description` | 含めることでソース接続に関する詳細情報を提供できるオプションの値です。 |
| `baseConnectionId` | [!DNL Oracle NetSuite Activities] のベース接続 ID。この ID は、前の手順で生成されました。 |
| `connectionSpec.id` | ソースに対応する接続仕様の ID。 |
| `data.format` | 取り込む [!DNL Oracle NetSuite Activities] データの形式。現在、サポートされているデータ形式は `json` のみです。 |

リクエストが成功した場合は、新たに作成されたソース接続の一意の ID（`id`）が返されます。この ID は、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "574c049f-29fc-411f-be0d-f80002025f51",
    "etag": "\"0704acb3-0000-0200-0000-6555c5470000\""
}
```

### ターゲット XDM スキーマの作成 {#target-schema}

ソースデータを Platform で使用するには、必要に応じてターゲットスキーマを作成してソースデータを構造化する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれる Platform データセットを作成します。

[Schema Registry API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/) に POST リクエストを実行することで、ターゲット XDM スキーマを作成できます。

ターゲット XDM スキーマの作成手順について詳しくは、 [API を使用したスキーマの作成](../../../../../xdm/api/schemas.md#create-a-schema)に関するチュートリアルを参照してください。

### ターゲットデータセットの作成 {#target-dataset}

[Catalog Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) に POST リクエストを実行し、その際にペイロード内でターゲットスキーマの ID を指定することで、ターゲットデータセットを作成できます。

ターゲットデータセットの作成手順について詳しくは、 [API を使用したデータセットの作成](../../../../../catalog/api/create-dataset.md)に関するチュートリアルを参照してください。

### ターゲット接続の作成 {#target-connection}

ターゲット接続は、取り込んだデータの保存先への接続を表します。 ターゲット接続を作成するには、データレイクに対応する固定接続仕様 ID を指定する必要があります。 この ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。

これで、ターゲットスキーマとターゲットデータセット、およびデータレイクへの接続仕様 ID の一意の識別子が得られました。 これらの識別子を使用すると、受信ソースデータを格納するデータセットを指定する [!DNL Flow Service] API を使用して、ターゲット接続を作成することができます。

**API 形式**

```https
POST /targetConnections
```

**リクエスト**

次のリクエストは、[!DNL Oracle NetSuite Activities] のターゲット接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Oracle NetSuite Activities Target Connection Generic Rest",
      "description": "Oracle NetSuite Activities Target Connection Generic Rest",
      "connectionSpec": {
          "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
          "version": "1.0"
      },
      "data": {
          "format": "parquet_xdm",
          "schema": {
              "id": "https://ns.adobe.com/{TENANT_ID}/schemas/325fd5394ba421246b05c0a3c2cd5efeec2131058a63d473",
              "version": "1.2"
          }
      },
      "params": {
          "dataSetId": "65004470082ac828d2c3d6a0"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | ターゲット接続の名前。ターゲット接続の情報を検索に使用できるように、ターゲット接続はわかりやすい名前にしてください。 |
| `description` | ターゲット接続に関する詳細を提供するために含めることができるオプションの値です。 |
| `connectionSpec.id` | データレイクに対応する接続仕様 ID。 この修正済み ID は `6b137bf6-d2a0-48c8-914b-d50f4942eb85` です。 |
| `data.format` | 取り込む [!DNL Oracle NetSuite Activities] データの形式。 |
| `params.dataSetId` | 前の手順で取得したターゲットデータセット ID。 |

**応答**

リクエストが成功した場合は、新しいターゲット接続の一意の ID（`id`）が返されます。この ID は、後の手順で必要になります。

```json
{
    "id": "382fc614-3c5b-46b9-a971-786fb0ae6c5d",
    "etag": "\"e0016100-0000-0200-0000-655707a40000\""
}
```

### マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。これは、次に対してPOSTリクエストを実行する [[!DNL Data Prep] API](https://www.adobe.io/experience-platform-apis/references/data-prep/) リクエストペイロード内で定義されたデータマッピングを使用して、

**API 形式**

```http
POST /conversion/mappingSets
```

**リクエスト**

次のリクエストは、 [!DNL DNL NetSuite Activities]

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/conversion/mappingSets' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "outputSchema": {
          "schemaRef": {
              "id": "https://ns.adobe.com/{TENANT_ID}/schemas/b156e6f818f923e048199173c45e55e20fd2487f5eb03d22",
              "contentType": "application/vnd.adobe.xed-full+json;version=1"
          }
      },
      "mappings": [
        {
            "sourceType": "ATTRIBUTE",
            "source": "items.id",
            "destination": "_extconndev.NSA_ID"
        },
        {
            "sourceType": "ATTRIBUTE",
            "source": "items.title",
            "destination": "_extconndev.NSA_title"
        },
        {
            "sourceType": "ATTRIBUTE",
            "source": "items.createddate",
            "destination": "_extconndev.NSA_createddate"
        },
        {
            "sourceType": "ATTRIBUTE",
            "source": "items.location",
            "destination": "_extconndev.NSA_location"
        },
        {
            "sourceType": "ATTRIBUTE",
            "source": "items.lastmodifieddate",
            "destination": "_extconndev.NSA_lastmodifieddate"
        }
      ],
    "outputSchema": {
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/325fd5394ba421246b05c0a3c2cd5efeec2131058a63d473",
            "contentType": "application/vnd.adobe.xed-full+json;version=1"
        }
    }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `outputSchema.schemaRef.id` | 以前の手順で生成された[ターゲット XDM スキーマ](#target-schema)の ID。 |
| `mappings.sourceType` | マッピングするソース属性タイプ。 |
| `mappings.source` | 宛先 XDM パスにマッピングする必要があるソース属性。 |
| `mappings.destination` | ソース属性がマッピングされている宛先 XDM パス。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成されたマッピングの詳細が返されます。この値は、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "ddf0592bcc9d4ac391803f15f2429f87",
    "version": 0,
    "createdDate": 1597784069368,
    "modifiedDate": 1597784069368,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}"
}
```

### フローの作成 {#flow}

からデータを取り込むための最後の手順 [!DNL Oracle NetSuite Activities] を Platform に送信する場合、データフローを作成します。 現時点で、次の必要な値の準備ができています。

* [ソース接続 ID](#source-connection)
* [ターゲット接続 ID](#target-connection)
* [マッピング ID](#mapping)

データフローは、ソースからデータをスケジュールおよび収集する役割を果たします。ペイロードに前述の値を提供しながら POST リクエストを実行することで、データフローを作成することができます。

**API 形式**

```http
POST /flows
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Oracle NetSuite Activities connector Flow Generic Rest",
    "description": "Oracle NetSuite Activities connector Description Flow Generic Rest",
    "flowSpec": {
        "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
        "version": "1.0"
    },
    "sourceConnectionIds": [
        "d8827440-339f-428d-bf38-5e2ab1f0f7bb"
    ],
    "targetConnectionIds": [
        "e349a15e-c639-4047-8b2a-154aa7a857d7"
    ],
    "transformations": [
        {
            "name": "Mapping",
            "params": {
                "mappingId": "10787532e0994eb686e76bdab69a9e88",
                "mappingVersion": 0
            }
        }
    ],
      "scheduleParams": {
          "startTime": 1700202649,
          "frequency": "once"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | データフローの名前。データフローの情報を検索する際に使用できるので、データフローはわかりやすい名前にしてください。 |
| `description` | データフローの詳細を指定するために含めることができるオプションの値です。 |
| `flowSpec.id` | データフローの作成に必要なフロー仕様 ID。この修正済み ID は `6499120c-0b15-42dc-936e-847ea3c24d72` です。 |
| `flowSpec.version` | フロー仕様 ID の対応するバージョン。この値のデフォルトは `1.0` です。 |
| `sourceConnectionIds` | 以前の手順で生成された[ソース接続 ID](#source-connection)。 |
| `targetConnectionIds` | 以前の手順で生成された[ターゲット接続 ID](#target-connection)。 |
| `transformations` | このプロパティには、データに適用する必要がある様々な変換が含まれています。このプロパティは、XDM に準拠していないデータを Platform に取り込む場合に必要です。 |
| `transformations.name` | 変換に割り当てられた名前。 |
| `transformations.params.mappingId` | 以前の手順で生成された[マッピング ID](#mapping)。 |
| `transformations.params.mappingVersion` | マッピング ID の対応するバージョン。この値のデフォルトは `0` です。 |
| `scheduleParams.startTime` | このプロパティには、データフローの取り込みスケジュールに関する情報が含まれています。 |
| `scheduleParams.frequency` | データフローがデータを収集する頻度。 |
| `scheduleParams.interval` | インターバルは 2 つの連続したフロー実行の間隔を指定します。間隔の値は、ゼロ以外の整数である必要があります。 |

**応答**

正常な応答は、新しく作成したデータフローの ID（`id`）を返します。この ID を使用して、データフローを監視、更新または削除できます。

```json
{
     "id": "84c64142-1741-4b0b-95a9-65644eba0cf6",
     "etag": "\"3901770b-0000-0200-0000-655708970000\""
}
```

## 付録

次の節では、データフローを監視、更新、削除する手順について説明します。

### データフローの監視

データフローが作成されると、それを通して取り込まれるデータを監視し、フローの実行状況、完了状況、エラーなどの情報を確認することができます。API の完全な例については、 [API を使用したソースデータフローの監視](../../monitor.md).

### データフローの更新

に対するPATCHリクエストを実行して、データフローの名前や説明、実行スケジュールおよび関連するマッピングセットなどの詳細を更新します。 `/flows` の終点 [!DNL Flow Service] API を使用してデータフローの ID を指定します。 PATCHリクエストをおこなう場合、データフローの一意の `etag` （内） `If-Match` ヘッダー。 API の完全な例については、 [API を使用したソースデータフローの更新](../../update-dataflows.md).

### アカウントを更新

に対してPATCHリクエストを実行して、ソースアカウントの名前、説明および資格情報を更新します。 [!DNL Flow Service] ベース接続 ID をクエリパラメーターとして指定する際の API。 PATCHリクエストをおこなう場合、ソースアカウントの一意の `etag` （内） `If-Match` ヘッダー。 API の完全な例については、 [API を使用したソースアカウントの更新](../../update.md).

### データフローの削除

に対してDELETEリクエストを実行して、データフローを削除する [!DNL Flow Service] クエリパラメーターの一部として削除するデータフローの ID を指定する際の API。 API の完全な例については、 [API を使用したデータフローの削除](../../delete-dataflows.md).

### アカウントを削除

アカウントを削除するには、 [!DNL Flow Service] 削除するアカウントのベース接続 ID を指定する際の API。 API の完全な例については、 [API を使用したソースアカウントの削除](../../delete.md).
