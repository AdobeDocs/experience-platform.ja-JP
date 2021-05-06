---
keywords: Experience Platform；ホーム；マッパ；マッピングセット；マッピング；
solution: Experience Platform
title: マッピングセットの概要
topic: 概要
description: Adobe Experience Platformデータ準備でのマッピングセットの使用方法を説明します。
translation-type: tm+mt
source-git-commit: 4c06f621eb6fba8daa6501d56255cddbbcfdbda2
workflow-type: tm+mt
source-wordcount: '960'
ht-degree: 5%

---


# マッピングセットの概要

マッピングセットは、あるスキーマから別のマッピングにデータを変換する一連のマッピングです。 このドキュメントでは、入力スキーマ、出力スキーマ、マッピングなど、マッピングセットの構成方法について説明します。

## はじめに

この概要には、以下のAdobe Experience Platformの構成要素について、実際に理解する必要があります。

- [データ準備](./home.md):データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル(XDM)との間でデータのマッピング、変換、検証を行うことができます。
- [データフロー](../dataflows/home.md):データフローは、プラットフォーム間でデータを移動するデータ・ジョブを表します。データフローは様々なサービスで構成され、ソースコネクタからターゲットデータセット、[!DNL Identity]と[!DNL Profile]、[!DNL Destinations]にデータを移動できます。
- [[!DNL Adobe Experience Platform Data Ingestion]](../ingestion/home.md):データの送信先のメソッド [!DNL Experience Platform]。
- [[!DNL Experience Data Model (XDM) System]](../xdm/home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。

## マッピングセットの構文

マッピングセットは、ID、名前、入力スキーマ、出力スキーマ、および関連付けられたマッピングのリストで構成されます。

次のJSONは、一般的なマッピングセットの例です。

```json
{
    "id" : "cbb0da769faa48fcb29e026a924ba29d",
    "name" : "Demo Mapping Set",
    "inputSchema" : {
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
            "name" : "Id",
            "description" : "Identifier field"
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
| `inputSchema` | 受信データのXDMスキーマです。 |
| `outputSchema` | 入力データが持つXDMスキーマは、準拠するように変換されます。 |
| `mappings` | ソース・スキーマからターゲット・スキーマへのフィールド間マッピングの配列。 |
| `sourceType` | リストに表示された各マッピングに対して、`sourceType`属性はマッピングするソースのタイプを示します。 `ATTRIBUTE`、`STATIC`、または`EXPRESSION`のいずれかです。 <ul><li> `ATTRIBUTE` は、ソースパスにある値に対して使用されます。 </li><li>`STATIC` は、宛先パスに挿入される値に使用されます。この値は一定のままで、ソーススキーマの影響を受けません。</li><li> `EXPRESSION` は式に使用され、実行時に解決されます。使用可能な式のリストは、[マッピング関数ガイド](./functions.md)に記載されています。</li> </ul> |
| `source` | リストに表示された各マッピングに対して、`source`属性はマッピングするフィールドを示します。 ソースの設定方法について詳しくは、[sourcesセクション](#sources)を参照してください。 |
| `destination` | リストされた各マッピングについて、`destination`属性はフィールド、または`source`フィールドから抽出された値が配置されるフィールドのパスを示します。 宛先の設定方法について詳しくは、[宛先セクション](#destination)を参照してください。 |
| `mappings.name` | （*オプション*）マッピングの名前。 |
| `mappings.description` | （*オプション*）マッピングの説明。 |

## マッピングソースの設定

マッピングでは、`source`はフィールド、式、または静的な値にすることができます。 指定したソースタイプに基づいて、様々な方法で値を抽出できます。

### 列データのフィールド

CSVファイルなど、列データのフィールドをマッピングする場合は、ソースタイプ`ATTRIBUTE`を使用します。 フィールド名の中に`.`が含まれる場合は、`\`を使用して値をエスケープします。 このマッピングの例を次に示します。

**サンプルのCSVファイル：**

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

**Transformed data**

```json
{
    "pi": {
        "name": "John Smith"
    }
}
```

### 入れ子データのフィールド

JSONファイルなど、ネストされたデータ内のフィールドをマッピングする場合は、ソースタイプ`ATTRIBUTE`を使用します。 フィールド名の中に`.`が含まれる場合は、`\`を使用して値をエスケープします。 このマッピングの例を次に示します。

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

**Transformed data**

```json
{
    "pi": {
        "name": "John Smith"
    }
}
```

### 配列内のフィールド

配列内のフィールドをマッピングする場合、インデックスを使用して特定の値を取得できます。 これを行うには、`ATTRIBUTE`ソースタイプと、マップする値のインデックスを使用します。 このマッピングの例を次に示します。

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

**Transformed data**

```json
{
    "pi": {
        "email": "js@example.com"
    }
}
```

### 配列またはオブジェクトをオブジェクトに配列

`ATTRIBUTE`ソースタイプを使用して、配列を配列またはオブジェクトに直接マップすることもできます。 このマッピングの例を次に示します。

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

**Transformed data**

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

### アレイに対する反復的な操作

`ATTRIBUTE`ソースタイプを使用すると、配列を繰り返しループ処理し、ワイルドカードインデックス(`[*]`)を使用してターゲットスキーマにマッピングできます。 このマッピングの例を次に示します。

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

**Transformed data**

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

定数または静的な値をマップする場合は、`STATIC`ソースタイプを使用します。  `STATIC`ソースタイプを使用する場合、`source`は`destination`に割り当てるハードコードされた値を表します。 このマッピングの例を次に示します。

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

**Transformed data**

```json
{
    "userType:": "CUSTOMER"
}
```

### 式

式をマップする場合は、`EXPRESSION`ソースタイプを使用します。 [マッピング関数ガイド](./functions.md)には、受け入れられる関数のリストが記載されています。 `EXPRESSION`ソースタイプを使用する場合、`source`は解決する関数を表します。 このマッピングの例を次に示します。

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

**Transformed data**

```json
{
    "pi": {
        "created": "SMITHJOHNFri Sep 25 15:17:31 PDT 2020"
    }
}
```

## マッピング先の設定

マッピングでは、`destination`は`source`から抽出された値が挿入される場所です。

### ルートレベルのフィールド

`source`値を変換後のデータのルートレベルにマップする場合は、次の例に従ってください。

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

**Transformed data**

```json
{
    "name": "John Smith"
}
```

### 階層化フィールド

`source`値を変換後のデータ内の入れ子のフィールドにマップする場合は、次の例に従ってください。

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

**Transformed data**

```json
{
    "pi": {
        "name": "John Smith"
    }
}
```

### 特定の配列インデックスのフィールド

`source`値を変換後のデータの配列内の特定のインデックスにマップする場合は、次の例に従ってください。

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

**Transformed data**

```json
{
    "piList": ["John Smith"]
}
```

### 反復配列演算

配列を繰り返しループ処理し、値をターゲットにマップする場合は、ワイルドカードインデックス(`[*]`)を使用できます。 この例を次に示します。

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

**Transformed data**

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

このドキュメントを読むことで、マッピングセット内の個々のマッピングを構成する方法など、マッピングセットの構築方法を理解する必要があります。 その他のデータ準備機能の詳細については、[データ準備の概要](./home.md)を参照してください。 データ準備API内でのマッピングセットの使用方法を学ぶには、[データ準備開発ガイド](./api/overview.md)を読んでください。