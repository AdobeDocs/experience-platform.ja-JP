---
keywords: Experience Platform;home;popular topics;api;API;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema registry;Schema Registry;compatibility;Compatibility;compatibility mode;Compatibility mode;field type;field types;
solution: Experience Platform
title: スキーマレジストリ開発者向けの付録
description: このドキュメントでは、スキーマレジストリ API の使用に関する補足情報を提供します。
topic: developer guide
translation-type: tm+mt
source-git-commit: 42d3bed14c5f926892467baeeea09ee7a140ebdc
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

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