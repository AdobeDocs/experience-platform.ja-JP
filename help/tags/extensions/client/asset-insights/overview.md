---
title: AEM Asset Insights 拡張機能の概要
description: Adobe Experience Platform の AEM Asset Insights タグ拡張機能について説明します。
exl-id: 7d3edd42-09fe-4e40-93dc-1edd2fdbb121
source-git-commit: 88939d674c0002590939004e0235d3da8b072118
workflow-type: tm+mt
source-wordcount: '1118'
ht-degree: 100%

---

# AEM Asset Insights 拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

この拡張機能は、[AEM Asset Insights](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/touch-ui-configuring-asset-insights.html?lang=ja) と共に使用することを意図しています。具体的には、「pageTracker」プロセスと埋め込みコードが置き換えられます。設定すると、この拡張機能によって Asset *Impression* と *Click* 指標が Adobe Analytics に送信され、その後、AEM Asset Insights レポートに読み込まれます。その後、AEM Asset Insights または Adobe Analytics プロジェクトの Workspace を使用して、アセット指標をレポートできます。

## 拡張機能の前提条件

### Analytics

Analytics の AEM Asset レポートには、次の 3 つのディメンションが含まれます。

* アセット ID
* アセットソース
* アセットのクリック

また、次の 2 つの指標があります。
* アセットのインプレッション
* アセットのクリック

この拡張機能を使用してレポートを入力するには、Analytics Administrator（**[!UICONTROL Analytics]／[!UICONTROL 管理]／[!UICONTROL レポートスイート]／`<report suite>`／[!UICONTROL 設定を編集]／[!UICONTROL AEM]／[!UICONTROL AEM Assets レポート]**）を使用してこれらのレポートを有効にする必要があります。

Adobe Experience Platform 用の「*Adobe Analytics*」タグ拡張機能は、同じ Web プロパティにインストールする必要があります。

### Adobe Experience Manager（AEM）

1. 「[AEM Asset Insights](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/touch-ui-configuring-asset-insights.html)」を有効にします。AEM で、**[!UICONTROL ツール／Assets]** を選択し、**[!UICONTROL インサイト設定]**&#x200B;パネルを開きます。

1. 「UUID トラッキング」を無効にします。

   >[!IMPORTANT]
   >
   >AEM Asset 設定の「**[!UICONTROL UUID の追跡を無効化]**」がオンになっている場合、この拡張機能は&#x200B;*機能しません*。デフォルトでは選択解除されています。

   ![UUID トラッキングの無効化](images/disableassets.jpg)

## Adobe Experience Manager（AEM）の設定

ここでは、Adobe Experience Platform でタグを使用して AEM を設定する方法、AEM で Asset Insight を有効にする方法、およびアセット の UUID トラッキングを有効にする方法について説明します。

### AEM とタグの統合

[Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=ja) を Adobe Experience Manager と統合する場合、Adobe I/O を使用して実行することをお勧めします。

1. [Adobe I/O を使用して AEM をタグに接続します](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/connect-aem-launch-adobe-io.html?lang=ja)。

2. [Adobe Experience Platform Cloud Service 設定を作成します](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/create-launch-cloud-service.html?lang=ja)。

### AEM での Asset Insight の有効化

Asset Insights を有効にする手順については、[Experience Manager 6.5 Assets ユーザーガイド](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/touch-ui-configuring-asset-insights.html)を参照してください。

### アセットの UUID トラッキングの有効化

AEM でアセットの UUID を使用して、Analytics のアセットをトラッキングします。

アセットの UUID を使用したトラッキングを有効にするには、編集可能なテンプレートのコンポーネントポリシーコンソールを開いて、「UUID の追跡を無効化」プロパティをオフにします（デフォルトでは、このプロパティは、OOTB 画像コンポーネント用にオンになっています）。

![](images/uuid.png)

UUID を有効にしたら、「data-asset-id」データ要素にアセットの UUID が入力されていることを確認する必要があります。Analytics は、このUUID を使用してアセットクリックまたはインプレッションをトラッキングします。

![](images/uuid-code.png)

## 拡張機能の使用

この拡張機能には 2 つのイベントと 1 つのアクションがあります。

* **アセットのクリック：**&#x200B;トラッキングが有効になっていて、宛先（href 属性）を持つ AEM Asset を訪問者が選択したときに発生する&#x200B;_イベント_。

* **アセットのクリック（宛先なし）：**&#x200B;トラッキングが有効になっていて、宛先を持たない（href 属性なし）AEM Asset を訪問者が選択したときに発生する&#x200B;_イベント_。

* **AA 変数の設定**：使用されたイベントと、イベントとアクションの設定方法に応じて、AEM Assets 用に予約された Analytics 変数（コンテキストデータ変数である`a.assets.source`、`a.assets.idlist`、`a.asset.clickedid`）を設定する&#x200B;_アクション_。この拡張機能では、Analytics のイベント、prop、eVar は使用されません。

### アセットのインプレッション

すべてのページで実行され、Analytics イメージリクエストを送信する新規または既存のタグルールに「AA 変数の設定」アクションを追加します。「AA 変数の設定」アクションは、「Adobe Analytics - ビーコンの送信」アクションの&#x200B;**前**&#x200B;に記述する必要があります。必要に応じて、追加のアクションを追加できます。

**[AA 変数の設定]**&#x200B;設定ページで、「**[表示されたアセット]**」（デフォルト）オプションを選択します。これは、訪問者が実際に表示するアセットのインプレッションイベントのみを設定します。

>[!NOTE]
>
>「AA 変数の設定」アクションでは、「loaded」オプションもサポートされます。このオプションは、訪問者がアセットを表示したかどうかに関係なく、ページ上のすべてのアセットに対するアセットのインプレッションを送信します。

![インプレッション](images/sendImpressions.jpg)


### アセットのクリック

「アセットのクリック」イベントと「AA 変数の設定」アクションを使用して、2 つ目のルールを設定します。「アセットのクリック」イベントは、「アセットのクリック - イメージリクエスト」が「ページロード時」（デフォルト）に設定されるように設定する必要があります。アセット ID は後続のインプレッションルールで `sessionStorage` に保存および送信されるので、このルールでは、Adobe Analytics のアクション（ビーコンの送信など）は必要ありません。

「アセットのクリック」イベントでは、「クリック時」の「アセットのクリック - イメージリクエスト」設定もサポートされています。これにより、クリック指標が即座に Analytics に送信され、Analytics の「ビーコンの送信」アクションも必要になります。

![ページの読み込み時のアセットのクリック](images/sendClickOnPageload.jpg)

リンク先を持たない（`href` 属性なし）ページにアセットが存在する場合に実行する 3 つ目のルールを設定します。少なくとも、新しいルールでは、「アセットのクリック（宛先なし）」イベントと、「AA 変数の設定」および「Adobe Analytics — ビーコンの送信」アクションを使用する必要があります。必要に応じて、追加の条件とアクションを追加できます。

![アセットのクリック（宛先なし）](images/sendClickOnClickNoDestination.jpg)

### 拡張機能のテストに関するヒント

上記の説明に従って、3 つのルールを設定します。

* アセットのインプレッション
* アセットのクリック
* アセットのクリック（宛先なし）

**インプレッション**

1. AEM アセットを含むページに移動します。

1. ブラウザーにアセットが表示されない場合は、1 つ以上のアセットが表示されるまでスクロールしてそのアセットを選択するか、別のページに移動します。

1. Analytics イメージリクエストを確認します。

   前のページに表示されたアセット ID が `a.assets.idlist` に含まれている場合は、ルールが正しく動作しています。

   `a.assets.idlist` がイメージリクエストに含まれていない場合、次の 2 つの理由のうちの 1 つが考えられます。

   * ブラウザーの表示領域にアセットがありませんでした

   * AEM で [Asset Insights](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/touch-ui-configuring-asset-insights.html) が有効になっているページにアセットが設定されていませんでした。

**クリック数**

1. AEM アセットを含むページに移動します。

1. いずれかのアセットを選択します。

（次のページからの）結果の Analytics イメージリクエストで、宛先ページのアセット ID が `a.assets.idlist` に含まれ、元のページで選択されたアセットのアセット ID が `a.assets.clickedid` に含まれる場合、ルールは正しく動作しています。

`a.assets.clickedid` がイメージリクエストに含まれていない場合、選択したアセットの[アセットインサイト](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/touch-ui-configuring-asset-insights.html)が AEM で有効になっていないことがほとんどです。

**クリック（宛先なし）**

1. 宛先のない（`href` 属性のない）1 つ以上の AEM アセットを含むページに移動します。

1. そのアセットを選択します。

結果の Analytics イメージリクエストで、`a.assets.clickedid` にアセット ID がある場合、ルールは正しく動作しています。

`a.assets.clickedid` がイメージリクエストに含まれていない場合、選択されたアセットの[アセットインサイト](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/touch-ui-configuring-asset-insights.html)が AEM で有効になっていないことがほとんどです。
