---
title: スキーマレジストリ API での XDM フィールドの定義
description: スキーマレジストリ API でカスタムの Experience Data Model(XDM) リソースを作成する際に、異なるフィールドを定義する方法を説明します。
exl-id: d79332e3-8448-42af-b250-882bcb0f1e7d
source-git-commit: f7a6f53c0993348c9a0fc0f935a9d02d54389311
workflow-type: tm+mt
source-wordcount: '1202'
ht-degree: 3%

---

# スキーマレジストリ API での XDM フィールドの定義

すべてのエクスペリエンスデータモデル (XDM) フィールドは、標準の [JSON スキーマ](https://json-schema.org/) フィールドタイプに適用される制約と、Adobe Experience Platformで適用されるフィールド名に対する追加の制約が含まれます。 スキーマレジストリ API を使用すると、形式とオプションの制約を使用して、スキーマ内のカスタムフィールドを定義できます。 XDM フィールドタイプは、フィールドレベルの属性で公開されます。 `meta:xdmType`.

>[!NOTE]
>
>`meta:xdmType` はシステムで生成される値なので、API を使用する場合 ( [カスタムマップタイプの作成](#custom-maps)) をクリックします。 ベストプラクティスは、JSON スキーマのタイプ ( `string` および `integer`) に適切な最小/最大制約を設定します。

このガイドでは、オプションのプロパティを持つフィールドを含め、様々なフィールドタイプを定義するための適切な書式の概要を説明します。 オプションのプロパティとタイプ固有のキーワードに関する詳細については、[JSON スキーマのドキュメント](https://json-schema.org/understanding-json-schema/reference/type.html)を参照してください。

最初に、目的のフィールドタイプを見つけ、提供されたサンプルコードを使用して、の API リクエストを作成します。 [フィールドグループの作成](../api/field-groups.md#create) または [データ型の作成](../api/data-types.md#create).

## [!UICONTROL 文字列] {#string}

[!UICONTROL 文字列] フィールドは、 `type: string`.

```json
"sampleField": {
  "title": "Sample String Field",
  "description": "An example string field.",
  "type": "string"
}
```

オプションで、次の追加のプロパティを使用して、文字列に入力できる値の種類を制限できます。

* `pattern`:拘束する正規表現パターン。
* `minLength`:文字列の最小の長さです。
* `maxLength`:文字列の最大長です。

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

[!UICONTROL URI] フィールドは、 `type: string` と `format` プロパティを `uri`. その他のプロパティは受け入れられません。

```json
"sampleField": {
  "title": "Sample URI Field",
  "description": "An example URI field.",
  "type": "string",
  "format": "uri"
}
```

## [!UICONTROL 列挙] {#enum}

[!UICONTROL Enum] フィールドを使用する必要がある `type: string`に値をセットする、列挙値自体が `enum` 配列：

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

オプションで、 `meta:enum` プロパティに追加します。各ラベルは、 `enum`.

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
>この `meta:enum` 値が **not** 列挙を宣言するか、データの検証を独自に実行します。 ほとんどの場合、文字列は `meta:enum` また、 `enum` データが制約を受けるようにする ただし、次のような場合に使用できます。 `meta:enum` 対応する `enum` 配列。 に関するチュートリアルを参照してください。 [API での推奨値の定義](../tutorials/suggested-values.md) を参照してください。

オプションで、 `default` デフォルトを示すプロパティ `enum` 値を指定しない場合にフィールドで使用する値。

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
>指定しない場合 `default` の値が指定され、列挙フィールドが `required`に値を指定しない場合、このフィールドで許可された値がレコードに含まれていない場合は、取得時に検証に失敗します。

## [!UICONTROL 数値] {#number}

数値フィールドは、 `type: number` およびには、他の必須プロパティはありません。

```json
"sampleField": {
  "title": "Sample Number Field",
  "description": "An example number field.",
  "type": "number"
}
```

>[!NOTE]
>
>`number` 型は任意の数値型（整数または浮動小数点数）に対して使用されますが、 [`integer` タイプ](#integer) は、特に整数に対して使用されます。 詳しくは、 [数値タイプに関する JSON スキーマのドキュメント](https://json-schema.org/understanding-json-schema/reference/numeric.html) を参照してください。

## [!UICONTROL 整数] {#integer}

[!UICONTROL 整数] フィールドは、 `type: integer` そして他の必須フィールドはありません。

```json
"sampleField": {
  "title": "Sample Integer Field",
  "description": "An example integer field.",
  "type": "integer"
}
```

>[!NOTE]
>
>While `integer` 型は特に整数を参照する。 [`number` タイプ](#number) は、整数または浮動小数点数など、任意の数値型に対して使用されます。 詳しくは、 [数値タイプに関する JSON スキーマのドキュメント](https://json-schema.org/understanding-json-schema/reference/numeric.html) を参照してください。

必要に応じて、 `minimum` および `maximum` プロパティを定義に追加します。 スキーマビルダー UI でサポートされるその他の数値タイプは、 `integer` 特定の `minimum` および `maximum` 制約（例： ） [[!UICONTROL Long]](#long), [[!UICONTROL Short]](#short)、および [[!UICONTROL Byte]](#byte).

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

の [!UICONTROL Long] スキーマビルダー UI で作成されたフィールドは、 [`integer` タイプフィールド](#integer) 特定の `minimum` および `maximum` 値 (`-9007199254740992` および `9007199254740992`、それぞれ )。

```json
"sampleField": {
  "title": "Sample Long Field",
  "description": "An example long field.",
  "type": "integer",
  "minimum": -9007199254740992,
  "maximum": 9007199254740992
}
```

## [!UICONTROL Short] {#short}

の [!UICONTROL Short] スキーマビルダー UI で作成されたフィールドは、 [`integer` タイプフィールド](#integer) 特定の `minimum` および `maximum` 値 (`-32768` および `32768`、それぞれ )。

```json
"sampleField": {
  "title": "Sample Short Field",
  "description": "An example short field.",
  "type": "integer",
  "minimum": -32768,
  "maximum": 32768
}
```

## [!UICONTROL Byte] {#byte}

の [!UICONTROL Byte] スキーマビルダー UI で作成されたフィールドは、 [`integer` タイプフィールド](#integer) 特定の `minimum` および `maximum` 値 (`-128` および `128`、それぞれ )。

```json
"sampleField": {
  "title": "Sample Byte Field",
  "description": "An example byte field.",
  "type": "integer",
  "minimum": -128,
  "maximum": 128
}
```

## [!UICONTROL ブール値] {#boolean}

[!UICONTROL ブール値] フィールドは、 `type: boolean`.

```json
"sampleField": {
  "title": "Sample Boolean Field",
  "description": "An example boolean field.",
  "type": "boolean"
}
```

オプションで、 `default` 取り込み中に明示的な値が指定されない場合にフィールドで使用する値。

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
>指定しない場合 `default` 値が指定され、ブール値フィールドが `required`に値を指定しない場合、このフィールドで許可された値がレコードに含まれていない場合は、取得時に検証に失敗します。

## [!UICONTROL 日付] {#date}

[!UICONTROL 日付] フィールドは、 `type: string` および `format: date`. オプションで、 `examples` を活用して、ユーザーが手動でデータを入力する際にサンプルの日付文字列を表示する場合に役立てます。

```json
"sampleField": {
  "title": "Sample Date Field",
  "description": "An example date field with an example array item.",
  "type": "string",
  "format": "date",
  "examples": ["2004-10-23"]
}
```

## [!UICONTROL 日時] {#date-time}

[!UICONTROL DateTime] フィールドは、 `type: string` および `format: date-time`. オプションで、 `examples` を活用して、ユーザーが手動でデータを入力する際に、サンプルの datetime 文字列を表示する場合に役立てます。

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

[!UICONTROL 配列] フィールドは、 `type: array` および `items` 配列が受け付ける項目のスキーマを定義するオブジェクト。

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

また、 `$id` の `$ref` プロパティ。 以下は、 [!UICONTROL 支払い項目] オブジェクト：

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

## [!UICONTROL オブジェクト] {#object}

[!UICONTROL オブジェクト] フィールドは、 `type: object` および `properties` スキーマフィールドのサブプロパティを定義するオブジェクト。

以下で定義される各サブフィールド `properties` 任意のプリミティブを使用して定義できます。 `type` または、 `$ref` を指すプロパティ `$id` 」の値が次のようになります。

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

また、対象のデータ型自体が `type: object`:

```json
"sampleField": {
  "title": "Sample Object Field",
  "description": "An example object field using a data type reference.",
  "$ref": "https://ns.adobe.com/xdm/common/phoneinteraction"
}
```

## [!UICONTROL マップ] {#map}

マップフィールドは、基本的には [`object`-type フィールド](#object) 制約のないキーのセットを持つ オブジェクトと同様に、マップには `type` 値 `object`ですが、 `meta:xdmType` が明示的にに設定されている `map`.

地図 **次の値を指定する** プロパティを定義します。 It **必須** 単一の `additionalProperties` マップ内に含まれる値のタイプを記述するスキーマ（各マップには 1 つのデータ型のみを含めることができます）。 この `type` 値は次のいずれかでなければなりません `string` または `integer`.

例えば、文字列タイプの値を持つ map フィールドは次のように定義します。

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

カスタムマップフィールドの作成について詳しくは、以下の「 」の節を参照してください。

### カスタムマップタイプの作成 {#custom-maps}

XDM で「マップに似た」データを効率的にサポートするために、オブジェクトに `meta:xdmType` に設定 `map` これは、キーセットが制約されていないかのようにオブジェクトを管理する必要があることを明確にするためです。 マップフィールドに取り込まれるデータは、文字列キーと、文字列値または整数値 ( `additionalProperties.type`) をクリックします。

XDM では、このストレージヒントの使用に次の制限を設けます。

* マップタイプはタイプである必要があります `object`.
* マップタイプには、プロパティを定義することはできません（つまり、「空の」オブジェクトを定義します）。
* マップタイプには `additionalProperties.type` マップ内に配置される値を示すフィールド。 `string` または `integer`.

必要に応じて、次のパフォーマンス上の欠点があるので、マップタイプのフィールドのみを使用していることを確認します。

* 応答時間： [Adobe Experience Platform Query Service](../../query-service/home.md) は 3 秒から 10 秒に分解し、1 億レコードになります。
* マップのキー数は 16 個未満にする必要があります。16 個未満にすると、さらに低下する可能性があります。

また、Platform のユーザーインターフェイスでは、マップタイプフィールドのキーを抽出する方法に制限があります。 オブジェクトタイプのフィールドは展開できますが、マップは代わりに 1 つのフィールドとして表示されます。

## 次の手順

このガイドでは、API で様々なフィールドタイプを定義する方法について説明しました。 XDM フィールドタイプの形式について詳しくは、 [XDM フィールドタイプ制約](../schema/field-constraints.md).
