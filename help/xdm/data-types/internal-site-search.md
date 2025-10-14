---
title: 内部サイト検索データタイプ
description: 内部サイト検索 XDM データタイプについて説明します。
exl-id: 3cab9445-f641-4a44-9699-cd8a62da8a61
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 3%

---

# [!UICONTROL &#x200B; 内部サイト検索 &#x200B;] データタイプ

[!UICONTROL &#x200B; 内部サイト検索 &#x200B;] は、関連するすべての検索行動や詳細を含む、内部サイト検索を説明する標準の XDM データタイプです。

![](../images/data-types/internal-site-search.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `autoCompleteClicked` | [!UICONTROL &#x200B; ブール値 &#x200B;] | 訪問者が検索実行に推奨値とオートコンプリート値のどちらを使用したかを示します。 |
| `autoCompleteTypedValue` | [!UICONTROL 文字列] | オートコンプリートのシナリオでは、ユーザーが検索を放棄して、ドロップダウンから特定の用語を選択することがあります。 この値は、提案された検索用語の特定のセットを生成するために、ユーザーが何を入力し始めたかを追跡します。 |
| `autoCompleteValue` | [!UICONTROL 文字列] | オートコンプリートのシナリオでは、ユーザーが検索を放棄して、ドロップダウンメニューから特定の用語を選択することがあります。 この値は、選択された特定の用語を追跡するために使用されます。 |
| `instances` | [!UICONTROL &#x200B; 整数 &#x200B;] | 内部サイト検索が発生した回数。 |
| `locationInPage` | [!UICONTROL 文字列] | ページ上に複数の検索ボックスが存在する場合、ユーザーが検索に使用した特定の場所を識別するために、この値を使用する必要があります。 |
| `nullInstances` | [!UICONTROL &#x200B; 整数 &#x200B;] | 結果がゼロの内部サイト検索が発生した回数。 |
| `numberOfResults` | [!UICONTROL &#x200B; 整数 &#x200B;] | 返された検索結果の合計数です。 |
| `postalCode` | [!UICONTROL 文字列] | 検索に使用される郵便番号（該当する場合）。 |
| `productFindingMethods` | [!UICONTROL 文字列] | マーチャンダイジングバインディングでの内部サイト検索語句の値。 この値は、商品が表示される直前に検索された語句を示します。 |
| `radiusDistance` | [!UICONTROL &#x200B; 整数 &#x200B;] | `radiusType` と組み合わせると、検索半径の選択した距離が示されます。 |
| `radiusType` | [!UICONTROL &#x200B; 整数 &#x200B;] | 選択した距離のタイプ（マイルまたはキ `radiusDistance` ロ）です。 |
| `refinementInstances` | [!UICONTROL &#x200B; 整数 &#x200B;] | 内部サイト検索が絞り込まれた回数。 |
| `refinementType` | 文字列の配列 | 検索結果に適用される絞り込みのタイプを一覧表示します。 例としては、部門、ブランド、価格、店舗、レビュー評価、色、素材などがあります。 |
| `refinementValue` | [!UICONTROL 文字列] | 検索が絞り込まれた値。 |
| `resultsPageNumber` | [!UICONTROL &#x200B; 整数 &#x200B;] | ページ分割された検索結果の場合、この値は、訪問者が表示している結果ページを追跡します。 |
| `resultsPerPage` | [!UICONTROL &#x200B; 整数 &#x200B;] | ページ分割された検索結果の場合、この値は、ページごとに表示される検索結果の数を追跡します。 |
| `searchType` | [!UICONTROL 文字列] | 実行中の検索方法をキャプチャします（該当する場合）。 例としては、先行入力検索、直接入力検索またはサイトが持つその他のカスタム検索機能があります。 |
| `sortOrder` | [!UICONTROL 文字列] | `sortType` と組み合わせると、検索結果の並べ替え順（昇順または降順）を示します。 |
| `term` | [!UICONTROL 文字列] | 訪問者が入力した内部サイト検索語句。 |

{style="table-layout:auto"}

データタイプについて詳しくは、[&#x200B; 公開 XDM リポジトリ &#x200B;](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/internal-site-search.schema.json) を参照してください。
