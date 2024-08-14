---
keywords: Experience Platform;ユーザーインターフェイス;UI;カスタマイズ;ライセンス使用状況ダッシュボード;ダッシュボード;ライセンス使用状況;使用権限;使用
title: ライセンス使用状況ダッシュボード
description: Adobe Experience Platformには、組織のライセンス使用状況に関する重要な情報を表示できるダッシュボードが用意されています。
type: Documentation
exl-id: 143d16bb-7dc3-47ab-9b93-9c16683b9f3f
source-git-commit: 090b870dcfb16e59831f1e03eb46b22da4f24f0f
workflow-type: tm+mt
source-wordcount: '2429'
ht-degree: 8%

---

# ライセンス使用状況ダッシュボード {#license-usage-dashboard}

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseUsage"
>title="ライセンス使用状況ダッシュボード"
>abstract="ライセンス使用状況ダッシュボードでは、購入した Adobe Experience Platform 製品に関するインサイトを得ることができます。ダッシュボードの概要には、各プライマリ指標の使用状況や契約済みのライセンス金額など、製品のプライマリ指標が表示されます。詳細ワークスペースは、特定のサンドボックス内の各製品の指標の分類を表示します。"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseUsage_prediction"
>title="ライセンス使用状況ダッシュボード"
>abstract="ライセンス使用状況ダッシュボードでは、購入した Adobe Experience Platform 製品に関するインサイトを得ることができます。ダッシュボードの概要には、各プライマリ指標の使用状況や契約済みのライセンス金額など、製品のプライマリ指標が表示されます。詳細ワークスペースは、特定のサンドボックス内の各製品の指標の分類を表示します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-lifecycle/ui/dataset-expiration.html?lang=ja" text="データセット有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="匿名プロファイルのデータの有効期限"

Adobe Experience Platformの [!UICONTROL  ライセンスの使用状況 ] ダッシュボードを使用して、組織のライセンスの使用状況に関する重要な情報を表示できます。 ここに表示される情報は、Platform インスタンスの毎日のスナップショット中にキャプチャされます。

ライセンス使用状況レポートを使用すると、ライセンス使用指標に対して高い精度でレポートを作成できます。 ダッシュボードには、購入した各製品の使用状況指標、すべての実稼動サンドボックスまたは開発サンドボックスにおける指標の統合使用状況、および特定のサンドボックスの使用状況指標が表示されます。 使用状況指標を使用して、Real-time Customer Data Platform、Adobe Journey Optimizer、Customer Journey AnalyticsのExperience Platformアプリケーションをトラッキングできます。

このガイドでは、UI でのライセンス使用状況ダッシュボードへのアクセス方法と操作方法の概要を説明し、ダッシュボードに表示されるビジュアライゼーションの詳細を説明します。

Platform UI の一般的な概要については、[Experience Platform UI ガイド ](../../landing/ui-guide.md) を参照してください。

## [!UICONTROL  ライセンス使用状況 ] ダッシュボードデータ

[!UICONTROL  ライセンスの使用状況 ] ダッシュボードには、購入したすべてのExperience Platform製品のリストが表示されます。 このリストから、関連付けられた任意のサンドボックス間でのExperience Platformに関する組織のライセンス関連データのスナップショットを見つけることができます。

このダッシュボードのデータは、スナップショットが作成された特定の時点とまったく同じように表示されます。 つまり、スナップショットはデータの近似やサンプルではなく、ダッシュボードはリアルタイムには更新されません。

>[!NOTE]
>
>スナップショットが作成された後にデータに加えられた変更や更新は、次のスナップショットが作成されるまでダッシュボードに反映されません。

## ライセンス使用状況ダッシュボードの確認 {#explore}

Platform UI 内でライセンス使用状況ダッシュボードに移動するには、左パネルで **[!UICONTROL ライセンス使用状況]** を選択します。 「[!UICONTROL  概要 ]」タブが開き、使用可能な製品のリストが表示されます。

>[!NOTE]
>
>ライセンス使用状況ダッシュボードは、デフォルトでは有効になっていません。 ダッシュボードを表示するには、「ライセンス使用状況ダッシュボードを表示」権限をユーザーに付与する必要があります。 ライセンス使用状況ダッシュボードを表示するためのアクセス権限の付与手順については、[ ダッシュボード権限ガイド ](../permissions.md) を参照してください。

![ 左側のナビゲーションサイドバーでライセンス使用状況がハイライト表示されたライセンス使用状況ダッシュボードの「概要」タブ ](../images/license-usage/dashboard-overview.png)

## [!UICONTROL  概要 ] タブ {#overview-tab}

このダッシュボードには、ライセンスを取得したすべてのAdobe Experience Platform製品（アドオンを含む）が表形式で表示されます。 この表には、使用可能なすべてのプロファイルをまたいだライセンス使用状況に関する主要な情報が表示されます。

| 列の名前 | 説明 |
|---|---|
| **[!UICONTROL 製品]** | 組織でライセンスされたAdobeソリューション。 |
| **[!UICONTROL プライマリ指標]** | その製品の内でのトラッキングに使用されるプライマリ指標。 |
| **[!UICONTROL 許可の金額]** | ライセンス契約で合意された、プライマリ指標の最大量に対する契約値。 |
| **[!UICONTROL 用途]** | 使用されたプライマリ指標の量。 この値は、実稼動または開発のすべてのサンドボックスにおける、その指標の合計使用量を提供します。 |
| **[!UICONTROL 使用方法 %]** | ライセンス量に応じて使用される主要指標の割合。 |
| **[!UICONTROL 予測の使用]** | （**Beta**） ライセンス量に応じた主要指標の予測使用率。 |

>[!NOTE]
>
>アドオンによる [!UICONTROL  ライセンス額 ] の加算は、Real-time Customer Data Platform、Adobe Journey Optimizer、Customer Journey Analyticsなどのベース商品の [!UICONTROL  ライセンス額 ] に加算されます。 そのライセンス金額の（アドオン後の）使用状況は、ベース製品を通じて追跡されます。 例えば、5 個のサンドボックスからなる 1 パックを購入した場合、5 個の数量が基本製品のに追加されます。この場合、アドオンには [!UICONTROL  ライセンス額 ] が 1 と表示され、そのアドオンの使用状況は基本製品全体で追跡されるので、「空白」になります。

各製品は多数の指標を追跡できるので、表には各製品の主要指標が示されます。

### [!BADGE Beta]{type=Informative} 予想される使用状況 {#predicted-usage}

>[!AVAILABILITY]
>
今後のライセンス使用状況を予測する機能は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。

インサイトに満ちた使用状況の予測に基づいて、ライセンスリソースをプロアクティブに管理および最適化します。 この [!UICONTROL  予測使用状況 ] 列は、購入したすべての製品に対して、すべての実稼動および開発用サンドボックスにわたって、サンドボックスレベルで今後のライセンスの使用を正確に予測します。 このアラート機能は、今月の 15 日までの使用状況に基づいて、6 週間後のライセンス使用状況の予測を提供します。 予測には、下限と上限が設定されています。

>[!IMPORTANT]
>
予測は毎月更新されます。 更新日は、情報アイコン（![ この情報アイコン](../images/license-usage/info-icon.png)）を選択します。

製品の使用権限の概要を確認するには、[!UICONTROL  概要 ] リストから製品を選択します。

![ 製品と予測される使用状況列がハイライト表示された [!UICONTROL  ライセンス使用状況 ] [!UICONTROL  概要 ]。](../images/license-usage/product-predicted-usage.png)

「概要」タブが表示されます。 [!UICONTROL  概要 ] タブと [!UICONTROL  詳細 ] タブで使用できる詳細な予測を使用して、情報に基づいた意思決定を確保し、効率的なライセンス使用を実現できます。

![ 予測された使用状況列がハイライト表示された Platform 製品の概要ビュー。](../images/license-usage/summary-predicted-usage.png)

予測される使用状況の割合は、次のように決定されます。

- 下限と上限が大きく異なる場合は、範囲として表示されます（例：32% ～ 35%）。
- 下限と上限がほぼ同じでゼロでない場合は、近似値（例：約 34%）として表示されます。
- 下限と上限がほぼ同じでゼロの場合、正確に 0% と表示されます。

>[!NOTE]
>
このコンテキストの「ほぼ同一」とは、値が統計的に小数点以下 2 桁まで有意であることを意味します（例えば、下限が 0.342、上限が 0.344 の場合、どちらも 34% に丸められます）。

予測使用状況機能では、次の指標をサポートしています。

- [!UICONTROL  アドレス可能なオーディエンス ]
- [!UICONTROL  平均プロファイル充実度 ]
- [!UICONTROL  時間を計算 ]
- [!UICONTROL  顧客ジャーニーオーディエンスの行数 ]
- [!UICONTROL  合計ストレージ ]

## [!UICONTROL  概要 ] タブ {#summary-tab}

製品ライセンスの使用状況に関するその他の指標と詳細なインサイトを表示するには、リストから製品名を選択します。 その製品の [!UICONTROL  概要 ] ビューが表示されます。 使用可能なすべての指標が「[!UICONTROL  概要 ]」タブに表示されます。 使用できる指標は、ライセンスを取得した製品によって異なります。 このビューには **すべての実稼動または開発用サンドボックスのすべての指標の統合ビュー** が表示されます。 実稼動用サンドボックスと開発用サンドボックスの両方に対して、同じレベルの分析が提供されます。

![ その製品で使用可能なすべての指標を表示する Platform 製品の概要ビュー ](../images/license-usage/summary-tab.png)

「概要」タブで、テーブルには [!UICONTROL  指標 ] 列が含まれます。 これらの人間が読み取れる説明は、そのタイプに使用されるすべての指標を示します of サンドボックス。

### サンドボックスを選択 {#select-sandbox}

実稼動用および開発用サンドボックスタイプ間の表示を変更するには、 select [!UICONTROL  実稼動用サンドボックス ] または [!UICONTROL  開発用サンドボックス ]。 選択されたサンドボックスタイプ is サンドボックス名の横のラジオボタンで示されます。

サンドボックスの消費レポートは、同じタイプのすべてのサンドボックスに対して累積的です。 In つまり、「[!UICONTROL  実稼動 ]」または「[!UICONTROL  開発 ]」を選択すると、それぞれ、すべての実稼動サンドボックスまたは開発用サンドボックスの消費レポートが表示されます。

![ 実稼動用サンドボックスと開発用サンドボックスがハイライト表示された Platform 製品の概要ビュー ](../images/license-usage/summary-tab-sandboxes.png)

>[!WARNING]
>
ライセンス使用状況ダッシュボードを表示する権限は、サンドボックスレベルで指定する必要があります。 個々のサンドボックスに権限を追加して、ダッシュボード内で表示します。 この制限は、今後のリリースで対処される予定です。 それまでの間、次の回避策を使用できます。
>
1. Adobe Admin Consoleで製品プロファイルを作成します。
2. サンドボックス カテゴリの権限で、表示するすべてのサンドボックスをライセンス使用状況ダッシュボードに追加します。
3. ユーザーダッシュボード権限カテゴリで、「ライセンス使用状況ダッシュボードを表示」権限を追加します。

## [!UICONTROL  詳細 ] タブ {#details-tab}

**特定のサンドボックスの特定の使用状況指標** を表示するには、「[!UICONTROL  詳細 ]」タブに移動します。 「[!UICONTROL  詳細 ]」タブには、実稼動用サンドボックスまたは開発用サンドボックス内で使用可能なすべてのサンドボックスが表示されます。

![ ライセンス使用状況ダッシュボードの「詳細」タブ ](../images/license-usage/details-tab.png)

このビューから、![ 検査アイコンを選択できます。サンドボックス名の横にあるを ](/help/images/icons/inspect.png) リックすると、その指標のビジュアライゼーションが表示されます。 ダイアログが開き、その指標のビジュアライゼーションが表示されます。

### ビジュアライゼーション {#visualizations}

各ビジュアライゼーションウィジェットには、次の側面が含まれています。

- 指標の変化の推移をトラッキングする折れ線グラフ
- 折れ線グラフのキー
- サンドボックス名
- 折れ線グラフの期間を調整するドロップダウンメニュー

折れ線グラフは、組織の使用状況数と組織のライセンスで利用可能な合計数を比較し、合計使用量の割合を示します。

![ 指標のビジュアライゼーション。](../images/license-usage/visualization.png)

分析のルックバック期間は、ドロップダウンメニューから調整できます。 過去 30 日間のデフォルト値

日付範囲を選択するには、日付範囲ドロップダウンを使用して、ダッシュボードに表示する期間を選択します。 過去 30 日間のデフォルト値など、複数のオプションが使用可能です。

![ 日付範囲のドロップダウンがハイライト表示されたビジュアライゼーションダイアログ。](../images/license-usage/date-range.png)

**[!UICONTROL カスタム日付]** を選択して、表示する期間を選択することもできます。

![ カスタムの日付範囲オプションがハイライト表示されたライセンス使用状況ダッシュボードの「概要」タブ。](../images/license-usage/custom-date-range.png)

## 使用可能な指標 {#available-metrics}

ライセンス使用状況ダッシュボードでは、組織内の複数の製品に適用できるいくつかの一意の指標についてレポートします。 使用できる指標は次のとおりです。

| 指標 | 説明 |
|---|---|
| [!UICONTROL Audience Activation サイズ ] | 1 年間に任意のファイルベースの宛先に対してアクティブ化されたプロファイルの合計サイズ。 注意：これには、ストリーミング宛先を介して送信されるプロファイルは含まれません。 |
| [!UICONTROL  アドレス可能なオーディエンス ] | ビジネスオーディエンスの使用権限と消費者オーディエンスの使用権限の合計。 消費者オーディエンスは、販売注文で「消費者オーディエンス」として識別されるユーザープロファイルの数として定義されます。 ビジネスオーディエンスは、販売注文で「ビジネスオーディエンス」として識別されるビジネスユーザープロファイルの数として定義されます。 |
| [!UICONTROL  アドホッククエリサービスユーザーパック ] | 承認された同時クエリサービスユーザーの使用権限を増やすアドオン。5 人の同時クエリサービスユーザーと、パックごとに 1 つの同時に実行されるアドホッククエリを追加できます。 複数の追加アドホック クエリ ユーザーパックがライセンスされている可能性があります。 |
| [!UICONTROL  平均プロファイル充実度 ] | 任意の時点でハブプロファイルサービス内に保存されたすべての実稼動データの合計を、許可されたビジネスユーザープロファイルの数の 5 倍で割った値です。 [!UICONTROL  平均的なプロファイル充実度 ] は共有機能です。 |
| [!UICONTROL  使用可能な CJA 行 ] | Customer Journey Analytics内の分析に使用できる 1 日あたりの平均データ行数。 |
| [!UICONTROL  計算属性 ] | 集計されたプロファイル行動データの合計数。 集計プロファイルの行動データは、プロファイル属性に変換されるエクスペリエンスイベントに基づいており、ユーザープロファイルまたはビジネスユーザープロファイルに含めることができます。 |
| [!UICONTROL  消費者オーディエンス ] | 販売注文で「消費者オーディエンス」として識別されたユーザープロファイルの数。 |
| [!UICONTROL  データの書き出しサイズ ] | 1 年にデータセットのアクティベーションを通じて送信されるデータの量。 |
| [!UICONTROL  データの書き出し ] | 1 年に（直接または間接的に）Adobe以外のソリューションに書き出すことができるデータセットの合計サイズ。 |
| [!UICONTROL Data Lake ストレージ ] | Adobe Experience Platform内で使用される分析データストアの量です。 |
| [!UICONTROL  エンゲージメント可能なオーディエンス ] | この指標は、エンゲージメント可能なプロファイルのオーディエンスを参照します。 エンゲージメント可能なプロファイルは、個人を表す情報のレコードで、プロファイルサービスで表されます。 これらのレコードは、過去 12 か月間に、Journey Optimizerのオーサリング、意思決定、配信、実験またはオーケストレーション機能を使用してエンゲージしようとしたプロファイルです。 |
| [!UICONTROL  類似オーディエンス ] | 既存の消費者オーディエンスをモデリングして、その既存の消費者オーディエンスに類似したユーザープロファイルを識別することで生成されるオーディエンスの数。 |
| [!UICONTROL AMM モデルの数 ] | 投資に基づいて指定された成果を測定または予測するために使用される機械学習モデル（Adobe Mix Modelerで構築）のカウント。 |
| [!UICONTROL  サンドボックスの数 ] | データと操作を分離してAdobe Experience PlatformにアクセスするAdobeのオンデマンドサービスのインスタンス内の論理分離の数。 |
| [!UICONTROL  プロファイル充実度パック数 ] | 追加のプロファイル充実度パックごとに、プロファイルごとに認証済みの平均プロファイル充実度が 25 KB 増加します。 |
| [!UICONTROL  クエリサービス計算時間 ] | バッチクエリの実行時に、クエリサービスエンジンがデータの読み取り、処理およびデータレイクへの書き込みに要した時間の測定値。 |
| [!UICONTROL  ストリーミングセグメント化パック数 ] | パックは、新しいデータがストリーミングフローを通じてセグメント化サービスに入ると、個人プロファイルのセグメントメンバーシップを更新します。 セグメントメンバーシップは、過去の行動を考慮せずに、現在の人物プロファイル属性と現在のイベントの値に基づいて評価されます。 ストリーミングセグメント化は共有機能です。 |

<!-- |  [!UICONTROL Sandbox No of Packs] |  A logical separation within your instance of any Adobe On-demand Service that accesses Adobe Experience Platform isolating data and operations | -->

>[!TIP]
>
販売注文でライセンスの使用権限を確認して、「ストレージ許可」などの指標を計算できます。<br> 例：<ul><li>ストレージ許可=契約内の「承認済みプロファイル」の数 X プロファイルの平均充実度</li></ul>

これらの指標の可用性と各指標の具体的な定義は、組織が購入したライセンスによって異なります。 各指標の定義について詳しくは、該当する製品説明ドキュメントを参照してください。

| ライセンス | 商品の説明 |
|---|---|
| <ul><li>ADOBE EXPERIENCE PLATFORM:OD LITE</li><li>ADOBE EXPERIENCE PLATFORM:OD 標準</li><li>ADOBE EXPERIENCE PLATFORM:OD ヘビー</li></ul> | [Adobe Experience Platform](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform.html) |
| <ul><li>ADOBE EXPERIENCE PLATFORM:OD</li></ul> | [Experience Platform、アプリ サービス、およびインテリジェント サービス ](https://helpx.adobe.com/legal/product-descriptions/exp-platform-app-svcs.html) |
| <ul><li>RT CUSTOMER DATA PLATFORM:OD</li><li>RT CUSTOMER DATA PLATFORM:OD PRFL ～ 10M</li><li>RT CUSTOMER DATA PLATFORM:OD PRFL ～ 50M</li></ul> | [Adobe Real-time Customer Data Platform](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) |
| <ul><li>AEP:OD アクティベーション</li><li>AEP:OD ACTIVATION PRFL TO 10M</li><li>AEP:OD ACTIVATION PRFL （最大 50 M）</li></ul> | [Adobe Experience Platformのライセンス認証 ](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-experience-platform0.html) |
| <ul><li>AEP:OD INTELLIGENCE</li></ul> | [Adobe Experience Platform インテリジェンス ](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform-intelligence---product-description.html) |
| <ul><li>JOURNEY OPTIMIZER SELECT:OD</li><li>JOURNEY OPTIMIZERプライム：OD</li><li>JOURNEY OPTIMIZER ULTIMATE:OD</li><li>UNP AJO PRIME スターター：OD</li><li>UNP AJO ULTIMATE スターター：OD</li><li>UNP Real-Time CDP:OD プロファイルオーケストレーション</li></ul> | [Adobe Journey Optimizer](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-journey-optimizer.html) |

>[!WARNING]
>
ライセンス使用状況ダッシュボードは、組織にプロビジョニングされた最新のライセンスに関してのみレポートします。 組織にプロビジョニングされた最新のライセンスが上の表に表示されない場合、ライセンス使用状況ダッシュボードが正しく表示されない可能性があります。 今後のリリースで、1 つの組織での追加ライセンスと複数ライセンスのサポートが予定されています。

## 次の手順

このドキュメントを読むと、ライセンス使用状況ダッシュボードを見つけ、購入した各製品、すべての実稼動または開発用サンドボックス、特定のサンドボックスの使用状況指標を表示できるようになります。 組織が購入したライセンスに基づいて、組織で使用可能な指標に関する詳細を確認できます。

Platform UI で使用できるその他の機能について詳しくは、[Experience Platform UI ガイド ](../../landing/ui-guide.md) を参照してください。
