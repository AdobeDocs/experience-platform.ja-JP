---
title: CSV テンプレートからスキーマ変換 API エンドポイントへ
description: スキーマレジストリ API の/rpc/csv2schema エンドポイントを使用すると、CSV テンプレートを使用して、エクスペリエンスデータモデル（XDM）スキーマを自動的に作成できます。
exl-id: cf08774a-db94-4ea1-a22e-bb06385f8d0e
source-git-commit: ba39f62cd77acedb7bfc0081dbb5f59906c9b287
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 6%

---

# CSV テンプレートからスキーマ変換 API エンドポイントへ

[!DNL Schema Registry] API の `/rpc/csv2schema` エンドポイントを使用すると、CSV ファイルをテンプレートとして使用して、エクスペリエンスデータモデル（XDM）スキーマを自動的に作成できます。 このエンドポイントを使用すると、テンプレートを作成してスキーマフィールドを一括読み込みし、手動の API や UI の作業を減らすことができます。

## はじめに

`/rpc/csv2schema` エンドポイントは、[[!DNL Schema Registry] API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の一部です。 先に進む前に、[ はじめる前に ](./getting-started.md) のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意のAdobe Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

`/rpc/csv2schema` エンドポイントは、[!DNL Schema Registry] でサポートされているリモート プロシージャ コール （RPC）の一部です。 [!DNL Schema Registry] API の他のエンドポイントとは異なり、RPC エンドポイントには `Accept` や `Content-Type` などの追加のヘッダーは必要なく、`CONTAINER_ID` も使用しません。 代わりに、以下の API 呼び出しで示すように、`/rpc` 名前空間を使用する必要があります。

## CSV ファイルの要件

このエンドポイントを利用するには、まず適切な列ヘッダーと対応する値を持つ CSV ファイルを作成する必要があります。 一部の列は必須ですが、残りはオプションです。 次の表に、これらの列と、スキーマ構築での列の役割を示します。

| CSV ヘッダーの位置 | CSV ヘッダー名 | 必須／オプション | 説明 |
| --- | --- | --- | --- |
| 1 | `isIgnored` | オプション | 含めて `true` に設定した場合、は、フィールドが API アップロードの準備ができていないので無視する必要があることを示します。 |
| 2 | `isCustom` | 必須 | フィールドがカスタムフィールドであるかどうかを示します。 |
| 3 | `fieldGroupId` | オプション | カスタムフィールドを関連付ける必要があるフィールドグループの ID。 |
| 4 | `fieldGroupName` | （説明を参照） | このフィールドを関連付けるフィールドグループの名前。<br><br> カスタムフィールドで既存の標準フィールドが拡張されない場合はオプション。 空白のままにすると、名前が自動的に割り当てられます。<br><br> 標準フィールドまたは標準フィールドグループを拡張するカスタムフィールドで必須。`fieldGroupId` のクエリに使用されます。 |
| 5 | `fieldPath` | 必須 | フィールドの完全な XED ドット表記パス。 標準フィールドグループのすべてのフィールド（`fieldGroupName` に示されている）を含めるには、値を `ALL` に設定します。 |
| 6 | `displayName` | オプション | フィールドのタイトル、またはわかりやすい表示名。 また、タイトルのエイリアスも指定できます（存在する場合）。 |
| 7 | `fieldDescription` | オプション | フィールドの説明。 また、説明が存在する場合は、その説明のエイリアスを指定することもできます。 |
| 8 | `dataType` | （説明を参照） | フィールドの [ 基本データタイプ ](../schema/field-constraints.md#basic-types) を示します。 すべてのカスタムフィールドに必須です。<br><br>`dataType` が `object` に設定されている場合、`properties` または `$ref` のいずれかを同じ行に対しても定義する必要がありますが、両方に定義する必要はありません。 |
| 9 | `isRequired` | オプション | データ取り込みにフィールドが必須かどうかを示します。 |
| 10 | `isArray` | オプション | フィールドが示された `dataType` の配列であるかどうかを示します。 |
| 11 | `isIdentity` | オプション | フィールドが ID フィールドであるかどうかを示します。 |
| 12 | `identityNamespace` | `isIdentity` が true の場合は必須 | ID フィールドの [ID 名前空間 ](../../identity-service/features/namespaces.md)。 |
| 13 | `isPrimaryIdentity` | オプション | フィールドがスキーマのプライマリ ID であるかどうかを示します。 |
| 14 | `minimum` | オプション | （数値フィールドのみ） フィールドの最小値。 |
| 15 | `maximum` | オプション | （数値フィールドのみ） フィールドの最大値。 |
| 16 | `enum` | オプション | フィールドの列挙値のリスト。配列で表します（例：`[value1,value2,value3]`）。 |
| 17 | `stringPattern` | オプション | （文字列フィールドのみ）データ取り込み中に検証に合格するために文字列値が一致する必要がある正規表現パターン。 |
| 18 | `format` | オプション | （文字列フィールドのみ）文字列フィールドの形式。 |
| 19 | `minLength` | オプション | （文字列フィールドのみ）文字列フィールドの最小長。 |
| 20 | `maxLength` | オプション | （文字列フィールドのみ）文字列フィールドの最大長。 |
| 21 | `properties` | （説明を参照） | `dataType` が `object` に設定され、`$ref` が定義されていない場合は必須です。 これは、オブジェクトの本文を JSON 文字列（例：`{"myField": {"type": "string"}}`）として定義します。 |
| 22 | `$ref` | （説明を参照） | `dataType` が `object` に設定され、`properties` が定義されていない場合は必須です。 これは、オブジェクトタイプ（例：`https://ns.adobe.com/xdm/context/person`）の参照オブジェクトの `$id` を定義します。 |
| 23 | `comment` | オプション | `isIgnored` を `true` に設定すると、この列を使用してスキーマのヘッダー情報を提供します。 |

{style="table-layout:auto"}

CSV ファイルの形式を決定するには、次の [CSV テンプレート ](../assets/sample-csv-template.csv) を参照してください。

## CSV ファイルからの書き出しペイロードの作成

CSV テンプレートを設定したら、ファイルを `/rpc/csv2schema` エンドポイントに送信し、書き出しペイロードに変換できます。

**API 形式**

```http
POST /rpc/csv2schema
```

**リクエスト**

リクエストペイロードでは、フォームデータを形式として使用する必要があります。 必須のフォームフィールドを以下に示します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/rpc/csv2schema \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -F 'csv-file=@"/Users/userName/Documents/sample-csv-template.csv"' \
  -F 'schema-class-id="https://ns.adobe.com/xdm/context/profile"' \
  -F 'schema-name="Example Schema"' \
  -F 'schema-description="Example schema description."'
```

| プロパティ | 説明 |
| --- | --- |
| `csv-file` | ローカルマシンに保存されている CSV テンプレートへのパス。 |
| `schema-class-id` | このスキーマが採用する XDM [ クラス ](../schema/composition.md#class) の `$id`。 |
| `schema-name` | スキーマの表示名。 |
| `schema-description` | スキーマの説明。 |

**応答**

応答が成功すると、CSV ファイルから生成された書き出しペイロードが返されます。 ペイロードは配列形式を取り、各配列項目はスキーマの依存 XDM コンポーネントを表すオブジェクトです。 以下の節を選択して、CSV ファイルから生成された書き出しペイロードの完全な例を表示します。

+++ 応答ペイロードの例

```json
[
    {
        "$id": "https://ns.adobe.com/ddgxdmint/mixins/68397e9293a6904b3006311fb46c9573a8aaad49780dd65a",
        "$schema": "http://json-schema.org/draft-06/schema#",
        "type": "object",
        "meta:xdmType": "object",
        "meta:resourceType": "mixins",
        "title": "Generated field group 68397e9293a6904b3006311fb46c9573a8aaad49780dd65a",
        "description": "Generated field group 68397e9293a6904b3006311fb46c9573a8aaad49780dd65a",
        "meta:intendedToExtend": [
            "https://ns.adobe.com/xdm/context/profile"
        ],
        "required": [
            "_ddgxdmint"
        ],
        "properties": {
            "_ddgxdmint": {
                "properties": {
                    "customField1": {
                        "type": "string",
                        "title": "my custom field1",
                        "description": "Sample custom field1"
                    },
                    "customField2": {
                        "properties": {
                            "customerField22": {
                                "type": "string",
                                "title": "my custom field22",
                                "description": "Sample custom field22"
                            }
                        },
                        "type": "object",
                        "required": [
                            "customerField22"
                        ]
                    },
                    "customField3": {
                        "type": "number",
                        "title": "my custom field3",
                        "description": "Sample custom field3",
                        "minimum": 0,
                        "maximum": 10
                    },
                    "customField4": {
                        "type": "string",
                        "title": "my custom field4",
                        "description": "Sample custom field4",
                        "enum": [
                            "one",
                            "two",
                            "three"
                        ]
                    },
                    "customField5": {
                        "type": "array",
                        "title": "my custom field5",
                        "description": "Sample custom field5",
                        "items": {
                            "type": "string"
                        }
                    },
                    "customField6": {
                        "type": "string",
                        "title": "my custom field6",
                        "description": "Sample custom field6",
                        "format": "date"
                    }
                },
                "type": "object",
                "required": [
                    "customField1",
                    "customField2"
                ]
            }
        }
    },
    {
        "$id": "https://ns.adobe.com/ddgxdmint/mixins/7d4edf1a4c2b8c97d31f9931a10618e0b7341cefc97edad6",
        "$schema": "http://json-schema.org/draft-06/schema#",
        "type": "object",
        "meta:xdmType": "object",
        "meta:resourceType": "mixins",
        "title": "SampleFieldGroup",
        "description": "Generated field group 7d4edf1a4c2b8c97d31f9931a10618e0b7341cefc97edad6",
        "meta:intendedToExtend": [
            "https://ns.adobe.com/xdm/context/profile"
        ],
        "properties": {
            "_ddgxdmint": {
                "properties": {
                    "customField7": {
                        "type": "object",
                        "title": "my custom field7",
                        "description": "Sample custom field7",
                        "properties": {
                            "myField": {
                                "type": "string"
                            }
                        }
                    },
                    "customField8": {
                        "type": "array",
                        "title": "my custom field8",
                        "description": "Sample custom field8",
                        "items": {
                            "type": "object",
                            "$ref": "https://ns.adobe.com/xdm/context/person"
                        }
                    }
                },
                "type": "object"
            }
        }
    },
    {
        "$id": "https://ns.adobe.com/ddgxdmint/mixins/6420020c79930a4c3aa7c9fd03b218e0e85c9a7d9990ecf",
        "$schema": "http://json-schema.org/draft-06/schema#",
        "type": "object",
        "meta:xdmType": "object",
        "meta:resourceType": "mixins",
        "title": "Demographic Details:Generated field group 6420020c79930a4c3aa7c9fd03b218e0e85c9a7d9990ecf",
        "description": "Generated field group 6420020c79930a4c3aa7c9fd03b218e0e85c9a7d9990ecf",
        "meta:intendedToExtend": [
            "https://ns.adobe.com/xdm/context/profile"
        ],
        "meta:extends": [
            "https://ns.adobe.com/xdm/context/profile-person-details"
        ],
        "allOf": [
            {
                "properties": {
                    "person": {
                        "properties": {
                            "name": {
                                "properties": {
                                    "_ddgxdmint": {
                                        "properties": {
                                            "nickName": {
                                                "type": "string"
                                            }
                                        },
                                        "type": "object",
                                        "required": [
                                            "nickName"
                                        ]
                                    }
                                },
                                "type": "object",
                                "required": [
                                    "_ddgxdmint"
                                ]
                            }
                        },
                        "type": "object",
                        "required": [
                            "name"
                        ]
                    }
                },
                "required": [
                    "person"
                ]
            },
            {
                "$ref": "https://ns.adobe.com/xdm/context/profile-person-details"
            }
        ]
    },
    {
        "$id": "https://ns.adobe.com/ddgxdmint/mixins/39e6bb291e5b9072878c5c80c0ff5a325df79385ed10d241",
        "$schema": "http://json-schema.org/draft-06/schema#",
        "type": "object",
        "meta:xdmType": "object",
        "meta:resourceType": "mixins",
        "title": "Loyalty Details:Generated field group 39e6bb291e5b9072878c5c80c0ff5a325df79385ed10d241",
        "description": "Generated field group 39e6bb291e5b9072878c5c80c0ff5a325df79385ed10d241",
        "meta:intendedToExtend": [
            "https://ns.adobe.com/xdm/context/profile"
        ],
        "meta:extends": [
            "https://ns.adobe.com/xdm/mixins/profile/profile-loyalty-details"
        ],
        "allOf": [
            {
                "properties": {
                    "loyalty": {
                        "properties": {
                            "_ddgxdmint": {
                                "properties": {
                                    "joinDate": {
                                        "type": "string"
                                    }
                                },
                                "type": "object"
                            }
                        },
                        "type": "object"
                    }
                }
            },
            {
                "$ref": "https://ns.adobe.com/xdm/mixins/profile/profile-loyalty-details"
            }
        ]
    },
    {
        "$id": "https://ns.adobe.com/ddgxdmint/schemas/632cb76723ce2fd3876385168c03eb5201c5997f3f367a2f",
        "$schema": "http://json-schema.org/draft-06/schema#",
        "type": "object",
        "meta:xdmType": "object",
        "meta:resourceType": "schemas",
        "title": "My Sample Schema",
        "description": "My Sample Schema",
        "meta:extends": [
            "https://ns.adobe.com/xdm/context/profile"
        ],
        "allOf": [
            {
                "$ref": "https://ns.adobe.com/xdm/context/profile"
            },
            {
                "$ref": "https://ns.adobe.com/ddgxdmint/mixins/68397e9293a6904b3006311fb46c9573a8aaad49780dd65a"
            },
            {
                "$ref": "https://ns.adobe.com/ddgxdmint/mixins/7d4edf1a4c2b8c97d31f9931a10618e0b7341cefc97edad6"
            },
            {
                "$ref": "https://ns.adobe.com/ddgxdmint/mixins/6420020c79930a4c3aa7c9fd03b218e0e85c9a7d9990ecf",
                "meta:refProperty": [
                    "/person/name/firstName",
                    "/person/name/lastName",
                    "/person/name/_ddgxdmint/nickName"
                ]
            },
            {
                "$ref": "https://ns.adobe.com/ddgxdmint/mixins/39e6bb291e5b9072878c5c80c0ff5a325df79385ed10d241"
            }
        ]
    },
    {
        "@type": "xdm:alternateDisplayInfo",
        "meta:resourceType": "descriptors",
        "xdm:sourceSchema": "https://ns.adobe.com/ddgxdmint/schemas/632cb76723ce2fd3876385168c03eb5201c5997f3f367a2f",
        "xdm:sourceVersion": 1,
        "xdm:sourceProperty": "/person/name/firstName",
        "xdm:title": {
            "en_us": "My first name"
        }
    },
    {
        "@type": "xdm:descriptorIdentity",
        "meta:resourceType": "descriptors",
        "xdm:sourceSchema": "https://ns.adobe.com/ddgxdmint/schemas/632cb76723ce2fd3876385168c03eb5201c5997f3f367a2f",
        "xdm:sourceVersion": 1,
        "xdm:sourceProperty": "/_ddgxdmint/customField1",
        "xdm:namespace": "email",
        "xdm:property": "xdm:code",
        "xdm:isPrimary": true
    }
]
```

+++

## スキーマペイロードの読み込み

CSV ファイルから書き出しペイロードを生成したら、そのペイロードを `/rpc/import` エンドポイントに送信して、スキーマを生成できます。

エクスポートペイロードからスキーマを生成する方法について詳しくは、[ インポートエンドポイントガイド ](./import.md) を参照してください。
