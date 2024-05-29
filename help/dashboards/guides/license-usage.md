---
keywords: Experience Platform;ユーザーインターフェイス;UI;カスタマイズ;ライセンス使用状況ダッシュボード;ダッシュボード;ライセンス使用状況;使用権限;使用
title: ライセンス使用状況ダッシュボード
description: Adobe Experience Platformには、組織のライセンス使用状況に関する重要な情報を表示できるダッシュボードが用意されています。
type: Documentation
exl-id: 143d16bb-7dc3-47ab-9b93-9c16683b9f3f
source-git-commit: b277de0bd7b65f8e3828c7ab0b4e00644eeddde5
workflow-type: tm+mt
source-wordcount: '2069'
ht-degree: 3%

---

# ライセンス使用状況ダッシュボード {#license-usage-dashboard}

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseUsage"
>title="ライセンス使用状況ダッシュボード"
>abstract="ライセンス使用状況ダッシュボードを使用すると、購入したAdobe Experience Platform製品に関するインサイトを得ることができます。 ダッシュボードの概要には、製品の主要指標が表示されます。これには、各主要指標の使用状況と契約ライセンス量が含まれます。 詳細ワークスペースには、特定のサンドボックス内の各製品の指標の分類が表示されます。"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseUsage_prediction"
>title="ライセンス使用状況ダッシュボード"
>abstract="ライセンス使用状況ダッシュボードを使用すると、購入したAdobe Experience Platform製品に関するインサイトを得ることができます。 ダッシュボードの概要には、製品の主要指標が表示されます。これには、各主要指標の使用状況と契約ライセンス量が含まれます。 詳細ワークスペースには、特定のサンドボックス内の各製品の指標の分類が表示されます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-lifecycle/ui/dataset-expiration.html" text="データセット有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

組織のライセンス使用状況に関する重要な情報は、Adobe Experience Platformで確認できます [!UICONTROL ライセンス使用状況] ダッシュボード。 ここに表示される情報は、Platform インスタンスの毎日のスナップショット中にキャプチャされます。

ライセンス使用状況レポートを使用すると、ライセンス使用指標に対して高い精度でレポートを作成できます。 ダッシュボードには、購入した各製品の使用状況指標、すべての実稼動サンドボックスまたは開発サンドボックスにおける指標の統合使用状況、および特定のサンドボックスの使用状況指標が表示されます。 使用状況指標を使用して、Real-time Customer Data Platform、Adobe Journey Optimizer、Customer Journey AnalyticsのExperience Platformアプリケーションをトラッキングできます。

このガイドでは、UI でのライセンス使用状況ダッシュボードへのアクセス方法と操作方法の概要を説明し、ダッシュボードに表示されるビジュアライゼーションの詳細を説明します。

Platform UI の一般的な概要については、次を参照してください [Experience PlatformUI ガイド](../../landing/ui-guide.md).

## [!UICONTROL ライセンス使用状況] ダッシュボードデータ

この [!UICONTROL ライセンス使用状況] ダッシュボード：購入したすべてのExperience Platform商品のリストが表示されます。 このリストから、関連付けられた任意のサンドボックス間でのExperience Platformに関する組織のライセンス関連データのスナップショットを見つけることができます。

このダッシュボードのデータは、スナップショットが作成された特定の時点とまったく同じように表示されます。 つまり、スナップショットはデータの近似やサンプルではなく、ダッシュボードはリアルタイムには更新されません。

>[!NOTE]
>
>スナップショットが作成された後にデータに加えられた変更や更新は、次のスナップショットが作成されるまでダッシュボードに反映されません。

## ライセンス使用状況ダッシュボードの確認 {#explore}

Platform UI 内でライセンス使用状況ダッシュボードに移動するには、次を選択します。 **[!UICONTROL ライセンス使用状況]** 左パネルで。 この [!UICONTROL 概要] タブが開き、使用可能な製品のリストが表示されます。

>[!NOTE]
>
>ライセンス使用状況ダッシュボードは、デフォルトでは有効になっていません。 ダッシュボードを表示するには、「ライセンス使用状況ダッシュボードを表示」権限をユーザーに付与する必要があります。 ライセンス使用状況ダッシュボードを表示するためのアクセス権限を付与する手順については、を参照してください [ダッシュボード権限ガイド](../permissions.md).

![左側のナビゲーションサイドバーでライセンス使用状況がハイライト表示されたライセンス使用状況ダッシュボードの「概要」タブ。](../images/license-usage/dashboard-overview.png)

## [!UICONTROL 概要] タブ {#overview-tab}

このダッシュボードには、ライセンスを取得したすべてのAdobe Experience Platform製品（アドオンを含む）が表形式で表示されます。 この表には、使用可能なすべてのプロファイルをまたいだライセンス使用状況に関する主要な情報が表示されます。

| 列の名前 | 説明 |
|---|---|
| **[!UICONTROL 製品]** | 組織でライセンスされたAdobeソリューション。 |
| **[!UICONTROL プライマリ指標]** | その製品の内でのトラッキングに使用されるプライマリ指標。 |
| **[!UICONTROL ライセンス量]** | ライセンス契約で合意された、プライマリ指標の最大量に対する契約値。 |
| **[!UICONTROL 用途]** | 使用されたプライマリ指標の量。 この値は、実稼動または開発のすべてのサンドボックスにおける、その指標の合計使用量を提供します。 |
| **[!UICONTROL 使用状況 %]** | ライセンス量に応じて使用される主要指標の割合。 |

>[!NOTE]
>
>への追加 [!UICONTROL ライセンス量] アドオンの結果、が次の項目の上に追加されます [!UICONTROL ライセンス量] （Real-time Customer Data Platform、Adobe Journey Optimizer、Customer Journey Analyticsなどのベース製品）。 そのライセンス金額の（アドオン後の）使用状況は、ベース製品を通じて追跡されます。 例えば、5 個のサンドボックスからなる 1 パックを購入した場合、5 個の数量が基本製品のに追加されます。この場合、アドオンには次が表示されます [!UICONTROL ライセンス量] そのうちの 1 つで、そのアドオンの使用状況は、基本製品全体で使用状況が追跡されるので、「空白」になります。

各製品は多数の指標を追跡できるので、表には各製品の主要指標が示されます。

## [!UICONTROL 概要] タブ {#summary-tab}

製品ライセンスの使用状況に関するその他の指標と詳細なインサイトを表示するには、リストから製品名を選択します。 この [!UICONTROL 概要] その製品のビューが表示されます。 使用可能なすべての指標がに表示されます [!UICONTROL 概要] タブ。 使用できる指標は、ライセンスを取得した製品によって異なります。 このビューの主な機能 **すべての実稼動サンドボックスまたは開発用サンドボックスにわたるすべての指標の統合ビュー**. 実稼動用サンドボックスと開発用サンドボックスの両方に対して、同じレベルの分析が提供されます。

![その製品で使用可能なすべての指標を表示する Platform 製品の概要ビュー。](../images/license-usage/summary-tab.png)

「概要」タブのテーブルには、次の内容が含まれます [!UICONTROL 指標] 列。 これらの人間が読み取れる説明は、そのタイプのサンドボックスに使用されるすべての指標を示します。

### サンドボックスを選択 {#select-sandbox}

実稼動用および開発用のサンドボックスタイプ間で表示を変更するには、次のいずれかを選択します [!UICONTROL 実稼動用サンドボックス] または [!UICONTROL 開発用サンドボックス]. 選択したサンドボックスタイプは、サンドボックス名の横のラジオボタンで示されます。

サンドボックスの消費レポートは、同じタイプのすべてのサンドボックスに対して累積的です。 つまり、を選択します [!UICONTROL 実稼動] または [!UICONTROL 開発] には、すべての実稼動サンドボックスまたは開発用サンドボックスの消費レポートがそれぞれ用意されています。

![実稼動用サンドボックスと開発用サンドボックスがハイライト表示された Platform 製品の概要ビュー。](../images/license-usage/summary-tab-sandboxes.png)

>[!WARNING]
>
>ライセンス使用状況ダッシュボードを表示する権限は、サンドボックスレベルで指定する必要があります。 個々のサンドボックスに権限を追加して、ダッシュボード内で表示します。 この制限は、今後のリリースで対処される予定です。 それまでの間、次の回避策を使用できます。
>
>1. Adobe Admin Consoleで製品プロファイルを作成します。
>2. サンドボックス カテゴリの権限で、表示するすべてのサンドボックスをライセンス使用状況ダッシュボードに追加します。
>3. ユーザーダッシュボード権限カテゴリで、「ライセンス使用状況ダッシュボードを表示」権限を追加します。

## この [!UICONTROL 詳細] タブ {#details-tab}

を表示 **特定のサンドボックスからの特定の使用状況指標**&#x200B;に移動し、に移動します [!UICONTROL 詳細] タブ。 この [!UICONTROL 詳細] タブには、実稼動用サンドボックスまたは開発用サンドボックス内で使用可能なすべてのサンドボックスが表示されます。

![ライセンス使用状況ダッシュボードの「詳細」タブ。](../images/license-usage/details-tab.png)

このビューから、次を選択できます ![検査アイコン。](../images/license-usage/inspect-icon.png) サンドボックス名の横に、その指標のビジュアライゼーションを表示します。 ダイアログが開き、その指標のビジュアライゼーションが表示されます。

### ビジュアライゼーション {#visualizations}

各ビジュアライゼーションウィジェットには、次の側面が含まれています。

- 指標の変化の推移をトラッキングする折れ線グラフ
- 折れ線グラフのキー
- サンドボックス名
- 折れ線グラフの期間を調整するドロップダウンメニュー

折れ線グラフは、組織の使用状況数と組織のライセンスで利用可能な合計数を比較し、合計使用量の割合を示します。

![指標のビジュアライゼーション。](../images/license-usage/visualization.png)

分析のルックバック期間は、ドロップダウンメニューから調整できます。 過去 30 日間のデフォルト値

日付範囲を選択するには、日付範囲ドロップダウンを使用して、ダッシュボードに表示する期間を選択します。 過去 30 日間のデフォルト値など、複数のオプションが使用可能です。

![日付範囲ドロップダウンがハイライト表示されたビジュアライゼーションダイアログ。](../images/license-usage/date-range.png)

以下を選択することもできます。 **[!UICONTROL カスタム日付]** 表示する期間を選択します。

![カスタムの日付範囲オプションが強調表示されたライセンス使用状況ダッシュボードの「概要」タブ](../images/license-usage/custom-date-range.png)

## 使用可能な指標 {#available-metrics}

ライセンス使用状況ダッシュボードでは、組織内の複数の製品に適用できるいくつかの一意の指標についてレポートします。 使用できる指標は次のとおりです。

| 指標 | 説明 |
|---|---|
| [!UICONTROL Audience Activationサイズ] | 1 年間に任意のファイルベースの宛先に対してアクティブ化されたプロファイルの合計サイズ。 注意：これには、ストリーミング宛先を介して送信されるプロファイルは含まれません。 |
| [!UICONTROL アドレス可能オーディエンス] | ビジネスオーディエンスの使用権限と消費者オーディエンスの使用権限の合計。 消費者オーディエンスは、販売注文で「消費者オーディエンス」として識別されるユーザープロファイルの数として定義されます。 ビジネスオーディエンスは、販売注文で「ビジネスオーディエンス」として識別されるビジネスユーザープロファイルの数として定義されます。 |
| [!UICONTROL アドホッククエリサービスユーザーパック] | 承認された同時クエリサービスユーザーの使用権限を増やすアドオン。5 人の同時クエリサービスユーザーと、パックごとに 1 つの同時に実行されるアドホッククエリを追加できます。 複数の追加アドホック クエリ ユーザーパックがライセンスされている可能性があります。 |
| [!UICONTROL 平均プロファイル充実度] | 任意の時点でハブプロファイルサービス内に保存されたすべての実稼動データの合計を、許可されたビジネスユーザープロファイルの数の 5 倍で割った値です。 [!UICONTROL 平均プロファイル充実度] は共有機能です。 |
| [!UICONTROL 使用可能な CJA 行] | Customer Journey Analytics内の分析に使用できる 1 日あたりの平均データ行数。 |
| [!UICONTROL 計算属性] | 集計されたプロファイル行動データの合計数。 集計プロファイルの行動データは、プロファイル属性に変換されるエクスペリエンスイベントに基づいており、ユーザープロファイルまたはビジネスユーザープロファイルに含めることができます。 |
| [!UICONTROL 消費者オーディエンス] | 販売注文で「消費者オーディエンス」として識別されたユーザープロファイルの数。 |
| [!UICONTROL データの書き出しサイズ] | 1 年にデータセットのアクティベーションを通じて送信されるデータの量。 |
| [!UICONTROL データの書き出し] | 1 年に（直接または間接的に）Adobe以外のソリューションに書き出すことができるデータセットの合計サイズ。 |
| [!UICONTROL Data Lake Storage] | Adobe Experience Platform内で使用される分析データストアの量です。 |
| [!UICONTROL エンゲージメント可能なオーディエンス] | この指標は、エンゲージメント可能なプロファイルのオーディエンスを参照します。 エンゲージメント可能なプロファイルは、個人を表す情報のレコードで、プロファイルサービスで表されます。 これらのレコードは、過去 12 か月間に、Journey Optimizerのオーサリング、意思決定、配信、実験またはオーケストレーション機能を使用してエンゲージしようとしたプロファイルです。 |
| [!UICONTROL 類似オーディエンス] | 既存の消費者オーディエンスをモデリングして、その既存の消費者オーディエンスに類似したユーザープロファイルを識別することで生成されるオーディエンスの数。 |
| [!UICONTROL AMM モデルの数] | 投資に基づいて指定された成果を測定または予測するために使用される機械学習モデル（Adobe Mix Modelerで構築）のカウント。 |
| [!UICONTROL サンドボックス数] | データと操作を分離してAdobe Experience PlatformにアクセスするAdobeのオンデマンドサービスのインスタンス内の論理分離の数。 |
| [!UICONTROL プロファイル充実度パック数] | 追加のプロファイル充実度パックごとに、プロファイルごとに認証済みの平均プロファイル充実度が 25 KB 増加します。 |
| [!UICONTROL クエリサービス計算時間] | バッチクエリの実行時に、クエリサービスエンジンがデータの読み取り、処理およびデータレイクへの書き込みに要した時間の測定値。 |
| [!UICONTROL ストリーミングセグメント化パック数] | パックは、新しいデータがストリーミングフローを通じてセグメント化サービスに入ると、個人プロファイルのセグメントメンバーシップを更新します。 セグメントメンバーシップは、過去の行動を考慮せずに、現在の人物プロファイル属性と現在のイベントの値に基づいて評価されます。 ストリーミングセグメント化は共有機能です。 |

<!-- |  [!UICONTROL Sandbox No of Packs] |  A logical separation within your instance of any Adobe On-demand Service that accesses Adobe Experience Platform isolating data and operations | -->

これらの指標の可用性と各指標の具体的な定義は、組織が購入したライセンスによって異なります。 各指標の定義について詳しくは、該当する製品説明ドキュメントを参照してください。

| ライセンス | 商品の説明 |
|---|---|
| <ul><li>ADOBE EXPERIENCE PLATFORM:OD LITE</li><li>ADOBE EXPERIENCE PLATFORM:OD 標準</li><li>ADOBE EXPERIENCE PLATFORM:OD ヘビー</li></ul> | [Adobe Experience Platform](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform.html) |
| <ul><li>ADOBE EXPERIENCE PLATFORM:OD</li></ul> | [Experience Platform、アプリサービスおよびインテリジェントサービス](https://helpx.adobe.com/legal/product-descriptions/exp-platform-app-svcs.html) |
| <ul><li>RT CUSTOMER DATA PLATFORM:OD</li><li>RT CUSTOMER DATA PLATFORM:OD PRFL ～ 10M</li><li>RT CUSTOMER DATA PLATFORM:OD PRFL ～ 50M</li></ul> | [Adobe Real-time Customer Data Platform](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) |
| <ul><li>AEP:OD アクティベーション</li><li>AEP:OD ACTIVATION PRFL TO 10M</li><li>AEP:OD ACTIVATION PRFL （最大 50 M）</li></ul> | [Adobe Experience Platform Activation](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-experience-platform0.html) |
| <ul><li>AEP:OD INTELLIGENCE</li></ul> | [Adobe Experience Platform Intelligence](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform-intelligence---product-description.html) |
| <ul><li>JOURNEY OPTIMIZER SELECT:OD</li><li>JOURNEY OPTIMIZERプライム：OD</li><li>JOURNEY OPTIMIZER ULTIMATE:OD</li><li>UNP AJO プライムスターター：OD</li><li>UNP AJO ULTIMATE スターター：OD</li><li>UNP Real-Time CDP:OD プロファイルオーケストレーション</li></ul> | [Adobe Journey Optimizer](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-journey-optimizer.html) |

>[!WARNING]
>
>ライセンス使用状況ダッシュボードは、組織にプロビジョニングされた最新のライセンスに関してのみレポートします。 組織にプロビジョニングされた最新のライセンスが上の表に表示されない場合、ライセンス使用状況ダッシュボードが正しく表示されない可能性があります。 今後のリリースで、1 つの組織での追加ライセンスと複数ライセンスのサポートが予定されています。

## 次の手順

このドキュメントを読むと、ライセンス使用状況ダッシュボードを見つけ、購入した各製品、すべての実稼動または開発用サンドボックス、特定のサンドボックスの使用状況指標を表示できるようになります。 組織が購入したライセンスに基づいて、組織で使用可能な指標に関する詳細を確認できます。

Experience PlatformUI で使用できるその他の機能について詳しくは、を参照してください。 [Platform UI ガイド](../../landing/ui-guide.md).
