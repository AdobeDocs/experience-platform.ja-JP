---
title: Schema Registry API での XDM フィールドの定義
description: Schema Registry API でカスタム Experience Data Model （XDM）リソースを作成する際に様々なフィールドを定義する方法を説明します。
exl-id: d79332e3-8448-42af-b250-882bcb0f1e7d
source-git-commit: a3140d5216857ef41c885bbad8c69d91493b619d
workflow-type: tm+mt
source-wordcount: '1188'
ht-degree: 1%

---

# Schema Registry API での XDM フィールドの定義

すべてのエクスペリエンスデータモデル（XDM）フィールドは、フィールドタイプに適用される標準 [JSON スキーマ ](https://json-schema.org/) 制約を使用して定義され、Adobe Experience Platformによって適用されるフィールド名の制約も追加されます。 Schema Registry API では、形式とオプションの制約を使用して、スキーマにカスタムフィールドを定義できます。 XDM フィールドタイプは、フィールドレベルの属性 `meta:xdmType` で公開されます。

>[!NOTE]
>
>`meta:xdmType` はシステムで生成される値なので、API を使用する場合（[ カスタムマップタイプの作成 ](#custom-maps) を除く）、このプロパティをフィールドの JSON に追加する必要はありません。 ベストプラクティスは、次の表で定義されている適切な min/max 制約を使用して JSON スキーマタイプ（`string` や `integer` など）を使用することです。

このガイドでは、オプションのプロパティを含む、様々なフィールドタイプを定義するための適切な形式について説明します。 オプションのプロパティとタイプ固有のキーワードに関する詳細については、[JSON スキーマのドキュメント](https://json-schema.org/understanding-json-schema/reference/type.html)を参照してください。

まず、目的のフィールドタイプを見つけ、提供されたサンプルコードを使用して [ フィールドグループの作成 ](../api/field-groups.md#create) または [ データタイプの作成 ](../api/data-types.md#create) のための API リクエストを作成します。

## [!UICONTROL 文字列] {#string}

[!UICONTROL  文字列 ] フィールドは `type: string` で示されます。

```json
"sampleField": {
  "title": "Sample String Field",
  "description": "An example string field.",
  "type": "string"
}
```

オプションで、次の追加のプロパティを使用して、文字列に入力できる値の種類を制限できます。

* `pattern`：制約の基準となる正規表現パターン。
* `minLength`：文字列の最小の長さ。
* `maxLength`：文字列の最大長。

```json
"sampleField": {
  "title": "Sample String Field",
  "description": "An example string field with added constraints.",
  "type": "string",
  "pattern": "^[A-Z]{2}$",
  "maxLength": 2
}
```

## [!UICONTROL URI] {#uri}

[!UICONTROL URI] フィールドは、`format` プロパティが `uri` に設定された `type: string` で示されます。 その他のプロパティは使用できません。

```json
"sampleField": {
  "title": "Sample URI Field",
  "description": "An example URI field.",
  "type": "string",
  "format": "uri"
}
```

## [!UICONTROL  列挙 ] {#enum}

[!UICONTROL  列挙 ] フィールドでは、`type: string` を使用し、列挙値自体を `enum` 配列の下に指定する必要があります。

```json
"sampleField": {
  "title": "Sample Enum Field",
  "description": "An example enum field.",
  "type": "string",
  "enum": [
      "value1",
      "value2",
      "value3"
  ]
}
```

オプションで、`meta:enum` プロパティの各値に顧客向けのラベルを提供でき、各ラベルは `enum` の下の対応する値にキーが設定されます。

```json
"sampleField": {
  "title": "Sample Enum Field",
  "description": "An example enum field with customer-facing labels.",
  "type": "string",
  "enum": [
      "value1",
      "value2",
      "value3"
  ],
  "meta:enum": {
      "value1": "Value 1",
      "value2": "Value 2",
      "value3": "Value 3"
  }
}
```

>[!NOTE]
>
>`meta:enum` 値は、列挙を宣言したり **データ検証を単独で実行したりしない** です。 ほとんどの場合、`meta:enum` で指定する文字列は、データの制約を確保するために `enum` でも指定します。 ただし、対応する `enum` 配列なしで `meta:enum` が提供されるユースケースもあります。 詳しくは、[ 推奨値の定義 ](../tutorials/suggested-values.md) に関するチュートリアルを参照してください。

オプションで `default` プロパティを指定して、値が指定されていない場合にフィールドで使用されるデフォルトの `enum` 値を指定できます。

```json
"sampleField": {
  "title": "Sample Enum Field",
  "description": "An example enum field with customer-facing labels and a default value.",
  "type": "string",
  "enum": [
      "value1",
      "value2",
      "value3"
  ],
  "meta:enum": {
      "value1": "Value 1",
      "value2": "Value 2",
      "value3": "Value 3"
  },
  "default": "value1"
}
```

>[!IMPORTANT]
>
>`default` 値が指定されておらず、列挙フィールドが `required` に設定されている場合、このフィールドの許容値がないレコードは、取り込み時に検証に失敗します。

## [!UICONTROL  数値 ] {#number}

数値フィールドは `type: number` で示され、その他の必須プロパティはありません。

```json
"sampleField": {
  "title": "Sample Number Field",
  "description": "An example number field.",
  "type": "number"
}
```

>[!NOTE]
>
>`number` 型は整数または浮動小数点数のいずれかの数値型に使用され、[`integer` 型は特に ](#integer) 整数に使用されます。 各タイプのユースケースについて詳しくは、[ 数値タイプに関する JSON スキーマのドキュメント ](https://json-schema.org/understanding-json-schema/reference/numeric.html) を参照してください。

## [!UICONTROL  整数 ] {#integer}

[!UICONTROL  整数 ] フィールドは `type: integer` で示され、その他の必須フィールドはありません。

```json
"sampleField": {
  "title": "Sample Integer Field",
  "description": "An example integer field.",
  "type": "integer"
}
```

>[!NOTE]
>
>`integer` 型は特に整数を指しますが、[`number` 型は ](#number) 整数または浮動小数点数の任意の数値型に使用されます。 各タイプのユースケースについて詳しくは、[ 数値タイプに関する JSON スキーマのドキュメント ](https://json-schema.org/understanding-json-schema/reference/numeric.html) を参照してください。

必要に応じて、定義に `minimum` と `maximum` のプロパティを追加して、整数の範囲を制限できます。 スキーマビルダー UI でサポートされているその他のいくつかの数値タイプは、[[!UICONTROL Long]](#long)、[[!UICONTROL Short]](#short)、[[!UICONTROL Byte]](#byte) など、特定の `minimum` と `maximum` の制約を持つ `integer` タイプです。

```json
"sampleField": {
  "title": "Sample Integer Field",
  "description": "An example integer field with added constraints.",
  "type": "integer",
  "minimum": 1,
  "maximum": 100
}
```

## [!UICONTROL Long] {#long}

スキーマビルダー UI を使用して作成した [!UICONTROL Long] フィールドに相当するのは ](#integer) 特定の `minimum` 値および `maximum` 値（それぞれ `-9007199254740992` および `9007199254740992`）を持つ [`integer` タイプのフィールドです。

```json
"sampleField": {
  "title": "Sample Long Field",
  "description": "An example long field.",
  "type": "integer",
  "minimum": -9007199254740992,
  "maximum": 9007199254740992
}
```

## [!UICONTROL  短い ] {#short}

スキーマビルダー UI を使用して作成した [!UICONTROL  短 ] フィールドに相当するのは ](#integer) 特定の `minimum` 値および `maximum` 値（それぞれ `-32768` および `32768`）を持つ [`integer` タイプのフィールドです。

```json
"sampleField": {
  "title": "Sample Short Field",
  "description": "An example short field.",
  "type": "integer",
  "minimum": -32768,
  "maximum": 32768
}
```

## [!UICONTROL  バイト ] {#byte}

スキーマビルダー UI を使用して作成した [!UICONTROL  バイト ] フィールドに相当するのは ](#integer) 特定の `minimum` 値および `maximum` 値（それぞれ `-128` および `128`）を持つ [`integer` タイプのフィールドです。

```json
"sampleField": {
  "title": "Sample Byte Field",
  "description": "An example byte field.",
  "type": "integer",
  "minimum": -128,
  "maximum": 128
}
```

## [!UICONTROL  ブール値 ] {#boolean}

[!UICONTROL  ブール値 ] フィールドは `type: boolean` で示されます。

```json
"sampleField": {
  "title": "Sample Boolean Field",
  "description": "An example boolean field.",
  "type": "boolean"
}
```

オプションで、取り込み中に明示的な値が指定されなかった場合にフィールドで使用する `default` 値を指定できます。

```json
"sampleField": {
  "title": "Sample Boolean Field",
  "description": "An example boolean field with a default value.",
  "type": "boolean",
  "default": false
}
```

>[!IMPORTANT]
>
>`default` 値が指定されておらず、ブール値フィールドが `required` に設定されている場合、このフィールドの許容値を持たないレコードは、取り込み時に検証に失敗します。

## [!UICONTROL 日付] {#date}

[!UICONTROL  日付 ] フィールドは `type: string` と `format: date` で示されます。 また、ユーザーが手動でデータを入力する際に、サンプルの日付文字列を表示する場合に活用する `examples` ータの配列を、オプションで指定することもできます。

```json
"sampleField": {
  "title": "Sample Date Field",
  "description": "An example date field with an example array item.",
  "type": "string",
  "format": "date",
  "examples": ["2004-10-23"]
}
```

## [!UICONTROL  日時 ] {#date-time}

[!UICONTROL  日時 ] フィールドは、`type: string` と `format: date-time` で示されます。 また、ユーザーが手動でデータを入力する際に、サンプルの日時文字列を表示する場合に利用する `examples` ータの配列を、オプションで指定することもできます。

```json
"sampleField": {
  "title": "Sample Datetime Field",
  "description": "An example datetime field with an example array item.",
  "type": "string",
  "format": "date-time",
  "examples": ["2004-10-23T12:00:00-06:00"]
}
```

## [!UICONTROL 配列] {#array}

[!UICONTROL  配列 ] フィールドは、`type: array` と、配列が受け入れる項目のスキーマを定義する `items` オブジェクトで示されます。

文字列の配列など、プリミティブ型を使用して配列項目を定義できます。

```json
"sampleField": {
  "title": "Sample Array Field",
  "description": "An example array field using a primitive type.",
  "type": "array",
  "items": {
    "type": "string"
  }
}
```

`$ref` プロパティを使用してデータタイプの `$id` を参照することで、既存のデータタイプに基づいて配列項目を定義することもできます。 次に、[!UICONTROL  支払い項目 ] オブジェクトの配列を示します。

```json
"sampleField": {
  "title": "Sample Array Field",
  "description": "An example array field using a data type reference.",
  "type": "array",
  "items": {
    "$ref": "https://ns.adobe.com/xdm/data/paymentitem"
  }
}
```

## [!UICONTROL  オブジェクト ] {#object}

[!UICONTROL  オブジェクト ] フィールドは、`type: object` と、スキーマフィールドのサブプロパティを定義する `properties` オブジェクトで示されます。

`properties` の下で定義される各サブフィールドは、任意のプリミティブ `type` を使用するか、該当するデータタイプの `$id` を指す `$ref` プロパティを使用して既存のデータタイプを参照することで定義できます。

```json
"sampleField": {
  "title": "Sample Object Field",
  "description": "An example object field.",
  "type": "object",
  "properties": {
    "field1": {
      "type": "string"
    },
    "field2": {
      "$ref": "https://ns.adobe.com/xdm/common/measure"
    }
  }
}
```

問題のデータタイプ自体が `type: object` のように定義されている場合は、データタイプを参照することで、オブジェクト全体を定義することもできます。

```json
"sampleField": {
  "title": "Sample Object Field",
  "description": "An example object field using a data type reference.",
  "$ref": "https://ns.adobe.com/xdm/common/phoneinteraction"
}
```

## [!UICONTROL マップ] {#map}

マップフィールドは基本的に [`object` 型のフィールドで ](#object) 制約のないキーセットを持ちます。 オブジェクトと同様に、マップの `type` 値は `object` ですが、マップの `meta:xdmType` は明示的に `map` に設定されます。

マップは、プロパティを定義 **してはいけません** します。 **必ず** 単一の `additionalProperties` スキーマを定義して、マップ内に含まれる値のタイプを記述します（各マップには、単一のデータタイプのみを含めることができます）。 `type` 値は `string` または `integer` にする必要があります。

例えば、文字列タイプの値を含むマップフィールドは、次のように定義されます。

```json
"sampleField": {
  "title": "Sample Map Field",
  "description": "An example map field.",
  "type": "object",
  "meta:xdmType": "map",
  "additionalProperties": {
    "type": "string"
  }
}
```

カスタムマップフィールドの作成について詳しくは、以下の節を参照してください。

### カスタムマップタイプの作成 {#custom-maps}

XDM で「マップのような」データを効率的にサポートするために、オブジェクトには、キーセットが制約されていないかのようにオブジェクトを管理する必要があることを明確にするために、`map` に設定された `meta:xdmType` ールで注釈が付けられる場合があります。 マップフィールドに取り込まれるデータでは、文字列キーと、（`additionalProperties.type` で決定される）文字列または整数値のみを使用する必要があります。

XDM は、このストレージヒントの使用に次の制限を設けます。

* マップの種類は `object` 型でなければなりません。
* マップの種類にはプロパティを定義できません（つまり、「空の」オブジェクトを定義します）。
* マップの種類には、`string` または `integer` のいずれかでマップ内に配置できる値を記述する `additionalProperties.type` フィールドを含める必要があります。

絶対に必要な場合にのみ、マップタイプのフィールドを使用するようにしてください。これには、次のパフォーマンス上の欠点があります。

* [Adobe Experience Platform クエリサービスからの応答時間が ](../../query-service/home.md)1 億件のレコードに対して 3 秒から 10 秒に短縮されます。
* マップのキー数は 16 未満にする必要があります。そうしないと、さらに低下する可能性があります。

Platform ユーザーインターフェイスには、マップタイプのフィールドのキーを抽出する方法に制限もあります。 オブジェクトタイプのフィールドは展開できますが、マップは代わりに単一のフィールドとして表示されます。

## 次の手順

このガイドでは、API で様々なフィールドタイプを定義する方法について説明しました。 XDM フィールドタイプの書式設定方法について詳しくは、[XDM フィールドタイプの制約 ](../schema/field-constraints.md) に関するガイドを参照してください。
