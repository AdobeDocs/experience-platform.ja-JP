---
keywords: Experience Platform;プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API
title: 計算済み属性フィールドの構成方法
topic-legacy: guide
type: Documentation
description: 計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。 計算済み属性を設定するには、まず、計算済み属性値を保持するフィールドを特定する必要があります。このフィールドは、スキーマレジストリAPIを使用して、計算済み属性フィールドを保持するスキーマとカスタムフィールドグループを定義するために作成できます。
exl-id: 91c5d125-8ab5-4291-a974-48dd44c68a13
translation-type: tm+mt
source-git-commit: 3985ba8f46a62e8d9ea8b1f084198b245318a24f
workflow-type: tm+mt
source-wordcount: '736'
ht-degree: 14%

---

# （アルファ）スキーマレジストリAPIを使用して、計算済み属性フィールドを設定する

>[!IMPORTANT]
>
>計算済み属性機能は現在アルファベットで表示されており、すべてのユーザーが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

計算済み属性を設定するには、まず、計算済み属性値を保持するフィールドを特定する必要があります。このフィールドは、スキーマレジストリAPIを使用して、スキーマと、計算済み属性フィールドを保持するカスタムスキーマフィールドグループを定義するために作成できます。 「計算済み属性」スキーマとフィールドグループを個別に作成し、計算済み属性として使用する属性を組織で追加できるようにすることをお勧めします。 これにより、計算された属性スキーマを、データ取り込みに使用されている他のスキーマから完全に分離できます。

このドキュメントのワークフローでは、スキーマレジストリAPIを使用して、カスタムフィールドグループを参照するプロファイル対応の「計算済み属性」スキーマを作成する方法について概説します。 このドキュメントには、計算済み属性に固有のサンプルコードが含まれていますが、APIを使用したフィールドグループおよびスキーマの定義の詳細については、『[スキーマレジストリAPIガイド](../../xdm/api/overview.md)』を参照してください。

## 計算済み属性フィールドグループの作成

スキーマレジストリAPIを使用してフィールドグループを作成するには、まず`/tenant/fieldgroups`エンドポイントにPOSTリクエストを行い、リクエスト本文にフィールドグループの詳細を入力します。 スキーマレジストリAPIを使用したフィールドグループの操作について詳しくは、[フィールドグループAPIエンドポイントガイド](../../xdm/api/field-groups.md)を参照してください。

**API 形式**

```http
POST /tenant/fieldgroups
```

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups\
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '{
        "title":"Computed Attributes Field Group",
        "description":"Description of the field group.",
        "type":"object",
        "meta:extensible": true,
        "meta:abstract": true,
        "meta:intendedToExtend": [
          "https://ns.adobe.com/xdm/context/profile"
        ],
        "definitions": {
          "computedAttributesFieldGroup": {
            "type": "object",
            "meta:xdmType": "object",
            "properties": {
              "_{TENANT_ID}": {
                "type": "object",
                "meta:xdmType": "object",
                "properties": {
                  "birthdayCurrentMonth": {
                    "type": "boolean",
                    "meta:xdmType": "boolean"
                  }
                }
              }
            }
          }
        },
        "allOf": [
          {
            "$ref": "#/definitions/computedAttributesFieldGroup"
          }
        ]
      }'
```

| プロパティ | 説明 |
|---|---|
| `title` | 作成するフィールドグループの名前。 |
| `meta:intendedToExtend` | フィールドグループを使用できるXDMクラスです。 |

**応答**

リクエストが成功すると、HTTP応答ステータス201（作成済み）が返されます。この状態には、新しく作成されたフィールドグループの詳細（`$id`、`meta:altIt`、`version`など）が含まれます。 これらの値は読み取り専用で、スキーマレジストリによって割り当てられます。

```json
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/fieldgroups/860ad1b1b35e0a88ecf6df92ebce08335c180313d5805352",
  "meta:altId": "_{TENANT_ID}.fieldgroups.860ad1b1b35e0a88ecf6df92ebce08335c180313d5805352",
  "meta:resourceType": "fieldgroups",
  "version": "1.0",
  "title": "Computed Attributes Field Group",
  "type": "object",
  "description": "Description of the field group.",
  "definitions": {
    "computedAttributesFieldGroup": {
      "type": "object",
      "meta:xdmType": "object",
      "properties": {
        "_{TENANT_ID}": {
          "type": "object",
          "meta:xdmType": "object",
          "properties": {
            "birthdayCurrentMonth": {
              "type": "boolean",
              "meta:xdmType": "boolean"
            }
          }
        }
      }
    }
  },
  "allOf": [
    {
      "$ref": "#/definitions/computedAttributesFieldGroup",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "imsOrg": "{IMS_ORG}",
  "meta:extensible": true,
  "meta:abstract": true,
  "meta:intendedToExtend": [
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1612861108205,
    "repo:lastModifiedDate": 1612861108205,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "627faa3346d3004aef2010e9bd2b7e721b19ae7857b276f3ef733e6e732d495f",
    "meta:globalLibVersion": "1.19.4"
  },
  "meta:containerId": "tenant",
  "meta:sandboxId": "{SANDBOX_ID}",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}"
}
```

## 計算済み属性を追加してフィールドグループを更新する

計算済み属性がさらに必要になる場合は、`/tenant/fieldgroups`エンドポイントにPUTリクエストを作成して、計算済み属性フィールドグループを追加の属性で更新できます。 このリクエストを行うには、パスに作成したフィールドグループの固有のIDと、本文に追加するすべての新しいフィールドを含める必要があります。

スキーマレジストリAPIを使用したフィールドグループの更新について詳しくは、[フィールドグループAPIエンドポイントガイド](../../xdm/api/field-groups.md)を参照してください。

**API 形式**

```http
PUT /tenant/fieldgroups/{FIELD_GROUP_ID}
```

**リクエスト**

このリクエストは、`purchaseSummary`情報に関連する新しいフィールドを追加します。

>[!NOTE]
>
>PUT要求を通じてフィールドグループを更新する場合、本文には、POST要求で新しいフィールドグループを作成する際に必要となるフィールドをすべて含める必要があります。

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups/_{TENANT_ID}.fieldgroups.8779fd45d6e4eb074300023a439862bbba359b60d451627a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "type": "object",
        "title": "Computed Attributes Field Group",
        "meta:extensible": true,
        "meta:abstract": true,
        "meta:intendedToExtend": [
          "https://ns.adobe.com/xdm/context/profile"
        ],
        "description": "Description of field group.",
        "definitions": {
          "computedAttributesFieldGroup": {
            "type": "object",
            "meta:xdmType": "object",
            "properties": {
              "_{TENANT_ID}": {
                "type": "object",
                "meta:xdmType": "object",
                "properties": {
                  "birthdayCurrentMonth": {
                    "type": "boolean",
                    "meta:xdmType": "boolean"
                  },
                  "purchaseSummary": {
                    "type": "object",
                    "meta:xdmType": "object",
                    "properties": {
                      "totalSpend": {
                        "type": "number",
                        "meta:xdmType": "number"
                      },
                      "countPurchases": {
                        "meta:xdmType": "int",
                        "type": "integer",
                        "minimum": -2147483648,
                        "maximum": 2147483647
                      },
                      "averageSpend": {
                        "type": "number",
                        "meta:xdmType": "number"
                      }
                    }
                  }
                }
              }
            }
          }
        },
        "allOf": [
          {
            "$ref": "#/definitions/computedAttributesFieldGroup"
          }
        ]
      }'
```

**応答**

正常に応答すると、更新されたフィールドグループの詳細が返されます。

```json
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/fieldgroups/860ad1b1b35e0a88ecf6df92ebce08335c180313d5805352",
  "meta:altId": "_{TENANT_ID}.fieldgroups.860ad1b1b35e0a88ecf6df92ebce08335c180313d5805352",
  "meta:resourceType": "fieldgroups",
  "version": "1.0",
  "title": "Computed Attributes Field Group",
  "type": "object",
  "description": "Description of field group.",
  "definitions": {
    "computedAttributesFieldGroup": {
      "type": "object",
      "meta:xdmType": "object",
      "properties": {
        "_{TENANT_ID}": {
          "type": "object",
          "meta:xdmType": "object",
          "properties": {
            "birthdayCurrentMonth": {
              "type": "boolean",
              "meta:xdmType": "boolean"
            },
            "purchaseSummary": {
              "type": "object",
              "meta:xdmType": "object",
              "properties": {
                "totalSpend": {
                  "type": "number",
                  "meta:xdmType": "number"
                },
                "countPurchases": {
                  "meta:xdmType": "int",
                  "type": "integer",
                  "minimum": -2147483648,
                  "maximum": 2147483647
                },
                "averageSpend": {
                  "type": "number",
                  "meta:xdmType": "number"
                }
              }
            }
          }
        }
      }
    }
  },
  "allOf": [
    {
      "$ref": "#/definitions/computedAttributesFieldGroup",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "imsOrg": "{IMS_ORG}",
  "meta:extensible": true,
  "meta:abstract": true,
  "meta:intendedToExtend": [
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1612861108205,
    "repo:lastModifiedDate": 1612861108205,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "627faa3346d3004aef2010e9bd2b7e721b19ae7857b276f3ef733e6e732d495f",
    "meta:globalLibVersion": "1.19.4"
  },
  "meta:containerId": "tenant",
  "meta:sandboxId": "{SANDBOX_ID}",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}"
}
```

## プロファイル対応スキーマの作成

スキーマレジストリAPIを使用してスキーマを作成するには、まず`/tenant/schemas`エンドポイントにPOSTリクエストを行い、リクエスト本文にスキーマの詳細を入力します。 スキーマは[!DNL Profile]に対しても有効にする必要があり、スキーマクラスの和集合スキーマの一部として表示されます。

[!DNL Profile]対応スキーマと和集合スキーマについて詳しくは、[[!DNL Schema Registry] APIガイド](../../xdm/api/overview.md)および[スキーマ構成の基本ドキュメント](../../xdm/schema/composition.md)を参照してください。

**API 形式**

```http
POST /tenants/schemas
```

**リクエスト**

次の要求では、このドキュメントで前に作成された`computedAttributesFieldGroup`を参照する新しいスキーマが作成され（一意のIDを使用）、プロファイル和集合スキーマに対して有効になります（`meta:immutableTags`配列を使用）。 スキーマレジストリAPIを使用してスキーマを作成する方法について詳しくは、[スキーマAPIエンドポイントガイド](../../xdm/api/schemas.md)を参照してください。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas \
  -H 'Authorization: Bearer {ACCESS_TOKEN' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "type": "object",
        "title": "Computed Attributes Schema",
        "meta:extensible": false,
        "meta:abstract": false,
        "meta:immutableTags": [
          "union"
        ],
        "meta:extends": [
          "https://ns.adobe.com/xdm/context/profile",
          "https://ns.adobe.com/xdm/context/identitymap",
          "https://ns.adobe.com/{TENANT_ID}/fieldgroups/860ad1b1b35e0a88ecf6df92ebce08335c180313d5805352"
        ],
        "description": "Description of schema.",
        "definitions": {
        },
        "allOf": [
          {
            "$ref": "https://ns.adobe.com/xdm/context/profile"
          },
          {
            "$ref": "https://ns.adobe.com/xdm/context/identitymap"
          },
          {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/fieldgroups/860ad1b1b35e0a88ecf6df92ebce08335c180313d5805352"
          }
        ],
        "meta:class": "https://ns.adobe.com/xdm/context/profile"
      }' 
```

**応答**

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたスキーマの詳細（`$id`、`meta:altId`、`version`など）を含むペイロードを返します。これらの値は読み取り専用で、スキーマレジストリによって割り当てられます。

```json
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/699c1f16821c51086394fef8233d7fdb61e6f5b574b5a230",
  "meta:altId": "_{TENANT}.schemas.699c1f16821c51086394fef8233d7fdb61e6f5b574b5a230",
  "meta:resourceType": "schemas",
  "version": "1.0",
  "title": "Computed Attributes Schema",
  "type": "object",
  "description": "Description of schema.",
  "definitions": {},
  "allOf": [
    {
      "$ref": "https://ns.adobe.com/xdm/context/profile",
      "type": "object",
      "meta:xdmType": "object"
    },
    {
      "$ref": "https://ns.adobe.com/xdm/context/identitymap",
      "type": "object",
      "meta:xdmType": "object"
    },
    {
      "$ref": "https://ns.adobe.com/{TENANT_ID}/fieldgroups/860ad1b1b35e0a88ecf6df92ebce08335c180313d5805352",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "refs": [
    "https://ns.adobe.com/xdm/context/profile",
    "https://ns.adobe.com/xdm/context/identitymap",
    "https://ns.adobe.com/{TENANT_ID}/fieldgroups/860ad1b1b35e0a88ecf6df92ebce08335c180313d5805352"
  ],
  "imsOrg": "{IMS_ORG}",
  "meta:extensible": false,
  "meta:abstract": false,
  "meta:extends": [
    "https://ns.adobe.com/xdm/common/auditable",
    "https://ns.adobe.com/xdm/data/record",
    "https://ns.adobe.com/xdm/context/profile",
    "https://ns.adobe.com/xdm/context/identitymap",
    "https://ns.adobe.com/{TENANT_ID}/fieldgroups/860ad1b1b35e0a88ecf6df92ebce08335c180313d5805352"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1612385766411,
    "repo:lastModifiedDate": 1612385766411,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "a9c6b5c25c109970ffa5eaeb3db2b47b59c696e1d9407fb39ccf7e48b74b558e",
    "meta:globalLibVersion": "1.18.4"
  },
  "meta:class": "https://ns.adobe.com/xdm/context/profile",
  "meta:containerId": "tenant",
  "meta:sandboxId": "{SANDBOX_ID}",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT}",
  "meta:immutableTags": [
    "union"
  ]
}
```

## 次の手順

計算済み属性が格納されるスキーマとフィールドグループを作成したら、`/computedattributes` APIエンドポイントを使用して計算済み属性を作成できます。 APIで計算済み属性を作成する詳細な手順については、[計算済み属性APIエンドポイントガイド](ca-api.md)に記載されている手順に従ってください。
