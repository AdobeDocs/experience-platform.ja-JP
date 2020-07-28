---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スキーマレジストリ開発者向けの付録
topic: developer guide
translation-type: tm+mt
source-git-commit: d04bf35e49488ab7d5e07de91eb77d0d9921b6fa
workflow-type: tm+mt
source-wordcount: '1265'
ht-degree: 87%

---


# 付録

This document provides supplemental information related to working with the [!DNL Schema Registry] API.

## 互換性モード

[!DNL Experience Data Model] （XDM）は、アドビが推進する公式に文書化された仕様であり、デジタルエクスペリエンスの相互運用性、表現力およびパワーを向上させます。アドビは、[GitHub のオープンソースプロジェクト](https://github.com/adobe/xdm/)でソースコードと公式の XDM 定義を公開しています。これらの定義は XDM 標準表記で記述され、JSON-LD （JavaScript Object Notation for Linked Data）および JSON スキーマを XDM スキーマを定義する文法として使用しています。

パブリックリポジトリーで公式の XDM 定義を見ると、標準 XDM は Adobe Experience Platform での表示とは異なることがわかります。What you are seeing in [!DNL Experience Platform] is called Compatibility Mode, and it provides a simple mapping between standard XDM and the way it is used within [!DNL Platform].

### 互換性モードの仕組み

互換性モードを使用すると、XDM JSON-LD モデルは、同じセマンティクスを保持したままで標準 XDM 内の値を変更することにより、既存のデータインフラストラクチャと連携できます。ネストされた JSON 構造を使用して、スキーマをツリーに類似した形式で表示します。

標準 XDM と互換性モードの主な違いは、フィールド名の「xdm:」プレフィックスが削除されていることです。

標準 XDM と互換性モードの誕生日関連のフィールド（&quot;description&quot; 属性が削除された）を並べて比較したものを次に示します。互換性モードのフィールドでは、&quot;meta:xdmField&quot; 属性と &quot;meta:xdmType&quot; 属性に XDM フィールドへの参照とそのデータ型が含まれていることに注意してください。

<table>
  <th>標準 XDM</th>
  <th>互換性モード</th>
  <tr>
  <td>
  <pre class="JSON language-JSON hljs">
        {
          "xdm:birthDate": {
              "title": "Birth Date",
              "type": "string",
              "format": "date",
          },
          "xdm:birthDayAndMonth": {
              "title": "Birth Date",
              "type": "string",
              "pattern": "[0-1][0-9]-[0-9][0-9]",
          },
          "xdm:birthYear": {
              "title": "Birth year",
              "type": "integer",
              "minimum": 1,
              "maximum": 32767
        }
      </pre>
  </td>
  <td>
  <pre class="JSON language-JSON hljs">
        {
          "birthDate": {
              "title": "Birth Date",
              "type": "string",
              "format": "date",
              "meta:xdmField": "xdm:birthDate",
              "meta:xdmType": "date"
          },
          "birthDayAndMonth": {
              "title": "Birth Date",
              "type": "string",
              "pattern": "[0-1][0-9]-[0-9][0-9]",
              "meta:xdmField": "xdm:birthDayAndMonth",
              "meta:xdmType": "string"
          },
          "birthYear": {
              "title": "Birth year",
              "type": "integer",
              "minimum": 1,
              "maximum": 32767,
              "meta:xdmField": "xdm:birthYear",
              "meta:xdmType": "short"
        }
      </pre>
  </td>
  </tr>
</table>

### 互換性モードが必要な理由

Adobe Experience Platform は、複数のソリューションやサービスと連携するように設計されており、各ソリューションおよびサービスには固有の技術的課題と制限（特定のテクノロジーが特殊文字を処理する方法など）があります。これらの制限を克服するために、互換モードが開発されました。

標準的なXDMの代わりに、、 [!DNL Experience Platform] 、 [!DNL Catalog]およびを含むほとんどの [!DNL Data Lake]サービスを [!DNL Real-time Customer Profile][!DNL Compatibility Mode] 使用します。 APIでも使用され [!DNL Schema Registry] ており、このドキュメントの例はすべてをを使用して示してい [!DNL Compatibility Mode][!DNL Compatibility Mode]ます。

It is worthwhile to know that a mapping takes place between standard XDM and the way it is operationalized in [!DNL Experience Platform], but it should not affect your use of [!DNL Platform] services.

The open source project is available to you, but when it comes to interacting with resources through the [!DNL Schema Registry], the API examples in this document provide the best practices you should know and follow.

## API での XDM フィールドタイプの定義 {#field-types}

XDM schemas are defined using JSON Schema standards and basic field types, with additional constraints for field names which are enforced by [!DNL Experience Platform]. XDM では、形式やオプションの制約を使用して、追加のフィールドタイプを定義できます。XDM フィールドタイプは、フィールドレベルの属性（`meta:xdmType`）で公開されます。

>[!NOTE]
>
>`meta:xdmType`  はシステムで生成される値であるため、フィールドのために、このプロパティを JSON に追加する必要はありません。ベストプラクティスは、JSON スキーマタイプ（文字列や整数など）を、以下の表で定義されている適切な最小 / 最大制約と共に使用することです。

次の表に、オプションのプロパティを使用してスカラーフィールドタイプとより具体的なフィールドタイプを定義するための適切な書式の概要を示します。オプションのプロパティとタイプ固有のキーワードに関する詳細については、[JSON スキーマのドキュメント](https://json-schema.org/understanding-json-schema/reference/type.html)を参照してください。

最初に、目的のフィールドタイプを探し、提供されたサンプルコードを使用して API リクエストを作成します。

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


## 他の形式への XDM タイプのマッピング

次の表に、&quot;meta:xdmType&quot; と他のシリアル化形式との間のマッピングを示します。

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
