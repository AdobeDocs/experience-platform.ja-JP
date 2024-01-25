---
title: フローサービス API を使用して、OracleNetSuite Entities のソース接続とデータフローを作成します。
description: フローサービス API を使用して、ソース接続とデータフローを作成し、OracleNetSuite の連絡先と顧客データをExperience Platformに取り込む方法を説明します。
hide: true
hidefromtoc: true
badge: ベータ版
source-git-commit: 053cf0af327b39830f025686e0f8f67c27f1c45c
workflow-type: tm+mt
source-wordcount: '2163'
ht-degree: 50%

---

# のソース接続とデータフローの作成 [!DNL Oracle NetSuite Entities] フローサービス API の使用

>[!NOTE]
>
>[!DNL Oracle NetSuite Entities] ソースはベータ版です。詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

次のチュートリアルを読んで、連絡先と顧客データを [!DNL Oracle NetSuite Activities Entities] を使用してAdobe Experience Platformにアカウント [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL Oracle NetSuite Entities] の使用 [!DNL Flow Service] API.

### 認証

詳しくは、 [[!DNL Oracle NetSuite] 概要](../../../../connectors/marketing-automation/oracle-netsuite.md) 認証資格情報の取得方法の詳細。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)のガイドを参照してください。

## 接続 [!DNL Oracle NetSuite Entities] を使用して Platform に [!DNL Flow Service] API

次に、の認証に必要な手順を示します [!DNL Oracle NetSuite Entities] ソース、ソース接続の作成、データフローの作成を行って、顧客および連絡先データをExperience Platformに取り込みます。

### ベース接続の作成 {#base-connection}

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Oracle NetSuite Entities] 認証資格情報をリクエスト本文の一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Oracle NetSuite Entities] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Oracle NetSuite Entities base connection",
      "description": "Authenticated base connection for Oracle NetSuite Entities",
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
| `auth.params.accessToken` | アクセストークンの値は、 [ステップ 2](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158081952044.html#Step-Two-POST-Request-to-the-Token-Endpoint) の [OAuth 2.0 認証コード付与フロー](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158074210415.html#OAuth-2.0-Authorization-Code-Grant-Flow) チュートリアル アクセストークンの有効期限は 60 分間のみ有効です。 値は、次のような 1024 文字の文字列で、JSON Web トークン (JWT) 形式で書式設定されます。 `eyJr......f4V0`. |

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
| `{SOURCE_PARAMS}` | Platform に取り込むソースファイルのパラメーターを定義します。 の受け入れ可能な format-type を取得するには `{SOURCE_PARAMS}`の場合は、base64 で文字列全体をエンコードする必要があります。 <br> [!DNL Oracle NetSuite Entities] は、顧客データと連絡先データの両方の取得をサポートしています。 利用するオブジェクトのタイプに応じて、次のいずれかを渡します。 <ul><li>`customer` ：特定の顧客データ（顧客名、住所、主要識別子などの詳細を含む）を取得します。</li><li>`contact` ：顧客に関連付けられた連絡先名、E メール、電話番号、およびカスタムの連絡先関連フィールドを取得します。</li></ul> |

>[!BEGINTABS]

>[!TAB 顧客]

の場合 [!DNL Oracle NetSuite Entities]（の値を取得するため） `{SOURCE_PARAMS}` が `{"object_type":"customer"}`. base64 でエンコードされた場合、はと同じ値になります。 `eyAib2JqZWN0X3R5cGUiOiAiY3VzdG9tZXIifQ%3D%3D` 以下に示すように。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/db7a6f4b-3f5d-487c-9a87-83e84df5074c/explore?objectType=rest&object=json&fileType=json&preview=true&sourceParams=eyAib2JqZWN0X3R5cGUiOiAiY3VzdG9tZXIifQ%3D%3D' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

>[!TAB 連絡先]

の場合 [!DNL Oracle NetSuite Entities]（の値を取得するため） `{SOURCE_PARAMS}` が `{"object_type":"contact"}`. base64 でエンコードされた場合、はと同じ値になります。 `eyAib2JqZWN0X3R5cGUiOiAiY29udGFjdCJ9` 以下に示すように。


```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/db7a6f4b-3f5d-487c-9a87-83e84df5074c/explore?objectType=rest&object=json&fileType=json&preview=true&sourceParams=eyAib2JqZWN0X3R5cGUiOiAiY29udGFjdCJ9' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

>[!ENDTABS]

**応答**

同様に、受信した応答を活用するオブジェクトのタイプに応じて、次のようになります。

>[!NOTE]
>
>一部のレコードが切り捨てられ、プレゼンテーションの質が向上しました。

>[!BEGINTABS]

>[!TAB 顧客]

正常な応答は、次のような構造を返します。

+++「 」を選択して JSON ペイロードを表示します。

```json
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
                    "isinactive": {
                        "type": "string"
                    },
                    "weblead": {
                        "type": "string"
                    },
                    "emailtransactions": {
                        "type": "string"
                    },
                    "entityid": {
                        "type": "string"
                    },
                    "dateclosed": {
                        "type": "string"
                    },
                    "entitynumber": {
                        "type": "string"
                    },
                    "emailpreference": {
                        "type": "string"
                    },
                    "creditholdoverride": {
                        "type": "string"
                    },
                    "entitystatus": {
                        "type": "string"
                    },
                    "giveaccess": {
                        "type": "string"
                    },
                    "isbudgetapproved": {
                        "type": "string"
                    },
                    "receivablesaccount": {
                        "type": "string"
                    },
                    "shippingcarrier": {
                        "type": "string"
                    },
                    "isperson": {
                        "type": "string"
                    },
                    "balancesearch": {
                        "type": "string"
                    },
                    "links": {
                        "type": "array",
                        "items": {
                            "type": "object",
                            "properties": {}
                        }
                    },
                    "currency": {
                        "type": "string"
                    },
                    "overduebalancesearch": {
                        "type": "string"
                    },
                    "id": {
                        "type": "string"
                    },
                    "entitytitle": {
                        "type": "string"
                    },
                    "email": {
                        "type": "string"
                    },
                    "displaysymbol": {
                        "type": "string"
                    },
                    "searchstage": {
                        "type": "string"
                    },
                    "lastmodifieddate": {
                        "type": "string"
                    },
                    "oncredithold": {
                        "type": "string"
                    },
                    "probability": {
                        "type": "string"
                    },
                    "faxtransactions": {
                        "type": "string"
                    },
                    "altname": {
                        "type": "string"
                    },
                    "datecreated": {
                        "type": "string"
                    },
                    "printtransactions": {
                        "type": "string"
                    },
                    "alcoholrecipienttype": {
                        "type": "string"
                    },
                    "overridecurrencyformat": {
                        "type": "string"
                    },
                    "companyname": {
                        "type": "string"
                    },
                    "unbilledorderssearch": {
                        "type": "string"
                    },
                    "shipcomplete": {
                        "type": "string"
                    },
                    "symbolplacement": {
                        "type": "string"
                    }
                }
            }
        }
    },
    "data": [
        {
            "totalResults": 10,
            "offset": 0,
            "count": 10,
            "hasMore": false,
            "links": [
                {
                    "rel": "self",
                    "href": "https://{ACCOUNT_ID}.suitetalk.api.netsuite.com/services/rest/query/v1/suiteql"
                }
            ],
            "items": {
                "alcoholrecipienttype": "CONSUMER",
                "altname": "Company 1615554322",
                "balancesearch": "0",
                "companyname": "Company 1615554322",
                "creditholdoverride": "AUTO",
                "currency": "1",
                "dateclosed": "2022-12-15",
                "datecreated": "2022-12-15",
                "displaysymbol": "£",
                "email": "customer3@example.com",
                "emailpreference": "DEFAULT",
                "emailtransactions": "F",
                "entityid": "4",
                "entitynumber": "4",
                "entitystatus": "13",
                "entitytitle": "4 Company 1615554322",
                "faxtransactions": "F",
                "giveaccess": "F",
                "id": "404",
                "isbudgetapproved": "F",
                "isinactive": "F",
                "isperson": "F",
                "lastmodifieddate": "2022-12-15",
                "oncredithold": "F",
                "overduebalancesearch": "0",
                "overridecurrencyformat": "F",
                "printtransactions": "F",
                "probability": "1",
                "receivablesaccount": "-10",
                "searchstage": "Customer",
                "shipcomplete": "F",
                "shippingcarrier": "nonups",
                "symbolplacement": "1",
                "unbilledorderssearch": "0",
                "weblead": "F"
            }
        },
        {
            "totalResults": 10,
            "offset": 0,
            "count": 10,
            "hasMore": false,
            "links": [
                {
                    "rel": "self",
                    "href": "https://{ACCOUNT_ID}.suitetalk.api.netsuite.com/services/rest/query/v1/suiteql"
                }
            ],
            "items": {
                "alcoholrecipienttype": "CONSUMER",
                "altname": "Company 1615554322",
                "balancesearch": "0",
                "companyname": "Company 1615554322",
                "creditholdoverride": "AUTO",
                "currency": "1",
                "dateclosed": "2023-01-19",
                "datecreated": "2023-01-19",
                "displaysymbol": "£",
                "email": "customer3@example.com",
                "emailpreference": "DEFAULT",
                "emailtransactions": "F",
                "entityid": "6",
                "entitynumber": "6",
                "entitystatus": "13",
                "entitytitle": "6 Company 1615554322",
                "faxtransactions": "F",
                "giveaccess": "F",
                "id": "605",
                "isbudgetapproved": "F",
                "isinactive": "F",
                "isperson": "F",
                "lastmodifieddate": "2023-01-19",
                "oncredithold": "F",
                "overduebalancesearch": "0",
                "overridecurrencyformat": "F",
                "printtransactions": "F",
                "probability": "1",
                "receivablesaccount": "-10",
                "searchstage": "Customer",
                "shipcomplete": "F",
                "shippingcarrier": "nonups",
                "symbolplacement": "1",
                "unbilledorderssearch": "0",
                "weblead": "F"
            }
        },
        {
            "totalResults": 10,
            "offset": 0,
            "count": 10,
            "hasMore": false,
            "links": [
                {
                    "rel": "self",
                    "href": "https://{ACCOUNT_ID}.suitetalk.api.netsuite.com/services/rest/query/v1/suiteql"
                }
            ],
            "items": {
                "alcoholrecipienttype": "CONSUMER",
                "altname": "Company 1615554322",
                "balancesearch": "0",
                "companyname": "Company 1615554322",
                "creditholdoverride": "AUTO",
                "currency": "1",
                "dateclosed": "2023-02-01",
                "datecreated": "2023-02-01",
                "displaysymbol": "£",
                "email": "customer3@example.com",
                "emailpreference": "DEFAULT",
                "emailtransactions": "F",
                "entityid": "9",
                "entitynumber": "9",
                "entitystatus": "13",
                "entitytitle": "9 Company 1615554322",
                "faxtransactions": "F",
                "giveaccess": "F",
                "id": "710",
                "isbudgetapproved": "F",
                "isinactive": "F",
                "isperson": "F",
                "lastmodifieddate": "2023-02-01",
                "oncredithold": "F",
                "overduebalancesearch": "0",
                "overridecurrencyformat": "F",
                "printtransactions": "F",
                "probability": "1",
                "receivablesaccount": "-10",
                "searchstage": "Customer",
                "shipcomplete": "F",
                "shippingcarrier": "nonups",
                "symbolplacement": "1",
                "unbilledorderssearch": "0",
                "weblead": "F"
            }
        },
    ]
},
```

+++

>[!TAB 連絡先]

正常な応答は、次のような構造を返します。

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
                    "isinactive": {
                        "type": "string"
                    },
                    "isprivate": {
                        "type": "string"
                    },
                    "phone": {
                        "type": "string"
                    },
                    "lastmodifieddate": {
                        "type": "string"
                    },
                    "links": {
                        "type": "array",
                        "items": {
                            "type": "object",
                            "properties": {}
                        }
                    },
                    "company": {
                        "type": "string"
                    },
                    "entityid": {
                        "type": "string"
                    },
                    "datecreated": {
                        "type": "string"
                    },
                    "id": {
                        "type": "string"
                    },
                    "entitytitle": {
                        "type": "string"
                    }
                }
            }
        }
    },
    "data": [
        {
            "totalResults": 10,
            "offset": 0,
            "count": 10,
            "hasMore": false,
            "links": [
                {
                    "rel": "self",
                    "href": "https://{ACCOUNT_ID}.suitetalk.api.netsuite.com/services/rest/query/v1/suiteql"
                }
            ],
            "items": {
                "company": "504",
                "datecreated": "2023-09-06",
                "entityid": "John Doe2",
                "entitytitle": "John Doe2",
                "id": "1210",
                "isinactive": "F",
                "isprivate": "F",
                "lastmodifieddate": "2023-09-06",
                "phone": "+9199999999998"
            }
        },
        {
            "totalResults": 10,
            "offset": 0,
            "count": 10,
            "hasMore": false,
            "links": [
                {
                    "rel": "self",
                    "href": "https://{ACCOUNT_ID}.suitetalk.api.netsuite.com/services/rest/query/v1/suiteql"
                }
            ],
            "items": {
                "company": "504",
                "datecreated": "2023-09-06",
                "entityid": "John Doe4",
                "entitytitle": "John Doe4",
                "id": "1212",
                "isinactive": "F",
                "isprivate": "F",
                "lastmodifieddate": "2023-09-06",
                "phone": "+9199999999996"
            }
        },
        {
            "totalResults": 10,
            "offset": 0,
            "count": 10,
            "hasMore": false,
            "links": [
                {
                    "rel": "self",
                    "href": "https://{ACCOUNT_ID}.suitetalk.api.netsuite.com/services/rest/query/v1/suiteql"
                }
            ],
            "items": {
                "company": "504",
                "datecreated": "2023-09-06",
                "entityid": "John Doe6",
                "entitytitle": "John Doe6",
                "id": "1214",
                "isinactive": "F",
                "isprivate": "F",
                "lastmodifieddate": "2023-09-06",
                "phone": "+9199999999994"
            }
        },
    ]
}
```

+++

>[!ENDTABS]

### ソース接続の作成 {#source-connection}

ソース接続を作成するには、 `/sourceConnections` エンドポイント [!DNL Flow Service] API. ソース接続は、接続 ID、ソースデータファイルへのパス、接続仕様 ID から構成されます。

**API 形式**

```https
POST /sourceConnections
```

**リクエスト**

次のリクエストは、[!DNL Oracle NetSuite Entities] のソース接続を作成します。

>[!BEGINTABS]

>[!TAB 顧客]

顧客データを取得する際に、 `object_type` プロパティの値は、次の値である必要があります `customer`.

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Oracle NetSuite Entities Source Connection",
      "description": "Oracle NetSuite Entities Source Connection",
      "baseConnectionId": "60c81023-99b4-4aae-9c31-472397576dd2",
      "connectionSpec": {
          "id": "fdf850b4-5a8d-4a5a-9ce8-4caef9abb2a8",
          "version": "1.0"
      },
      "data": {
          "format": "json"
      },
      "params": {
            "object_type": "customer"        
      }
  }'
```

>[!TAB 連絡先]

連絡先データの取得時に、 `object_type` プロパティの値は、次の値である必要があります `contact`.

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Oracle NetSuite Entities Source Connection",
      "description": "Oracle NetSuite Entities Source Connection",
      "baseConnectionId": "60c81023-99b4-4aae-9c31-472397576dd2",
      "connectionSpec": {
          "id": "fdf850b4-5a8d-4a5a-9ce8-4caef9abb2a8",
          "version": "1.0"
      },
      "data": {
          "format": "json"
      },
      "params": {
            "object_type": "contact"        
      }
  }'
```

>[!ENDTABS]

| プロパティ | 説明 |
| --- | --- |
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前はわかりやすいものにしてください。 |
| `description` | 含めることでソース接続に関する詳細情報を提供できるオプションの値です。 |
| `baseConnectionId` | [!DNL Oracle NetSuite Entities] のベース接続 ID。この ID は、前の手順で生成されました。 |
| `connectionSpec.id` | ソースに対応する接続仕様の ID。 |
| `data.format` | 取り込む [!DNL Oracle NetSuite Entities] データの形式。現在、サポートされているデータ形式は `json` のみです。 |
| `object_type` | [!DNL Oracle NetSuite Entities] は、顧客と連絡先の両方の取得をサポートします。 必要なエンティティに応じて、次のいずれかを渡します。 <ul><li>`customer` ：特定の顧客データ（顧客名、住所、主要識別子などの詳細を含む）を取得します。</li><li>`contact` ：顧客に関連付けられた連絡先名、E メール、電話番号、およびカスタムの連絡先関連フィールドを取得します。</li></ul> |

**応答**

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

次のリクエストは、[!DNL Oracle NetSuite Entities] のターゲット接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Oracle NetSuite Entities Target Connection Generic Rest",
      "description": " Oracle NetSuite Entities Connection Generic Rest",
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
| `data.format` | 取り込む [!DNL Oracle NetSuite Entities] データの形式。 |
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

次のリクエストは、 [!DNL DNL NetSuite Entities]

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
            "destination": "_extconndev.NS_ID"
        },
        {
            "sourceType": "ATTRIBUTE",
            "source": "items.entitytitle",
            "destination": "_extconndev.NS_entity_title"
        },
        {
            "sourceType": "ATTRIBUTE",
            "source": "items.datecreated",
            "destination": "_extconndev.NS_datecreated"
        },
        {
            "sourceType": "ATTRIBUTE",
            "destination": "_extconndev.NS_email",
            "source": "items.email"
        },
        {
            "sourceType": "ATTRIBUTE",
            "source": "items.lastmodifieddate",
            "destination": "_extconndev.NS_lastmodified"
        }
    ]
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

からデータを取り込むための最後の手順 [!DNL Oracle NetSuite Entities] を Platform に送信する場合、データフローを作成します。 現時点で、次の必要な値の準備ができています。

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
    "name": "Oracle NetSuite Entities connector Flow Generic Rest",
    "description": "Oracle NetSuite Entities connector Description Flow Generic Rest",
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
