---
title: スキーマレジストリ API での XDM フィールドの定義
description: スキーマレジストリ API でカスタムの Experience Data Model(XDM) リソースを作成する際に、異なるフィールドを定義する方法を説明します。
source-git-commit: af4c345819d3e293af4e888c9cabba6bd874583b
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 14%

---

# スキーマレジストリ API での XDM フィールドの定義

すべてのエクスペリエンスデータモデル (XDM) フィールドは、標準の [JSON スキーマ](https://json-schema.org/) フィールドタイプに適用される制約と、Adobe Experience Platformで適用されるフィールド名に対する追加の制約が含まれます。 スキーマレジストリ API を使用すると、形式とオプションの制約を使用して、スキーマ内のカスタムフィールドを定義できます。 XDM フィールドタイプは、フィールドレベルの属性で公開されます。 `meta:xdmType`.

>[!NOTE]
>
>`meta:xdmType` はシステムで生成される値なので、API を使用する場合 ( [カスタムマップタイプの作成](#maps)) をクリックします。 ベストプラクティスは、JSON スキーマのタイプ ( `string` および `integer`) に適切な最小/最大制約を設定します。

次の表に、オプションのプロパティを含む、様々なフィールドタイプを定義するための適切な書式の概要を示します。 オプションのプロパティとタイプ固有のキーワードに関する詳細については、[JSON スキーマのドキュメント](https://json-schema.org/understanding-json-schema/reference/type.html)を参照してください。

最初に、目的のフィールドタイプを見つけ、提供されたサンプルコードを使用して、の API リクエストを作成します。 [フィールドグループの作成](../api/field-groups.md#create) または [データ型の作成](../api/data-types.md#create).

<table style="table-layout:auto">
  <tr>
    <th>XDM タイプ</th>
    <th>オプションで指定できるプロパティ</th>
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
    <td>マップタイプフィールドは、基本的には、制限のないキーのセットを持つオブジェクトタイプのフィールドです。 オブジェクトと同様に、マップには <code>type</code> 値 <code>object</code>ですが、 <code>meta:xdmType</code> が明示的にに設定されている <code>map</code>.<br><br>地図 <strong>次の値を指定する</strong> プロパティを定義します。 It <strong>必須</strong> 単一の <code>additionalProperties</code> マップ内に含まれる値のタイプを記述するスキーマ（各マップには 1 つのデータ型のみを含めることができます）。 この <code>type</code> 値は次のいずれかでなければなりません <code>string</code> または <code>integer</code>.<br/><br/>string-type 値を持つ map フィールド：
      <pre class="JSON language-JSON hljs">
"sampleField":{ "type":"object", "meta:xdmType":"map", "additionalProperties":{ "type":"string" } }</pre>
    XDM でのカスタムマップタイプの作成について詳しくは、以下の節を参照してください。
    </td>
  </tr>
</table>

## カスタムマップタイプの作成 {#maps}

XDM で「マップに似た」データを効率的にサポートするために、オブジェクトに `meta:xdmType` に設定 `map` これは、キーセットが制約されていないかのようにオブジェクトを管理する必要があることを明確にするためです。 XDM では、このストレージヒントの使用に次の制限を設けます。

* マップタイプはタイプである必要があります `object`
* マップタイプには、プロパティを定義することはできません（つまり、「空の」オブジェクトを定義します）
* マップタイプには 1 つの `additionalProperties` マップ内に配置される値を記述するスキーマ

必要に応じて、次のパフォーマンス上の欠点があるので、マップタイプのフィールドのみを使用していることを確認します。

* Adobe Experience Platform Query Service からの応答時間は、1 億件のレコードに対して 3 秒から 10 秒に短縮されます
* マップのキー数は 16 個未満にする必要があります。16 個未満にすると、さらに低下する可能性があります

また、Platform のユーザーインターフェイスでは、マップタイプフィールドのキーを抽出する方法に制限があります。 オブジェクトタイプのフィールドは展開できますが、マップは代わりに 1 つのフィールドとして表示されます。

## 次の手順

このガイドでは、API で様々なフィールドタイプを定義する方法について説明しました。 XDM フィールドタイプの形式について詳しくは、 [XDM フィールドタイプ制約](../schema/field-constraints.md).
