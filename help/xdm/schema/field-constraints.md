---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；フィールドグループ；フィールドグループ；フィールドグループ；フィールドグループ；データ型；データ型；データ型；データ型；データ型；データ型；データ型；スキーマ；スキーマ；スキーマ；スキーマデザイン；マップ；
solution: Experience Platform
title: XDM フィールドタイプ制約
topic-legacy: overview
description: Experience Data Model(XDM) のフィールドタイプ制約の参照です。これには、マッピングできる他のシリアル化形式や、API で独自のフィールドタイプを定義する方法が含まれます。
exl-id: 63839a28-6d26-46f1-8bbf-b524e82ac4df
source-git-commit: 2a58236031834bbe298576e2fcab54b04ec16ac3
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 13%

---

# XDM フィールドタイプ制約

Experience Data Model(XDM) スキーマでは、フィールドのタイプによって、フィールドに格納できるデータの種類が制限されます。 このドキュメントでは、各コアフィールドタイプの概要を示します。マッピングできるその他のシリアル化形式や、異なる制約を実施するために API で独自のフィールドタイプを定義する方法も示します。

## はじめに

このガイドを使用する前に、 [スキーマ構成の基本](./composition.md) XDM スキーマ、クラス、スキーマフィールドグループの紹介。

API で独自のフィールドタイプを定義する予定がある場合は、まず [スキーマレジストリ開発者ガイド](../api/getting-started.md) を参照して、カスタムフィールドを含めるフィールドグループとデータ型を作成する方法を確認してください。 Experience PlatformUI を使用してスキーマを作成する場合は、 [UI でのフィールドの定義](../ui/fields/overview.md) を参照して、カスタムフィールドグループおよびデータ型内で定義するフィールドに制約を実装する方法を確認してください。

## 基本構造と例 {#basic-types}

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

| XDM タイプ | PARQUET | Spark SQL | Java |
| --- | --- | --- | --- |
| [!UICONTROL 文字列] | タイプ： `BYTE_ARRAY`<br>注釈： `UTF8` | `StringType` | `java.lang.String` |
| [!UICONTROL Double] | 型：`DOUBLE` | `LongType` | `java.lang.Double` |
| [!UICONTROL Long] | 型：`INT64` | `LongType` | `java.lang.Long` |
| [!UICONTROL 整数] | タイプ： `INT32`<br>注釈： `INT_32` | `IntegerType` | `java.lang.Integer` |
| [!UICONTROL Short] | タイプ： `INT32`<br>注釈： `INT_16` | `ShortType` | `java.lang.Short` |
| [!UICONTROL Byte] | タイプ： `INT32`<br>注釈： `INT_8` | `ByteType` | `java.lang.Short` |
| [!UICONTROL 日付] | タイプ： `INT32`<br>注釈： `DATE` | `DateType` | `java.util.Date` |
| [!UICONTROL 日時] | タイプ： `INT64`<br>注釈： `TIMESTAMP_MILLIS` | `TimestampType` | `java.util.Date` |
| [!UICONTROL ブール値] | 型：`BOOLEAN` | `BooleanType` | `java.lang.Boolean` |
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
| [!UICONTROL 日時] | `java.util.Date` | `System.DateTime` | `String` |
| [!UICONTROL ブール値] | `Boolean` | `System.Boolean` | `Boolean` |
| [!UICONTROL マップ] | `Map` | (N/A) | `object` |

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
| [!UICONTROL 日時] | `timestamp` | `Integer`<br>（Unix ミリ秒） | `int64`<br>（Unix ミリ秒） |
| [!UICONTROL ブール値] | `bool` | `Integer`<br>（0/1 バイナリ） | `bool` |
| [!UICONTROL マップ] | `object` | `map` | `map<key_type, value_type>` |

{style=&quot;table-layout:auto&quot;}

## API での XDM フィールドタイプの定義 {#define-fields}

スキーマレジストリ API を使用すると、形式とオプションの制約を使用してカスタムフィールドを定義できます。 詳しくは、 [スキーマレジストリ API でのカスタムフィールドの定義](../tutorials/custom-fields-api.md) を参照してください。
