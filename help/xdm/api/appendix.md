---
keywords: Experience Platform;home;popular topics;api;API;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema registry;Schema Registry;compatibility;Compatibility;compatibility mode;Compatibility mode;field type;field types;
solution: Experience Platform
title: スキーマレジストリ開発者向けの付録
description: このドキュメントでは、スキーマレジストリ API の使用に関する補足情報を提供します。
topic: developer guide
translation-type: tm+mt
source-git-commit: 0b55f18eabcf1d7c5c233234c59eb074b2670b93
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 50%

---


# 付録

This document provides supplemental information related to working with the [!DNL Schema Registry] API.

## クエリパラメーターの使用 {#query}

は、リソースのリスト表示時に、ページを作成し結果をフィルタリングするクエリパラメーターの使用をサポートしています。 [!DNL Schema Registry]

>[!NOTE]
>
>複数のクエリパラメーターを組み合わせる場合は、アンパサンド（`&`）で区切る必要があります。

### ページング {#paging}

ページングに最も一般的なクエリパラメーターは次のとおりです。

| パラメーター | 説明 |
| --- | --- |
| `start` | リストの結果を開始する場所を指定します。 この値はリスト応答の `_page.next` 属性から取得でき、結果の次のページにアクセスするのに使用されます。 この `_page.next` 値がnullの場合、追加のページはありません。 |
| `limit` | 返されるリソースの数を制限する。 例：`limit=5` は 5 つのリソースのリストを返します。 |
| `orderby` | 特定のプロパティで結果を並べ替える。 例：`orderby=title` は昇順（A ～ Z）のタイトルで結果を並べ替えます。Adding a `-` before the parameter value (`orderby=-title`) will sort items by title in descending order (Z-A). |

### フィルター {#filtering}

パラメーターを使用して結果をフィルターできます。この `property` パラメーターは、取得したリソース内の特定のJSONプロパティに対して特定の演算子を適用するのに使用されます。 次の演算子がサポートされています。

| 演算子 | 説明 | 例 |
| --- | --- | --- |
| `==` | プロパティが指定された値と等しいかどうかを示すフィルター。 | `property=title==test` |
| `!=` | プロパティが指定された値と等しくないかどうかによるフィルター。 | `property=title!=test` |
| `<` | プロパティが指定された値より小さいかどうかによるフィルター。 | `property=version<5` |
| `>` | プロパティが指定された値より大きいかどうかによるフィルター。 | `property=version>5` |
| `<=` | プロパティが指定した値以下か、または指定した値と等しいかによるフィルター。 | `property=version<=5` |
| `>=` | プロパティが指定された値以上かどうかによるフィルター。 | `property=version>=5` |
| `~` | 指定された正規式とプロパティが一致するかどうかを示すフィルター。 | `property=title~test$` |
| なし | プロパティ名のみをステートすると、プロパティが存在するエントリのみが返されます。 | `property=title` |

>[!TIP]
>
>この `property` パラメーターを使用して、互換性のあるクラスでミックスインをフィルタリングできます。 For example, `property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile` returns only mixins that are compatible with the [!DNL XDM Individual Profile] class.

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