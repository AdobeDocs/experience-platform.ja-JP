---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ；ミックスイン；ミックスイン；ミックスイン；データ型；データ型；データ型；データ型；スキーマ型；データ型；データ型；スキーマ;スキーマ;スキーマ設計；マップ；
solution: Experience Platform
title: XDM Field Typeの制約
topic: 概要
description: Experience Data Model(XDM)のフィールドタイプ制約のリファレンスです。マッピングできるその他のシリアル化形式や、APIで独自のフィールドタイプを定義する方法が含まれます。
translation-type: tm+mt
source-git-commit: bb5880340ca4c01d0b25c7cb16fd422d3182a89e
workflow-type: tm+mt
source-wordcount: '1080'
ht-degree: 18%

---


# XDMフィールド型の制約

Experience Data Model(XDM)スキーマでは、フィールドに格納できるデータの種類をフィールドの種類に制限します。 このドキュメントでは、各コアフィールドタイプの概要を示します。この概要には、マッピングできる他のシリアル化形式や、異なる制約を適用するためにAPIで独自のフィールドタイプを定義する方法が含まれます。

## はじめに

このガイドを使用する前に、XDMスキーマ、クラス、ミックスインの紹介について、[スキーマ構成の基本事項](./composition.md)を参照してください。

APIで独自のフィールドの種類を定義する場合は、カスタムフィールドを含めるミックスインとデータ型の作成方法について、『[スキーマレジストリ開発者ガイド](../api/getting-started.md)』に開始することを強くお勧めします。 Experience PlatformUIを使用してスキーマを作成する場合は、[UI](../ui/fields/overview.md)でのフィールドの定義のガイドを参照して、カスタムミックスインとデータ型内で定義するフィールドに制約を適用する方法を確認してください。

## 基本構造と例

XDMはJSONスキーマの上に構築されているので、XDMフィールドの型を定義する場合も同じ構文を継承します。 JSONスキーマでの各フィールドタイプの表現方法を理解することは、各タイプの基本制約を示すのに役立ちます。

>[!NOTE]
>
>Platform APIのJSONスキーマとその他の基盤となるテクノロジーについて詳しくは、[APIの基本的なガイド](../../landing/api-fundamentals.md#json-schema)を参照してください。

次の表に、各XDM型がJSONスキーマでどのように表されるか、および型に準拠する値の例を示します。

<table>
  <thead>
    <tr>
      <th>XDM タイプ</th>
      <th>JSON スキーマ</th>
      <th>例</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[!UICONTROL文字列]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{"type":"string"}</pre>
      </td>
      <td><code>"Platinum"</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL重複]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{"type":"number"}</pre>
      </td>
      <td><code>12925.49</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL長]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type":"integer",
  "maximum":9007199254740991,
  "最小値":-9007199254740991
}</pre>
      </td>
      <td><code>1478108935</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL整数]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type":"integer",
  "maximum":2147483648,
  "最小値":-2147483648
}</pre>
      </td>
      <td><code>24906290</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL Short]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type":"integer",
  "maximum":32768,
  "最小値":-32768
}</pre>
      </td>
      <td><code>15781</code></td>
    </tr>
    <tr>
      <td>[!UICONTROLバイト]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type":"integer",
  "maximum":128,
  "最小値":-128
}</pre>
      </td>
      <td><code>90</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL日付]*</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type":"string",
  "format":"日付"
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
  "format":"日付時間"
}</pre>
      </td>
      <td><code>"2019-05-15T20:20:39+00:00"</code></td>
    </tr>
    <tr>
      <td>[!UICONTROLブール値]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{"type":"string"}</pre>
      </td>
      <td><code>true</code></td>
    </tr>
  </tbody>
</table>

**すべての日付形式の文字列は、ISO 8601標準（[RFC 3339、セクション5.6](https://tools.ietf.org/html/rfc3339#section-5.6)）に準拠している必要があります。*

## 他の形式への XDM タイプのマッピング

以下の節では、各XDMタイプと他の一般的なシリアル化形式との対応を説明します。

* [Parket、Spark SQL、およびJava](#parquet)
* [Scala、.NET、CosmosDB](#scala)
* [MongoDB、Aerospike、Protobuf 2](#mongo)

>[!IMPORTANT]
>
>以下の表に示す標準的なXDM型の中には、[!UICONTROL Map]型も含まれています。 マップは、特定の値にマップするキーとしてデータが表される場合、または静的なスキーマにキーを適切に含めることができず、データ値として扱う必要がある場合に、標準スキーマで使用されます。
>
>マップタイプフィールドは、業界やベンダーのスキーマ使用のために予約されているので、ユーザーが定義したカスタムリソースでは使用できません。 次の表に示すマップタイプの組み込みは、既存のデータが現在、以下に示す形式のいずれかで保存されている場合に、そのデータをXDMにマップする方法を判断するのに役立つものです。

### Parket、Spark SQL、およびJava {#parquet}

| XDM タイプ | パーケ | Spark SQL | Java |
| --- | --- | --- | --- |
| [!UICONTROL String] | タイプ：`BYTE_ARRAY`<br>注釈：`UTF8` | `StringType` | `java.lang.String` |
| [!UICONTROL Double] | 型：`DOUBLE` | `LongType` | `java.lang.Double` |
| [!UICONTROL Long] | 型：`INT64` | `LongType` | `java.lang.Long` |
| [!UICONTROL 整数] | タイプ：`INT32`<br>注釈：`INT_32` | `IntegerType` | `java.lang.Integer` |
| [!UICONTROL Short] | タイプ：`INT32`<br>注釈：`INT_16` | `ShortType` | `java.lang.Short` |
| [!UICONTROL Byte] | タイプ：`INT32`<br>注釈：`INT_8` | `ByteType` | `java.lang.Short` |
| [!UICONTROL 日付] | タイプ：`INT32`<br>注釈：`DATE` | `DateType` | `java.util.Date` |
| [!UICONTROL DateTime] | タイプ：`INT64`<br>注釈：`TIMESTAMP_MILLIS` | `TimestampType` | `java.util.Date` |
| [!UICONTROL ブール型] | 型：`BOOLEAN` | `BooleanType` | `java.lang.Boolean` |
| [!UICONTROL マップ] | `MAP`-annotated group<br><br>(`<key-type>` 必須 `STRING`) | `MapType`<br><br>(`keyType` 必須 `StringType`) | `java.util.Map` |

### Scala、.NET、CosmosDB {#scala}

| XDM タイプ | Scala | .NET | CosmosDB |
| --- | --- | --- | --- |
| [!UICONTROL 文字列] | `String` | `System.String` | `String` |
| [!UICONTROL 重複] | `Double` | `System.Double` | `Number` |
| [!UICONTROL ロング] | `Long` | `System.Int64` | `Number` |
| [!UICONTROL 整数] | `Int` | `System.Int32` | `Number` |
| [!UICONTROL Short] | `Short` | `System.Int16` | `Number` |
| [!UICONTROL バイト] | `Byte` | `System.SByte` | `Number` |
| [!UICONTROL 日付] | `java.util.Date` | `System.DateTime` | `String` |
| [!UICONTROL DateTime] | `java.util.Date` | `System.DateTime` | `String` |
| [!UICONTROL ブール型] | `Boolean` | `System.Boolean` | `Boolean` |
| [!UICONTROL マップ] | `Map` | (なし) | `object` |

### MongoDB、Aerospike、Protobuf 2 {#mongo}

| XDM タイプ | MongoDB | Aerospike | Protobuf 2 |
| --- | --- | --- | --- |
| [!UICONTROL 文字列] | `string` | `String` | `string` |
| [!UICONTROL 重複] | `double` | `Double` | `double` |
| [!UICONTROL ロング] | `long` | `Integer` | `int64` |
| [!UICONTROL 整数] | `int` | `Integer` | `int32` |
| [!UICONTROL Short] | `int` | `Integer` | `int32` |
| [!UICONTROL バイト] | `int` | `Integer` | `int32` |
| [!UICONTROL 日付] | `date` | `Integer`<br>（UNIXミリ秒） | `int64`<br>（UNIXミリ秒） |
| [!UICONTROL DateTime] | `timestamp` | `Integer`<br>（UNIXミリ秒） | `int64`<br>（UNIXミリ秒） |
| [!UICONTROL ブール型] | `bool` | `Integer`<br>（0/1バイナリ） | `bool` |
| [!UICONTROL マップ] | `object` | `map` | `map<key_type, value_type>` |

## API での XDM フィールドタイプの定義 {#define-fields}

すべてのXDMフィールドは、フィールドタイプに適用される標準の[JSONスキーマ](https://json-schema.org/)制約を使用して定義され、[!DNL Experience Platform]によって適用されるフィールド名に対する追加の制約が適用されます。 スキーマレジストリAPIを使用すると、形式とオプションの制約を使用して、追加のフィールドの種類を定義できます。 XDMのフィールド型は、フィールドレベルの属性`meta:xdmType`で公開されます。

>[!NOTE]
>
>`meta:xdmType` はシステム生成値なので、APIを使用する場合に、このプロパティをフィールドのJSONに追加する必要はありません。ベストプラクティスとして、JSONスキーマタイプ（`string`や`integer`など）を、以下の表に定義されている適切な最小/最大制約と共に使用します。

次の表に、オプションのプロパティを含む、様々なフィールドの種類を定義するための適切な形式設定の概要を示します。 オプションのプロパティとタイプ固有のキーワードに関する詳細については、[JSON スキーマのドキュメント](https://json-schema.org/understanding-json-schema/reference/type.html)を参照してください。

最初に、目的のフィールドタイプを探し、提供されたサンプルコードを使用して[mixin](../api/mixins.md#create)または[creating a data type](../api/data-types.md#create)のAPIリクエストを作成します。

<table style="table-layout:auto">
  <tr>
    <th>XDM タイプ</th>
    <th>オプションのプロパティ</th>
    <th>例</th>
  </tr>
  <tr>
    <td>[!UICONTROL文字列]</td>
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
    <td>[!UICONTROL列挙]</td>
    <td>
      <ul>
        <li><code>default</code></li>
        <li><code>meta:enum</code></li>
      </ul>
    </td>
    <td>制約付きの列挙値は<code>enum</code>配列の下に提供されますが、オプションで顧客向けの各ラベルは<code>meta:enum</code>の下に提供できます。
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
    <td>[!UICONTROL番号]</td>
    <td></td>
    <td>
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "number"
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL長]</td>
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
    <td>[!UICONTROL整数]</td>
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
    <td>[!UICONTROLバイト]</td>
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
    <td>[!UICONTROLブール値]</td>
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
    <td>[!UICONTROL日付]</td>
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
"sampleField": {
          "type": "string",
          "format": "date-time",
          "examples": ["2004-10-23T12:00:00-06:00"]
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL配列]</td>
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
    <td>[!UICONTROLオブジェクト]</td>
    <td></td>
    <td><code>properties</code>で定義された各サブフィールドの<code>type</code>属性は、次のスカラー型を使用して定義できます。
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
      オブジェクト型のフィールドは、データ型の<code>$id</code>を参照することで定義できます。
      <pre class="JSON language-JSON hljs">
"sampleField":{
  "type":"object",
  "$ref":"https://ns.adobe.com/xdm/common/phoneinteraction"
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROLマップ]</td>
    <td></td>
    <td>マップ<strong>は、プロパティを定義</strong>してはなりません。 マップ内に含まれる値のタイプを記述するには、<strong></strong>単一の<code>additionalProperties</code>スキーマを定義する必要があります（各マップには1つのデータタイプのみを含めることができます）。 値には、有効なXDM <code>type</code>属性、または<code>$ref</code>属性を使用した別のスキーマへの参照を指定できます。<br/><br/>文字列型の値を持つmapフィールド：
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "object",
          "additionalProperties":{
            "type": "string"
  }
}</pre>
    値に対する文字列の配列を含むmapフィールド：
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
    別のデータ型を参照するmapフィールド：
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
