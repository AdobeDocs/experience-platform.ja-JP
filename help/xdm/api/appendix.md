---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマレジストリ；スキーマレジストリ；互換性；互換性；互換性モード；互換性モード；フィールドタイプ；フィールドタイプ；
solution: Experience Platform
title: スキーマレジストリ API ガイドの付録
description: このドキュメントでは、スキーマレジストリ API の使用に関する補足情報を提供します。
exl-id: 2ddc7fe8-dd0b-4cf9-8561-e89fcdadbfce
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '986'
ht-degree: 28%

---

# スキーマレジストリ API ガイドの付録

このドキュメントでは、[!DNL Schema Registry] API の操作に関する補足情報を提供します。

## クエリパラメーターの使用 {#query}

[!DNL Schema Registry] では、リソースをリスト表示する際に、クエリパラメーターを使用して結果をページ化およびフィルタリングする機能をサポートしています。

>[!NOTE]
>
>複数のクエリパラメーターを組み合わせる場合は、アンパサンド（`&`）で区切る必要があります。

### ページング {#paging}

ページングに最も一般的なクエリパラメーターは次のとおりです。

| パラメーター | 説明 |
| --- | --- |
| `orderby` | 特定のプロパティで結果を並べ替えます。例：`orderby=title` は昇順（A ～ Z）のタイトルで結果を並べ替えます。パラメーター値（`orderby=-title`）の前に `-` を追加すると、項目がタイトルで降順（Z ～ A）に並べ替えられます。 |
| `limit` | `orderby` パラメーターと共に使用す `limit` と、特定のリクエストに対して返される項目の最大数が制限されます。 このパラメーターは、`orderby` パラメーターが存在しない場合は使用できません。<br><br>`limit` パラメーターは、返される項目の最大数に関して、*ヒント* として正の整数（`0` ～ `500`）を指定します。 例えば、`limit=5` はリスト内の 5 つのリソースのみを返します。 ただし、この値は厳密には適用されません。 実際の応答サイズは、`start` パラメーターの信頼性の高い動作を提供する必要がある（提供されている場合）という制約の下で、小さくなることも大きくなることもあります。 |
| `start` | `orderby` パラメーターと共に使用すると、`start` は項目のサブセット化されたリストを開始する場所を指定します。 このパラメーターは、`orderby` パラメーターが存在しない場合は使用できません。 この値は、リスト応答の `_page.next` 属性から取得でき、結果の次のページにアクセスするために使用できます。 `_page.next` の値が null の場合、使用可能な追加のページはありません。<br><br> 通常、結果の最初のページを取得するために、このパラメーターは省略されます。 その後、前のページ `start` 受け取った `orderby` フィールドのプライマリソートプロパティの最大値に設定する必要があります。 次に、API 応答は、指定された値より厳密に大きい（昇順の場合）または厳密 `orderby` 小さい（降順の場合）プライマリ並べ替えプロパティを持つエントリで始まるエントリを返します。<br><br> 例えば、`orderby` パラメーターが `orderby=name,firstname` に設定されている場合、`start` パラメーターには `name` プロパティの値が含まれます。 この場合、「Miller」という名前の直後にリソースの次の 20 個のエントリを表示する場合は、`?orderby=name,firstname&start=Miller&limit=20` を使用します。 |

{style="table-layout:auto"}

### フィルタリング {#filtering}

`property` パラメーターを使用して結果をフィルタリングできます。このパラメーターは、取得したリソース内の特定の JSON プロパティに対して特定の演算子を適用するために使用されます。 次の演算子がサポートされています。

| 演算子 | 説明 | 例 |
| --- | --- | --- |
| `==` | プロパティが指定された値に等しいかどうかを基準にフィルタリングします。 | `property=title==test` |
| `!=` | プロパティが指定された値に等しくないかどうかをフィルタリングします。 | `property=title!=test` |
| `<` | プロパティが指定された値より小さいかどうかを基準にフィルタリングします。 | `property=version<5` |
| `>` | プロパティが指定された値より大きいかどうかを基準にフィルタリングします。 | `property=version>5` |
| `<=` | プロパティが指定された値以下であるかによってフィルタリングします。 | `property=version<=5` |
| `>=` | プロパティが指定された値以上であるかによってフィルタリングします。 | `property=version>=5` |
| なし | プロパティ名のみを指定すると、プロパティが存在するエントリのみが返されます。 | `property=title` |

{style="table-layout:auto"}

>[!TIP]
>
>`property` パラメーターを使用して、互換性のあるクラスでスキーマフィールドグループをフィルタリングできます。 例えば、`property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile` は [!DNL XDM Individual Profile] クラスと互換性のあるフィールドグループのみを返します。

## 互換性モード {#compatibility}

[!DNL Experience Data Model] （XDM）はパブリックに文書化された仕様で、Adobeによって推進され、デジタルエクスペリエンスの相互運用性、表現力、パワーを向上させます。 アドビは、[GitHub のオープンソースプロジェクト](https://github.com/adobe/xdm/)でソースコードと公式の XDM 定義を公開しています。これらの定義は XDM 標準表記で記述され、JSON-LD （JavaScript Object Notation for Linked Data）および JSON スキーマを XDM スキーマを定義する文法として使用しています。

パブリックリポジトリーで公式の XDM 定義を見ると、標準 XDM は Adobe Experience Platform での表示とは異なることがわかります。[!DNL Experience Platform] に表示されるものは互換性モードと呼ばれ、標準 XDM と [!DNL Experience Platform] 内での使用方法との間の簡単なマッピングを提供します。

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
{
  "xdm:birthDate": {
    "title": "生年月日",
    "type": "string",
    "format": "date"
  }、
  "xdm:birthDayAndMonth": {
    "title": "生年月日",
    "type": "string",
    "pattern": "[0-1][0-9]-[0-9][0-9]"
  }、
  "xdm:birthYear": {
    "title": "Birth year",
    "type": "integer",
    "minimum":1,
    "maximum": 32767
  }
}
  </pre>
  </td>
  <td>
  <pre class=" language-json">
{
  "birthDate": {
    "title": "生年月日",
    "type": "string",
    "format": "date",
    "meta:xdmField": "xdm:birthDate",
    "meta:xdmType": "date"
  }、
  "birthDayAndMonth": {
    "title": "生年月日",
    "type": "string",
    "pattern": "[0-1][0-9]-[0-9][0-9]",
    "meta:xdmField": "xdm:birthDayAndMonth",
    "meta:xdmType": "string"
  }、
  "birthYear": {
    "title": "Birth year",
    "type": "integer",
    "minimum":1,
    「maximum」:32767、
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

[!DNL Catalog]、[!DNL Data Lake]、[!DNL Real-Time Customer Profile] など、ほとんどの [!DNL Experience Platform] サービスは、標準の XDM の代わりに [!DNL Compatibility Mode] を使用します。 [!DNL Schema Registry] API でも [!DNL Compatibility Mode] を使用しています。このドキュメントの例はすべて、[!DNL Compatibility Mode] を使用して示しています。

標準 XDM と [!DNL Experience Platform] での運用化の方法の間でマッピングが行われることを知っておくことは価値がありますが、[!DNL Experience Platform] サービスの使用には影響しません。

オープンソースプロジェクトは利用可能ですが、[!DNL Schema Registry] を通じてリソースを操作する場合、このドキュメントの API の例では、知っていて従う必要があるベストプラクティスを提供しています。
