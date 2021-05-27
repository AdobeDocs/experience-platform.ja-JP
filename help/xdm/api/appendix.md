---
keywords: Experience Platform；ホーム；人気のあるトピック；API;API;XDM;XDMシステム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマレジストリ；互換性；互換性モード；フィールドタイプ；フィールドタイプ；
solution: Experience Platform
title: スキーマレジストリAPIガイドの付録
description: このドキュメントでは、スキーマレジストリ API の使用に関する補足情報を提供します。
topic-legacy: developer guide
exl-id: 2ddc7fe8-dd0b-4cf9-8561-e89fcdadbfce
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '781'
ht-degree: 49%

---

# スキーマレジストリAPIガイドの付録

このドキュメントでは、[!DNL Schema Registry] APIの使用に関する補足情報を提供します。

## クエリパラメーターの使用 {#query}

[!DNL Schema Registry]は、リソースをリストする際のページへのクエリパラメーターの使用と、結果のフィルタリングをサポートしています。

>[!NOTE]
>
>複数のクエリパラメーターを組み合わせる場合は、アンパサンド（`&`）で区切る必要があります。

### ページング {#paging}

ページングに最も一般的なクエリパラメーターは次のとおりです。

| パラメーター | 説明 |
| --- | --- |
| `start` | 表示された結果の開始位置を指定します。 この値は、リスト応答の`_page.next`属性から取得でき、結果の次のページにアクセスするために使用されます。 `_page.next`値がnullの場合、追加のページは使用できません。 |
| `limit` | 返されるリソースの数を制限します。 例：`limit=5` は 5 つのリソースのリストを返します。 |
| `orderby` | 特定のプロパティで結果を並べ替えます。例：`orderby=title` は昇順（A ～ Z）のタイトルで結果を並べ替えます。パラメーター値(`orderby=-title`)の前に`-`を追加すると、降順(Z ～ A)のタイトルで項目が並べ替えられます。 |

{style=&quot;table-layout:auto&quot;}

### フィルタリング {#filtering}

`property`パラメーターを使用して結果をフィルタリングできます。このパラメーターは、取得したリソース内の特定のJSONプロパティに対して特定の演算子を適用するために使用されます。 次の演算子がサポートされます。

| 演算子 | 説明 | 例 |
| --- | --- | --- |
| `==` | プロパティが指定された値と等しいかどうかでフィルターします。 | `property=title==test` |
| `!=` | プロパティが指定された値と等しくないかどうかでフィルターします。 | `property=title!=test` |
| `<` | プロパティが指定された値より小さいかどうかでフィルターします。 | `property=version<5` |
| `>` | プロパティが指定された値より大きいかどうかでフィルターします。 | `property=version>5` |
| `<=` | プロパティが指定された値以下かどうかでフィルターします。 | `property=version<=5` |
| `>=` | プロパティが指定された値以上かどうかでフィルターします。 | `property=version>=5` |
| `~` | プロパティが指定した正規表現と一致するかどうかでフィルターします。 | `property=title~test$` |
| なし | プロパティ名のみを指定すると、プロパティが存在するエントリのみが返されます。 | `property=title` |

{style=&quot;table-layout:auto&quot;}

>[!TIP]
>
>`property`パラメーターを使用して、互換性のあるクラスでスキーマフィールドグループをフィルタリングできます。 例えば、`property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile`は、[!DNL XDM Individual Profile]クラスと互換性のあるフィールドグループのみを返します。

## 互換性モード {#compatibility}

[!DNL Experience Data Model] （XDM）は、アドビが推進する公式に文書化された仕様であり、デジタルエクスペリエンスの相互運用性、表現力およびパワーを向上させます。アドビは、[GitHub のオープンソースプロジェクト](https://github.com/adobe/xdm/)でソースコードと公式の XDM 定義を公開しています。これらの定義は XDM 標準表記で記述され、JSON-LD （JavaScript Object Notation for Linked Data）および JSON スキーマを XDM スキーマを定義する文法として使用しています。

パブリックリポジトリーで公式の XDM 定義を見ると、標準 XDM は Adobe Experience Platform での表示とは異なることがわかります。[!DNL Experience Platform]で表示される内容は互換モードと呼ばれ、標準XDMと[!DNL Platform]内での使用方法との間の単純なマッピングを提供します。

### 互換性モードの仕組み

互換性モードを使用すると、XDM JSON-LD モデルは、同じセマンティクスを保持したままで標準 XDM 内の値を変更することにより、既存のデータインフラストラクチャと連携できます。ネストされた JSON 構造を使用して、スキーマをツリーに類似した形式で表示します。

標準 XDM と互換性モードの主な違いは、フィールド名の「xdm:」プレフィックスが削除されていることです。

標準 XDM と互換性モードの誕生日関連のフィールド（&quot;description&quot; 属性が削除された）を並べて比較したものを次に示します。互換性モードのフィールドでは、&quot;meta:xdmField&quot; 属性と &quot;meta:xdmType&quot; 属性に XDM フィールドへの参照とそのデータ型が含まれていることに注意してください。

<table style="table-layout:auto">
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

[!DNL Catalog]、[!DNL Data Lake]、[!DNL Real-time Customer Profile]を含むほとんどの[!DNL Experience Platform]サービスでは、標準のXDMの代わりに[!DNL Compatibility Mode]を使用します。 [!DNL Schema Registry] APIも[!DNL Compatibility Mode]を使用しています。このドキュメントの例はすべて[!DNL Compatibility Mode]を使用して示しています。

標準のXDMと[!DNL Experience Platform]での運用方法との間でマッピングが行われることを知っておくことは重要ですが、[!DNL Platform]サービスの使用に影響を与えることはありません。

オープンソースプロジェクトを利用できますが、[!DNL Schema Registry]を使用してリソースを操作する場合、このドキュメントのAPIの例は、理解し、従うべきベストプラクティスです。
