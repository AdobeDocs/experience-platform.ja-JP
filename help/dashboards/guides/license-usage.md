---
keywords: Experience Platform;ユーザーインターフェイス;UI;カスタマイズ;ライセンス使用状況ダッシュボード;ダッシュボード;ライセンス使用状況;使用権限;使用
title: ライセンス使用状況ダッシュボード ガイド
description: Adobe Experience Platformには、組織のライセンス使用状況に関する重要な情報を表示できるダッシュボードが用意されています。
type: Documentation
exl-id: 143d16bb-7dc3-47ab-9b93-9c16683b9f3f
source-git-commit: e9c4068419b36da6ffaec67f0d1c39fe87c2bc4c
workflow-type: tm+mt
source-wordcount: '1987'
ht-degree: 9%

---

# ライセンス使用状況ダッシュボード {#license-usage-dashboard}

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseUsage"
>title="ライセンス使用状況ダッシュボード"
>abstract="ライセンス使用状況ダッシュボードでは、購入した Adobe Experience Platform 製品に関するインサイトを得ることができます。ダッシュボードの概要には、各プライマリ指標の使用状況や契約済みのライセンス金額など、製品のプライマリ指標が表示されます。詳細ワークスペースは、特定のサンドボックス内の各製品の指標の分類を表示します。"

Adobe Experience Platformから、組織のライセンス使用状況に関する重要な情報を表示できます [!UICONTROL ライセンスの使用状況] ダッシュボード。 ここに表示される情報は、Platform インスタンスの毎日のスナップショットの間にキャプチャされます。

ライセンス使用状況レポートは、ライセンス使用状況の指標に対して高い精度を提供します。 ダッシュボードには、購入した各製品の使用状況指標、すべての実稼動または開発サンドボックスの指標の統合的な使用状況、特定のサンドボックスの使用状況指標が表示されます。 Real-time Customer Data Platform、Adobe Journey OptimizerおよびCustomer Journey AnalyticsのExperience Platform指標を使用して追跡できるアプリケーションがあります。

このガイドでは、UI でのライセンス使用状況ダッシュボードへのアクセスと操作の方法を説明し、ダッシュボードに表示されるビジュアライゼーションに関する詳細を提供します。

Platform UI の一般的な概要については、 [Experience PlatformUI ガイド](../../landing/ui-guide.md).

## [!UICONTROL ライセンスの使用状況] ダッシュボードデータ

The [!UICONTROL ライセンスの使用状況] 「ダッシュボード」には、購入したすべてのExperience Platform製品のリストが表示されます。 このリストから、関連するサンドボックス全体でのExperience Platformに関する、組織のライセンス関連データのスナップショットを見つけることができます。

このダッシュボードのデータは、スナップショットが作成された特定の時点で表示されたとおりに表示されます。 つまり、スナップショットはデータの近似やサンプルではなく、ダッシュボードはリアルタイムで更新されません。

>[!NOTE]
>
>スナップショットが作成された後にデータに加えられた変更や更新は、次のスナップショットが作成されるまでダッシュボードに反映されません。

## ライセンス使用状況ダッシュボードの確認 {#explore}

Platform UI 内でライセンス使用状況ダッシュボードに移動するには、「 」を選択します。 **[!UICONTROL ライセンスの使用状況]** をクリックします。 The [!UICONTROL 概要] 「 」タブが開き、使用可能な製品のリストが表示されます。

>[!NOTE]
>
>ライセンス使用状況ダッシュボードは、デフォルトでは有効になっていません。 ダッシュボードを表示するには、「ライセンス使用状況ダッシュボードの表示」権限が必要です。 ライセンス使用状況ダッシュボードを表示するためのアクセス権限の付与手順については、 [ダッシュボード権限ガイド](../permissions.md).

![左側のナビゲーションサイドバーで「ライセンス使用状況ダッシュボードの概要」タブの「ライセンス使用状況」がハイライト表示されています。](../images/license-usage/dashboard-overview.png)

## [!UICONTROL 「概要」タブ] {#overview-tab}

このダッシュボードには、アドオンを含む、ライセンスされたAdobe Experience Platform製品がすべて表形式で表示されます。 表には、使用可能なすべてのプロファイルにわたるライセンスの使用状況に関する主な情報が表示されます。

| 列名 | 説明 |
|---|---|
| **[!UICONTROL 製品]** | 組織がライセンスをAdobeしたソリューション。 |
| **[!UICONTROL プライマリ指標]** | その製品の内でのトラッキングに使用される主要指標。 |
| **[!UICONTROL ライセンス数]** | 製品使用許諾契約で合意したプライマリ指標の最大金額の契約値。 |
| **[!UICONTROL 用途]** | 使用するプライマリ指標の量。 この値は、実稼働または開発のいずれかの、すべてのサンドボックスでのその指標の合計使用量を提供します。 |
| **[!UICONTROL 用途 %]** | 使用しているプライマリ指標の割合（ライセンス金額に応じた）。 |

>[!NOTE]
>
>への追加 [!UICONTROL ライセンス数] アドオンの結果、 [!UICONTROL ライセンス数] Real-time Customer Data Platform、Adobe Journey Optimizer、Customer Journey Analyticsなどの基本製品用。 （アドオンの後に）そのライセンス金額の使用状況がベース製品を通じて追跡されます。 例えば、5 つのサンドボックスの 1 パックを購入した場合、数量 5 が基本製品の数量に追加されます。この場合、アドオンには [!UICONTROL ライセンス数] が 1 つで、そのアドオンの使用状況は、ベース製品を通じて使用状況が追跡されるので、「空白」になります。

この表は、各製品が多数の指標を追跡できるので、各製品の主要指標を示しています。

## [!UICONTROL 「概要」タブ] {#summary-tab}

製品ライセンスの使用状況に関するその他の指標や詳細なインサイトを表示するには、リストから製品名を選択します。 The [!UICONTROL 概要] その製品のビューが表示されます。 使用可能なすべての指標が [!UICONTROL 概要] タブをクリックします。 使用できる指標は、ライセンスされた製品によって異なります。 このビューには、 **すべての実稼動または開発用サンドボックスのすべての指標の統合表示**. 実稼働用サンドボックスと開発用サンドボックスの両方に同じレベルの分析が提供されます。

![Platform 製品の概要表示。その製品で使用可能なすべての指標が表示されます。](../images/license-usage/summary-tab.png)

「概要」タブのテーブルには、 [!UICONTROL 指標] 列。 これらの人間が判読できる説明は、そのタイプのサンドボックスで使用されるすべての指標を示します。

### サンドボックスを選択 {#select-sandbox}

実稼動サンドボックスタイプと開発サンドボックスタイプの間で表示を変更するには、次のいずれかを選択します。 [!UICONTROL 実稼働用サンドボックス] または [!UICONTROL 開発サンドボックス]. 選択したサンドボックスタイプは、サンドボックス名の横にあるラジオボタンで示されます。

サンドボックスの使用状況レポートは、同じタイプのすべてのサンドボックスの累積的なものです。 つまり、選択 [!UICONTROL 実稼動] または [!UICONTROL 開発] は、それぞれすべての実稼動サンドボックスまたは開発サンドボックスの使用状況レポートを提供します。

![実稼働用サンドボックスと開発用サンドボックスがハイライトされた Platform 製品の概要ビュー。](../images/license-usage/summary-tab-sandboxes.png)

>[!WARNING]
>
>ライセンス使用状況ダッシュボードを表示する権限は、サンドボックスレベルで指定する必要があります。 個々のサンドボックスに権限を追加して、ダッシュボード内で表示します。 この制限は、今後のリリースで対処される予定です。 それまでの間、次の回避策を使用できます。
>
>1. Adobe Admin Consoleで製品プロファイルを作成します。
>2. 「サンドボックス」カテゴリの「権限」で、ライセンス使用状況ダッシュボードに表示するすべてのサンドボックスを追加します。
>3. 「ユーザーダッシュボード権限」カテゴリで、「ライセンス使用状況ダッシュボードの表示」権限を追加します。

## The [!UICONTROL 詳細] タブ {#details-tab}

参照： **特定のサンドボックスの特定の使用指標**&#x200B;をクリックし、 [!UICONTROL 詳細] タブをクリックします。 The [!UICONTROL 詳細] 「 」タブには、実稼動または開発サンドボックス内の使用可能なすべてのサンドボックスが表示されます。

![ライセンス使用状況ダッシュボードの「詳細」タブ。](../images/license-usage/details-tab.png)

このビューから、 ![検査アイコン。](../images/license-usage/inspect-icon.png) サンドボックス名の横に表示され、その指標のビジュアライゼーションを表示できます。 指標のビジュアライゼーションが表示されたダイアログが開きます。

### ビジュアライゼーション {#visualizations}

各ビジュアライゼーションウィジェットには、次の側面が含まれます。

- 経時的な指標の変化を追跡する折れ線グラフ
- 折れ線グラフのキー
- サンドボックス名
- 折れ線グラフの期間を調整するドロップダウンメニュー

折れ線グラフは、組織の使用状況数を、組織のライセンスで使用可能な合計と比較し、合計使用量の割合を示します。

![指標のビジュアライゼーション。](../images/license-usage/visualization.png)

分析のルックバック期間は、ドロップダウンメニューから調整できます。 過去 30 日間のデフォルト値

日付範囲を選択するには、日付範囲ドロップダウンを使用して、ダッシュボードに表示する期間を選択します。 過去 30 日間のデフォルト値を含む、複数のオプションを使用できます。

![日付範囲ドロップダウンがハイライト表示されたビジュアライゼーションダイアログ。](../images/license-usage/date-range.png)

また、 **[!UICONTROL カスタム日付]** をクリックして、表示する期間を選択します。

![カスタムの日付範囲オプションが強調表示された「ライセンス使用状況ダッシュボードの概要」タブ。](../images/license-usage/custom-date-range.png)

## 使用可能な指標

ライセンス使用状況ダッシュボードは、組織内の複数の製品に適用できる、複数の一意の指標に関するレポートを提供します。 使用可能な指標は次のとおりです。

| 指標 | 説明 |
|---|---|
| [!UICONTROL データエクスポート] | 1 年間に（直接または間接的に）非Adobeソリューションに書き出すことができるデータセットの合計サイズ。 |
| [!UICONTROL AMM モデルの数] | 投資に基づいて特定の結果を測定または予測するために使用される機械学習モデル ( 組み込みAdobe Mix Modeler) の数。 |
| [!UICONTROL データレイクストレージ] | Adobe Experience Platform内の分析データストアの使用量。 |
| [!UICONTROL 計算済み属性] | 集計されたプロファイル行動データの合計数。 集計されたプロファイル行動データは、プロファイル属性に変換され、人物プロファイルまたはビジネス人物プロファイルに含めることができるエクスペリエンスイベントに基づいています。 |
| [!UICONTROL 類似オーディエンス] | 既存の消費者オーディエンスをモデリングして、既存の消費者オーディエンスと類似した人物プロファイルを識別することで生成されたオーディエンスの数。 |
| [!UICONTROL アドレス可能なオーディエンス] | ビジネスオーディエンスの使用権限と消費者オーディエンスの使用権限の合計。 消費者オーディエンスは、販売注文で「消費者オーディエンス」として識別された人物プロファイルの数として定義されます。 ビジネスオーディエンスは、販売注文の「ビジネスオーディエンス」として識別されたビジネスユーザープロファイルの数として定義されます。 |
| [!UICONTROL サンドボックスの数] | Adobe Experience Platformの分離データと操作にアクセスするAdobeオンデマンドサービスのインスタンス内の論理分離の数。 |
| [!UICONTROL 平均プロファイル充実度] | 任意の時点でハブプロファイルサービスに保存されるすべての実稼動データの合計を、許可されたビジネス担当者のプロファイル数の 5 倍で割った値です。 [!UICONTROL 平均プロファイル充実度] は共有機能です。 |
| [!UICONTROL パックのストリーミングセグメント化番号] | パックは、新しいデータがストリーミングフローを通じてセグメント化サービスに入ると、担当者プロファイルのセグメントメンバーシップを更新します。 セグメントのメンバーシップは、過去の行動を考慮せずに、現在の人物プロファイル属性と現在のイベントの値に基づいて評価されます。 ストリーミングセグメント化は共有機能です。 |
| [!UICONTROL 消費者オーディエンス] | 販売注文で「消費者オーディエンス」として識別された人物プロファイルの数。 |
| [!UICONTROL CJA 行が使用可能] | Customer Journey Analytics内の分析に使用できるデータの日平均行。 |
| [!UICONTROL Profile Richness No of Packs] | 追加のプロファイル充実度パックごとに、認定された平均プロファイル充実度がプロファイルあたり 25 KB 増加した場合。 |
| [!UICONTROL アドホッククエリサービスユーザーパック] | 認証済みの同時クエリサービスユーザーの権限を、5 人の同時クエリサービスユーザーと、1 つのパックにつき 1 回のアドホッククエリを同時に実行する追加のユーザーによって増やすためのアドオン。 複数の追加のアドホッククエリユーザーパックのライセンスを取得できます。 |
| [!UICONTROL エンゲージ可能なオーディエンス] | この指標は、エンゲージメント可能なプロファイルのオーディエンスを示します。 エンゲージメント可能なプロファイルは、個人を表す情報のレコードで、プロファイルサービスに表されます。 これらのレコードは、過去 12 ヶ月間にJourney Optimizerのオーサリング、判定、配信、実験またはオーケストレーション機能を使用してエンゲージメントを試みたプロファイルです。 |
| [!UICONTROL Query Service Compute Hours] | バッチクエリが実行されたときに、クエリサービスエンジンがデータの読み取り、処理、データレイクへの書き戻しに要した時間の測定値。 |

<!-- |  [!UICONTROL Sandbox No of Packs] |  A logical separation within your instance of any Adobe On-demand Service that accesses Adobe Experience Platform isolating data and operations | -->

これらの指標の可用性と各指標の具体的な定義は、お客様の組織が購入したライセンスによって異なります。各指標の詳細な定義については、該当する製品の説明ドキュメントを参照してください。

| ライセンス | 製品の説明 |
|---|---|
| <ul><li>ADOBE EXPERIENCE PLATFORM:OD LITE</li><li>ADOBE EXPERIENCE PLATFORM:OD STANDARD</li><li>ADOBE EXPERIENCE PLATFORM:OD HEAVY</li></ul> | [Adobe Experience Platform](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform.html) |
| <ul><li>ADOBE EXPERIENCE PLATFORM:OD</li></ul> | [Experience Platform、アプリサービスおよびインテリジェントサービス](https://helpx.adobe.com/legal/product-descriptions/exp-platform-app-svcs.html) |
| <ul><li>RT 顧客データプラットフォーム：OD</li><li>RT 顧客データプラットフォーム：OD PRFL ～ 10M</li><li>RT 顧客データプラットフォーム：OD PRFL ～ 50M</li></ul> | [Adobe Real-time Customer Data Platform](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) |
| <ul><li>AEP:OD のアクティベーション</li><li>AEP:OD アクティベーション PRFL から 10M</li><li>AEP:OD アクティベーション（最大 50M）</li></ul> | [Adobe Experience Platform Activation](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-experience-platform0.html) |
| <ul><li>AEP:OD インテリジェンス</li></ul> | [Adobe Experience Platform Intelligence](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform-intelligence---product-description.html) |
| <ul><li>JOURNEY OPTIMIZER SELECT:OD</li><li>JOURNEY OPTIMIZER PRIME:OD</li><li>JOURNEY OPTIMIZER ULTIMATE:OD</li><li>UNP AJO PRIME STARTER:OD</li><li>UNP AJO ULTIMATE STARTER:OD</li><li>UNP Real-Time CDP:OD PROFILE ORCHESTRATION</li></ul> | [Adobe Journey Optimizer](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-journey-optimizer.html) |

>[!WARNING]
>
>ライセンス使用状況ダッシュボードは、組織用にプロビジョニングされた最新のライセンスに関するレポートのみを表示します。 お客様の組織でプロビジョニングされた最新のライセンスが上記の表に表示されない場合は、ライセンス使用状況ダッシュボードが正しく表示されないことがあります。 1 つの組織で追加のライセンスと複数のライセンスをサポートする予定は、今後のリリースです。

## 次の手順

このドキュメントを読むと、ライセンス使用状況ダッシュボードを見つけて、購入した各製品、すべての実稼動または開発サンドボックス、特定のサンドボックスの使用状況指標を表示できます。 組織が購入したライセンスに基づく、組織で使用可能な指標の詳細を確認できます。

Experience PlatformUI で使用できるその他の機能について詳しくは、 [Platform UI ガイド](../../landing/ui-guide.md).
