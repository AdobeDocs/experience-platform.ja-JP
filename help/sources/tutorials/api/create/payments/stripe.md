---
title: お客様からの支払いデータの取り込み [!DNL Stripe] API を使用したExperience Platformへのアカウント
description: フローサービス API を使用して、StripeアカウントからExperience Platformに支払データを取り込む方法を説明します
badge: ベータ版
source-git-commit: f8df3ddb96ad0810a7a46b0a55125336c427aebd
workflow-type: tm+mt
source-wordcount: '1998'
ht-degree: 45%

---

# お客様からの支払いデータの取り込み [!DNL Stripe] API を使用したExperience Platformへのアカウント

>[!NOTE]
>
>[!DNL Stripe] ソースはベータ版です。詳しくは、 [利用条件](../../../../home.md#terms-and-conditions) （ソースの概要）を参照して、ベータラベル付きのソースの使用に関する詳細を確認してください。

次のチュートリアルを読んで、から支払データを取り込む方法を学んでください。 [!DNL Stripe] を使用してAdobe Experience Platformに [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを個別の仮想環境に分割する仮想サンドボックスを提供し、デジタル体験アプリケーションの開発および進化を支援します。

### 認証

詳しくは、 [[!DNL Stripe] 概要](../../../../connectors/payments/stripe.md) 認証資格情報の取得方法の詳細。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)のガイドを参照してください。

## 接続 [!DNL Stripe] Experience Platform

以下のガイドに従って、 [!DNL Stripe] ソース、ソース接続を作成し、データフローを作成して支払いデータをExperience Platformに取り込みます。

### ベース接続の作成 {#base-connection}

ベース接続では、ソースとExperience Platform間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID など）が保持されます。 ベース接続 ID を使用して、ソース内からファイルを参照およびナビゲートできます。 さらに、取り込む特定の項目を識別できます。これには、これらの項目のデータ型や形式の詳細も含まれます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Stripe] 認証資格情報をリクエスト本文の一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Stripe] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Stripe base connection",
      "description": "Authenticated base connection for Stripe",
      "connectionSpec": {
          "id": "cc2c31d6-7b8c-4581-b49f-5c8698aa3ab3",
          "version": "1.0"
      },
      "auth": {
          "specName": "OAuth2 Refresh Code",
          "params": {
            "accessToken": "{ACCESS_TOKEN}",
          }
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ベース接続の名前。ベース接続の情報を検索する際に使用できるので、ベース接続の名前はわかりやすいものにしてください。 |
| `description` | ベース接続に関する詳細情報を提供するために含めることができるオプションの値です。 |
| `connectionSpec.id` | ソースの接続仕様 ID。 の接続仕様 ID [!DNL Stripe] 次に該当 `cc2c31d6-7b8c-4581-b49f-5c8698aa3ab3`に設定され、この ID は修正されました。 |
| `auth.specName` | ソースを認証するために使用する認証タイプをExperience Platformに対して設定します。 |
| `auth.params.accessToken` | のアクセストークン [!DNL Stripe] アカウント。 詳しくは、 [[!DNL Stripe] 認証ガイド](../../../../connectors/payments/stripe.md#prerequisites) を参照してください。 |

**応答**

リクエストが成功した場合は、一意の接続識別子（`id`）を含む、新しく作成されたベース接続が返されます。この ID は、次の手順でソースのファイル構造と内容を調べるために必要です。

```json
{
  "id": "a9950001-a386-4642-a0cd-5eaac6db5556",
  "etag": "\"dc01244d-0000-0200-0000-65ea4e500000\""
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
| `objectType=rest` | 参照するオブジェクトのタイプ。 この値は常にに設定されます。 `rest`. |
| `{OBJECT}` | このパラメーターは、特定のディレクトリを表示する場合にのみ必要です。 その値は、参照するディレクトリのパスを表します。 このソースの場合、値は次のようになります。 `json`. |
| `fileType=json` | Platform に取り込むファイルのファイルタイプ。 現在、 `json` は、サポートされている唯一のファイルタイプです。 |
| `{PREVIEW}` | 接続のコンテンツがプレビューをサポートするかどうかを定義するブール値です。 |
| `{SOURCE_PARAMS}` | A [!DNL Base64-]参照するリソースパスを指すエンコードされた文字列。 リソースパスは、でエンコードする必要があります。 [!DNL Base64] ～の承認済みフォーマットを得るために `{SOURCE_PARAMS}`. 例： `{"resourcePath":"charges"}` は次のようにエンコードされます： `eyJyZXNvdXJjZVBhdGgiOiJjaGFyZ2VzIn0%3D`. 使用可能なリソースパスのリストは次のとおりです。 <ul><li>`charges`</li><li>`subscriptions`</li><li>`refunds`</li><li>`balance_transactions`</li><li>`customers`</li><li>`prices`</li></ul> |

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/a9950001-a386-4642-a0cd-5eaac6db5556/explore?objectType=rest&object=json&fileType=json&preview=false&sourceParams=eyJyZXNvdXJjZVBhdGgiOiJjaGFyZ2VzIn0%3D' \
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
      "data": {
        "type": "object",
        "properties": {
          "balance_transaction": {},
          "billing_details": {
            "type": "object",
            "properties": {
              "address": {
                "type": "object",
                "properties": {
                  "country": {},
                  "city": {},
                  "state": {},
                  "postal_code": {},
                  "line2": {},
                  "line1": {}
                }
              },
              "phone": {},
              "name": {},
              "email": {}
            }
          },
          "metadata": {
            "type": "object",
            "properties": {}
          },
          "livemode": {
            "type": "boolean"
          },
          "radar_options": {
            "type": "object",
            "properties": {}
          },
          "destination": {},
          "description": {
            "type": "string"
          },
          "failure_message": {},
          "fraud_details": {
            "type": "object",
            "properties": {}
          },
          "source": {},
          "amount_refunded": {
            "type": "integer",
            "minimum": -9007199254740992,
            "maximum": 9007199254740991
          },
          "statement_descriptor": {
            "type": "string"
          },
          "transfer_data": {},
          "receipt_url": {
            "type": "string"
          },
          "shipping": {},
          "review": {},
          "captured": {
            "type": "boolean"
          },
          "calculated_statement_descriptor": {
            "type": "string"
          },
          "currency": {
            "type": "string"
          },
          "refunded": {
            "type": "boolean"
          },
          "id": {
            "type": "string"
          },
          "outcome": {
            "type": "object",
            "properties": {
              "reason": {},
              "risk_level": {
                "type": "string"
              },
              "risk_score": {
                "type": "integer",
                "minimum": -9007199254740992,
                "maximum": 9007199254740991
              },
              "seller_message": {
                "type": "string"
              },
              "network_status": {
                "type": "string"
              },
              "type": {
                "type": "string"
              }
            }
          },
          "payment_method": {
            "type": "string"
          },
          "order": {},
          "dispute": {},
          "amount": {
            "type": "integer",
            "minimum": -9007199254740992,
            "maximum": 9007199254740991
          },
          "disputed": {
            "type": "boolean"
          },
          "failure_code": {},
          "transfer_group": {},
          "on_behalf_of": {},
          "created": {
            "type": "integer",
            "minimum": -9007199254740992,
            "maximum": 9007199254740991
          },
          "payment_method_details": {
            "type": "object",
            "properties": {
              "type": {
                "type": "string"
              },
              "card": {
                "type": "object",
                "properties": {
                  "country": {
                    "type": "string"
                  },
                  "last4": {
                    "type": "string"
                  },
                  "funding": {
                    "type": "string"
                  },
                  "mandate": {},
                  "wallet": {},
                  "exp_month": {
                    "type": "integer",
                    "minimum": -9007199254740992,
                    "maximum": 9007199254740991
                  },
                  "exp_year": {
                    "type": "integer",
                    "minimum": -9007199254740992,
                    "maximum": 9007199254740991
                  },
                  "overcapture": {
                    "type": "object",
                    "properties": {
                      "maximum_amount_capturable": {
                        "type": "integer",
                        "minimum": -9007199254740992,
                        "maximum": 9007199254740991
                      },
                      "status": {
                        "type": "string"
                      }
                    }
                  },
                  "amount_authorized": {
                    "type": "integer",
                    "minimum": -9007199254740992,
                    "maximum": 9007199254740991
                  },
                  "network": {
                    "type": "string"
                  },
                  "network_token": {
                    "type": "object",
                    "properties": {
                      "used": {
                        "type": "boolean"
                      }
                    }
                  },
                  "incremental_authorization": {
                    "type": "object",
                    "properties": {
                      "status": {
                        "type": "string"
                      }
                    }
                  },
                  "checks": {
                    "type": "object",
                    "properties": {
                      "cvc_check": {
                        "type": "string"
                      },
                      "address_line1_check": {},
                      "address_postal_code_check": {}
                    }
                  },
                  "extended_authorization": {
                    "type": "object",
                    "properties": {
                      "status": {
                        "type": "string"
                      }
                    }
                  },
                  "installments": {},
                  "capture_before": {
                    "type": "integer",
                    "minimum": -9007199254740992,
                    "maximum": 9007199254740991
                  },
                  "fingerprint": {
                    "type": "string"
                  },
                  "three_d_secure": {},
                  "brand": {
                    "type": "string"
                  },
                  "multicapture": {
                    "type": "object",
                    "properties": {
                      "status": {
                        "type": "string"
                      }
                    }
                  }
                }
              }
            }
          },
          "amount_captured": {
            "type": "integer",
            "minimum": -9007199254740992,
            "maximum": 9007199254740991
          },
          "source_transfer": {},
          "failure_balance_transaction": {},
          "receipt_number": {},
          "application": {},
          "receipt_email": {},
          "paid": {
            "type": "boolean"
          },
          "application_fee": {},
          "payment_intent": {
            "type": "string"
          },
          "invoice": {},
          "statement_descriptor_suffix": {},
          "application_fee_amount": {},
          "object": {
            "type": "string"
          },
          "customer": {
            "type": "string"
          },
          "status": {
            "type": "string"
          }
        }
      }
    }
  }
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

次のリクエストは、 [!DNL Stripe].

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Stripe Source Connection For Charges Data",
      "description": "Stripe source connection for charges data",
      "baseConnectionId": "a9950001-a386-4642-a0cd-5eaac6db5556",
      "connectionSpec": {
        "id": "cc2c31d6-7b8c-4581-b49f-5c8698aa3ab3",
        "version": "1.0"
      },
      "data": {
        "format": "json"
      },
      "params": {
        "resourcePath": "charges"
      },
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前がわかりやすい名前になっていることを確認します。 |
| `description` | 含めることでソース接続に関する詳細情報を提供できるオプションの値です。 |
| `baseConnectionId` | [!DNL Stripe] のベース接続 ID。この ID は、前の手順で生成されました。 |
| `connectionSpec.id` | ソースに対応する接続仕様 ID。 |
| `data.format` | 取り込む [!DNL Stripe] データの形式。現在、サポートされているデータ形式は `json` のみです。 |

リクエストが成功した場合は、新たに作成されたソース接続の一意の ID（`id`）が返されます。この ID は、後の手順でデータフローを作成する際に必要になります。

```json
{
  "id": "abbfac4e-202c-4e04-902d-6f73e9041068",
  "etag": "\"0a033818-0000-0200-0000-65ea5a770000\""
}
```

### ターゲット XDM スキーマの作成 {#target-schema}

ソースデータをExperience Platformで使用するには、必要に応じてソースデータを構造化するために、ターゲットスキーマを作成する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれる Platform データセットを作成します。

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

次のリクエストは、[!DNL Stripe] のターゲット接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Stripe Target Connection For Charges Data",
      "description": "Stripe target connection for charges data",
      "connectionSpec": {
          "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
          "version": "1.0"
      },
      "data": {
          "format": "parquet_xdm",
          "schema": {
              "id": "https://ns.adobe.com/{ORG_ID}/schemas/5f76be8c4e4b847fdac13ca42aa6b596a89a5b91dea48b16",
              "version": "application/vnd.adobe.xed-full+json;version=1.3"
          }
      },
      "params": {
          "dataSetId": "65e622315f78042c9e8166e8"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | ターゲット接続の名前。ターゲット接続の情報を検索に使用できるように、ターゲット接続はわかりやすい名前にしてください。 |
| `description` | ターゲット接続に関する詳細を提供するために含めることができるオプションの値です。 |
| `connectionSpec.id` | データレイクに対応する接続仕様 ID。 この修正済み ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。 |
| `data.format` | 取り込む [!DNL Stripe] データの形式。 |
| `params.dataSetId` | ターゲットデータセットの ID。 この ID は [target データセットの作成](#target-dataset). |

**応答**

リクエストが成功した場合は、新しいターゲット接続の一意の ID（`id`）が返されます。この ID は、後の手順で必要になります。

```json
{
  "id": "69879751-ba43-48df-8cd0-39d2bb76a5b8",
  "etag": "\"4b02ef5b-0000-0200-0000-65ea5f730000\""
}
```

### マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。これを実現するには、リクエストペイロード内で定義されたデータマッピングを使用して、[[!DNL Data Prep]  API](https://www.adobe.io/experience-platform-apis/references/data-prep/) に対して POST リクエストを実行します。

**API 形式**

```http
POST /conversion/mappingSets
```

次のリクエストは、 [!DNL Stripe].

+++選択してリクエストの例を表示

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/conversion/mappingSets' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "version": 0,
      "xdmSchema": "https://ns.adobe.com/exchangesandboxcharlie/schemas/5f76be8c4e4b847fdac13ca42aa6b596a89a5b91dea48b16",
      "xdmVersion": "1.0",
      },
      "mappings":[
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.id",
            "sourceAttribute":"data.id",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.refunded",
            "sourceAttribute":"data.refunded",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.disputed",
            "sourceAttribute":"data.disputed",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.last4",
            "sourceAttribute":"data.payment_method_details.card.last4",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.livemode",
            "sourceAttribute":"data.livemode",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.status",
            "sourceAttribute":"data.status",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.overcapture.maximum_amount_capturable",
            "sourceAttribute":"data.payment_method_details.card.overcapture.maximum_amount_capturable",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.receipt_url",
            "sourceAttribute":"data.receipt_url",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.fingerprint",
            "sourceAttribute":"data.payment_method_details.card.fingerprint",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_intent",
            "sourceAttribute":"data.payment_intent",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.overcapture.status",
            "sourceAttribute":"data.payment_method_details.card.overcapture.status",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.network_token.used",
            "sourceAttribute":"data.payment_method_details.card.network_token.used",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.funding",
            "sourceAttribute":"data.payment_method_details.card.funding",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.amount",
            "sourceAttribute":"data.amount",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.customer",
            "sourceAttribute":"data.customer",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.incremental_authorization.status",
            "sourceAttribute":"data.payment_method_details.card.incremental_authorization.status",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.multicapture.status",
            "sourceAttribute":"data.payment_method_details.card.multicapture.status",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.amount_captured",
            "sourceAttribute":"data.amount_captured",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method",
            "sourceAttribute":"data.payment_method",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.object",
            "sourceAttribute":"data.object",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.captured",
            "sourceAttribute":"data.captured",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.created",
            "sourceAttribute":"data.created",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.paid",
            "sourceAttribute":"data.paid",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.amount_refunded",
            "sourceAttribute":"data.amount_refunded",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.currency",
            "sourceAttribute":"data.currency",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.country",
            "sourceAttribute":"data.payment_method_details.card.country",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.exp_year",
            "sourceAttribute":"data.payment_method_details.card.exp_year",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.amount_authorized",
            "sourceAttribute":"data.payment_method_details.card.amount_authorized",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.network",
            "sourceAttribute":"data.payment_method_details.card.network",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details",
            "sourceAttribute":"data.payment_method_details",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.exp_month",
            "sourceAttribute":"data.payment_method_details.card.exp_month",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.calculated_statement_descriptor",
            "sourceAttribute":"data.calculated_statement_descriptor",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.brand",
            "sourceAttribute":"data.payment_method_details.card.brand",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.balance_transaction",
            "sourceAttribute":"data.balance_transaction",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.payment_method_details.card.extended_authorization.status",
            "sourceAttribute":"data.payment_method_details.card.extended_authorization.status",
            "identity":false,
            "version":0
        },
        {
            "destinationXdmPath":"_{ORG_ID}.charges_data.outcome",
            "sourceAttribute":"data.outcome",
            "identity":false,
            "version":0
        }
      ]
}
```


| プロパティ | 説明 |
| --- | --- |
| `xdmSchema` | ターゲット XDM スキーマの ID。 この ID は、 [target XDM スキーマ](#target-schema). |
| `destinationXdmPath` | ソース属性のマッピング先の XDM フィールド。 |
| `sourceAttribute` | マッピングするソースデータフィールド。 |
| `identity` | フィールドをで保持するかどうかを定義する boolean 値です [ID サービス](../../../../../identity-service/home.md). |
| `version` | 使用しているマッピングバージョン。 |

+++

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成されたマッピングの詳細が返されます。この値は、後の手順でデータフローを作成する際に必要になります。

```json
{
  "id": "f4aad280fdec4770b7e33066945919d8",
  "version": 0,
  "createdDate": 1709860257007,
  "modifiedDate": 1709860257007,
  "createdBy": "{CREATED_BY}",
  "modifiedBy": "{MODIFIED_BY}"
}
```

### フローの作成 {#flow}

からデータを取り込むための最後の手順 [!DNL Stripe] を Platform に送信する場合、データフローを作成します。 現時点で、次の必要な値の準備ができています。

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
    "name": "Stripe Connector Flow Generic Rest",
    "description": "Stripe Connector Description Flow Generic Rest",
    "flowSpec": {
        "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
        "version": "1.0"
    },
    "sourceConnectionIds": [
        "abbfac4e-202c-4e04-902d-6f73e9041068"
    ],
    "targetConnectionIds": [
        "69879751-ba43-48df-8cd0-39d2bb76a5b8"
    ],
    "transformations": [
        {
            "name": "Mapping",
            "params": {
                "mappingId": "f4aad280fdec4770b7e33066945919d8",
                "mappingVersion": 0
            }
        }
    ],
    "scheduleParams": {
        "startTime": "1710267858",
        "frequency": "minute",
        "interval": {{interval}}
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
| `transformations` | このプロパティには、データに適用する必要がある様々な変換が含まれています。このプロパティは、XDM に準拠していないデータをExperience Platformに取り込む場合に必要です。 |
| `transformations.name` | 変換に割り当てられた名前。 |
| `transformations.params.mappingId` | 以前の手順で生成された[マッピング ID](#mapping)。 |
| `transformations.params.mappingVersion` | マッピング ID の対応するバージョン。この値のデフォルトは `0` です。 |
| `scheduleParams.startTime` | データフローが開始される時刻。 開始時刻の値は、Unix タイムスタンプの形式で指定する必要があります。 |
| `scheduleParams.frequency` | データフローがデータを収集する頻度。取り込み頻度を設定して、次の操作を実行できます。  <ul><li>**1 回**：頻度をに設定します。 `once` :1 回限りの取り込みを作成します。 1 回限りの取り込みデータフローを作成する場合、間隔とバックフィルの設定は使用できません。 デフォルトでは、スケジュールの頻度は 1 回に設定されています。</li><li>**分**：頻度をに設定します。 `minute` を使用して、1 分ごとにデータを取り込むようにデータフローをスケジュールします。</li><li>**時間**：頻度をに設定します。 `hour` を使用して、1 時間ごとにデータを取り込むようにデータフローをスケジュールします。</li><li>**日**：頻度をに設定します。 `day` を使用して、データを日単位で取り込むようにデータフローをスケジュールします。</li><li>**週**：頻度をに設定します。 `week` を使用して、週単位でデータを取り込むようにデータフローをスケジュールします。</li></ul> |
| `scheduleParams.interval` | インターバルは 2 つの連続したフロー実行の間隔を指定します。例えば、頻度を「day」に設定し、間隔を 15 に設定した場合、データフローは 15 日ごとに実行されます。 間隔の値は、ゼロ以外の整数にする必要があります。 |

**応答**

正常な応答は、新しく作成したデータフローの ID（`id`）を返します。この ID を使用して、データフローを監視、更新または削除できます。

```json
{
     "id": "84c64142-1741-4b0b-95a9-65644eba0cf6",
     "etag": "\"3901770b-0000-0200-0000-655708970000\""
}
```

## 付録

次の節では、データフローを監視、更新および削除するための手順について説明します。

### データフローの監視

データフローが作成されると、それを通して取り込まれるデータを監視し、フローの実行状況、完了状況、エラーなどの情報を確認することができます。API の完全な例については、 [API を使用したソースデータフローの監視](../../monitor.md).

### データフローの更新

の/flows エンドポイントに対してPATCHリクエストを実行することで、データフローの名前や説明、実行スケジュールおよび関連するマッピングセットなど、データフローの詳細を更新します。 [!DNL Flow Service] データフローの ID を指定する際の API。 PATCHリクエストをおこなう場合、データフローの一意の `etag` （内） `If-Match` ヘッダー。 API の完全な例については、 [API を使用したソースデータフローの更新](../../update-dataflows.md).

### アカウントを更新

に対してPATCHリクエストを実行して、ソースアカウントの名前、説明および資格情報を更新します。 [!DNL Flow Service] ベース接続 ID をクエリパラメーターとして指定する際の API。 PATCHリクエストをおこなう場合、ソースアカウントの一意の `etag` （内） `If-Match` ヘッダー。 API の完全な例については、 [API を使用したソースアカウントの更新](../../update.md).

### データフローの削除

に対してDELETEリクエストを実行して、データフローを削除する [!DNL Flow Service] クエリパラメーターの一部として削除するデータフローの ID を指定する際の API。 API の完全な例については、 [API を使用したデータフローの削除](../../delete-dataflows.md).

### アカウントを削除

アカウントを削除するには、 [!DNL Flow Service] 削除するアカウントのベース接続 ID を指定する際の API。 API の完全な例については、 [API を使用したソースアカウントの削除](../../delete.md).

