---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；フィールドグループ；フィールドグループ；フィールドグループ；フィールドグループ；データ型；データ型；データ型；データ型；データ型；データ型；データ型；データ型；スキーマ；スキーマ；スキーマ設計；マップ；
solution: Experience Platform
title: XDM フィールドタイプの制約
topic-legacy: overview
description: マッピングできる他のシリアル化形式や API で独自のフィールドタイプを定義する方法など、エクスペリエンスデータモデル (XDM) のフィールドタイプ制約のリファレンスです。
exl-id: 63839a28-6d26-46f1-8bbf-b524e82ac4df
source-git-commit: 61025ada3a900a5bd7682e3bb7d4f6cd23347231
workflow-type: tm+mt
source-wordcount: '1097'
ht-degree: 18%

---

# XDM フィールドタイプの制約

エクスペリエンスデータモデル (XDM) スキーマでは、フィールドのタイプによって、フィールドに格納できるデータの種類が制限されます。 このドキュメントでは、各コアフィールドタイプの概要を示します。マッピングできるその他のシリアル化形式や、異なる制約を適用するために API で独自のフィールドタイプを定義する方法も示します。

## はじめに

このガイドを使用する前に、[ スキーマ構成の基本 ](./composition.md) を参照して、XDM スキーマ、クラス、スキーマフィールドグループの概要を確認してください。

API で独自のフィールドの種類を定義する場合は、『[ スキーマレジストリ開発者ガイド ](../api/getting-started.md)』から始めて、カスタムフィールドを含めるフィールドグループとデータ型の作成方法を学ぶことを強くお勧めします。 Experience PlatformUI を使用してスキーマを作成する場合は、[UI でのフィールドの定義 ](../ui/fields/overview.md) のガイドを参照して、カスタムフィールドグループおよびデータ型内で定義するフィールドに制約を実装する方法を確認してください。

## 基本構造と例

XDM は JSON スキーマに基づいて構築されているので、XDM フィールドのタイプを定義する際に、同様の構文を継承します。 JSON スキーマで異なるフィールドタイプがどのように表されるかを理解すると、各タイプの基本制約を示すのに役立ちます。

>[!NOTE]
>
>JSON スキーマと Platform API のその他の基盤となるテクノロジーの詳細については、『[API の基本原則ガイド ](../../landing/api-fundamentals.md#json-schema)』を参照してください。

次の表に、各 XDM タイプが JSON スキーマでどのように表されるかと、タイプに準拠するサンプル値の概要を示します。

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
{
  "type":"integer",
  "maximum":9007199254740991,
  "minimum":-9007199254740991
}</pre>
      </td>
      <td><code>1478108935</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL 整数 ]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type":"integer",
  "maximum":2147483648,
  "minimum":-2147483648
}</pre>
      </td>
      <td><code>24906290</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL 短 ]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type":"integer",
  "maximum":32768,
  "minimum":-32768
}</pre>
      </td>
      <td><code>15781</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL バイト ]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type":"integer",
  "maximum":128,
  "minimum":-128
}</pre>
      </td>
      <td><code>90</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL 日付 ]*</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type":"string",
  "format":"date"
}</pre>
      </td>
      <td><code>"2019-05-15"</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL DateTime]*</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type":"string",
  "format":"date-time"
}</pre>
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

**すべての日付形式の文字列は、ISO 8601 標準（[RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6)）に準拠している必要があります。*

## 他の形式への XDM タイプのマッピング

以下の節では、各 XDM タイプを他の一般的なシリアル化形式にマッピングする方法について説明します。

* [Parquet、Spark SQL、Java](#parquet)
* [Scala、.NET、CosmosDB](#scala)
* [MongoDB、Aerospike、および Protobuf 2](#mongo)

>[!IMPORTANT]
>
>以下の表に示す標準の XDM タイプの中で、[!UICONTROL Map] タイプも含まれます。 マップは、特定の値にマッピングされるキーとしてデータが表される場合や、キーを静的スキーマに合理的に含めることができず、データ値として扱う必要がある場合に、標準スキーマで使用されます。
>
>マップタイプのフィールドは、業界およびベンダースキーマの使用のために予約されているので、定義したカスタムリソースでは使用できません。 以下の表に示すマップタイプの組み込みは、既存のデータが次のいずれかの形式で格納されている場合に、そのデータを XDM にマッピングする方法を判断するのに役立ちます。

### Parquet、Spark SQL、Java {#parquet}

| XDM タイプ | Parquet | Spark SQL | Java |
| --- | --- | --- | --- |
| [!UICONTROL 文字列] | 型：`BYTE_ARRAY`<br> 注釈：`UTF8` | `StringType` | `java.lang.String` |
| [!UICONTROL Double] | 型：`DOUBLE` | `LongType` | `java.lang.Double` |
| [!UICONTROL Long] | 型：`INT64` | `LongType` | `java.lang.Long` |
| [!UICONTROL 整数] | 型：`INT32`<br> 注釈：`INT_32` | `IntegerType` | `java.lang.Integer` |
| [!UICONTROL Short] | 型：`INT32`<br> 注釈：`INT_16` | `ShortType` | `java.lang.Short` |
| [!UICONTROL Byte] | 型：`INT32`<br> 注釈：`INT_8` | `ByteType` | `java.lang.Short` |
| [!UICONTROL 日付] | 型：`INT32`<br> 注釈：`DATE` | `DateType` | `java.util.Date` |
| [!UICONTROL DateTime] | 型：`INT64`<br> 注釈：`TIMESTAMP_MILLIS` | `TimestampType` | `java.util.Date` |
| [!UICONTROL ブール型] | 型：`BOOLEAN` | `BooleanType` | `java.lang.Boolean` |
| [!UICONTROL マップ] | `MAP`-annotated group<br><br>(`<key-type>` 必ず `STRING`) | `MapType`<br><br>( 必`keyType` 須 `StringType`) | `java.util.Map` |

{style=&quot;table-layout:auto&quot;}

### Scala、.NET、CosmosDB {#scala}

| XDM タイプ | Scala | .NET | CosmosDB |
| --- | --- | --- | --- |
| [!UICONTROL 文字列] | `String` | `System.String` | `String` |
| [!UICONTROL ダブル] | `Double` | `System.Double` | `Number` |
| [!UICONTROL Long] | `Long` | `System.Int64` | `Number` |
| [!UICONTROL 整数] | `Int` | `System.Int32` | `Number` |
| [!UICONTROL Short] | `Short` | `System.Int16` | `Number` |
| [!UICONTROL バイト] | `Byte` | `System.SByte` | `Number` |
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
| [!UICONTROL バイト] | `int` | `Integer` | `int32` |
| [!UICONTROL 日付] | `date` | `Integer`<br>（UNIX ミリ秒） | `int64`<br>（UNIX ミリ秒） |
| [!UICONTROL DateTime] | `timestamp` | `Integer`<br>（UNIX ミリ秒） | `int64`<br>（UNIX ミリ秒） |
| [!UICONTROL ブール型] | `bool` | `Integer`<br>（0/1 バイナリ） | `bool` |
| [!UICONTROL マップ] | `object` | `map` | `map<key_type, value_type>` |

{style=&quot;table-layout:auto&quot;}

## API での XDM フィールドタイプの定義 {#define-fields}

すべての XDM フィールドは、フィールドタイプに適用される標準の [JSON スキーマ ](https://json-schema.org/) 制約を使用して定義され、[!DNL Experience Platform] によって適用されるフィールド名に対する追加の制約が適用されます。 スキーマレジストリ API を使用すると、形式とオプションの制約を使用して、追加のフィールドの種類を定義できます。 XDM フィールドタイプは、フィールドレベルの属性 `meta:xdmType` で公開されます。

>[!NOTE]
>
>`meta:xdmType` はシステムで生成される値なので、API を使用する際に、フィールドの JSON にこのプロパティを追加する必要はありません。ベストプラクティスは、JSON スキーマタイプ（`string` や `integer` など）を、次の表で定義されている適切な最小/最大制約と共に使用することです。

次の表に、各種のフィールドタイプを定義する適切な書式の概要を示します。オプションのプロパティを含むフィールドタイプも同様です。 オプションのプロパティとタイプ固有のキーワードに関する詳細については、[JSON スキーマのドキュメント](https://json-schema.org/understanding-json-schema/reference/type.html)を参照してください。

まず、目的のフィールドタイプを探し、提供されたサンプルコードを使用して、[ フィールドグループ ](../api/field-groups.md#create) または [ データ型 ](../api/data-types.md#create) を作成するための API リクエストを作成します。

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
    <td>制約付き列挙値は <code>enum</code> 配列の下に提供されますが、各値のオプションの顧客向けラベルは <code>meta:enum</code> の下に提供できます。
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
    <td>[!UICONTROL 整数 ]</td>
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
    <td>[!UICONTROL 短 ]</td>
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
"sampleField":{
  "type":"string",
  "format":"date-time",
  "examples":["2004-10-23T12:00:00-06:00"]
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL 配列 ]</td>
    <td></td>
    <td>基本的なスカラー型の配列（例：文字列）:
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "array",
          "items": {
            "type": "string"
  }
}</pre>
      別のスキーマで定義されたオブジェクトの配列：<br/>
      <pre class="JSON language-JSON hljs">
"sampleField":{
  "type":"array",
  "items":{
    "$ref":"https://ns.adobe.com/xdm/data/paymentitem"
  }
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL オブジェクト ]</td>
    <td></td>
    <td><code>properties</code> の下に定義された各サブフィールドの <code>type</code> 属性は、任意のスカラー型を使用して定義できます。
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
      オブジェクトタイプのフィールドは、データ型の <code>$id</code> を参照することで定義できます。
      <pre class="JSON language-JSON hljs">
"sampleField":{
  "type":"object",
  "$ref":"https://ns.adobe.com/xdm/common/phoneinteraction"
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL マップ ]</td>
    <td></td>
    <td>マップ <strong> でプロパティを定義することはできません </strong>。 この <strong> は、マップ内に含まれる値のタイプを記述する単一の <code>additionalProperties</code> スキーマを定義する必要があります（各マップには 1 つのデータタイプのみを含めることができます）。 </strong>値には、任意の有効な XDM <code>type</code> 属性、または <code>$ref</code> 属性を使用した別のスキーマへの参照を指定できます。<br/><br/>文字列型の値を持つ map フィールド：
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
    別のデータ型を参照するマップフィールド：
      <pre class="JSON language-JSON hljs">
"sampleField":{
  "type":"object",
  "additionalProperties":{
    "$ref":"https://ns.adobe.com/xdm/data/paymentitem"
  }
}</pre>
    </td>
  </tr>
</table>
