---
title: Define XDM Fields in the Schema Registry API
description: Learn how to define different fields when creating custom Experience Data Model (XDM) resources in the Schema Registry API.
exl-id: d79332e3-8448-42af-b250-882bcb0f1e7d
source-git-commit: 4ce9e53ec420a8c9ba07cdfd75e66d854989f8d2
workflow-type: tm+mt
source-wordcount: '783'
ht-degree: 13%

---

# Define XDM fields in the Schema Registry API

[](https://json-schema.org/)The Schema Registry API allows you to define custom fields in your schemas through the use of formats and optional constraints. `meta:xdmType`

>[!NOTE]
>
>`meta:xdmType`[](#maps)`string``integer`

The following table outlines the appropriate formatting to define different field types, including those with optional properties. オプションのプロパティとタイプ固有のキーワードに関する詳細については、[JSON スキーマのドキュメント](https://json-schema.org/understanding-json-schema/reference/type.html)を参照してください。

[](../api/field-groups.md#create)[](../api/data-types.md#create)

<table style="table-layout:auto">
  <tr>
    <th>XDM タイプ</th>
    <th>オプションで指定できるプロパティ</th>
    <th>例</th>
  </tr>
  <tr>
    <td>[!UICONTROL String]</td>
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
    <td><code>enum</code><code>meta:enum</code>
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
    <br><code>meta:enum</code><strong></strong><code>meta:enum</code><code>enum</code><code>meta:enum</code><code>enum</code><a href="../tutorials/suggested-values.md"></a>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL Number]</td>
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
    <td>[!UICONTROL Byte]</td>
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
    <td>[!UICONTROL Date]</td>
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
:00:</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL Array]</td>
    <td></td>
    <td>An array of basic scalar types (e.g. strings):
      <pre class="JSON language-JSON hljs">
"sampleField": {
          "type": "array",
          "items": {
            "type": "string"
  }
}</pre>
      <br/>
      <pre class="JSON language-JSON hljs">
"sampleField": {
  "type": "array",
  "items": {
    "$ref": "https://ns.adobe.com/xdm/data/paymentitem"
  }
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL Object]</td>
    <td></td>
    <td><code>type</code><code>properties</code>
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
      <code>$id</code>
      <pre class="JSON language-JSON hljs">
"sampleField": {
  "type": "object",
  "$ref": "https://ns.adobe.com/xdm/common/phoneinteraction"
}</pre>
    </td>
  </tr>
  <tr>
    <td>[!UICONTROL Map]</td>
    <td></td>
    <td>A map-type field is essentially an object-type field with an unconstrained set of keys. <code>type</code><code>object</code><code>meta:xdmType</code><code>map</code><br><br><strong></strong><strong></strong><code>additionalProperties</code><code>type</code><code>string</code><code>integer</code><br/><br/>
      <pre class="JSON language-JSON hljs">
"sampleField": {
  "type": "object",
  "meta:xdmType": "map",
  "additionalProperties":{
    "type": "string"
  }
}</pre>
    See the section below for more information on creating custom map types in XDM.
    </td>
  </tr>
</table>

## Creating custom map types {#maps}

`meta:xdmType``map``additionalProperties.type`

XDM places the following restrictions on the use of this storage hint:

* `object`
* Map types MUST NOT have properties defined (in other words, they define &quot;empty&quot; objects).
* `additionalProperties.type``string``integer`

Ensure that you are only using map-type fields when absolutely necessary, as they carry the following performance drawbacks:

* Response time from Adobe Experience Platform Query Service degrades from three seconds to ten seconds for 100 million records.
* Maps must have fewer than 16 keys or else risk further degradation.

The Platform user interface also has limitations in how it can extract the keys of map-type fields. Whereas object-type fields can be expanded, maps are displayed as a single field instead.

## 次の手順

This guide covered how to define different field types in the API. [](../schema/field-constraints.md)
