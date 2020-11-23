---
keywords: Experience Platform;home;popular topics;schema;Schema;mixin;Mixin;Mixins;mixins;data type;data types;Data types;Data type;schema design;datatype;Datatype;data type;Data type;schemas;Schemas;Schema design;map;Map;
solution: Experience Platform
title: XDMフィールド型の制約
topic: overview
description: XDMのフィールド型制約に関するリファレンスです。マッピングできるその他のシリアル化形式や、APIで独自のフィールド型を定義する方法などが含まれます。
translation-type: tm+mt
source-git-commit: e92294b9dcea37ae2a4a398c9d3397dcf5aa9b9e
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 74%

---


# XDMフィールド型の制約

スキーマに対して選択するXDMフィールドタイプは、これらのフィールドに格納できるデータの種類を制限します。 このドキュメントでは、各コアフィールドタイプの概要を示します。この中に、マッピングできる他のシリアル化形式や、異なる制約を適用するためにAPIで独自のフィールドタイプを定義する方法を示します。

## はじめに

このガイドを使用する前に、XDMスキーマ、クラス [、ミックスインの紹介について、スキーマ構成の](./composition.md) 基本事項を確認してください。

独自のフィールドの種類を定義する場合は、『 [スキーマレジストリ開発者ガイド](../api/getting-started.md) 』に開始して、カスタムフィールドを含めるためのミックスインとデータ型の作成方法を学ぶことを強くお勧めします。

## 他の形式への XDM タイプのマッピング

The table below describes the mapping between each XDM type (`meta:xdmType`) and other serialization formats.

| XDM タイプ<br>（meta:xdmType） | JSON<br>（JSON スキーマ） | Parquet<br>（タイプ / 注釈） | [!DNL Spark] SQL | Java | Scala | .NET | CosmosDB | MongoDB | Aerospike | Protobuf 2 |
|---|---|---|---|---|---|---|---|---|---|---|
| string | type:string | BYTE_ARRAY/UTF8 | StringType | java.lang.String | String | System.String | String | string | String | string |
| number | type:number | DOUBLE | DoubleType | java.lang.Double | Double | System.Double | Number | double | Double | double |
| long | type:integer<br>maximum:2^53+1<br>minimum:-2^53+1 | INT64 | LongType | java.lang.Long | Long | System.Int64 | Number | long | Integer | int64 |
| int | type:integer<br>maximum:2^31<br>minimum:-2^31 | INT32/INT_32 | IntegerType | java.lang.Integer | Int | System.Int32 | Number | int | Integer | int32 |
| short | type:integer<br>maximum:2^15<br>minimum:-2^15 | INT32/INT_16 | ShortType | java.lang.Short | Short | System.Int16 | Number | int | Integer | int32 |
| byte | type:integer<br>maximum:2^7<br>minimum:-2^7 | INT32/INT_8 | ByteType | java.lang.Short | Byte | System.SByte | Number | int | Integer | int32 |
| boolean | type:boolean | BOOLEAN | BooleanType | java.lang.Boolean | Boolean | System.Boolean | Boolean | bool | Integer | Integer | bool |
| date | type:string<br>format:date<br>（RFC 3339、セクション 5.6） | INT32/DATE | DateType | java.util.Date | java.util.Date | System.DateTime | String | date | Integer<br>（unix ミリ秒） | int64<br>（unix ミリ秒） |
| date-time | type:string<br>format:date-time<br>（RFC 3339、セクション 5.6） | INT64/TIMESTAMP_MILLIS | TimestampType | java.util.Date | java.util.Date | System.DateTime | String | timestamp | Integer<br>（unix ミリ秒） | int64<br>（unix ミリ秒） |
| map | object | MAP annotated group<br><br>&lt;<span>key_type</span>> MUST be STRING<br><br>&lt;<span>value_type</span>> type of map values | MapType<br><br>&quot;keyType&quot; MUST be StringType<br><br>&quot;valueType&quot; is type of map values. | java.util.Map | Map | --- | object | object | map | map&lt;<span>key_type, value_type</span>> |

## API での XDM フィールドタイプの定義 {#define-fields}

XDM schemas are defined using [JSON Schema](https://json-schema.org/) standards and basic field types, with additional constraints for field names which are enforced by [!DNL Experience Platform]. [スキーマレジストリAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml) (R)を使用すると、形式やオプションの制約を使用して、追加のフィールドの種類を定義できます。 XDM field types are exposed by the field-level attribute, `meta:xdmType`.

>[!NOTE]
>
>`meta:xdmType`  はシステムで生成される値であるため、フィールドのために、このプロパティを JSON に追加する必要はありません。ベストプラクティスは、JSON スキーマタイプ（文字列や整数など）を、以下の表で定義されている適切な最小 / 最大制約と共に使用することです。

次の表に、オプションのプロパティを使用してスカラーフィールドタイプとより具体的なフィールドタイプを定義するための適切な書式の概要を示します。オプションのプロパティとタイプ固有のキーワードに関する詳細については、[JSON スキーマのドキュメント](https://json-schema.org/understanding-json-schema/reference/type.html)を参照してください。

最初に、目的のフィールドタイプを探し、提供されたサンプルコードを使用して、mixinを [作成またはデータタイプを](../api/mixins.md#create) 作成するためのAPIリクエストを作成します [](../api/data-types.md#create)。

<table>
  <tr>
    <th>目的のタイプ<br/>（meta:xdmType）</th>
    <th>JSON<br/>（JSON スキーマ）</th>
    <th>コードサンプル</th>
  </tr>
  <tr>
    <td>string</td>
    <td>type: string<br/><br/><strong>オプションのプロパティ：</strong><br/>
      <ul>
        <li>pattern</li>
        <li>minLength</li>
        <li>maxLength</li>
      </ul>
    </td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": {
            "type": "string",
            "pattern": "^[A-Z]{2}$",
            "maxLength": 2
        }
      </pre>
    </td>
  </tr>
  <tr>
    <td>uri<br/>(xdmType:string)</td>
    <td>type: string<br/>format: uri</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "string",
          "format": "uri"
        }
      </pre>
    </td>
  </tr>
  <tr>
    <td>enum<br/>(xdmType: string)</td>
    <td>type: string<br/><br/><strong>オプションのプロパティ：</strong><br/>
      <ul>
        <li>default</li>
      </ul>
    </td>
    <td>"meta:enum" を使用して、顧客に表示するオプションのラベルを指定します：
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
        }
      </pre>
    </td>
  </tr>
  <tr>
    <td>number</td>
    <td>type: number<br/>minimum: ±2.23×10^308<br/>maximum: ±1.80×10^308</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "number"
        }
      </pre>
    </td>
  </tr>
  <tr>
    <td>long</td>
    <td>type: integer<br/>maximum:2^53+1<br>minimum:-2^53+1</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "integer",
          "minimum": -9007199254740992,
          "maximum": 9007199254740992
        }
      </pre>
    </td>
  </tr>
  <tr>
    <td>int</td>
    <td>type: integer<br/>maximum:2^31<br>minimum:-2^31</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "integer",
          "minimum": -2147483648,
          "maximum": 2147483648
        }
      </pre>
    </td>
  </tr>
  <tr>
    <td>short</td>
    <td>type: integer<br/>maximum:2^15<br>minimum:-2^15</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "integer",
          "minimum": -32768,
          "maximum": 32768
        }
      </pre>
    </td>
  </tr>
  <tr>
    <td>byte</td>
    <td>type: integer<br/>maximum:2^7<br>minimum:-2^7</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "integer",
          "minimum": -128,
          "maximum": 128
          }
      </pre>
    </td>
  </tr>
  <tr>
    <td>boolean</td>
    <td><br/>type: boolean<br/>{true, false}<br/><br/><strong>オプションのプロパティ：</strong><br/>
      <ul>
        <li>default</li>
      </ul>
    </td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "boolean",
          "default": false
        }
      </pre>
    </td>
  </tr>
  <tr>
    <td>date</td>
    <td>type: string<br/>format: date</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "string",
          "format": "date",
          "examples": ["2004-10-23"]
        }
      </pre>
      <a href="https://tools.ietf.org/html/rfc3339#section-5.6" target="_blank">RFC 3339 のセクション 5.6</a> で定義されている日付。ここでは、"full-date" = date-fullyear "-" date-month "-" date-mday (YYYY-MM-DD)
    </td>
  </tr>
  <tr>
    <td>date-time</td>
    <td>type: string<br/>format: date-time</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "string",
          "format": "date-time",
          "examples": ["2004-10-23T12:00:00-06:00"]
        }
      </pre>
      <a href="https://tools.ietf.org/html/rfc3339#section-5.6" target="_blank">RFC 3339 のセクション 5.6</a> で定義されている日時。ここでは、"date-time" = full-date "T" full-time:<br/>(YYYY-MM-DD'T'HH:MM:SS.SSSSX)
    </td>
  </tr>
  <tr>
    <td>array</td>
    <td>type: array</td>
    <td>items.type は、任意のスカラー型を使用して定義できます：
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "array",
          "items": {
            "type": "string"
          }
        }
      </pre>
      別のスキーマで定義されたオブジェクトの配列：<br/>
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "array",
          "items": {
            "$ref": "id"
          }
        }
      </pre>
      ここで、"id" は参照スキーマの {id} です。
    </td>
  </tr>
  <tr>
    <td>object</td>
    <td>type: object</td>
    <td>properties.{field}.type は、任意のスカラー型を使用して定義できます：
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
        }
      </pre>
      参照スキーマで定義される type "object" のフィールド：
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "object",
          "$ref": "id"
        }
      </pre>
      ここで、"id" は参照スキーマの {id} です。
    </td>
  </tr>
  <tr>
    <td>map</td>
    <td>type: object<br/><br/><strong>注：</strong><br/>'map' データ型の使用は、業界およびベンダースキーマでの使用のために予約されており、テナント定義フィールドでは使用できません。このデータ型は、値にマッピングされるキーとしてデータが表される場合、またはキーを静的スキーマに合理的に含めることができないため、データ値として処理する必要がある場合に標準スキーマで使用されます。</td>
    <td>'map' はプロパティを定義できません。'map'に含まれる値の種類を記述するために、"[!UICONTROL additionalProperties]"スキーマを1つ定義する必要があります。 XDM の 'map' には、1 つのデータ型のみを含めることができます。値は、配列やオブジェクトなどの任意の有効な XDM スキーマ定義または別のスキーマへの参照（$ref 経由）です。<br/><br/>type 'string' の値を持つ map フィールド：
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "object",
          "additionalProperties":{
            "type": "string"
          }
        }
      </pre>
    文字列の配列を持つ map フィールド：
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "object",
          "additionalProperties":{
            "type": "array",
            "items": {
              "type": "string"
            }
          }
        }
      </pre>
    別のスキーマを参照する map フィールド：
      <pre class="JSON language-JSON hljs">
        "sampleField": {
          "type": "object",
          "additionalProperties":{
            "$ref": "id"
          }
        }
      </pre>
      ここで、"id" は参照スキーマの {id} です。
    </td>
  </tr>
</table>
