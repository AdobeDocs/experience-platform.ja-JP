---
title: CSV テンプレートからスキーマ変換 API エンドポイント
description: スキーマレジストリ API の/rpc/csv2schema エンドポイントを使用すると、CSV テンプレートを使用して Experience Data Model(XDM) スキーマを自動的に作成できます。
exl-id: cf08774a-db94-4ea1-a22e-bb06385f8d0e
source-git-commit: b4c186c8c40d1372fb5011f49979523e1201fb0b
workflow-type: tm+mt
source-wordcount: '857'
ht-degree: 7%

---

# CSV テンプレートからスキーマ変換 API エンドポイント

この `/rpc/csv2schema` エンドポイント [!DNL Schema Registry] API を使用すると、CSV ファイルをテンプレートとして使用して、Experience Data Model(XDM) スキーマを自動的に作成できます。 このエンドポイントを使用して、テンプレートを作成し、スキーマフィールドを一括で読み込み、API や UI の手動動作を削減できます。

## はじめに

この `/rpc/csv2schema` エンドポイントが [[!DNL Schema Registry] API](https://www.adobe.io/experience-platform-apis/references/schema-registry/). 続行する前に、 [入門ガイド](./getting-started.md) 関連ドキュメントへのリンク、このドキュメントの API 呼び出し例の読み方のガイド、および任意のAdobe Experience Platform API を正しく呼び出すために必要な必須ヘッダーに関する重要な情報。

この `/rpc/csv2schema` endpoint は、 [!DNL Schema Registry]. の他のエンドポイントとは異なり、 [!DNL Schema Registry] API、RPC エンドポイントは、 `Accept` または `Content-Type`、およびを使用しない `CONTAINER_ID`. 代わりに、 `/rpc` 名前空間と呼ばれます。

## CSV ファイルの要件

このエンドポイントを利用するには、まず適切な列ヘッダーと対応する値を含む CSV ファイルを作成する必要があります。 一部の列は必須ですが、残りの列はオプションです。 次の表に、これらの列とスキーマ構築での役割を示します。

| CSV ヘッダーの位置 | CSV ヘッダー名 | 必須／オプション | 説明 |
| --- | --- | --- | --- |
| 1 | `isIgnored` | オプション | 含まれる場合、およびに設定される場合 `true`は、フィールドが API アップロードの準備ができていないことを示します。このフィールドを無視する必要があります。 |
| 2 | `isCustom` | 必須 | フィールドがカスタムフィールドかどうかを示します。 |
| 3 | `fieldGroupId` | オプション | カスタムフィールドを関連付ける必要があるフィールドグループの ID。 |
| 4 | `fieldGroupName` | （説明を参照） | このフィールドを関連付けるフィールドグループの名前。<br><br>既存の標準フィールドを拡張しないカスタムフィールドの場合は、オプションです。 空白の場合は、名前が自動的に割り当てられます。<br><br>標準フィールドグループを拡張する標準フィールドまたはカスタムフィールドに必須です。標準フィールドグループは、 `fieldGroupId`. |
| 5 | `fieldPath` | 必須 | フィールドの完全な XED ドット表記のパス。 標準フィールドグループのすべてのフィールドを含めるには ( `fieldGroupName`)、値を `ALL`. |
| 6 | `displayName` | オプション | フィールドのタイトルまたはわかりやすい表示名。 存在する場合は、タイトルのエイリアスを指定することもできます。 |
| 7 | `fieldDescription` | オプション | フィールドの説明。 説明のエイリアス（存在する場合）を指定することもできます。 |
| 8 | `dataType` | （説明を参照） | を示します。 [基本データタイプ](../schema/field-constraints.md#basic-types) フィールドの すべてのカスタムフィールドに必須です。<br><br>If `dataType` が `object`、 `properties` または `$ref` 同じ行に対しても定義する必要がありますが、両方は定義できません。 |
| 9 | `isRequired` | オプション | データの取り込みにフィールドが必須かどうかを示します。 |
| 10 | `isArray` | オプション | フィールドが、示されたの配列かどうかを示します `dataType`. |
| 11 | `isIdentity` | オプション | フィールドが ID フィールドかどうかを示します。 |
| 12 | `identityNamespace` | 必須 `isIdentity` が true | この [id 名前空間](../../identity-service/namespaces.md) id フィールド用。 |
| 13 | `isPrimaryIdentity` | オプション | フィールドがスキーマのプライマリ ID であるかどうかを示します。 |
| 14 | `minimum` | オプション | （数値フィールドのみ）フィールドの最小値。 |
| 15 | `maximum` | オプション | （数値フィールドのみ）フィールドの最大値。 |
| 16 | `enum` | オプション | フィールドの列挙値のリスト。配列 ( 例： `[value1,value2,value3]`) をクリックします。 |
| 17 | `stringPattern` | オプション | （文字列フィールドのみ）データ取得時の検証に合格するために文字列値が一致する必要がある正規表現パターン。 |
| 18 | `format` | オプション | （文字列フィールドのみ）文字列フィールドの形式。 |
| 19 | `minLength` | オプション | （文字列フィールドのみ）文字列フィールドの最小の長さです。 |
| 20 | `maxLength` | オプション | （文字列フィールドのみ）文字列フィールドの最大長。 |
| 21 | `properties` | （説明を参照） | 必須 `dataType` が `object` および `$ref` が定義されていません。 これにより、オブジェクトの本文が JSON 文字列 ( 例： `{"myField": {"type": "string"}}`) をクリックします。 |
| 22 | `$ref` | （説明を参照） | 必須 `dataType` が `object` および `properties` が定義されていません。 これにより、 `$id` オブジェクトタイプの参照オブジェクトの ( 例： `https://ns.adobe.com/xdm/context/person`) をクリックします。 |
| 23 | `comment` | オプション | 条件 `isIgnored` が `true`の場合、この列は、スキーマのヘッダー情報を提供するために使用されます。 |

{style=&quot;table-layout:auto&quot;}

次を参照してください。 [CSV テンプレート](../assets/sample-csv-template.csv) をクリックして、CSV ファイルのフォーマット方法を決定します。

## CSV ファイルからの書き出しペイロードの作成

CSV テンプレートを設定したら、ファイルを `/rpc/csv2schema` エンドポイントを作成し、書き出しペイロードに変換します。

**API 形式**

```http
POST /rpc/csv2schema
```

**リクエスト**

リクエストペイロードでは、フォームデータをその形式として使用する必要があります。 必須フォームフィールドを以下に示します。

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
| `schema-class-id` | この `$id` XDM の [クラス](../schema/composition.md#class) このスキーマが使用する |
| `schema-name` | スキーマの表示名。 |
| `schema-description` | スキーマの説明。 |

**応答**

正常な応答は、CSV ファイルから生成された書き出しペイロードを返します。 ペイロードは配列の形式を取り、各配列項目は、スキーマの依存する XDM コンポーネントを表すオブジェクトです。 以下のセクションを選択して、CSV ファイルから生成された書き出しペイロードの完全な例を表示します。

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

## スキーマペイロードをインポート

CSV ファイルから書き出しペイロードを生成したら、そのペイロードを `/rpc/import` endpoint ：スキーマを生成します。

詳しくは、 [インポートエンドポイントガイド](./import.md) を参照してください。
