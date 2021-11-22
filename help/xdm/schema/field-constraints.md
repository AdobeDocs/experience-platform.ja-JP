---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；フィールドグループ；フィールドグループ；フィールドグループ；フィールドグループ；データ型；データ型；データ型；データ型；データ型；データ型；データ型；スキーマ；スキーマ；スキーマ；スキーマデザイン；マップ；
solution: Experience Platform
title: XDM フィールドタイプ制約
topic-legacy: overview
description: Experience Data Model(XDM) のフィールドタイプ制約の参照です。これには、マッピングできる他のシリアル化形式や、API で独自のフィールドタイプを定義する方法が含まれます。
exl-id: 63839a28-6d26-46f1-8bbf-b524e82ac4df
source-git-commit: 684237122e7384f6c611e1c602c30af2518aba58
workflow-type: tm+mt
source-wordcount: '1153'
ht-degree: 17%

---

# XDM フィールドタイプ制約

Experience Data Model(XDM) スキーマでは、フィールドのタイプによって、フィールドに格納できるデータの種類が制限されます。 このドキュメントでは、各コアフィールドタイプの概要を示します。マッピングできるその他のシリアル化形式や、異なる制約を実施するために API で独自のフィールドタイプを定義する方法も示します。

## はじめに

このガイドを使用する前に、 [スキーマ構成の基本](./composition.md) XDM スキーマ、クラス、スキーマフィールドグループの紹介。

API で独自のフィールドタイプを定義する予定がある場合は、まず [スキーマレジストリ開発者ガイド](../api/getting-started.md) を参照して、カスタムフィールドを含めるフィールドグループとデータ型を作成する方法を確認してください。 Experience PlatformUI を使用してスキーマを作成する場合は、 [UI でのフィールドの定義](../ui/fields/overview.md) を参照して、カスタムフィールドグループおよびデータ型内で定義するフィールドに制約を実装する方法を確認してください。

## 基本構造と例

XDM は JSON スキーマの上に構築されているので、XDM フィールドの型を定義する際に、同様の構文を継承します。 JSON スキーマで異なるフィールドタイプがどのように表されるかを理解すると、各タイプの基本制約を示すのに役立ちます。

>[!NOTE]
>
>詳しくは、 [API の基本事項ガイド](../../landing/api-fundamentals.md#json-schema) を参照してください。

次の表に、各 XDM タイプが JSON スキーマで表される方法と、タイプに準拠する値の例を示します。

<table style="table-layout:auto">
  <thead>
    <tr>
      <th>XDM タイプ</th>
      <th>JSON スキーマ</th>
      <th>例</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[!UICONTROL 文字列 ]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{"type":"string"}</pre>
      </td>
      <td><code>"Platinum"</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL Double]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{"type":"number"}</pre>
      </td>
      <td><code>12925.49</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL Long]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{ "type":"integer", "maximum":9007199254740991, "minimum":-9007199254740991 }</pre>
      </td>
      <td><code>1478108935</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL Integer]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{ "type":"integer", "maximum":2147483648, "minimum":-2147483648 }</pre>
      </td>
      <td><code>24906290</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL Short]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{ "type":"integer", "maximum":32768, "minimum":-32768 }</pre>
      </td>
      <td><code>15781</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL バイト ]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{ "type":"integer", "maximum":128、"minimum":-128 }</pre>
      </td>
      <td><code>90</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL 日付 ]*</td>
      <td>
        <pre class="JSON language-JSON hljs">
{ "type":"string", "format":"date" }</pre>
      </td>
      <td><code>"2019-05-15"</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL DateTime]*</td>
      <td>
        <pre class="JSON language-JSON hljs">
{ "type":"string", "format":"date-time" }</pre>
      </td>
      <td><code>"2019-05-15T20:20:39+00:00"</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL Boolean]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{"type":"string"}</pre>
      </td>
      <td><code>true</code></td>
    </tr>
  </tbody>
</table>

**すべての日付形式の文字列は、ISO 8601 標準 ([RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6)) をクリックします。*

## 他の形式への XDM タイプのマッピング

以下の節では、各 XDM タイプを他の一般的なシリアル化形式にどのようにマッピングするかを説明します。

* [Parquet、Spark SQL、Java](#parquet)
* [Scala、.NET、CosmosDB](#scala)
* [MongoDB、Aerospike、および Protobuf 2](#mongo)

>[!IMPORTANT]
>
>次の表に示す標準 XDM タイプの中で、 [!UICONTROL マップ] タイプも含まれます。 マップは、特定の値にマッピングされるキーとしてデータが表される場合、またはキーを静的スキーマに合理的に含めることができず、データ値として処理する必要がある場合に、標準スキーマで使用されます。
>
>マップタイプフィールドは、業界およびベンダースキーマでの使用のために予約されているので、定義したカスタムリソースでは使用できません。 以下の表に含まれるマップタイプの組み込みは、既存のデータが以下に示す形式のいずれかで格納されている場合に、そのデータを XDM にマッピングする方法を判断するのに役立つように作成されています。

### Parquet、Spark SQL、Java {#parquet}

| XDM タイプ | Parquet | Spark SQL | Java |
| --- | --- | --- | --- |
| [!UICONTROL 文字列] | タイプ： `BYTE_ARRAY`<br>注釈： `UTF8` | `StringType` | `java.lang.String` |
| [!UICONTROL Double] | 型：`DOUBLE` | `LongType` | `java.lang.Double` |
| [!UICONTROL Long] | 型：`INT64` | `LongType` | `java.lang.Long` |
| [!UICONTROL 整数] | タイプ： `INT32`<br>注釈： `INT_32` | `IntegerType` | `java.lang.Integer` |
| [!UICONTROL Short] | タイプ： `INT32`<br>注釈： `INT_16` | `ShortType` | `java.lang.Short` |
| [!UICONTROL Byte] | タイプ： `INT32`<br>注釈： `INT_8` | `ByteType` | `java.lang.Short` |
| [!UICONTROL 日付] | タイプ： `INT32`<br>注釈： `DATE` | `DateType` | `java.util.Date` |
| [!UICONTROL DateTime] | タイプ： `INT64`<br>注釈： `TIMESTAMP_MILLIS` | `TimestampType` | `java.util.Date` |
| [!UICONTROL ブール型] | 型：`BOOLEAN` | `BooleanType` | `java.lang.Boolean` |
| [!UICONTROL マップ] | `MAP`注釈付きグループ<br><br>(`<key-type>` は、 `STRING`) | `MapType`<br><br>(`keyType` は、 `StringType`) | `java.util.Map` |

{style=&quot;table-layout:auto&quot;}

### Scala、.NET、CosmosDB {#scala}

| XDM タイプ | Scala | .NET | CosmosDB |
| --- | --- | --- | --- |
| [!UICONTROL 文字列] | `String` | `System.String` | `String` |
| [!UICONTROL ダブル] | `Double` | `System.Double` | `Number` |
| [!UICONTROL Long] | `Long` | `System.Int64` | `Number` |
| [!UICONTROL 整数] | `Int` | `System.Int32` | `Number` |
| [!UICONTROL Short] | `Short` | `System.Int16` | `Number` |
| [!UICONTROL Byte] | `Byte` | `System.SByte` | `Number` |
| [!UICONTROL 日付] | `java.util.Date` | `System.DateTime` | `String` |
| [!UICONTROL DateTime] | `java.util.Date` | `System.DateTime` | `String` |
| [!UICONTROL ブール型] | `Boolean` | `System.Boolean` | `Boolean` |
| [!UICONTROL マップ] | `Map` | (なし) | `object` |

{style=&quot;table-layout:auto&quot;}

### MongoDB、Aerospike、および Protobuf 2 {#mongo}

| XDM タイプ | MongoDB | Aerospike | Protobuf 2 |
| --- | --- | --- | --- |
| [!UICONTROL 文字列] | `string` | `String` | `string` |
| [!UICONTROL ダブル] | `double` | `Double` | `double` |
| [!UICONTROL Long] | `long` | `Integer` | `int64` |
| [!UICONTROL 整数] | `int` | `Integer` | `int32` |
| [!UICONTROL Short] | `int` | `Integer` | `int32` |
| [!UICONTROL Byte] | `int` | `Integer` | `int32` |
| [!UICONTROL 日付] | `date` | `Integer`<br>（Unix ミリ秒） | `int64`<br>（Unix ミリ秒） |
| [!UICONTROL DateTime] | `timestamp` | `Integer`<br>（Unix ミリ秒） | `int64`<br>（Unix ミリ秒） |
| [!UICONTROL ブール型] | `bool` | `Integer`<br>（0/1 バイナリ） | `bool` |
| [!UICONTROL マップ] | `object` | `map` | `map<key_type, value_type>` |

{style=&quot;table-layout:auto&quot;}

## API での XDM フィールドタイプの定義 {#define-fields}

すべての XDM フィールドは、標準の [JSON スキーマ](https://json-schema.org/) フィールドの種類に適用される制約。 [!DNL Experience Platform]. スキーマレジストリ API を使用すると、形式とオプションの制約を使用して、追加のフィールドの種類を定義できます。 XDM フィールドタイプは、フィールドレベルの属性で公開されます。 `meta:xdmType`.

>[!NOTE]
>
>`meta:xdmType` はシステムで生成される値なので、API を使用する際に、フィールド用にこのプロパティを JSON に追加する必要はありません。 ベストプラクティスは、JSON スキーマのタイプ ( `string` および `integer`) に適切な最小/最大制約を設定します。

次の表に、オプションのプロパティを含む、様々なフィールドタイプを定義するための適切な書式の概要を示します。 オプションのプロパティとタイプ固有のキーワードに関する詳細については、[JSON スキーマのドキュメント](https://json-schema.org/understanding-json-schema/reference/type.html)を参照してください。

最初に、目的のフィールドタイプを見つけ、提供されたサンプルコードを使用して、の API リクエストを作成します。 [フィールドグループの作成](../api/field-groups.md#create) または [データ型の作成](../api/data-types.md#create).

<table style="table-layout:auto">
  <tr>
    <th>XDM タイプ</th>
    <th>オプションのプロパティ</th>
    <th>例</th>
  </tr>
  <tr>
    <td>[!UICONTROL 文字列 ]</td>
    <td>
      <ul>
        <li><code>pattern</code></li>
        <li><code>minLength</code></li>
        <li><code>maxLength</code></li>
      </ul>
    </td>
    <td>
      <pre class="JSON language-JSON hljs">
"sampleField": {
            "type": "string",
            "pattern": "^[A-Z]{2}$",
            "maxLength": 2
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL URI]</td>
    <td></td>
    <td>
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "string",
          "format": "uri"
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL Enum]</td>
    <td>
      <ul>
        <li><code>default</code></li>
        <li><code>meta:enum</code></li>
      </ul>
    </td>
    <td>制約付き列挙値は、 <code>enum</code> 配列を使用する場合、各値のオプションの顧客向けラベルを <code>meta:enum</code>:
      <pre class="JSON language-JSON hljs">
"sampleField": {
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
}</pre>
    <br>なお、 <code>meta:enum</code> 値が <strong>not</strong> 列挙を宣言するか、データの検証を独自に実行します。 ほとんどの場合、文字列は <code>meta:enum</code> また、 <code>enum</code> データが制約を受けるようにする ただし、次のような場合に使用できます。 <code>meta:enum</code> 対応する <code>enum</code> 配列。 に関するチュートリアルを参照してください。 <a href="../tutorials/extend-soft-enum.md">ソフトエナムの拡張</a> を参照してください。
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL 数値 ]</td>
    <td></td>
    <td>
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "number"
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL Long]</td>
    <td></td>
    <td>
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "integer",
          "minimum": -9007199254740992,
          "maximum": 9007199254740992
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL Integer]</td>
    <td></td>
    <td>
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "integer",
          "minimum": -2147483648,
          "maximum": 2147483648
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL Short]</td>
    <td></td>
    <td>
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "integer",
          "minimum": -32768,
          "maximum": 32768
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL バイト ]</td>
    <td></td>
    <td>
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "integer",
          "minimum": -128,
          "maximum": 128
  }</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL Boolean]</td>
    <td>
      <ul>
        <li><code>default</code></li>
      </ul>
    </td>
    <td>
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "boolean",
          "default": false
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL 日付 ]</td>
    <td></td>
    <td>
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "string",
          "format": "date",
          "examples": ["2004-10-23"]
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL DateTime]</td>
    <td></td>
    <td>
      <pre class="JSON language-JSON hljs">
"sampleField":{ "type":"string", "format":"date-time", "examples":["2004-10-23T12:00:00-06:00"] }</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL 配列 ]</td>
    <td></td>
    <td>基本的なスカラー型（例：文字列）の配列：
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "array",
          "items": {
            "type": "string"
  }
}</pre>
      別のスキーマで定義されたオブジェクトの配列：<br/>
      <pre class="JSON language-JSON hljs">
"sampleField":{ "type":"array", "items":{ "$ref":"https://ns.adobe.com/xdm/data/paymentitem" } }</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL オブジェクト ]</td>
    <td></td>
    <td>この <code>type</code> 以下で定義された各サブフィールドの属性 <code>properties</code> 任意のスカラー型を使用して定義できます。
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "object",
          "properties": {
            "field1": {
              "type": "string"
            },
            "field2": {
              "type": "number"
    }
  }
}</pre>
      オブジェクトタイプのフィールドは、 <code>$id</code> データ型：
      <pre class="JSON language-JSON hljs">
"sampleField":{ "type":"object", "$ref":"https://ns.adobe.com/xdm/common/phoneinteraction" }</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL マップ ]</td>
    <td></td>
    <td>地図 <strong>次の値を指定する</strong> プロパティを定義します。 It <strong>必須</strong> 単一の <code>additionalProperties</code> マップ内に含まれる値のタイプを記述するスキーマ（各マップには 1 つのデータ型のみを含めることができます）。 値には任意の有効な XDM を指定できます <code>type</code> 属性、または <code>$ref</code> 属性。<br/><br/>string-type 値を持つ map フィールド：
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "object",
          "additionalProperties":{
            "type": "string"
  }
}</pre>
    値の文字列配列を持つ map フィールド：
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "object",
          "additionalProperties":{
            "type": "array",
            "items": {
              "type": "string"
    }
  }
}</pre>
    別のデータ型を参照する map フィールド：
      <pre class="JSON language-JSON hljs">
"sampleField":{ "type":"object", "additionalProperties":{ "$ref":"https://ns.adobe.com/xdm/data/paymentitem" } }</pre>
    </td>
  </tr>
</table>
