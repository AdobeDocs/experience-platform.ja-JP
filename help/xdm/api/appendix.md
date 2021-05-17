---
keywords: Experience Platform；ホーム；人気の高いトピック；API;XDM;XDM;XDMシステム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマレジストリ；互換性；互換性；互換性モード；フィールドタイプ；
solution: Experience Platform
title: スキーマレジストリAPIガイド付録
description: このドキュメントでは、スキーマレジストリ API の使用に関する補足情報を提供します。
topic-legacy: developer guide
exl-id: 2ddc7fe8-dd0b-4cf9-8561-e89fcdadbfce
source-git-commit: dcfdc9c479e8a77296f7cb0bf9f5bb36e9261b75
workflow-type: tm+mt
source-wordcount: '775'
ht-degree: 49%

---

# スキーマレジストリAPIガイドの付録

このドキュメントでは、[!DNL Schema Registry] APIの操作に関する補足情報を提供します。

## クエリパラメーターの使用 {#query}

[!DNL Schema Registry]は、クエリをリストする際に、ページを作成し結果をフィルターするためのリソースパラメーターの使用をサポートしています。

>[!NOTE]
>
>複数のクエリパラメーターを組み合わせる場合は、アンパサンド（`&`）で区切る必要があります。

### ページング {#paging}

ページングに最も一般的なクエリパラメーターは次のとおりです。

| パラメーター | 説明 |
| --- | --- |
| `start` | リストの結果を開始する場所を指定します。 この値はリスト応答の`_page.next`属性から取得でき、結果の次のページにアクセスするのに使用されます。 `_page.next`値がnullの場合は、追加のページはありません。 |
| `limit` | 返されるリソースの数を制限する。 例：`limit=5` は 5 つのリソースのリストを返します。 |
| `orderby` | 特定のプロパティで結果を並べ替えます。例：`orderby=title` は昇順（A ～ Z）のタイトルで結果を並べ替えます。パラメーター値(`orderby=-title`)の前に`-`を追加すると、タイトル順に項目が降順(Z-A)で並べ替えられます。 |

### フィルタリング {#filtering}

`property`パラメーターを使用して結果をフィルターできます。このパラメーターは、取得したリソース内の特定のJSONプロパティに対して特定の演算子を適用するのに使用されます。 次の演算子がサポートされています。

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
>`property`パラメーターを使用すると、互換性のあるクラスでスキーマフィールドグループをフィルタリングできます。 例えば、`property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile`は、[!DNL XDM Individual Profile]クラスと互換性のあるフィールドグループのみを返します。

## 互換性モード {#compatibility}

[!DNL Experience Data Model] （XDM）は、アドビが推進する公式に文書化された仕様であり、デジタルエクスペリエンスの相互運用性、表現力およびパワーを向上させます。アドビは、[GitHub のオープンソースプロジェクト](https://github.com/adobe/xdm/)でソースコードと公式の XDM 定義を公開しています。これらの定義は XDM 標準表記で記述され、JSON-LD （JavaScript Object Notation for Linked Data）および JSON スキーマを XDM スキーマを定義する文法として使用しています。

パブリックリポジトリーで公式の XDM 定義を見ると、標準 XDM は Adobe Experience Platform での表示とは異なることがわかります。[!DNL Experience Platform]で見ているものは「互換性モード」と呼ばれ、標準のXDMと[!DNL Platform]内で使われている方法との間の単純なマッピングを提供します。

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

[!DNL Catalog]、[!DNL Data Lake]、[!DNL Real-time Customer Profile]を含むほとんどの[!DNL Experience Platform]サービスは、標準のXDMの代わりに[!DNL Compatibility Mode]を使用します。 [!DNL Schema Registry] APIは[!DNL Compatibility Mode]も使用します。このドキュメントの例はすべて[!DNL Compatibility Mode]を使用して表示されます。

標準のXDMと[!DNL Experience Platform]での動作との間でマッピングが行われることを知っておく価値はありますが、[!DNL Platform]サービスの使用に影響を与えることはありません。

オープンソースプロジェクトは利用できますが、[!DNL Schema Registry]を通じてリソースと対話する際には、このドキュメントのAPIの例が、知り、従うべきベストプラクティスを提供します。
