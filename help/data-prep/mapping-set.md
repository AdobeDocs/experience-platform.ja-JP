---
keywords: Experience Platform;ホーム;マッパー;マッピングセット;マッピング;
solution: Experience Platform
title: マッピングセットの概要
topic-legacy: overview
description: Adobe Experience Platform Data Prep でのマッピングセットの使用方法を説明します。
exl-id: b45545b7-3ae7-400d-b6fd-b2cb76061093
source-git-commit: 27e5c64f31b9a68252d262b531660811a0576177
workflow-type: tm+mt
source-wordcount: '959'
ht-degree: 100%

---

# マッピングセットの概要

マッピングセットは、あるスキーマから別のスキーマへとデータを変換する一連のマッピングです。このドキュメントでは、入力スキーマ、出力スキーマ、マッピングなど、マッピングセットの構成方法に関する情報を提供します。

## はじめに

この概要では、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

- [Data Prep](./home.md)：Data Prep を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換、検証をおこなうことができます。
- [データフロー](../dataflows/home.md)：データフローは、Platform 間でデータを移動するデータジョブを表します。データフローは異なるサービスをまたいで設定され、ソースコネクタからターゲットデータセット、[!DNL Identity] および [!DNL Profile]、[!DNL Destinations] へとデータを移動できます。
- [[!DNL Adobe Experience Platform Data Ingestion]](../ingestion/home.md)：[!DNL Experience Platform] にデータを送信するメソッド。
- [[!DNL Experience Data Model (XDM) System]](../xdm/home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。

## マッピングセットの構文

マッピングセットは、ID、名前、入力スキーマ、出力スキーマ、および関連付けられたマッピングのリストで構成されます。

次の JSON は、一般的なマッピングセットの例を示しています。

```json
{
    "id": "cbb0da769faa48fcb29e026a924ba29d",
    "name": "Demo Mapping Set",
    "inputSchema": {
        "id": "a167ff2947ff447ebd8bcf7ef6756232",
        "version": 0
    },
    "outputSchema": {
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/6dd1768be928c36d58ad4897219bb52d491671f966084bc0",
            "contentType": "application/vnd.adobe.xed-full+json;version=1"
        }
    },
    "mappings": [
        {
            "sourceType": "ATTRIBUTE",
            "source": "Id",
            "destination": "_id",
            "name": "Id",
            "description": "Identifier field"
        },
        {
            "sourceType": "ATTRIBUTE",
            "source": "FirstName",
            "destination": "person.name.firstName"
        },
        {
            "sourceType": "ATTRIBUTE",
            "source": "LastName",
            "destination": "person.name.lastName"
        }
    ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | マッピングセットの一意の識別子。 |
| `name` | マッピングセットの名前。 |
| `inputSchema` | 受信データの XDM スキーマ。 |
| `outputSchema` | 入力データが保持する XDM スキーマは、準拠するように変換されます。 |
| `mappings` | ソーススキーマから宛先スキーマへのフィールド間マッピングの配列。 |
| `sourceType` | リストされた各マッピングについて、`sourceType` 属性はマッピングするソースのタイプを示します。`ATTRIBUTE`、`STATIC`、`EXPRESSION` のいずれかです。 <ul><li> `ATTRIBUTE` は、ソースパス内の任意の値に対して使用されます。 </li><li>`STATIC` は、宛先パスに挿入される値に使用されます。この値は一定で、ソーススキーマの影響を受けません。</li><li> `EXPRESSION` は、実行時に解決される式に使用されます。使用可能な式のリストは、[マッピング関数ガイド](./functions.md)に記載されています。</li> </ul> |
| `source` | リストされた各マッピングについて、`source` 属性はマッピングするフィールドを示します。ソースの設定方法について詳しくは、[ソースの節](#sources)を参照してください。 |
| `destination` | リストされた各マッピングついて、`destination` 属性はフィールド（`source` フィールドから抽出された値が配置されるフィールドのパス）を示します。宛先の設定方法について詳しくは、[宛先の節](#destination)を参照してください。 |
| `mappings.name` | （*オプション*）マッピングの名前。 |
| `mappings.description` | （*オプション*）マッピングの説明。 |

## マッピングソースの設定

マッピングでは、`source` はフィールド、式、または静的な値にすることができます。指定されたソースタイプに基づいて、様々な方法で値を抽出できます。

### 列データのフィールド

CSV ファイルなど、列データのフィールドをマッピングする場合は、ソースタイプ `ATTRIBUTE` を使用します。フィールド名に `.` が含まれている場合は、`\` を使用して値をエスケープします。このマッピング例を次に示します。

**サンプル CSV ファイル：**

```csv
Full.Name, Email
John Smith, js@example.com
```

**サンプルマッピング**

```json
{
    "source": "Full.Name",
    "destination": "pi.name",
    "sourceType": "ATTRIBUTE"
}
```

**変換済みデータ**

```json
{
    "pi": {
        "name": "John Smith"
    }
}
```

### ネストされたデータのフィールド

JSON ファイルなど、ネストされたデータ内のフィールドをマッピングする場合は、ソースタイプ `ATTRIBUTE` を使用します。フィールド名に `.` が含まれている場合は、`\` を使用して値をエスケープします。このマッピング例を次に示します。

**サンプル JSON ファイル**

```json
{
    "customerInfo": {
        "name": "John Smith",
        "email": "js@example.com"
    }
}
```

**サンプルマッピング**

```json
{
    "source": "customerInfo.name",
    "destination": "pi.name",
    "sourceType": "ATTRIBUTE"
}
```

**変換済みデータ**

```json
{
    "pi": {
        "name": "John Smith"
    }
}
```

### 配列内のフィールド

配列内のフィールドをマッピングする場合は、インデックスを使用して特定の値を取得できます。これをおこなうには、`ATTRIBUTE` ソースタイプと、マッピングする値のインデックスを使用します。このマッピング例を次に示します。

**サンプル JSON ファイル**

```json
{
    "customerInfo": {
        "emails": [
            {
                "name": "John Smith",
                "email": "js@example.com"
            },
            {
                "name": "Jane Smith",
                "email": "jane@example.com"
            }
        ]
    }
}
```

**サンプルマッピング**

```json
{
    "source": "customerInfo.emails[0].email",
    "destination": "pi.email",
    "sourceType": "ATTRIBUTE"
}
```

**変換済みデータ**

```json
{
    "pi": {
        "email": "js@example.com"
    }
}
```

### 配列から配列、またはオブジェクトからオブジェクト

`ATTRIBUTE` ソース型を使用して、ある配列から別の配列、またはあるオブジェクトを別のオブジェクトに直接マッピングすることもできます。このマッピング例を次に示します。

**サンプル JSON ファイル**

```json
{
    "customerInfo": {
        "emails": [
            {
                "name": "John Smith",
                "email": "js@example.com"
            },
            {
                "name": "Jane Smith",
                "email": "jane@example.com"
            }
        ]
    }
}
```

**サンプルマッピング**

```json
{
    "source": "customerInfo.emails",
    "destination": "pi.emailList",
    "sourceType": "ATTRIBUTE"
}
```

**変換済みデータ**

```json
{
    "pi": {
        "emailList": [
            {
                "name": "John Smith",
                "email": "js@example.com"
            },
            {
                "name": "Jane Smith",
                "email": "jane@example.com"
            }
        ]
    }
}
```

### 配列に対する反復操作

`ATTRIBUTE` ソース型を使用すると、配列を反復的にループし、ワイルドカードインデックス（`[*]`）を使用してターゲットスキーマにマッピングできます。このマッピング例を次に示します。

**サンプル JSON ファイル**

```json
{
    "customerInfo": {
        "emails": [
            {
                "name": "John Smith",
                "email": "js@example.com"
            },
            {
                "name": "Jane Smith",
                "email": "jane@example.com"
            }
        ]
    }
}
```

**サンプルマッピング**

```json
{
    "source": "customerInfo.emails[*].name",
    "destination": "pi[*].names",
    "sourceType": "ATTRIBUTE"
}
```

**変換済みデータ**

```json
{
    "pi": [
        {
            "names": {
                "name": "John Smith"
            } 
        },
        {
            "names": {
                "name": "Jane Smith"
            }
        }
    ]
}
```

### 定数値

定数または静的な値をマッピングする場合は、`STATIC` ソースタイプを使用します。`STATIC` ソースタイプを使用する場合、`source` は、`destination`に割り当てる、ハードコーディングされた値を表します。このマッピング例を次に示します。

**サンプル JSON ファイル**

```json
{
    "name": "John Smith",
    "email": "js@example.com"
}
```

**サンプルマッピング**

```json
{
    "source": "CUSTOMER",
    "destination": "userType",
    "sourceType": "STATIC"
}
```

**変換済みデータ**

```json
{
    "userType:": "CUSTOMER"
}
```

### 式

式をマッピングする場合は、ソースタイプ `EXPRESSION` を使用します。使用可能な関数のリストは、[マッピング関数ガイド](./functions.md)に記載されています。`EXPRESSION` ソースタイプを使用する場合、`source` は解決する関数を表します。このマッピング例を次に示します。

**サンプル JSON ファイル**

```json
{
    "firstName": "John",
    "lastName": "Smith",
    "email": "js@example.com"
}
```

**サンプルマッピング**

```json
{
    "source": "concat(upper(lastName), upper(firstName), now())",
    "destination": "pi.created",
    "sourceType": "EXPRESSION"
}
```

**変換済みデータ**

```json
{
    "pi": {
        "created": "SMITHJOHNFri Sep 25 15:17:31 PDT 2020"
    }
}
```

## マッピングの宛先の設定

マッピングにおいて、`destination` は `source` から抽出された値が挿入される場所を表します。

### ルートレベルのフィールド

`source` 値を変換後のデータのルートレベルにマッピングする場合は、次の例に従います。

**サンプル JSON ファイル**

```json
{
    "customerInfo": {
        "name": "John Smith",
        "email": "js@example.com"
    }
}
```

**サンプルマッピング**

```json
{
    "source": "customerInfo.name",
    "destination": "name",
    "sourceType": "ATTRIBUTE"
}
```

**変換済みデータ**

```json
{
    "name": "John Smith"
}
```

### ネストされたフィールド

`source` 値を、変換後のデータでネストされたフィールドにマッピングする場合は、次の例に従います。

**サンプル JSON ファイル**

```json
{
    "name": "John Smith",
    "email": "js@example.com"
}
```

**サンプルマッピング**

```json
{
    "source": "name",
    "destination": "pi.name",
    "sourceType": "ATTRIBUTE"
}
```

**変換済みデータ**

```json
{
    "pi": {
        "name": "John Smith"
    }
}
```

### 特定の配列インデックスのフィールド

`source` 値を、変換後のデータで特定のインデックスにマッピングする場合は、次の例に従います。

**サンプル JSON ファイル**

```json
{
    "customerInfo": {
        "name": "John Smith",
        "email": "js@example.com"
    }
}
```

**サンプルマッピング**

```json
{
    "source": "customerInfo.name",
    "destination": "piList[0]",
    "sourceType": "ATTRIBUTE"
}
```

**変換済みデータ**

```json
{
    "piList": ["John Smith"]
}
```

### 反復配列操作

配列を繰り返しループして値をターゲットにマッピングする場合は、ワイルドカードインデックス（`[*]`）を使用できます。この例を次に示します。

```json
{
    "customerInfo": {
        "emails": [
            {
                "name": "John Smith",
                "email": "js@example.com"
            },
            {
                "name": "Jane Smith",
                "email": "jane@example.com"
            }
        ]
    }
}
```

**サンプルマッピング**

```json
{
    "source": "customerInfo.emails[*].name",
    "destination": "pi[*].names",
    "sourceType": "ATTRIBUTE"
}
```

**変換済みデータ**

```json
{
    "pi": [
        {
            "names": {
                "name": "John Smith"
            } 
        },
        {
            "names": {
                "name": "Jane Smith"
            }
        }
    ]
}
```

## 次の手順

このドキュメントでは、マッピングセット内で個別マッピングを設定する方法など、マッピングセットの構築方法を理解する必要があります。他の Data Prep 機能の詳細については、[Data Prep の概要](./home.md)を参照してください。Data Prep API 内でのマッピングセットの使用方法については、『[Data Prep 開発者ガイド](./api/overview.md)』を参照してください。
