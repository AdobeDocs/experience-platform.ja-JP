---
title: Algolia タグ拡張機能の概要
description: Adobe Experience Platformの Algolia タグ拡張機能について説明します。
exl-id: 8409bf8b-fae2-44cc-8466-9942f7d92613
source-git-commit: 6eee26df3841a7829625361fc726bf59a278f867
workflow-type: tm+mt
source-wordcount: '1954'
ht-degree: 2%

---

# [!DNL Algolia] Tags 拡張機能の概要

[!DNL Algolia] タグ拡張機能を使用すると、マーケターはユーザーインタラクションデータを [!DNL Algolia] に送信するルールを簡単に設定でき、AI 検索および検出エクスペリエンスをよりパーソナライズできるようになります。

この拡張機能は、次のような主要な機能によって動作します。

* **[!DNL Algolia]Insights**: ユーザーインタラクションイベントを自動的にキャプチャして [!DNL Algolia] に送信します。これにより、強力な分析、パーソナライズされたエクスペリエンス、検索の関連性の向上を可能にします。

## 前提条件 {#prerequisites}

この拡張機能を使用するには、有効な [!DNL Algolia] アカウントが必要です。 アカウントをまだお持ちでない場合は、[[!DNL Algolia]  新規登録ページ &#x200B;](https://dashboard.algolia.com/users/sign_up) に移動してアカウントを作成してください。

### 必要な設定の詳細の収集 {#configuration-details}

[!DNL Algolia] をAdobe Experience Platformに接続するには、次の情報が必要です。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| アプリケーション ID | お使いのアプリケーション ID は、アプリ [&#x200B; ーションダッシュボードの「](https://www.algolia.com/account/api-keys/all)API キー [!DNL Algolia]」セクションにあります。 | 0ABCDEFG12 |
| 検索 API キー | 検索 API キーは、[&#x200B; ールダッシュボードの「](https://www.algolia.com/account/api-keys/all)API キー [!DNL Algolia]」セクションにあります。 | 1234a12345678901b1234567890c1ab1 |

## [!DNL Algolia] Insights 拡張機能のインストールと設定 {#install-configure}

[!DNL Algolia] Insights 拡張機能をインストールするには、[!UICONTROL Data Collection UI] ージに移動し、左側のナビゲーションから **[!UICONTROL Tags]** を選択します。 ここから、拡張機能を追加するプロパティを選択するか、代わりに新しいプロパティを作成します。

目的のプロパティを選択または作成したら、左側のナビゲーションで「**[!UICONTROL Extensions]**」を選択し、「**[!UICONTROL Catalog]**」タブを選択します。 [!DNL Algolia] Insights カードを検索し、「**[!UICONTROL Install]**」を選択します。

![](../../../images/extensions/client/algolia/install.png)

表示される設定ビューで、次の詳細を指定する必要があります。

| プロパティ | 説明 |
| --- | --- |
| [!UICONTROL Application ID] | 以前に収集した [!UICONTROL Application Id] を「[&#x200B; 設定の詳細 &#x200B;](#configuration-details)」セクションに入力します。 |
| [!UICONTROL Search API Key] | 以前に収集した [!UICONTROL Search API Key] を「[&#x200B; 設定の詳細 &#x200B;](#configuration-details)」セクションに入力します。 |
| [!UICONTROL Index Name] | [!UICONTROL Index Name] には、製品またはコンテンツが含まれます。  このインデックスは、デフォルトとして使用されます。 |
| [!UICONTROL User Token Data Element] | ユーザートークンを返すデータ要素。 |
| [!UICONTROL Authenticated User Token Data Element] | 認証済みユーザートークンを返すデータ要素を設定します。 |
| [!UICONTROL Currency Code] | ISO-4217 形式で通貨コード（USD、EUR など）を入力します。 このフィールドはデータ要素をサポートします。 |

![](../../../images/extensions/client/algolia/configure.png)

## [!DNL Algolia] Insights 拡張機能のアクションタイプ {#action-types}

[!DNL Algolia] れぞれ特定のコンテキストとプロパティを持つ、事前定義済みの一連の標準イベントをサポートしています。 [!DNL Algolia] 拡張機能で使用できるアクションはこれらのイベントタイプに一致するため、タイプに基づいて [!DNL Algolia] に送信するイベントを簡単に分類および設定できます。

### インサイトの読み込み {#load-insights}

>[!NOTE]
>
>ほとんどの場合、サイトのすべてのページに [!DNL Algolia] Insights を読み込むことをお勧めします。

ルールのコンテキストに基づいて **[!UICONTROL Load Insights]** インサイトを読み込む場合に最も理にかなっている場所に、タグルールに [!DNL Algolia] アクションを追加します。 `search-insights.js` ライブラリをページに読み込みます。

新しいタグルールを作成するか、既存のルールを開きます。 要件に従って条件を定義し、**[!UICONTROL Algolia]** として [!UICONTROL Extension] を選択し、**[!UICONTROL Load Insights]** として [!UICONTROL Action Type] を選択します。

| プロパティ | 説明 |
| --- | --- |
| [!UICONTROL Insight Library Version] | [!DNL Algolia] Insights のバージョン。 デフォルトは `2.17.3` です。 |
| [!UICONTROL User Opt Out Data Element] | ユーザーのトラッキング環境設定をキャプチャするデータ要素。 |
| [!UICONTROL Use User Token Cookie] | ユーザートークン Cookie の生成を許可する [!DNL Algolia] 合は、このチェックボックスをオンにします。 デフォルトでは、このオプションは `true` に設定されています。 |

![](../../../images/extensions/client/algolia/load-insights.png)

### クリック済み {#clicked}

タグのルールに **[!UICONTROL Click]** アクションを追加して、クリックされたイベントを [!DNL Algolia] に送信します。 新しいタグルールを作成するか、既存のルールを開きます。 要件に従って条件を定義し、**[!UICONTROL Algolia]** として [!UICONTROL Extension] を選択し、**[!UICONTROL Clicked]** として [!UICONTROL Action Type] を選択します。

| プロパティ | 説明 |
| --- | --- |
| [!UICONTROL Event Name] | このクリックイベントをさらに絞り込むために使用できるイベント名。 |
| [!UICONTROL Event Details Data Element] | データ要素は、次のような JSON 形式のイベントの詳細を返します。 <ul><li>`indexName`</li><li>`objectIDs`</li><li>`queryID` （オプション）</li><li>`positions` （オプション）</li><li>`price` （オプション）</li><li>`quantity` （オプション）</li><li>`discount` （オプション）</li><li>`objectData` （オプション）</li><li>`currency` （オプション）</li></ul> |


>[!NOTE]
>
>`queryID` と `positions` の両方が含まれる場合、イベントは **検索後にクリックされたオブジェクト ID** に分類されます。 それ以外の場合は、「クリックされたオブジェクト ID **イベントとしてクラス** されます。
><br>
>データ要素で `indexName` が指定されない場合、イベントが送信される際に **デフォルトのインデックス名** が使用されます。

![](../../../images/extensions/client/algolia/clicked.png)

イベントカテゴリについて詳しくは、[&#x200B; 検索後にクリックされたオブジェクト ID](https://www.algolia.com/doc/api-reference/api-methods/clicked-object-ids-after-search/) を参照してください。
と [&#x200B; クリックオブジェクト ID](https://www.algolia.com/doc/api-reference/api-methods/clicked-object-ids/) ガイド。

### 変換済 {#converted}

タグルールに **[!UICONTROL Converted]** アクションを追加して、変換後のイベントを [!DNL Algolia] に送信します。 新しいタグルールを作成するか、既存のルールを開きます。 要件に従って条件を定義し、**[!UICONTROL Algolia]** として [!UICONTROL Extension] を選択し、**[!UICONTROL Converted]** として [!UICONTROL Action Type] を選択します。

| プロパティ | 説明 |
| --- | --- |
| [!UICONTROL Event Name] | この **convert** イベントをさらに絞り込むために使用されるイベント名。 |
| [!UICONTROL Event Details Data Element] | データ要素は、次のようなイベントの詳細を返します。 <ul><li>`indexName`</li><li>`objectIDs`</li><li>`queryID` （オプション）</li><li>`recordID` （オプション）</li></ul> |

>[!NOTE]
>
>データ要素に `queryId` が含まれる場合、イベントは **検索後に変換済み** と分類されます。 そうでない場合は、**変換済み** イベントとして分類されます。
><br>
>データ要素で `indexName` が指定されない場合、イベントが送信される際に **デフォルトのインデックス名** が使用されます。

![](../../../images/extensions/client/algolia/converted.png)

イベント カテゴリの詳細については、「[&#x200B; 検索後の変換済みオブジェクト ID](https://www.algolia.com/doc/api-reference/api-methods/converted-object-ids-after-search/)」および [&#x200B; 変換済みオブジェクト ID](https://www.algolia.com/doc/api-reference/api-methods/converted-object-ids/) ガイドを参照してください。

### 買い物かごに追加 {#added-to-cart}

タグルールに **[!UICONTROL Added to Cart]** アクションを追加して、追加された買い物かごイベントを [!DNL Algolia] に送信します。 新しいタグルールを作成するか、既存のルールを開きます。 要件に従って条件を定義し、**[!UICONTROL Algolia]** として [!UICONTROL Extension] を選択し、**[!UICONTROL Added to cart]** として [!UICONTROL Action Type] を選択します。

| プロパティ | 説明 |
| --- | --- |
| [!UICONTROL Event Name] | この **買い物かごに追加** イベントをさらに絞り込むために使用されるイベント名。 |
| [!UICONTROL Event Details Data Element] | データ要素は、次のような JSON 形式のイベントの詳細を返します。 <ul><li>`indexName`</li><li>`objectIDs`</li><li>`objectData`</li><li>`price`</li><li>`quantity`</li><li>`discount` （オプション）</li><li>`queryID` （オプション）</li><li>`currency` （オプション）</li></ul>。 |

>[!NOTE]
>
>データ要素に「`queryId`」が含まれる場合、イベントは「**検索後に買い物かごのオブジェクト ID に追加** と分類されます。 そうでない場合は、**買い物かごオブジェクト ID に追加** イベントとして分類されます。
><br>
>データ要素で `indexName` が指定されない場合、イベントが送信される際に **デフォルトのインデックス名** が使用されます。
><br>
>デフォルトのデータ要素が要件を満たさない場合は、目的のイベントの詳細を返すカスタムのデータ要素を作成できます。

![](../../../images/extensions/client/algolia/added-to-cart.png)

イベントカテゴリについて詳しくは、[&#x200B; 検索後に買い物かごオブジェクト ID に追加 &#x200B;](https://www.algolia.com/doc/api-reference/api-methods/added-to-cart-object-ids-after-search/) および [&#x200B; 買い物かごオブジェクト ID に追加 &#x200B;](https://www.algolia.com/doc/api-reference/api-methods/added-to-cart-object-ids/) ガイドを参照してください。

### Purchased {#purchased}

タグルールに **[!UICONTROL Purchased]** アクションを追加して、購入したイベントを [!DNL Algolia] に送信します。 新しいタグルールを作成するか、既存のルールを開きます。 要件に従って条件を定義し、**[!UICONTROL Algolia]** として [!UICONTROL Extension] を選択し、**[!UICONTROL Purchased]** として [!UICONTROL Action Type] を選択します。

| プロパティ | 説明 |
| --- | --- |
| [!UICONTROL Event Name] | この **購入** イベントをさらに絞り込むために使用されるイベント名。 |
| [!UICONTROL Event Details Data Element] | データ要素は、次のような JSON 形式のイベントの詳細を返します。 <ul><li>`indexName`</li><li>`objectIDs`</li><li>`objectData`</li><li>`price`</li><li>`quantity`</li><li>`discount` （オプション）</li><li>`queryID` （オプション）</li><li>`currency` （オプション）</li></ul>。 |

>[!NOTE]
>
>購入したアクションは、購入したアイテム ID に基づいてブラウザーストレージからイベントデータを取得します。 購入したアイテムに保存されたデータに `queryID` が含まれている場合、イベントは **検索後の購入したオブジェクト ID** として分類されます。 そうでない場合は、**購入したオブジェクト ID** イベントとして分類されます。
><br>
>このアプローチにより、ユーザーが以前に品目とやり取りした内容から、関連するすべてのコンテキスト（クエリ ID、インデックス名、価格、数量、割引）を購入イベントに自動的に含めることができます。

![](../../../images/extensions/client/algolia/purchased.png)

イベントカテゴリについて詳しくは、[&#x200B; 検索後の購入したオブジェクト ID](https://www.algolia.com/doc/api-reference/api-methods/purchased-object-ids-after-search/) を参照してください
と [&#x200B; 購入済みオブジェクト ID](https://www.algolia.com/doc/api-reference/api-methods/purchased-object-ids/) ガイド。

### 表示済み {#viewed}

タグルールに **[!UICONTROL Viewed]** アクションを追加して、購入したイベントを [!DNL Algolia] に送信します。 新しいタグルールを作成するか、既存のルールを開きます。 要件に従って条件を定義し、**[!UICONTROL Algolia]** として [!UICONTROL Extension] を選択し、**[!UICONTROL Viewed]** として [!UICONTROL Action Type] を選択します。

| プロパティ | 説明 |
| --- | --- |
| [!UICONTROL Event Name] | この **表示** イベントをさらに絞り込むために使用されるイベント名。 |
| [!UICONTROL Event Details Data Element] | データ要素は、次のような JSON 形式のイベントの詳細を返します。 <ul><li>`indexName`</li><li>`objectIDs`</li></ul> |

>[!NOTE]
>
>データ要素で `indexName` が指定されない場合は、イベントの送信時に **デフォルトのインデックス名** が使用されます。

![](../../../images/extensions/client/algolia/viewed.png)

表示イベントについて詳しくは、「[&#x200B; 表示されたオブジェクト ID](https://www.algolia.com/doc/api-reference/api-methods/viewed-object-ids/)」ガイドを参照してください。

## [!DNL Algolia] Insights 拡張機能のデータ要素 {#data-elements}

[!DNL Algolia] は、それぞれ特定のコンテキストとプロパティを持つ、事前定義済みの一連のデータ要素をサポートしています。 以下の節では、[!DNL Algolia] Insights 拡張機能で使用できるデータ要素について説明します。

### DataSet {#dataset}

DataSet Data Element は、HTML要素に関連付けられたデータを取得し、そのデータを [!DNL Algolia] のアクションで使用します。 このデータ要素は、取得したイベントデータを後で使用するために（コンバージョンや購入イベントなど）、ブラウザーストレージに自動的に保存します。

**一般設定：**

| プロパティ | 説明 |
| --- | --- |
| [!UICONTROL Hit Element Div/Class Name] | HTML要素名や、HTML要素の `data-insights-object-id`、オプションで `data-insights-query-id` および `data-insights-position` を含むデータセット属性を含む CSS クラス名。 |
| [!UICONTROL Index Name Element Div/Class Name] | HTML要素のデータセット属性（`data-indexname`）を持つHTML要素名や CSS クラス名。 |

**Commerceの構成（オプション）:**

| プロパティ | 説明 |
| --- | --- |
| [!UICONTROL Price Data Element] | 品目の価格を返すデータ要素。 指定した場合、これはコマースイベント用に保存されたイベントデータに含まれます。 |
| [!UICONTROL Quantity Data Element] | 品目の数量を戻すデータ要素。 指定しない場合のデフォルトは 1 です。 |
| [!UICONTROL Discount Data Element] | 品目の割引小数値を返すデータ要素。 |
| [!UICONTROL Currency Code] | ISO-4217 形式の通貨コード。 通貨コードが指定されていない場合、拡張機能の設定のデフォルト通貨が使用されます。 |

**上書き（任意）:**

これらのフィールドを使用すると、HTML データセット属性からデータを取得するデフォルトの動作を上書きできます。

| プロパティ | 説明 |
| --- | --- |
| [!UICONTROL Record ID Data Element] | ページ URL をレコード ID として使用するデフォルトのアプローチを上書きします。 レコード ID は、この製品/ページの [!DNL Algolia] に送信するデータを保存および検索するために使用されます。 |
| [!UICONTROL Query ID Data Element] | クエリ ID は、HTML要素のデータセットから取得されます。 この動作をオーバーライドするには、このプロパティを使用して、クエリ ID を文字列として返すデータ要素を指定します。 |
| [!UICONTROL Object IDs Data Element] | オブジェクト ID は、HTML要素のデータセットから取得されます。 この動作をオーバーライドするには、このプロパティを使用して、オブジェクト ID を配列として返すデータ要素を指定します。 |
| [!UICONTROL Positions Data Element] | ポジションは、HTML要素のデータセットから取得されます。 この動作をオーバーライドするには、このプロパティを使用して、位置を配列として返すデータ要素を指定します。 |
| [!UICONTROL Index Name Data Element] | インデックス名は、HTML要素のデータセットから取得されます。 この動作をオーバーライドするには、このプロパティを使用して、インデックス名を文字列として返すデータ要素を指定します。 |

![](../../../images/extensions/client/algolia/dataset.png)

このデータ要素は次を返します。

```javascript
{
  timestamp,
  queryID,
  indexName,
  objectIDs,
  positions,
  objectData,  // Optional: commerce data if price is provided
  currency,    // Optional: if provided
  recordID
}
```

データセットを含むHTMLの例：

```html
<div data-indexname="acme_master_default_products" class="instant-search-comp__hits">
  <div class="hit-card"
    data-insights-object-id="${hit.objectID}"
    data-insights-position="${hit.__position}"
    data-insights-query-id="${hit.__queryID}">
    <h4 class="hit-name">...</h4>   
  </div>
</div>
```

### クエリ文字列 {#query-string}

クエリ文字列データ要素は、URL クエリ文字列からデータを抽出し、[!DNL Algolia] のアクションで使用します。

| プロパティ | 説明 |
| --- | --- |
| [!UICONTROL Object ID Param Name] | オブジェクト ID を含むクエリパラメーター名。 |
| [!UICONTROL Index Name Param Name] | インデックス名を含んだクエリパラメーター名。 |
| [!UICONTROL Query ID Param Name] | クエリ ID を含むクエリパラメーター名。 |
| [!UICONTROL Position Param Name] | Position を含むクエリパラメーター名。 |

![](../../../images/extensions/client/algolia/query-string.png)

このデータ要素は次を返します。

```javascript
{
  timestamp,
  queryID,
  indexName,
  objectIDs,
  positions
}
```

クエリパラメーターを含むHTMLの例は次のとおりです。

```html
<a href="product.html?objectID=${hit.objectID}&queryID=${hit.__queryID}&indexName=${indexName}&position=${hit.position}">Read More</a>
```

### ストレージ {#storage}

ストレージデータ要素は、[!DNL Algolia] のアクションで使用するために、ブラウザーセッションストレージからデータを取得します。 このデータ要素は、追加のコマース情報で保存されたデータを強化するためにも使用できます。

このデータ要素は、以前にセッションストレージに保存されたイベントの詳細を取得します（通常、クリックイベント中に DataSet データ要素によって取得されます）。 データは、明示的に無効にしない限り、コンバージョンイベント中に自動的に削除されます。

**上書き（任意）:**

| プロパティ | 説明 |
| --- | --- |
| [!UICONTROL Record ID Data Element] | レコード ID は、ブラウザーストレージに保存されているイベントデータを検索するためのキーとして使用されます。 デフォルトのレコード ID はページ URL です。 この動作をオーバーライドするには、このプロパティを使用して、レコード ID を文字列として返すデータ要素を指定します。 |
| [!UICONTROL Price Data Element] | 品目の価格を返すデータ要素。 指定した場合、保存されたイベントデータが価格情報で更新されます。 |
| [!UICONTROL Quantity Data Element] | 品目の数量を戻すデータ要素。 指定した場合、保存されたイベントデータが数量情報で更新されます。 |
| [!UICONTROL Discount Data Element] | 品目の割引小数値を返すデータ要素。 指定した場合、保存されたイベントデータが割引情報で更新されます。 |
| [!UICONTROL Currency Code] | ISO-4217 形式で通貨コードを入力します。 指定した場合、保存されたイベントデータが通貨情報で更新されます。 |

![](../../../images/extensions/client/algolia/storage.png)

このデータ要素は、拡張されたコマースデータを含め、セッションストレージに保存されているものを返します。

```javascript
{
  timestamp,
  queryID,
  indexName,
  objectIDs,
  positions,      // If available from original event
  objectData,     // Optional: commerce data if price is provided
  currency,       // Optional: if provided
  recordID
}
```

## 検索後にクリックまたは変換 {#clicked-converted-after-search}

*検索後にクリック済み* または *検索後に変換済み* イベントには `queryID` が必要です。また、`positions` 検索後にクリック済み *にも* が必要です。 これらのプロパティは、InstantSearch やオートコンプリートのクエリパラメーターで `insights` フラグが有効な場合に使用できます。 サイトに関するインサイトの設定方法については、次のリソースを参照してください。

* [&#x200B; オートコンプリートでのインサイトの設定 &#x200B;](https://www.algolia.com/doc/ui-libraries/autocomplete/api-reference/autocomplete-js/autocomplete/#param-insights)
* [InstantSearch.js での Insights の設定 &#x200B;](https://www.algolia.com/doc/guides/building-search-ui/events/js/#set-the-insights-option-to-true)
* [&#x200B; クリックおよびコンバージョンイベントの概要 &#x200B;](https://www.algolia.com/doc/guides/sending-events/implementing/how-to/sending-events-backend/)
* [Sending [!DNL Algolia] Insights イベント &#x200B;](https://www.algolia.com/doc/ui-libraries/autocomplete/guides/sending-algolia-insights-events/)
* [[!DNL Algolia] Launch 拡張機能 GitHub リポジトリ &#x200B;](https://github.com/algolia/algolia-launch-extension)
* [InstantSearch.js のドキュメント &#x200B;](https://www.algolia.com/doc/guides/building-search-ui/what-is-instantsearch/js/)
* [[!DNL Algolia] Insights API ドキュメント &#x200B;](https://www.algolia.com/doc/rest-api/insights/)
* [Algolia Launch 拡張機能コードリポジトリ &#x200B;](https://github.com/algolia/algolia-launch-extension)

## 次の手順 {#next-steps}

このガイドでは、[!DNL Algolia] タグ拡張機能を使用して [!DNL Algolia Insights] にデータを送信する方法について説明しました。 サーバーサイドのイベントも [!DNL Algolia] に送信することを計画している場合は、[[!DNL Conversions API]  イベント転送拡張機能 &#x200B;](../../server/algolia/overview.md) のインストールと設定に進むことができます。

Experience Platformのタグについて詳しくは、[&#x200B; タグの概要 &#x200B;](../../../home.md) を参照してください。
