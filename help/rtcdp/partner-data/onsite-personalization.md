---
title: パートナー支援による訪問者認識を使用して、不明な訪問者に対するオンサイトエクスペリエンスをパーソナライズ
description: パートナー支援による訪問者認識を使用して、パーソナライズされたオンサイトエクスペリエンスを訪問者に提供する方法を説明します。
exl-id: 99677988-1df8-47b1-96b1-0ef6db818a1d
source-git-commit: 77eb2492259cc813db2d6c514d123fa2ad698f67
workflow-type: tm+mt
source-wordcount: '2696'
ht-degree: 91%

---

# パートナー支援による訪問者認識を使用して、不明な訪問者に対するオンサイトエクスペリエンスをパーソナライズ

>[!AVAILABILITY]
>
>この機能は、Real-Time CDP(App Service)、Adobe Experience Platform Activation、Real-Time CDP、Real-Time CDP Prime、Real-Time CDP Ultimate のライセンスを持つお客様が利用できます。 これらのパッケージについて詳しくは、[製品の説明](https://helpx.adobe.com/jp/legal/product-descriptions.html)を参照し、アドビ担当者にお問い合わせください。

パートナー支援による認識を使用して、パーソナライズされたエクスペリエンスを web プロパティ訪問者に提供する方法を説明します。このチュートリアルを使用すると、Experience Platform および他の Experience Cloud ソリューションにおける様々な要素の実装シーケンスを理解して、パーソナライズされたエクスペリエンスを認証済みおよび未認証の訪問者に表示できます。

![パートナーが提供する属性を使用して、パーソナライズされたエクスペリエンスを訪問者に提供する方法を説明するインフォグラフィック。](/help/rtcdp/assets/partner-data/onsite-personalization/onsite-personalization-overview.png)

## この使用例を検討する理由 {#why-this-use-case}

消費者が様々な方法でブランドとやり取りする際のデジタルエクスペリエンスの断片化は非常に現実的で、解決がより難しくなっています。 まとまったエクスペリエンス、ターゲットを絞ったレコメンデーション、およびカスタマイズされたインタラクションに対する最高の顧客エンゲージメント戦略は、すべてユーザーの認識によって制限されます。

パートナーが支援するリアルタイム認識が、意味のある違いを生み出す場所です。 Adobeにより、ID パートナーは、高度なクライアント側のデータ収集および市場最先端のエクスペリエンス最適化サービスにプラグインし、以前の履歴や認証なしに、初回訪問以降のエクスペリエンス配信の範囲を効果的に拡大できます。

これは、消費者パッケージ商品やオンライン小売など、低い認証率を持つ業種にとって特に役立ちます。

## ある業界の例 {#industry-example}

例として、認証率が低いホームセンターブランドについて考えてみます。このブランドでは、以前の履歴や認証がなくても、また、サードパーティのクッキーに依存することなく、パーソナライズされたエクスペリエンスを初めての訪問者に提供したいと考えています。

このブランドでは、パートナーの認識技術を活用して、訪問者を確率論的に認識し、よりパーソナライズされたエクスペリエンスを提供することにしました。これは、訪問者がマーケティングファネルを下に移動する過程での事前の検討に役立ちます。例えば、最近引っ越した人にアピールするオンサイトコンテンツにパートナー提供のデモグラフィックシグナルを使用したり、人気の DIY 製品の割引を行ったりすることもあるでしょう。

## 前提条件と計画 {#prerequisites-and-planning}

パートナー提供の属性を使用して、パーソナライズされたエクスペリエンスを認証済みおよび未認証の訪問者に提供することを計画する際は、計画プロセスで次の前提条件を考慮します。

* パートナーの認識技術では、追加の属性に重ねることができるように、どのような入力を想定しているか。
* 決定論的に確認された属性ではなく、確率論的に導き出された属性に基づいて、様々なチャネルや様々なユースケースでパーソナライゼーションを提供することに、どの程度慣れているか。
* 認証前の認識済み訪問者に対するエクスペリエンスは、認証後にどのように変更されるべきか。

### 使用する UI 機能、プラットフォームコンポーネントおよび Experience Cloud 製品 {#ui-functionality-and-elements}

このユースケースをうまく実装するには、Real-time Customer Data Platform や他の Experience Cloud ソリューションの複数の領域を使用する必要があります。これらすべての領域に必要な[属性ベースのアクセス制御権限](/help/access-control/abac/overview.md)があることを確認するか、必要な権限の付与をシステム管理者に依頼してください。

* データ収集
   * [Adobe Experience Platform Web SDK](/help/edge/home.md)
   * [タグ](/help/tags/home.md)
   * [データストリーム](/help/datastreams/overview.md)
* Real-Time CDP におけるデータ管理
   * [ID](/help/identity-service/namespaces.md)
   * [スキーマ](/help/xdm/home.md)
   * [データ使用ラベル](/help/data-governance/labels/overview.md)
   * [データセット](/help/catalog/datasets/overview.md)
* Web プロパティのパーソナライゼーション
   * [エッジセグメント化](/help/segmentation/ui/edge-segmentation.md)
   * [エッジパーソナライゼーション宛先](/help/destinations/destination-types.md#edge-personalization-destinations)
   * [Adobe Target](/help/destinations/catalog/personalization/adobe-target-connection.md)（または、任意のパーソナライゼーションプラットフォーム。このユースケースのチュートリアルでは、パーソナライゼーションエンジンとして Adobe Target を使用）

## ビデオチュートリアル {#video-walkthrough}

不明な訪問者に対するオンサイトエクスペリエンスのパーソナライズ方法に関するチュートリアルについては、以下のビデオチュートリアルを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/3423076/?learn=on)

## ユースケースの達成方法：概要 {#achieve-the-use-case-high-level}

![パートナー提供の属性を使用して、パーソナライズされたエクスペリエンスを訪問者に提供する方法を説明するインフォグラフィック。](/help/rtcdp/assets/partner-data/onsite-personalization/onsite-personalization-steps.png)

1. **顧客**&#x200B;が、匿名の web サイト訪問者に関するインサイトをリアルタイムで取得する機能のライセンスを&#x200B;**データパートナー**&#x200B;から取得します。
2. **顧客**&#x200B;が、**パートナーの** API を呼び出すためにクライアントサイドライブラリをプロパティにデプロイし、パートナー提供のシグナルを Real-Time CDP に送信するように Web SDK またはモバイル SDK を設定します。
3. Web サイトまたはアプリのブラウジング時に、**訪問者**&#x200B;が&#x200B;**パートナー**&#x200B;によって確率論的に認識され、パートナーから ID と共に属性が返されます。
4. Real-Time CDP が、エッジセグメント化を実行して受信イベントのヒットを評価し、[ECID 識別子](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja)と対照して結果を保持します。
5. Adobe Target が、エッジセグメント化の出力を使用して&#x200B;**訪問者**&#x200B;にエクスペリエンスをレンダリングし、セッション内でのパーソナライゼーションを実現します。
6. 分析やリターゲティングなどのダウンストリームワークフローについて、イベント全体が保持されます。

## ユースケースの達成方法：手順 {#step-by-step-instructions}

上記の概要の各手順を完了するには、詳しいドキュメントへのリンクを含む以下の節を参照してください。

### データ管理：ID 名前空間、スキーマおよびデータセットを作成して、データ属性を管理します。 {#data-management}

未認証訪問者のエクスペリエンスをパーソナライズするユースケースを実現するための準備として、まず、リアルタイム受信イベントとオーディエンス選定データを受け取るためのデータ管理構造を Real-Time CDP でセットアップする必要があります。

#### パートナー ID の ID 名前空間の作成

まず、パートナー ID の ID 名前空間を作成する必要があります。左側のパネルで **[!UICONTROL 顧客]**／**[!UICONTROL ID]** に移動し、画面の右上隅にある「**[!UICONTROL ID 名前空間の作成]**」を選択します。

![パートナー ID が強調表示された「ID 名前空間の作成」ダイアログ。](/help/rtcdp/assets/partner-data/onsite-personalization/create-identity-namespace.png)

詳しくは、[パートナー ID の ID 名前空間の作成](/help/rtcdp/partner-data/prospecting.md#create-partner-id-namespace)を参照してください。

#### スキーマの作成

次に、後で web プロパティから収集する時系列データを保持するために、[!UICONTROL エクスペリエンスイベント]スキーマを作成します。そのスキーマの基本クラスには、[!UICONTROL XDM ExperienceEvent] を使用します。[Experience Platform の UI を使用したスキーマの作成](/help/xdm/ui/resources/schemas.md#create)を参照してください。

![スキーマの作成と XDM エクスペリエンスイベントが強調表示されたスキーマワークスペース。](/help/rtcdp/assets/partner-data/onsite-personalization/create-experience-event-schema.png)

スキーマを作成して[フィールドグループをスキーマへ追加](/help/xdm/ui/resources/schemas.md#add-field-groups)する場合は、「[Web ページにアクセス](/help/xdm/field-groups/event/web-details.md)」と [ID マップ](/help/xdm/field-groups/profile/identitymap.md)のフィールドグループを追加することを検討します。これは、デジタル資産やデータ収集の実践に適用される他のフィールドグループに加えて行われるものです。

さらに、既存のフィールドグループを作成または再利用してスキーマに追加することにより、訪問者に関するパートナー提供のインサイトを取り込むこともできます。[フィールドグループの作成](/help/xdm/ui/resources/field-groups.md)方法と、フィールドグループへの[フィールドの追加](/help/xdm/ui/resources/field-groups.md)方法を参照してください。例えば、年齢層、雇用形態、月々の購買力、購買行動など、パートナーが提供するインサイトに合わせてパーソナライズする場合は、フィールドグループに適切なフィールドを含めます。

データパートナーが訪問者に安定した識別子を提供し、それを Real-Time CDP に取り込みたい場合は、カスタムフィールドグループにその識別子用の適切な名前のフィールドがあることを確認します。また、前に作成した ID 名前空間で、そのフィールドを ID としてマークする必要があります。[プロファイルに含めるスキーマを有効にすることも必要です](/help/xdm/ui/resources/schemas.md#profile)。

#### データセットの作成

次に、web プロパティの訪問者から収集してオンサイトのパーソナライズ機能に使用する、時系列データを保持するデータセットを作成する必要があります。

[データセットの作成方法](/help/catalog/datasets/user-guide.md#create)に関するチュートリアルを読み、スキーマからデータセットを作成するオプションを忘れずに選択してください。前の手順で作成したスキーマに基づいてデータセットを作成します。

スキーマを作成するときの手順と同様に、データセットを[!UICONTROL リアルタイム顧客プロファイル]に含めるようにする必要があります。[!UICONTROL リアルタイム顧客プロファイル]でデータセットを使用できるようにする方法について詳しくは、[スキーマ作成チュートリアル](/help/xdm/tutorials/create-schema-ui.md#profile)を参照してください。

### Web プロパティでのイベントデータ収集の実装 {#implement-data-collection}

データ管理を設定したら、web プロパティにリアルタイムのイベント[データ収集](/help/collection/home.md)を実装する必要があります。リアルタイムのイベント呼び出しを収集して Real-Time CDP に送り返すには、Adobe データ収集ライブラリ（[!UICONTROL Web SDK]）を使用してプロパティをインストルメント化する必要があります。この項目は、いくつかのデータ収集コンポーネントをまたいだいくつかの個別のタスクで構成されます。

>[!IMPORTANT]
>
>パートナー提供の属性を取得するには、*web プロパティをパートナー API や他のメソッドと統合して、データパートナーからリアルタイムで属性を呼び出して取得する必要があります。*&#x200B;この点については、このチュートリアルの対象ではないため、パートナーと話し合ってください。

まず、画面の右上隅にあるアプリケーション切り替えボタンを使用して、「**[!UICONTROL データ収集]**」セクションに移動します。

>[!TIP]
>
>アプリケーション切り替えボタンで「[!UICONTROL データ収集]」が表示されない場合は、システム管理者に連絡してアクセス権を依頼してください。

![データ収集セクションに移動する、アプリ切り替えボタン。](/help/rtcdp/assets/partner-data/onsite-personalization/app-switcher-data-collection.png)

UI の&#x200B;**[!UICONTROL データ収集]**&#x200B;セクションは、以下の画像のようになります。

![Platform UI のデータ収集セクション。](/help/rtcdp/assets/partner-data/onsite-personalization/data-collection-home.png)

#### データストリームを作成

データ収集セクションの最初の手順として、[新しいデータストリームを作成](/help/datastreams/configure.md)します。データがどのように収集され、適切な Adobe アプリ（この場合は Experience Platform）に正しくルーティングされるかは、データストリームに基づいています。

データストリームを作成する際に、「**[!UICONTROL イベントスキーマ]**」フィールドで、以前に作成したスキーマを選択します。

![新しいデータストリームを設定する際に強調表示されたイベントスキーマセレクター。](/help/rtcdp/assets/partner-data/onsite-personalization/event-schema-selector-datastream.png)

先ほど作成した[イベントデータセットをドロップダウンから選択](/help/datastreams/configure.md#aep)し、「**[!UICONTROL エッジ セグメント化]**」と「**[!UICONTROL パーソナライズ機能の宛先]**」の横のボックスをオンにして、「**[!UICONTROL 保存]**」を選択します。

イベントベースの時系列データを取り込むので、このシナリオではプロファイルデータセットを選択する必要はありません。

#### タグプロパティの作成

プロパティは、サイトにタグを導入する際に、拡張機能、ルール、データ要素およびライブラリを入力するコンテナと考えてください。

「**[!UICONTROL タグ]**」に移動し、「**[!UICONTROL 新しいプロパティ]**」を選択します。

![新しいタグプロパティを作成します。](/help/rtcdp/assets/partner-data/onsite-personalization/create-tag-property.png)

必須フィールドに入力し、「**[!UICONTROL 保存]**」を選択します。

![新しいプロパティの必須フィールドに入力します。](/help/rtcdp/assets/partner-data/onsite-personalization/tag-property-fields.png)

[タグプロパティを作成する](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=ja)方法に関する完全な情報を取得します。

次に、プロパティ内に様々な拡張機能をインストールする必要があります。タグプロパティを選択し、「[!UICONTROL 拡張機能]」セクションに移動します。

![新しいタグプロパティを選択します。](/help/rtcdp/assets/partner-data/onsite-personalization/select-tag-property.png)

[!UICONTROL コア]拡張機能はすでにインストールされていることに注意してください。次の節で説明するように、さらに 2 つの拡張機能をインストールする必要があります。

![インストールされた拡張機能を表示します。](/help/rtcdp/assets/partner-data/onsite-personalization/view-existing-extensions.png)

#### Web SDK 拡張機能のインストール

このチュートリアルでは、Web SDK を使用して web サイトをインストルメント化する方法を説明します。また、[Mobile SDK](https://developer.adobe.com/client-sdks/documentation/) をアプリで使用すると、パーソナライズされたエクスペリエンスをアプリの訪問者に提供することもできます。

![拡張機能カタログ内の Web SDK 拡張機能の表示。](/help/rtcdp/assets/partner-data/onsite-personalization/web-sdk-extension.png)

Web SDK を設定する画面で、「**[!UICONTROL データストリーム]**」セクションに移動し、使用している Experience Platform サンドボックスに関する情報を入力します。次のドロップダウンから、適切なサンドボックスと前の手順で作成したデータストリームを選択します。他のすべての環境に対して同じサンドボックスとデータストリームの値を選択できます。その他の設定は変更せず、「**[!UICONTROL 保存]**」を選択します。

完全な情報については、「[Web SDK のインストール方法](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=ja)」を参照してください。

#### ID サービス拡張機能のインストール

[Experience Cloud の ID サービス拡張機能](/help/tags/extensions/client/id-service/overview.md)を使用すると、すべての Experience Cloud ソリューションにわたって、訪問者に固有のデバイスベースのファーストパーティ ID を作成できます。拡張機能カタログで **[!UICONTROL ID サービス]**&#x200B;を検索し、インストールします。拡張機能をインストールする際は、すべてのデフォルト設定をそのまま使用します。

![拡張機能カタログ内の ID サービス拡張機能の表示。](/help/rtcdp/assets/partner-data/onsite-personalization/id-service-extension.png)

#### 環境の設定

次に、左側のナビゲーションから「**[!UICONTROL 環境]**」セクションに進みます。この手順では、web サイトを Adobe Edge Network に接続して、訪問者情報をリアルタイムに取得し、配信する必要があります。

開発環境の右側にあるボックスアイコンを選択し、モーダルウィンドウに表示される JavaScript コードスニペットの標準バージョンをコピーします。

![データ収集 UI の環境セクションで強調表示されているボックスアイコンを選択します。](/help/rtcdp/assets/partner-data/onsite-personalization/select-box-icon.png)

このコードスニペットを web サイトの最上部に追加する必要があります。その結果、web サイトは Adobe Edge Network を呼び出して、JavaScript ロジックを取得し、ページ上に読み込んで実行します。これにより、訪問者 ID 生成、データ収集、リアルタイムなエクスペリエンスのパーソナライズなどの機能を使用できます。

#### データ要素の設定

データ要素は、データディクショナリ（またはデータマップ）の構築ブロックです。単一のデータ要素は、クエリ文字列、URL、cookie 値、JavaScript 変数などに値をマッピングできる変数です。詳しくは、[データ要素](/help/tags/ui/managing-resources/data-elements.md)を参照してください。

このユースケースでは、2 つのデータ要素を設定する必要があります。

まず、`partnerData` 要素を設定します。「**[!UICONTROL データ要素]**」セクションに移動し、「**[!UICONTROL 新しいデータ要素の作成]**」を選択します。

![新しいデータ要素の作成.](/help/rtcdp/assets/partner-data/onsite-personalization/create-data-element.gif)

データ要素に `partnerData` という名前を付け、[!UICONTROL 拡張機能]の値を「[!UICONTROL コア]」のままにし、「**[!UICONTROL データ要素タイプ]**」を「**[!UICONTROL JavaScript 変数]**」に設定します。**[!UICONTROL JavaScript 変数名]**&#x200B;というタイトルのフィールドに `partnerData` を入力し、「**[!UICONTROL 保存]**」を選択します。

![選択項目が強調表示され、partnerData データ要素が正しく設定されています。](/help/rtcdp/assets/partner-data/onsite-personalization/create-partnerdata-data-element.png)

2 番目のデータ要素を設定するには、新しい変数に `pageVisit` という名前を付け、**[!UICONTROL 拡張機能]**&#x200B;を「**[!UICONTROL Adobe Experience Platform]**」に設定し、データタイプとして「**[!UICONTROL XDM オブジェクト]**」を選択します。

![選択項目が強調表示され、pageVisit データ要素が正しく設定されています。](/help/rtcdp/assets/partner-data/onsite-personalization/page-visit-data-element.png)

スキーマから、データパートナーに期待する値に対応するサードパーティ属性を選択します。次に、「**[!UICONTROL オブジェクト全体を提供]**」というタイトルのラジオボタンを選択します。データベースに似たアイコンを選択し、前に作成した `partnerData` データ要素を選択します。

#### ルールを設定

「**[!UICONTROL ルール]**」セクションでは、作成したデータ要素に読み込まれた属性を使用して、Adobe にパーソナライズ機能のリクエストを送信するように web サイトを設定できます。詳しくは、[ルールの作成](/help/tags/ui/managing-resources/rules.md)を参照してください。

「**[!UICONTROL 新規ルールを作成]**」を選択します。このルールに「**[!UICONTROL パーソナライズ]**」という名前を付け、「**[!UICONTROL イベント]**」の隣にある + 記号を選択します。イベントとして「**[!UICONTROL ページ下部]**」を選択し、保存します。

![ルールのイベントタイプ部分の選択。](/help/rtcdp/assets/partner-data/onsite-personalization/add-events-rule.png)

**[!UICONTROL アクション]**&#x200B;の横にある + 記号を選択します。拡張機能を **[!UICONTROL Adobe Experience Platform Web SDK]** に更新し、「**[!UICONTROL アクションタイプ]**」を「**[!UICONTROL イベントの送信]**」に設定します。

![ルールのアクションタイプの部分の選択。](/help/rtcdp/assets/partner-data/onsite-personalization/add-action-rule.png)

右側の「**[!UICONTROL タイプ]**」ドロップダウンセレクターから、イベントタイプとして `web.webpagedetails.pageViews` を選択します。

![イベントタイプを選択します。](/help/rtcdp/assets/partner-data/onsite-personalization/add-pageview-type-rule.png)

XDM データの横にあるデータベースアイコンを選択し、`pageVisit` データ要素を選択します。

アクション設定のリストを下にスクロールし、「**[!UICONTROL 視覚的なパーソナライズ機能の決定をレンダリング]**」というタイトルのボックスを必ずオンにします。これは、Adobe Target またはその他の類似製品を介して提供されたエクスペリエンスを web ページ上で視覚的にレンダリングするために重要です。「**[!UICONTROL 変更を維持]**」を選択し、ルールを「**[!UICONTROL 保存]**」します。

![「視覚的なパーソナライズ機能の決定をレンダリング」チェックボックスを選択します。](/help/rtcdp/assets/partner-data/onsite-personalization/render-visual-personalization-toggle.png)

#### 公開ワークフローを設定

この設定を web ページにデプロイするには、次の手順で、作成したリソースを含むライブラリを構築します。詳しくは、[フロー公開の設定](/help/tags/ui/publishing/publishing-flow.md)を参照してください。

「**[!UICONTROL フローの公開]**」、「**[!UICONTROL ライブラリを追加]**」の順に選択します。

「**[!UICONTROL 変更されたリソースをすべて追加]**」を選択し、ライブラリに名前を付けて、環境を「**[!UICONTROL 開発]**」に設定し、「**[!UICONTROL 開発に保存してビルド]**」を選択します。

![ライブラリの作成と開発環境へのデプロイ](/help/rtcdp/assets/partner-data/onsite-personalization/create-publishing-workflow.gif)

#### Web サイトをテスト

この時点で、web サイトに web SDK が完全に実装されている必要があります。データ収集が期待どおりに動作しているかをテストするには、web サイトに移動し、ブラウザーの開発者ツールを使用してネットワークトラフィックを調べます。

検索ボックスに `interact` と入力してページを更新すると、web サイトから Adobe Edge ネットワークへのネットワーク呼び出しが表示されます。

![開発者ツールに入力されたネットワークイベントの表示。](/help/rtcdp/assets/partner-data/onsite-personalization/events-filtered.png)

### パーソナライズ機能 {#personalization}

これで、パーソナライズ機能用にオーディエンスを作成してアクティブ化する準備が整いました。

#### オーディエンスの作成とエッジセグメント化の設定

Platform UI で、に移動します。 **[!UICONTROL 顧客]** > **[!UICONTROL オーディエンス]** をクリックして、web サイトの訪問者をキャプチャするためのオーディエンスを作成します。

![オーディエンスへの移動方法のビュー。](/help/rtcdp/assets/partner-data/onsite-personalization/navigate-to-audiences.png)

オーディエンスを設定するには、 [エッジセグメント化](/help/segmentation/ui/edge-segmentation.md) したがって、訪問者のオーディエンスメンバーシップは、Web プロパティを訪問する際にリアルタイムに評価されます。

エッジオーディエンスに対しては、必ず [active-on-edge 結合ポリシー](/help/destinations/ui/activate-edge-personalization-destinations.md#create-merge-policy)も設定してください。

#### Adobe Target または他のカスタムパーソナライズ機能の宛先との統合

これで、パーソナライズ機能エンジンと統合して、web サイトやアプリの訪問者にパーソナライズされたコンテンツを表示する準備が整いました。Adobe では、この目的のために [Adobe Target の宛先](/help/destinations/catalog/personalization/adobe-target-connection.md)を使用することを推奨します。

>[!IMPORTANT]
>
>オーディエンスをアクティブ化するために必要な手順について詳しくは、「[オーディエンスをエッジパーソナライズ機能の宛先へアクティブ化する](/help/destinations/ui/activate-edge-personalization-destinations.md)」のチュートリアルを参照してください。

## 制限事項とトラブルシューティング {#limitations-and-troubleshooting}

このページで説明するユースケースを参照する際は、次の制限事項に注意してください。

* パートナー ID を使用する場合、これらの ID は [ID グラフの作成](/help/identity-service/ui/identity-graph-viewer.md)時に使用されません。

## パートナーデータサポートを通じて達成されるその他のユースケース {#other-use-cases}

Real-Time CDP のパートナーデータサポートを通じて達成されるその他のユースケースを調べます。

* [信頼できるデータパートナーからの属性でファーストパーティプロファイルを補完し、データ基盤を改善し、顧客ベースに関する新しいインサイトを得て、オーディエンスの最適化を改善します。](/help/rtcdp/partner-data/supplement-first-party-profiles.md)
* Real-Time CDP のサードパーティデータのサポートを使用して、[データパートナーの見込み客プロファイルでプロファイルベースを拡張し、新規顧客の獲得またはリーチのために見込み客との関わりを深めます](/help/rtcdp/partner-data/prospecting.md)。
* [見込み客プロファイルと見込み客オーディエンスのアクティブ化の拡張](/help/destinations/ui/activate-prospect-audiences.md) をクリックして、宛先を選択します。
