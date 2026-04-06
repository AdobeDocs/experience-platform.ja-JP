---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；フィールドグループ；フィールドグループ；フィールドグループ；データタイプ；データタイプ；データタイプ；データタイプ；データタイプ；データタイプ；データタイプ；データタイプ；データタイプ；スキーマ；スキーマ；スキーマ；スキーマデザイン；マップ；マップ；
solution: Experience Platform
title: XDM フィールドタイプの制約
description: マッピング可能なその他のシリアル化形式や、APIで独自のフィールドタイプを定義する方法など、Experience Data Model （XDM）のフィールドタイプ制約のリファレンス。
exl-id: 63839a28-6d26-46f1-8bbf-b524e82ac4df
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 6%

---

# XDM フィールドタイプ制約

Experience Data Model （XDM）スキーマでは、フィールドのタイプによって、フィールドに含めることができるデータの種類が制限されます。 このドキュメントでは、マッピング可能なその他のシリアル化形式を含む各コアフィールドタイプの概要と、異なる制約を適用するためにAPIで独自のフィールドタイプを定義する方法について説明します。

## はじめに

このガイドを使用する前に、XDM スキーマ、クラス、スキーマフィールドグループの概要について、[&#x200B; スキーマ構成の基本](./composition.md)を確認してください。

APIで独自のフィールドタイプを定義する場合は、[&#x200B; スキーマレジストリ開発者ガイド &#x200B;](../api/getting-started.md)から始めて、カスタムフィールドを含めるフィールドグループとデータタイプを作成する方法を学ぶことを強くお勧めします。 Experience Platform UIを使用してスキーマを作成する場合は、[UIでのフィールドの定義](../ui/fields/overview.md)に関するガイドを参照して、カスタムフィールドグループおよびデータタイプ内で定義したフィールドに対する制約を実装する方法を説明します。

## ベース構造と例 {#basic-types}

XDMはJSON スキーマ上に構築されているため、XDM フィールドはタイプを定義する際に同様の構文を継承します。 さまざまなフィールドタイプがJSON スキーマでどのように表現されるかを理解すると、各タイプの基本制約を示すのに役立ちます。 カスタムフィールド名では大文字と小文字が区別されないため、スキーマ内の同じレベルに異なる名前を付ける必要があります。

>[!NOTE]
>
>Experience Platform APIのJSON スキーマおよびその他の基盤テクノロジーについて詳しくは、[API基本ガイド &#x200B;](../../landing/api-fundamentals.md#json-schema)を参照してください。

次の表は、各XDM タイプをJSON スキーマで表す方法と、そのタイプに準拠する値の例の概要を示しています。

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
      <td>[!UICONTROL String]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{"type": "string"}</pre>
      </td>
      <td><code>"Platinum"</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL Number]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{"type": "number"}</pre>
      </td>
      <td><code>12925.49</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL Long]</td>
      <td>
        <pre class="JSON language-JSON hljs">
&lbrace;
  "type": "integer",
  "maximum": 9007199254740991,
  "minimum": -9007199254740991
&rbrace;</pre>
      </td>
      <td><code>1478108935</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL Integer]</td>
      <td>
        <pre class="JSON language-JSON hljs">
&lbrace;
  "type": "integer",
  "maximum": 2147483648,
  "minimum": -2147483648
&rbrace;</pre>
      </td>
      <td><code>24906290</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL Short]</td>
      <td>
        <pre class="JSON language-JSON hljs">
&lbrace;
  "type": "integer",
  "maximum": 32767,
  "minimum": -32768
&rbrace;</pre>
      </td>
      <td><code>15781</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL Byte]</td>
      <td>
        <pre class="JSON language-JSON hljs">
&lbrace;
  "type": "integer",
  "maximum": 128,
  「最小」: -128
&rbrace;</pre>
      </td>
      <td><code>90</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL Date]*</td>
      <td>
        <pre class="JSON language-JSON hljs">
&lbrace;
  "type": "string",
  "format": "date"
&rbrace;</pre>
      </td>
      <td><code>"2019-05-15"</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL DateTime]*</td>
      <td>
        <pre class="JSON language-JSON hljs">
&lbrace;
  "type": "string",
  "format": "date-time"
&rbrace;</pre>
      </td>
      <td><code>"2019-05-15T20:20:39+00:00"</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL Boolean]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{"type": "boolean"}</pre>
      </td>
      <td><code>true</code></td>
    </tr>
  </tbody>
</table>

**日付でフォーマットされたすべての文字列は、ISO 8601規格（[RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6)）に準拠している必要があります。*

## 他の形式への XDM タイプのマッピング

以下の節では、各XDM タイプを他の一般的なシリアル化形式にマッピングする方法について説明します。

* [Parquet、Spark SQL、Java](#parquet)
* [Scala、.NET、およびCosmosDB](#scala)
* [MongoDB、Aerospike、およびProtobuf 2](#mongo)

>[!NOTE]
>
>以下の表に記載されている標準XDM タイプの中には、[!UICONTROL Map] タイプも含まれています。 マップは、データが特定の値にマッピングするキーとして表される場合、またはキーが静的スキーマに合理的に含まれず、データ値として扱われる必要がある場合に、標準スキーマで使用されます。
>
>多くの標準XDM コンポーネントはマップタイプを使用しており、必要に応じて[&#x200B; カスタムマップフィールドを定義することもできます](../tutorials/custom-fields-api.md#custom-maps)。 以下の表に含まれるマップタイプは、既存のデータが以下のいずれかの形式で現在保存されている場合に、XDMにマッピングする方法を決定するのに役立ちます。

### Parquet、Spark SQL、Java {#parquet}

| XDM タイプ | PARQUET | Spark SQL | Java |
| --- | --- | --- | --- |
| [!UICONTROL String] | 種類：`BYTE_ARRAY`<br>注釈：`UTF8` | `StringType` | `java.lang.String` |
| [!UICONTROL Number] | 型：`DOUBLE` | `LongType` | `java.lang.Double` |
| [!UICONTROL Long] | 型：`INT64` | `LongType` | `java.lang.Long` |
| [!UICONTROL Integer] | 種類：`INT32`<br>注釈：`INT_32` | `IntegerType` | `java.lang.Integer` |
| [!UICONTROL Short] | 種類：`INT32`<br>注釈：`INT_16` | `ShortType` | `java.lang.Short` |
| [!UICONTROL Byte] | 種類：`INT32`<br>注釈：`INT_8` | `ByteType` | `java.lang.Short` |
| [!UICONTROL Date] | 種類：`INT32`<br>注釈：`DATE` | `DateType` | `java.util.Date` |
| [!UICONTROL DateTime] | 種類：`INT64`<br>注釈：`TIMESTAMP_MILLIS` | `TimestampType` | `java.util.Date` |
| [!UICONTROL Boolean] | 型：`BOOLEAN` | `BooleanType` | `java.lang.Boolean` |
| [!UICONTROL Map] | `MAP`注釈付きグループ <br><br> （`<key-type>`は`STRING`である必要があります） | `MapType`<br><br> （`keyType`は`StringType`である必要があります） | `java.util.Map` |

{style="table-layout:auto"}

### Scala、.NET、およびCosmosDB {#scala}

| XDM タイプ | Scala | .NET | CosmosDB |
| --- | --- | --- | --- |
| [!UICONTROL String] | `String` | `System.String` | `String` |
| [!UICONTROL Number] | `Double` | `System.Double` | `Number` |
| [!UICONTROL Long] | `Long` | `System.Int64` | `Number` |
| [!UICONTROL Integer] | `Int` | `System.Int32` | `Number` |
| [!UICONTROL Short] | `Short` | `System.Int16` | `Number` |
| [!UICONTROL Byte] | `Byte` | `System.SByte` | `Number` |
| [!UICONTROL Date] | `java.util.Date` | `System.DateTime` | `String` |
| [!UICONTROL DateTime] | `java.util.Date` | `System.DateTime` | `String` |
| [!UICONTROL Boolean] | `Boolean` | `System.Boolean` | `Boolean` |
| [!UICONTROL Map] | `Map` | （N/A） | `object` |

{style="table-layout:auto"}

### MongoDB、Aerospike、およびProtobuf 2 {#mongo}

| XDM タイプ | MongoDB | Aerospike | Protobuf 2 |
| --- | --- | --- | --- |
| [!UICONTROL String] | `string` | `String` | `string` |
| [!UICONTROL Number] | `double` | `Double` | `double` |
| [!UICONTROL Long] | `long` | `Integer` | `int64` |
| [!UICONTROL Integer] | `int` | `Integer` | `int32` |
| [!UICONTROL Short] | `int` | `Integer` | `int32` |
| [!UICONTROL Byte] | `int` | `Integer` | `int32` |
| [!UICONTROL Date] | `date` | `Integer`<br> （Unix ミリ秒） | `int64`<br> （Unix ミリ秒） |
| [!UICONTROL DateTime] | `timestamp` | `Integer`<br> （Unix ミリ秒） | `int64`<br> （Unix ミリ秒） |
| [!UICONTROL Boolean] | `bool` | `Integer`<br> （0/1 バイナリ） | `bool` |
| [!UICONTROL Map] | `object` | `map` | `map<key_type, value_type>` |

{style="table-layout:auto"}

## APIでのXDM フィールドタイプの定義 {#define-fields}

Schema Registry APIでは、形式とオプションの制約を使用してカスタムフィールドを定義できます。 詳しくは、[Schema Registry API](../tutorials/custom-fields-api.md)でのカスタムフィールドの定義に関するガイドを参照してください。
