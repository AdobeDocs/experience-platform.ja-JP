---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スキーマレジストリ開発者付録
topic: developer guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '1296'
ht-degree: 4%

---


# 付録

このドキュメントでは、スキーマレジストリAPIの操作に関する補足情報を提供します。

## 互換モード

エクスペリエンスデータモデル(XDM)は、アドビが推進する公式に文書化された仕様で、デジタルエクスペリエンスの相互運用性、表現力、パワーを向上させます。 アドビは、GitHub上の [オープンソースプロジェクトにソースコードと正式なXDM定義を保持しています](https://github.com/adobe/xdm/)。 これらの定義はXDM標準表記で記述され、JSON-LD（リンクデータのJavaScript Object Notation）とJSONスキーマをXDMスキーマを定義する文法として使用します。

パブリックリポジトリで正式なXDM定義を見ると、標準のXDMはAdobe Experience Platformで見るものとは異なることがわかります。 Experience Platformで表示される内容はCompatibility Modeと呼ばれ、標準のXDMとPlatform内での使い方との間の単純なマッピングを提供します。

### 互換モードの動作

互換モードを使用すると、標準のXDM内の値を変更しながらセマンティックを同じままにすることで、XDM JSON-LDモデルが既存のデータインフラストラクチャと連携できます。 ネストされたJSON構造を使用し、スキーマをツリー形式で表示します。

標準のXDMと互換モードの主な違いは、フィールド名のプレフィックス「xdm:」が削除されたことです。

標準のXDMと互換モードの両方で、誕生日関連のフィールド（「description」属性を削除した状態）を並べて比較した結果を次に示します。 「Compatibility Mode」フィールドには、「meta:xdmField」属性と「meta:xdmType」属性に、XDMフィールドへの参照とそのデータ型が含まれています。

<table>
  <th>標準XDM</th>
  <th>互換モード</th>
  <tr>
  <td>
  <pre class="JSON language-JSON hljs">
        { "xdm:birthDate": { "title": "生年月日", "種類": "string", "format": "date", }, "xdm:birthDayAndMonth": { "title": "生年月日", "種類": "string", "pattern": "[0-1][0-9]-[0-9][0-9]", }, "xdm:birthYear": { "title": "生年"、"タイプ": "integer", "minimum": 1, "maximum": 32767 }
      </pre>
  </td>
  <td>
  <pre class="JSON language-JSON hljs">
        { "birthDate": { "title": "生年月日", "種類": "string", "format": "date", "meta:xdmField": "xdm:birthDate", "meta:xdmType": "date" }, "birthDayAndMonth": { "title": "生年月日", "種類": "string", "pattern": "[0-1][0-9]-[0-9][0-9]"、"meta:xdmField": "xdm:birthDayAndMonth", "meta:xdmType": "string" }, "birthYear": { "title": "生年"、"タイプ": "integer", "minimum": 1, "maximum": 32767, "meta:xdmField": "xdm:birthYear", "meta:xdmType": "short" }
      </pre>
  </td>
  </tr>
</table>

### 互換モードが必要な理由

Adobe Experience Platformは、複数のソリューションやサービスを使用するように設計されており、それぞれが独自の技術的な課題と制限（特定のテクノロジーが特殊文字をどのように処理するかなど）を持っています。 これらの制限を克服するため、互換モードが開発されました。

カタログ、Data Lake、リアルタイム顧客プロファイルを含むほとんどのExperience Platformサービスでは、標準のXDMの代わりに互換モードが使用されます。 スキーマレジストリAPIでも互換モードが使用され、このドキュメントの例はすべて互換モードを使用して表示されます。

標準のXDMとExperience Platformでの動作との間でマッピングが行われることを知っておく価値はありますが、Platformサービスの使用に影響を与えることはありません。

オープンソースプロジェクトは利用できますが、スキーマレジストリを通じてリソースとの対話に関しては、このドキュメントのAPIの例が、知り、従うべきベストプラクティスを提供します。

## APIでのXDMフィールドタイプの定義 {#field-types}

XDMスキーマは、JSONスキーマ標準規格と基本的なフィールドタイプを使用して定義され、Experience Platformによって適用されるフィールド名に対する追加の制限があります。 XDMでは、形式やオプションの制約を使用して、追加のフィールドの種類を定義できます。 XDMフィールドタイプは、フィールドレベルの属性( )で公開され `meta:xdmType`ます。

>[!NOTE]
>
>`meta:xdmType` はシステム生成値なので、このプロパティをフィールドのJSONに追加する必要はありません。 ベストプラクティスは、JSONスキーマタイプ（文字列や整数など）と、以下の表に定義されている適切な最小/最大制約を使用することです。

次の表に、オプションのプロパティを使用して、スカラーフィールドの種類とより具体的なフィールドの種類を定義するための適切な形式の概要を示します。 オプションのプロパティとタイプ固有のキーワードに関する詳細は、 [JSONスキーマのドキュメントを参照してください](https://json-schema.org/understanding-json-schema/reference/type.html)。

まず、目的のフィールドタイプを探し、提供されたサンプルコードを使用してAPIリクエストを作成します。

<table>
  <tr>
    <th>目的のタイプ<br/>(meta:xdmType)</th>
    <th>JSON<br/>(JSONスキーマ)</th>
    <th>コードサンプル</th>
  </tr>
  <tr>
    <td>string</td>
    <td>type:<br/><br/><strong>stringOptionalプロパティ：</strong><br/>
      <ul>
        <li>pattern</li>
        <li>minLength</li>
        <li>maxLength</li>
      </ul>
    </td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "string", "pattern": "^[A-Z]{2}$", "maxLength": 2 }
      </pre>
    </td>
  </tr>
  <tr>
    <td>uri<br/>(xdmType:string)</td>
    <td>type:<br/>stringformat: uri</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "string", "format": "uri" }
      </pre>
    </td>
  </tr>
  <tr>
    <td>enum<br/>(xdmType: 文字列)</td>
    <td>type:<br/><br/><strong>stringOptionalプロパティ：</strong><br/>
      <ul>
        <li>default</li>
      </ul>
    </td>
    <td>「meta:enum」を使用して、顧客に表示するオプションラベルを指定します。
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "string", "enum": [ "value1", "value2", "value3" ], "meta:enum": { "value1": "値1", "値2": "値2", "値3": "値3" }, "デフォルト": "value1" }
      </pre>
    </td>
  </tr>
  <tr>
    <td>数値</td>
    <td>type:<br/>numberminimum: ±2.23×10^308<br/>最大： ±1.80×10^308</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "number" }
      </pre>
    </td>
  </tr>
  <tr>
    <td>long</td>
    <td>type: 最<br/>大整数：2^53+1<br>最小：-2^53+1</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "integer", "minimum": -9007199254740992、「maximum」: 9007199254740992 }
      </pre>
    </td>
  </tr>
  <tr>
    <td>int</td>
    <td>type: 最大<br/>整数：2^31<br>最小：-2^31</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "integer", "minimum": -2147483648、「maximum」: 2147483648 }
      </pre>
    </td>
  </tr>
  <tr>
    <td>short</td>
    <td>type: 最大<br/>整数：2^15<br>最小：-2^15</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "integer", "minimum": -32768、「maximum」: 32768 }
      </pre>
    </td>
  </tr>
  <tr>
    <td>byte</td>
    <td>type: 最大<br/>整数：2^7<br>最小：-2^7</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "integer", "minimum": -128、"maximum": 128 }
      </pre>
    </td>
  </tr>
  <tr>
    <td>ブール型</td>
    <td><br/>type: boolean<br/>{true, false}<br/><br/><strong>オプションのプロパティ：</strong><br/>
      <ul>
        <li>default</li>
      </ul>
    </td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "boolean", "default": false }
      </pre>
    </td>
  </tr>
  <tr>
    <td>date</td>
    <td>type:<br/>stringformat: date</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "string", "format": "日付", "例": ["2004-10-23"] }
      </pre>
      RFC 3339の <a href="https://tools.ietf.org/html/rfc3339#section-5.6" target="_blank">RFC 3339セクション5.6</a>で定義される日付。ここで、"full-date" = date-fullyear "-" date-month "-" date-mday (YYYY-MM-DD)
    </td>
  </tr>
  <tr>
    <td>date-time</td>
    <td>type:<br/>stringformat: date-time</td>
    <td>
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "string", "format": "日付時間"、"例": ["2004-10-23T12:00:00-06:00"] }
      </pre>
      RFC 3339の <a href="https://tools.ietf.org/html/rfc3339#section-5.6" target="_blank">SSXセクション5.6</a>で定義される日付 — 時刻。ここで、"date-time" = full-date "T" full-time:<br/>(YYYY-MM-DD'T'HH:MM:SS.SSX)
    </td>
  </tr>
  <tr>
    <td>array</td>
    <td>type: array</td>
    <td>items.typeは、任意のスカラー型を使用して定義できます。
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "array", "items": { "type": "string" } }
      </pre>
      別のスキーマで定義されたオブジェクトの配列：<br/>
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "array", "items": { "$ref": "id" } }
      </pre>
      ここで、"id"は参照スキーマーの{id}です。
    </td>
  </tr>
  <tr>
    <td>object</td>
    <td>type: object</td>
    <td>。{field}.typeは、任意のスカラー型を使用して定義できます。
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "object", "properties": { "field1": { "type": "string" }, "field2": { "type": "number" } } } }
      </pre>
      参照スキーマで定義されるタイプ「object」のフィールド：
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "オブジェクト", "$ref": "id" }
      </pre>
      ここで、"id"は参照スキーマーの{id}です。
    </td>
  </tr>
  <tr>
    <td>map</td>
    <td>type:<br/><br/><strong>objectNote: 'map'データ型の</strong><br/>使用は、業界およびベンダーのスキーマ使用のために予約されており、テナント定義フィールドでは使用できません。 ある値にマップするキーとしてデータが表される場合や、静的なスキーマにキーを適切に含めることができず、データ値として扱う必要がある場合は、標準スキーマで使用されます。</td>
    <td>'map'はプロパティを定義してはいけません。 'map'に含まれる値のタイプを記述する「additionalProperties」スキーマを1つ定義する必要があります。 XDMの「map」には、1つのデータ型のみを含めることができます。 値は、配列やオブジェクト、または別のスキーマへの参照を含む任意の有効なXDMスキーマ定義です（$ref経由）。<br/><br/>値のタイプが「string」のフィールドをマップ：
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "object", "additionalProperties":{ "type": "string" } }
      </pre>
    文字列の配列である値を持つフィールドをマップします。
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "object", "additionalProperties":{ "type": "array", "items": { "type": "string" } } }
      </pre>
    別のスキーマを参照するMapフィールド：
      <pre class="JSON language-JSON hljs">
        "sampleField": { "type": "object", "additionalProperties":{ "$ref": "id" } }
      </pre>
      ここで、"id"は参照スキーマーの{id}です。
    </td>
  </tr>
</table>


## XDM型の他の形式へのマッピング

次の表に、「meta:xdmType」と他のシリアル化形式の対応付けを示します。

| XDM Type<br>(meta:xdmType) | JSON<br>(JSONスキーマ) | パーケ<br>（タイプ/注釈） | Spark SQL | Java | スカラ | .NET | CosmosDB | MongoDB | エアロスパイク | プロトバフ2 |
|---|---|---|---|---|---|---|---|---|---|---|
| string | type:string | BYTE_ARRAY/UTF8 | StringType | java.lang.String | 文字列 | System.String | 文字列 | string | 文字列 | string |
| 数値 | type:number | 重複 | DoubleType | java.lang.Double | 重複 | System.Double | 数値 | 重複 | 重複 | 重複 |
| long | type:<br>integermaximum:2^53+1<br>最小：-2^53+1 | INT64 | LongType | java.lang.Long | ロング | System.Int64 | 数値 | long | 整数 | int64 |
| int | type:<br>integermaximum:2^31<br>最小：-2^31 | INT32/INT_32 | IntegerType | java.lang.Integer | 整数 | System.Int32 | 数値 | int | 整数 | int32 |
| short | type:<br>integermaximum:2^15<br>最小：-2^15 | INT32/INT_16 | ShortType | java.lang.Short | Short | System.Int16 | 数値 | int | 整数 | int32 |
| byte | type:<br>integermaximum:2^7<br>minimum:-2^7 | INT32/INT_8 | ByteType | java.lang.Short | バイト | System.SByte | 数値 | int | 整数 | int32 |
| ブール型 | type:boolean | BOOLEAN | BooleanType | java.lang.Boolean | Boolean | System.Boolean | Boolean | bool | 整数 | 整数 | bool |
| date | type:<br>stringformat:date<br>（RFC 3339、セクション5.6） | INT32/DATE | DateType | java.util.Date | java.util.Date | System.DateTime | 文字列 | date | 整数<br>(unix millis) | int64<br>(unix millis) |
| date-time | type:<br>stringformat:date-time<br>（RFC 3339、セクション5.6） | INT64/TIMESTAMP_MILLIS | TimestampType | java.util.Date | java.util.Date | System.DateTime | 文字列 | timestamp | 整数<br>(unix millis) | int64<br>(unix millis) |
| map | object | MAP注釈付きグループ<br><br>&lt;<span>key_type</span>>は、マップ値のSTRING<br><br>&lt;<span>value_type</span>>型である必要があります | MapType<br><br>&quot;keyType&quot;はStringType<br><br>&quot;valueType&quot;はmap値の型である必要があります。 | java.util.Map | マップ | --- | object | object | map | map&lt;<span>key_type, value_type</span>> |
