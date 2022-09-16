---
title: 内部サイト検索データタイプ
description: このドキュメントでは、内部サイト検索 XDM データタイプの概要を説明します。
exl-id: 3cab9445-f641-4a44-9699-cd8a62da8a61
source-git-commit: f5df893260f0772ad54ccdb00d99ed8f328d35a9
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 7%

---

# [!UICONTROL 内部サイト検索] データタイプ

[!UICONTROL 内部サイト検索] は、関連するすべての検索動作や詳細を含む、内部サイト検索を表す標準的な XDM データ型です。

![](../images/data-types/internal-site-search.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `autoCompleteClicked` | [!UICONTROL ブール値] | 訪問者が推奨検索値を使用したか、自動入力検索値を使用して検索を実行したかを示します。 |
| `autoCompleteTypedValue` | [!UICONTROL 文字列] | オートコンプリートの場合、ユーザーは検索を放棄し、ドロップダウンから特定の語句を選択することがあります。 この値は、ユーザーが入力し始めた内容を追跡して、推奨検索用語の特定のセットを生成します。 |
| `autoCompleteValue` | [!UICONTROL 文字列] | オートコンプリートの場合、ユーザーは検索を放棄し、ドロップダウンメニューから特定の語句を選択することがあります。 この値は、選択した特定のキーワードを追跡するために使用されます。 |
| `instances` | [!UICONTROL 整数] | 内部サイト検索が発生した回数。 |
| `locationInPage` | [!UICONTROL 文字列] | ページ上に複数の検索ボックスが存在する場合、この値を使用して、ユーザーが検索に使用した特定の場所を識別する必要があります。 |
| `nullInstances` | [!UICONTROL 整数] | 結果がゼロの内部サイト検索が発生した回数。 |
| `numberOfResults` | [!UICONTROL 整数] | 返された検索結果の合計数。 |
| `postalCode` | [!UICONTROL 文字列] | 検索に使用する郵便番号（該当する場合）。 |
| `productFindingMethods` | [!UICONTROL 文字列] | マーチャンダイジングバインディングを持つ内部サイト検索用語値。 この値は、製品を表示する直前に検索された用語を示します。 |
| `radiusDistance` | [!UICONTROL 整数] | 組み合わせ先 `radiusType`は、選択した検索半径の距離を示します。 |
| `radiusType` | [!UICONTROL 整数] | 選択した距離のタイプ： `radiusDistance`マイルまたはキロメートル。 |
| `refinementInstances` | [!UICONTROL 整数] | 内部サイト検索が絞り込まれた回数。 |
| `refinementType` | 文字列の配列 | 検索結果に適用される絞り込みのタイプをリストします。 例としては、部門、ブランド、価格、店舗、レビュー評価、色、素材などがあります。 |
| `refinementValue` | [!UICONTROL 文字列] | 検索の絞り込み先の値。 |
| `resultsPageNumber` | [!UICONTROL 整数] | ページ分割された検索結果の場合、この値は、訪問者が表示している結果のページを追跡します。 |
| `resultsPerPage` | [!UICONTROL 整数] | ページ分割された検索結果の場合、この値は、1 ページに表示された検索結果の数を追跡します。 |
| `searchType` | [!UICONTROL 文字列] | 実行される検索方法をキャプチャします（該当する場合）。 例としては、先行入力検索、直接入力検索、またはサイトに含まれるその他のカスタム検索機能などがあります。 |
| `sortOrder` | [!UICONTROL 文字列] | 組み合わせ先 `sortType`は、検索結果の並べ替え順を昇順または降順で示します。 |
| `term` | [!UICONTROL 文字列] | 訪問者が入力した内部サイト検索用語。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/internal-site-search.schema.json).
