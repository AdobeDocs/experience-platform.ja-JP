---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；フィールドグループ；フィールドグループ；フィールドグループ；データタイプ；データタイプ；データタイプ；データタイプ；スキーマデザイン；データタイプ；データタイプ；データタイプ；スキーマ；スキーマデザイン；マップ；
solution: Experience Platform
title: XDM フィールドタイプ制約
description: マッピング可能なその他のシリアル化形式や、API で独自のフィールドタイプを定義する方法など、エクスペリエンスデータモデル（XDM）のフィールドタイプ制約のリファレンスです。
exl-id: 63839a28-6d26-46f1-8bbf-b524e82ac4df
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 7%

---

# XDM フィールドタイプ制約

エクスペリエンスデータモデル（XDM）スキーマでは、フィールドのタイプによって、フィールドに含めることができるデータの種類が制限されます。 このドキュメントでは、マッピング可能なその他のシリアル化形式や、様々な制約を適用するために API で独自のフィールドタイプを定義する方法など、各コアフィールドタイプの概要を説明します。

## はじめに

このガイドを使用する前に、[ スキーマ構成の基本 ](./composition.md) を参照して、XDM スキーマ、クラス、スキーマフィールドグループの概要を確認してください。

API で独自のフィールドタイプを定義する予定がある場合は、[ スキーマレジストリ開発者ガイド ](../api/getting-started.md) を参照して、フィールドグループとデータタイプを作成し、カスタムフィールドを含める方法を確認することを強くお勧めします。 Experience Platform UI を使用してスキーマを作成する場合は、[UI でのフィールドの定義 ](../ui/fields/overview.md) に関するガイドを参照して、カスタムフィールドグループおよびデータタイプ内で定義するフィールドに対する制約の実装方法を確認してください。

## 基本構造と例 {#basic-types}

XDM は JSON スキーマを基に構築されているので、XDM フィールドはタイプを定義する際に同様の構文を継承します。 JSON スキーマで様々なフィールドタイプがどのように表現されるかを理解することは、各タイプのベース制約を示すのに役立ちます。 カスタムフィールド名では、大文字と小文字が区別されず、スキーマの同じレベルで異なる名前を持つ必要があります。

>[!NOTE]
>
>JSON スキーマおよびExperience Platform API のその他の基盤となるテクノロジーについて詳しくは、[API の基本ガイド ](../../landing/api-fundamentals.md#json-schema) を参照してください。

次の表に、各 XDM タイプが JSON スキーマでどのように表現されるかを、タイプに準拠する値の例とともに示します。

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
{"type": "string"}</pre>
      </td>
      <td><code>"Platinum"</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL 番号 ]</td>
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
{
  "type": "integer",
  「maximum」:9007199254740991、
  "minimum": -9007199254740991
}</pre>
      </td>
      <td><code>1478108935</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL 整数 ]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type": "integer",
  「maximum」:2147483648、
  "minimum": -2147483648
}</pre>
      </td>
      <td><code>24906290</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL 短 ]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type": "integer",
  「maximum」:32768、
  "minimum": -32768
}</pre>
      </td>
      <td><code>15781</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL バイト ]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type": "integer",
  「maximum」:128,
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
  "type": "string",
  "format": "date"
}</pre>
      </td>
      <td><code>"2019-05-15"</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL 日時 ]*</td>
      <td>
        <pre class="JSON language-JSON hljs">
{
  "type": "string",
  "format": "date-time"
}</pre>
      </td>
      <td><code>"2019-05-15T20:20:39+00:00"</code></td>
    </tr>
    <tr>
      <td>[!UICONTROL ブール値 ]</td>
      <td>
        <pre class="JSON language-JSON hljs">
{"type": "boolean"}</pre>
      </td>
      <td><code>true</code></td>
    </tr>
  </tbody>
</table>

**すべての日付形式の文字列は、ISO 8601 規格（[RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6)）に準拠している必要があります。*

## 他の形式への XDM タイプのマッピング

以下の節では、各 XDM タイプがその他の一般的なシリアル化形式にどのようにマッピングされるかを説明します。

* [Parquet、Spark SQL および Java](#parquet)
* [Scala、.NET、CosmosDB](#scala)
* [MongoDB、Aerospike、Protobuf 2](#mongo)

>[!NOTE]
>
>以下の表に示す標準の XDM タイプの中に、[!UICONTROL Map] タイプも含まれています。 マップは、データが特定の値にマッピングされるキーとして表される場合、またはキーが静的スキーマに合理的に含まれず、データ値として扱われる必要がある場合に、標準スキーマで使用されます。
>
>多くの標準 XDM コンポーネントはマップタイプを使用し、必要に応じて [ カスタムマップフィールドを定義 ](../tutorials/custom-fields-api.md#custom-maps) することもできます。 次の表に含まれるマップタイプは、既存のデータが以下に示すいずれかの形式に現在保存されている場合、そのデータを XDM にマッピングする方法を決定する際に役立ちます。

### Parquet、Spark SQL および Java {#parquet}

| XDM タイプ | PARQUET | Spark SQL | Java |
| --- | --- | --- | --- |
| [!UICONTROL 文字列] | 型：`BYTE_ARRAY`<br> 注釈：`UTF8` | `StringType` | `java.lang.String` |
| [!UICONTROL  数値 ] | 型：`DOUBLE` | `LongType` | `java.lang.Double` |
| [!UICONTROL Long] | 型：`INT64` | `LongType` | `java.lang.Long` |
| [!UICONTROL  整数 ] | 型：`INT32`<br> 注釈：`INT_32` | `IntegerType` | `java.lang.Integer` |
| [!UICONTROL  短い ] | 型：`INT32`<br> 注釈：`INT_16` | `ShortType` | `java.lang.Short` |
| [!UICONTROL  バイト ] | 型：`INT32`<br> 注釈：`INT_8` | `ByteType` | `java.lang.Short` |
| [!UICONTROL 日付] | 型：`INT32`<br> 注釈：`DATE` | `DateType` | `java.util.Date` |
| [!UICONTROL  日時 ] | 型：`INT64`<br> 注釈：`TIMESTAMP_MILLIS` | `TimestampType` | `java.util.Date` |
| [!UICONTROL  ブール値 ] | 型：`BOOLEAN` | `BooleanType` | `java.lang.Boolean` |
| [!UICONTROL マップ] | `MAP` 注釈グループ <br><br> （`<key-type>` は `STRING` にする必要があります） | `MapType`<br><br> （`keyType` は `StringType` でなければなりません） | `java.util.Map` |

{style="table-layout:auto"}

### Scala、.NET、CosmosDB {#scala}

| XDM タイプ | Scala | .NET | CosmosDB |
| --- | --- | --- | --- |
| [!UICONTROL 文字列] | `String` | `System.String` | `String` |
| [!UICONTROL  数値 ] | `Double` | `System.Double` | `Number` |
| [!UICONTROL Long] | `Long` | `System.Int64` | `Number` |
| [!UICONTROL  整数 ] | `Int` | `System.Int32` | `Number` |
| [!UICONTROL  短い ] | `Short` | `System.Int16` | `Number` |
| [!UICONTROL  バイト ] | `Byte` | `System.SByte` | `Number` |
| [!UICONTROL 日付] | `java.util.Date` | `System.DateTime` | `String` |
| [!UICONTROL  日時 ] | `java.util.Date` | `System.DateTime` | `String` |
| [!UICONTROL  ブール値 ] | `Boolean` | `System.Boolean` | `Boolean` |
| [!UICONTROL マップ] | `Map` | （N/A） | `object` |

{style="table-layout:auto"}

### MongoDB、Aerospike、Protobuf 2 {#mongo}

| XDM タイプ | MongoDB | Aerospike | Protobuf 2 |
| --- | --- | --- | --- |
| [!UICONTROL 文字列] | `string` | `String` | `string` |
| [!UICONTROL  数値 ] | `double` | `Double` | `double` |
| [!UICONTROL Long] | `long` | `Integer` | `int64` |
| [!UICONTROL  整数 ] | `int` | `Integer` | `int32` |
| [!UICONTROL  短い ] | `int` | `Integer` | `int32` |
| [!UICONTROL  バイト ] | `int` | `Integer` | `int32` |
| [!UICONTROL 日付] | `date` | `Integer`<br> （Unix ミリ秒） | `int64`<br> （Unix ミリ秒） |
| [!UICONTROL  日時 ] | `timestamp` | `Integer`<br> （Unix ミリ秒） | `int64`<br> （Unix ミリ秒） |
| [!UICONTROL  ブール値 ] | `bool` | `Integer`<br> （0/1 バイナリ） | `bool` |
| [!UICONTROL マップ] | `object` | `map` | `map<key_type, value_type>` |

{style="table-layout:auto"}

## API での XDM フィールドタイプの定義 {#define-fields}

Schema Registry API では、形式とオプションの制約を使用して、カスタムフィールドを定義できます。 詳しくは、[ スキーマレジストリ API でのカスタムフィールドの定義 ](../tutorials/custom-fields-api.md) に関するガイドを参照してください。
