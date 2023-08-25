---
keywords: Experience Platform；ホーム；人気のトピック；API;XDM;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマレジストリ；互換性；互換性モード；フィールドタイプ；
solution: Experience Platform
title: スキーマレジストリ API ガイドの付録
description: このドキュメントでは、スキーマレジストリ API の使用に関する補足情報を提供します。
exl-id: 2ddc7fe8-dd0b-4cf9-8561-e89fcdadbfce
source-git-commit: 28891cf37dc9ffcc548f4c0565a77f62432c0b44
workflow-type: tm+mt
source-wordcount: '968'
ht-degree: 36%

---

# スキーマレジストリ API ガイドの付録

このドキュメントでは、 [!DNL Schema Registry] API.

## クエリパラメーターの使用 {#query}

The [!DNL Schema Registry] では、リソースをリストする際のページへのクエリパラメーターの使用、および結果のフィルタリングをサポートしています。

>[!NOTE]
>
>複数のクエリパラメーターを組み合わせる場合は、アンパサンド（`&`）で区切る必要があります。

### ページング {#paging}

ページングに最も一般的なクエリパラメーターは次のとおりです。

| パラメーター | 説明 |
| --- | --- |
| `orderby` | 特定のプロパティで結果を並べ替えます。例：`orderby=title` は昇順（A ～ Z）のタイトルで結果を並べ替えます。の追加 `-` パラメータ値 (`orderby=-title`) は、降順 (Z ～ A) のタイトルで項目を並べ替えます。 |
| `limit` | を `orderby` パラメータ `limit` は、特定のリクエストに対して返す項目の最大数を制限します。 このパラメーターは、 `orderby` パラメーターが存在する。<br><br>The `limit` パラメータは正の整数を指定します ( `0` および `500`) として *ヒント* 返す項目の最大数に関して。 例： `limit=5` は、リスト内の 5 つのリソースのみを返します。 ただし、この値は厳密に遵守されているわけではありません。 実際の応答サイズは、 `start` パラメーターを指定する場合は、その値を指定します。 |
| `start` | を `orderby` パラメータ `start` サブセット化された項目リストの開始位置を指定します。 このパラメーターは、 `orderby` パラメーターが存在する。 この値は、 `_page.next` リスト応答の属性で、結果の次のページへのアクセスに使用されます。 次の場合、 `_page.next` の値が null の場合、追加のページは使用できません。<br><br>通常、結果の最初のページを取得するために、このパラメーターは省略されます。 その後 `start` は、 `orderby` 前のページで受け取ったフィールド。 次に、API 応答は、プライマリ並べ替えプロパティを持つエントリで始まるエントリを返します。 `orderby` 厳密により大きい（昇順の場合）か、厳密により小さい（降順の場合）。<br><br>例えば、 `orderby` パラメーターがに設定されている `orderby=name,firstname`、 `start` パラメーターには `name` プロパティ。 この場合、「Miller」という名前の直後に、リソースの次の 20 個のエントリを表示するには、次のように指定します。 `?orderby=name,firstname&start=Miller&limit=20`. |

{style="table-layout:auto"}

### フィルター {#filtering}

結果のフィルタリングは、 `property` パラメーター。取得したリソース内の特定の JSON プロパティに対して特定の演算子を適用するために使用されます。 次の演算子がサポートされます。

| 演算子 | 説明 | 例 |
| --- | --- | --- |
| `==` | プロパティが指定された値と等しいかどうかでフィルターします。 | `property=title==test` |
| `!=` | プロパティが指定された値と等しくないかどうかでフィルターします。 | `property=title!=test` |
| `<` | プロパティが指定された値より小さいかどうかでフィルターします。 | `property=version<5` |
| `>` | プロパティが指定された値より大きいかどうかでフィルターします。 | `property=version>5` |
| `<=` | プロパティが指定された値以下かどうかでフィルターします。 | `property=version<=5` |
| `>=` | プロパティが指定された値以上かどうかでフィルターします。 | `property=version>=5` |
| なし | プロパティ名のみを指定すると、プロパティが存在するエントリのみが返されます。 | `property=title` |

{style="table-layout:auto"}

>[!TIP]
>
>以下を使用すると、 `property` 互換性のあるクラスでスキーマフィールドグループをフィルタリングするためのパラメーター。 例： `property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile` は、 [!DNL XDM Individual Profile] クラス。

## 互換性モード {#compatibility}

[!DNL Experience Data Model] （XDM）は、アドビが推進する公式に文書化された仕様であり、デジタルエクスペリエンスの相互運用性、表現力およびパワーを向上させます。アドビは、[GitHub のオープンソースプロジェクト](https://github.com/adobe/xdm/)でソースコードと公式の XDM 定義を公開しています。これらの定義は XDM 標準表記で記述され、JSON-LD （JavaScript Object Notation for Linked Data）および JSON スキーマを XDM スキーマを定義する文法として使用しています。

パブリックリポジトリーで公式の XDM 定義を見ると、標準 XDM は Adobe Experience Platform での表示とは異なることがわかります。に表示される内容 [!DNL Experience Platform] は互換性モードと呼ばれ、標準 XDM と内での使用方法との間の単純なマッピングを提供します [!DNL Platform].

### 互換性モードの仕組み

互換性モードを使用すると、XDM JSON-LD モデルは、同じセマンティクスを保持したままで標準 XDM 内の値を変更することにより、既存のデータインフラストラクチャと連携できます。ネストされた JSON 構造を使用して、スキーマをツリーに類似した形式で表示します。

標準 XDM と互換性モードの主な違いは、フィールド名の「xdm:」プレフィックスが削除されていることです。

標準 XDM と互換性モードの誕生日関連のフィールド（&quot;description&quot; 属性が削除された）を並べて比較したものを次に示します。互換性モードのフィールドでは、&quot;meta:xdmField&quot; 属性と &quot;meta:xdmType&quot; 属性に XDM フィールドへの参照とそのデータ型が含まれていることに注意してください。

<table style="table-layout:auto">
  <th>標準 XDM</th>
  <th>互換性モード</th>
  <tr>
  <td>
  <pre class=" language-json">
{ "xdm:birthDate": { "title": "生年月日", "タイプ": "文字列", "形式": "日付" }, "xdm:birthDayAndMonth": { "title": "タイプ", "文字列", "パターン": "[0-1][0-9]-[0-9]-[0]9]" }, "xdm:birthYear": { "title": "Birth year", "type": "integer", "minimum": 1, "maximum": 32767 }
  </pre>
  </td>
  <td>
  <pre class=" language-json">
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
}
      </pre>
  </td>
  </tr>
</table>

### 互換性モードが必要な理由

Adobe Experience Platform は、複数のソリューションやサービスと連携するように設計されており、各ソリューションおよびサービスには固有の技術的課題と制限（特定のテクノロジーが特殊文字を処理する方法など）があります。これらの制限を克服するために、互換モードが開発されました。

最も多い [!DNL Experience Platform] 次を含むサービス [!DNL Catalog], [!DNL Data Lake]、および [!DNL Real-Time Customer Profile] use [!DNL Compatibility Mode] 標準の XDM の代わりに使用します。 The [!DNL Schema Registry] API は [!DNL Compatibility Mode]を使用し、このドキュメントの例はすべて [!DNL Compatibility Mode].

標準 XDM とでの運用方法との間でマッピングがおこなわれることを知っておくことをお勧めします。 [!DNL Experience Platform]を使用している場合は、 [!DNL Platform] サービス。

オープンソースプロジェクトを使用できますが、次の操作を通じてリソースを操作する場合には、 [!DNL Schema Registry]には、このドキュメントの API の例が、理解し従う必要のあるベストプラクティスを示します。
