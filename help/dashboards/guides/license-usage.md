---
keywords: Experience Platform;ユーザーインターフェイス;UI;カスタマイズ;ライセンス使用状況ダッシュボード;ダッシュボード;ライセンス使用状況;使用権限;使用
title: ライセンス使用状況ダッシュボード
description: Adobe Experience Platform、組織のライセンス版使用状況に関する重要な情報表示できるダッシュボードを提供します。
type: Documentation
exl-id: 143d16bb-7dc3-47ab-9b93-9c16683b9f3f
source-git-commit: 62f5ecf82df46284365e64d633c8242ac45567bc
workflow-type: tm+mt
source-wordcount: '3275'
ht-degree: 41%

---

# ライセンス使用状況ダッシュボード {#license-usage-dashboard}

>[!CONTEXTUALHELP]
>id="testy-mctestface"
>title="表示してはいけないテストダイアログ"
>abstract="オブジェクト {name} は {date} に表示されています。"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_core"
>title="コア製品テーブル"
>abstract="テーブルにリストされているコア製品には、サンドボックスレベルでの独自の指標、使用状況トラッキング、ドリルスルービューがあります。これらのコア製品は、トラッキングの主要指標を提供し、これらの指標には任意のアドオンが含まれます。"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_addons"
>title="アドオンテーブル"
>abstract="アドオンテーブルには、ライセンス数がコア製品でサポートされる指標と組み合わされる製品がリストされます。これらのアドオンには、個別の指標はありませんが、関連付けられているコア製品の使用状況トラッキングを強化します。"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseUsage"
>title="ライセンス使用状況ダッシュボード"
>abstract="ライセンス使用状況ダッシュボードでは、購入した Adobe Experience Platform 製品に関するインサイトを得ることができます。ダッシュボードの概要には、各プライマリ指標の使用状況や契約済みのライセンス金額など、製品のプライマリ指標が表示されます。詳細ワークスペースは、特定のサンドボックス内の各製品の指標の分類を表示します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-lifecycle/ui/dataset-expiration.html?lang=ja" text="データセットの有効期限の自動化"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_licenseusage"
>title="ライセンス使用状況ダッシュボード"
>abstract="ライセンス使用状況ダッシュボードでは、購入した Adobe Experience Platform 製品に関するインサイトを得ることができます。ダッシュボードの概要には、各プライマリ指標の使用状況や契約済みのライセンス金額など、製品のプライマリ指標が表示されます。詳細ワークスペースは、特定のサンドボックス内の各製品の指標の分類を表示します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-lifecycle/ui/dataset-expiration.html?lang=ja" text="データセットの有効期限の自動化"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_predictedusage_computehours"
>title="予測される計算時間"
>abstract="計算時間は、バッチクエリの実行時に、クエリサービスエンジンがデータの読み取り、処理、書き込みに費やす時間を測定します。<br>使用量がライセンス量に達する可能性があります。使用量を評価または削減するには、クエリ／ログに移動してクエリ履歴を確認します。クエリワークスペースに対するアクセス権がない場合は、管理者にお問い合わせください。"
>additional-url="https://experience.adobe.com/#/platform/query/log.html" text="クエリログワークスペース"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_predictedusage_addressableaudience"
>title="予測されるアドレス可能なオーディエンス"
>abstract="アドレス可能なオーディエンスは、組織が関与する資格のあるリアルタイム顧客プロファイルの一連のユーザープロファイルです。この指標には、直接識別可能なプロファイルと偽名プロファイルの両方が含まれます。<br>使用量がライセンス量に達する可能性があります。使用量を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_predictedusage_engageableprofiles"
>title="予測されるエンゲージ可能なプロファイル"
>abstract="エンゲージ可能なプロファイルは、組織が過去 12 か月以内に Journey Optimizer を使用してエンゲージを試みた、リアルタイム顧客プロファイルのユーザープロファイルです。<br>使用量がライセンス量に達する可能性があります。使用量を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_predictedusage_businesspersonprofile"
>title="予測されるビジネスユーザーのプロファイル"
>abstract="ビジネスユーザーのプロファイルは、B2B コンテキストで個人を表すリアルタイム顧客プロファイルのレコードです。<br>使用量がライセンス量に達する可能性があります。使用量を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_predictedusage_corehours"
>title="予測されるコア時間"
>abstract="コア時間は、Experience Platform サービス全体で消費される処理時間を表します。<br>使用量がライセンス量に達する可能性があります。使用量を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_predictedusage_totaldatavolume"
>title="予測される合計データボリューム"
>abstract="合計データボリュームは、エンゲージメントワークフローとパーソナライゼーションワークフローで使用するリアルタイム顧客プロファイルで使用可能なデータの量です。<br>使用量がライセンス量に達する可能性があります。使用量を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_predictedusage_cjaRowsAvailable"
>title="予測される使用可能な CJA 行数"
>abstract="使用可能な CJA 行数は、Customer Journey Analytics で分析に使用できるデータの 1 日あたりの平均行数を指します。<br>使用量がライセンス量に達する可能性があります。使用量を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_exceededusage_addressableaudience"
>title="予測されるアドレス可能なオーディエンス"
>abstract="アドレス可能なオーディエンスは、組織が関与する資格のあるリアルタイム顧客プロファイルの一連のユーザープロファイルです。これには、直接識別可能なプロファイルと偽名プロファイルの両方が含まれます。<br>使用量がライセンス量を超えました。使用量を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_exceededusage_engageableprofiles"
>title="予測されるエンゲージ可能なプロファイル"
>abstract="エンゲージ可能なプロファイルは、組織が過去 12 か月以内に Journey Optimizer を使用してエンゲージを試みた、リアルタイム顧客プロファイルのユーザープロファイルです。<br>使用量がライセンス量を超えました。使用量を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_exceededusage_businesspersonprofile"
>title="予測されるビジネスユーザーのプロファイル"
>abstract="ビジネスユーザーのプロファイルは、B2B コンテキストで個人を表すリアルタイム顧客プロファイルのレコードです。<br>使用量がライセンス量を超えました。使用量を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_exceededusage_corehours"
>title="予測されるコア時間"
>abstract="コア時間は、Experience Platform サービス全体で消費される処理時間を表します。<br>使用量がライセンス量を超えました。使用量を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_exceededusage_totaldatavolume"
>title="予測される合計データボリューム"
>abstract="合計データボリュームは、エンゲージメントワークフローとパーソナライゼーションワークフローで使用するリアルタイム顧客プロファイルで使用可能なデータの量です。<br>使用量がライセンス量を超えました。使用量を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_exceededusage_cjaRowsAvailable"
>title="予測される使用可能な CJA 行数"
>abstract="使用可能な CJA 行数は、Customer Journey Analytics で分析に使用できるデータの 1 日あたりの平均行数を指します。<br>使用量がライセンス量を超えました。使用量を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

組織のライセンス版使用状況に関する重要な情報は、Adobe Experience Platform [!UICONTROL License usage] ダッシュボードから表示できます。 ここに表示される情報は、Experience Platformインスタンスの毎日のスナップショット中にキャプチャされます。

ライセンス使用状況レポートは、高度な精度を提供します。 ほとんどの指標は複数の製品で共有され、製品ごとの合計ではなく、それらを使用するすべての製品の集計使用量を反映しています。 ダッシュボードでは、これらの指標をすべての実稼動サンドボックスまたは開発サンドボックスで使用する統合された使用状況と、特定のサンドボックスからの使用状況指標を提供します。 使用状況指標を使用して追跡できるExperience Platform アプリケーションは、Real-Time Customer Data Platform、Adobe Journey Optimizer、Customer Journey Analyticsです。

このガイドでは、UIでライセンス版使用状況ダッシュボードにアクセスして操作する方法の概要を説明し、ダッシュボードに表示される視覚化に関する詳細情報を提供します。

Experience Platform UIの概要については、 [Experience Platform UI ガイド](../../landing/ui-guide.md) を参照してください。

## [!UICONTROL License usage] ダッシュボードデータ

[!UICONTROL License usage]ダッシュボードには、購入したすべてのExperience Platform製品のリストと、それらの製品のアドオンが表示されます。このダッシュボードから、関連付けられているサンドボックス全体のExperience Platform組織のライセンス版関連データのスナップショットを見つけることができます。

このダッシュボードのデータは、スナップショットが作成された特定の時点で表示されていたとおりに表示されます。 これは近似値やサンプルではありませんが、ダッシュボードはリアルタイムで更新されません。

>[!NOTE]
>
>ダッシュボード内のほとんどの指標は、Experience Platformインスタンスのスナップショットに基づいて毎日更新されます。 [!UICONTROL CJA Rows Available] は例外で、毎月更新されます。 [!UICONTROL Adhoc Query Service Users Packs]、[!UICONTROL Profile Richness No of Packs]、[!UICONTROL Streaming Segmentation No of Packs]などの &quot;パック&quot; でラベル付けされたメトリックは、アドオン オファリングのライセンス版権利を反映し、継続的な使用状況を追跡しません。スナップショット後に行われた変更は、次のスナップショットが作成されるまで表示されません。

## ライセンス版使用状況ダッシュボードの詳細 {#explore}

Experience Platform UI内のライセンス版使用状況ダッシュボードに移動するには、左側のパネルで [ **[!UICONTROL License usage]** ] を選択します。 ダッシュボードには、[ **[!UICONTROL Metrics]** ] と [ **[!UICONTROL Products]**] の 2 つのタブがあります。

>[!NOTE]
>
>ライセンス使用状況ダッシュボードは、デフォルトでは有効になっていません。 ダッシュボードを表示するには、「ライセンス使用状況ダッシュボードを表示」権限をユーザーに付与する必要があります。 アクセス権限の付与手順については、[&#x200B; ダッシュボード権限ガイド &#x200B;](../permissions.md) を参照してください。

## 「[!UICONTROL Metrics]」タブ {#metrics-tab}

「**[!UICONTROL Metrics]**」タブには、組織全体のすべてのライセンス使用状況の指標が一元的に表示されます。 ほとんどの指標は製品間で共有されるため、これらの指標に製品ごとの個別の分類はありません。

指標の表には、以下の列が含まれます。

| 列の名前 | 説明 |
|---|---|
| **[!UICONTROL Metric Name]** | ライセンス版使用法指標の名前。 各エントリには、関連付けられた製品の説明とリストを表示する情報アイコン(`ⓘ`)が含まれています。 |
| **[!UICONTROL Licensed]** | 契約で定義されている、組織が使用する資格のあるユニットの数。 この指標は、製品タブの **ライセンス量** と同じ値です。 |
| **[!UICONTROL Measured]** | 組織で現在使用されている指標の量。 |
| **[!UICONTROL Usage %]** | ライセンス値の現在使用中の割合。 |
| **[!UICONTROL Predicted Usage %]** | 今後 6 週間の指標使用量の予測範囲。 |

**[!UICONTROL Production]** または **[!UICONTROL Development]** サンドボックスの切り替えを使用して、サンドボックスによって表示される指標をフィルタリングします。

>[!NOTE]
>
>消費レポートはサンドボックスタイプ別の累積です。 [!UICONTROL Production]または[!UICONTROL Development]を選択すると、そのタイプのすべてのサンドボックスでの合計使用量が表示されます。

![[ライセンス使用状況] ダッシュボードの [メトリック] タブ、メトリック、ライセンス版量、および使用状況データのリストが表示されます。](../images/license-usage/metrics-tab.png)

>[!WARNING]
>
>ライセンス版使用状況ダッシュボード表示権限は、サンドボックスレベルで指定する必要があります。 個々のサンドボックスに権限を追加して、ダッシュボード内で表示します。 この制限は、今後のリリースで対処される予定です。 それまでの間、次の回避策を使用できます。
>
>1. Adobe Admin Consoleで製品プロファイルを作成します。
>2. サンドボックス カテゴリの権限で、表示するすべてのサンドボックスをライセンス使用状況ダッシュボードに追加します。
>3. ユーザーダッシュボード権限カテゴリで、「ライセンス使用状況ダッシュボードを表示」権限を追加します。

### 指標の詳細の表示 {#view-metric-details}

特定の指標の使用状況詳細を表示するには、リストで指標名を選択します。 次のような指標の詳細ビューが表示されます。

- 使用状況の推移を示す履歴の折れ線グラフ
- ライセンス値と測定値の比較
- 個人サンドボックスによる使用状況
- データをフィルタリングするためのサンドボックスセレクター
- CSV ダウンロード用の書き出しオプション

このビジュアライゼーションを使用すると、トレンドの追跡、各サンドボックスが全体的な使用状況にどのように貢献しているかを把握、オフライン分析用にデータを書き出すことができます。

各グラフには、データをフィルタリングするためのドロップダウンメニューが含まれています。 日付範囲ドロップダウンを使用して、ルックバック期間（デフォルト：過去 30 日間）を調整するか、サンドボックスドロップダウンを使用して、特定の実稼動用または開発用サンドボックスの使用状況を表示します。

![&#x200B; 履歴使用状況グラフ、サンドボックステーブル、エクスポートボタンを含んだアドレス可能なオーディエンス指標の詳細ビュー &#x200B;](../images/license-usage/metric-details-view.png)

また、 **[!UICONTROL Custom date]** を選択して、表示する期間を選択することもできます。

![[ライセンス使用状況] ダッシュボード概要タブ、カスタム 日付範囲 オプションが強調表示されます。](../images/license-usage/custom-date-range.png)

### CSVエクスポート {#export-metric-usage-data}

選択した指標とサンドボックスの使用状況履歴データを、指標の詳細表示から直接CSVファイルとしてエクスポートできます。 **[!UICONTROL Export]** アイコンを選択して、グラフのデータを表形式で形式ダウンロードするします。エクスポートされたCSVを使用すると、傾向オフライン分析したり、チーム間で使用状況の分析情報を共有したりすることが容易になります。

## [!UICONTROL Products] タブ {#products-tab}

**[!UICONTROL Products]**&#x200B;タブにはライセンス版購入した製品と関連するアドオンごとにグループ化された使用状況データが表示されます。[!UICONTROL Products]タブには、次の 2 つのテーブルがあります。

- **[!UICONTROL Core products]table**: 次の表に、組織によってライセンスされている主なAdobe Experience Platform製品が一覧表示されます。 各製品には、主要指標、使用状況トラッキングおよび予測される使用量が一覧表示されます。
- **[!UICONTROL Add-ons]table**: コア製品指標に貢献するライセンス版金額の補足品目を一覧表示します。 追加オンには個別のメトリックはありませんが、関連付けられているコア製品の使用状況トラッキングが向上します。

| 列の名前 | 説明 |
|---|---|
| **[!UICONTROL Product]** | 組織によってライセンスされたAdobe Systemsソリューション。 |
| **[!UICONTROL Primary Metric]** | 製品内のトラッキングに使用されるプライマリ指標。 |
| **[!UICONTROL License Amount]** | プライマリ指標の最大量に対する契約値。 |
| **[!UICONTROL Usage]** | 使用されたプライマリ指標の量。 |
| **[!UICONTROL Usage %]** | プライマリ指標のうち、ライセンス版量に応じた割合が使用されます。 |
| **[!UICONTROL Predicted Usage]** | プライマリ指標の予測される使用率。 |

>[!NOTE]
>
>アドオンの [!UICONTROL License Amount] は、コア製品の合計ライセンス版量に含まれます。 追加オンは個別に追跡されませんが、関連する製品の機能が強化されます。 たとえば、アドオンとして5つのサンドボックスの1パックを購入した場合、その金額は基本製品の金額に追加されます。 アドオンの表にはアドオンに固有の [!UICONTROL License Amount] が表示されますが、実際の使用状況は基本製品を通じて追跡されます。

![ライセンス使用状況ダッシュボード製品、コア製品と追加オンの表を含むタブ。](../images/license-usage/products-tab.png)

### 予測される使用状況 {#predicted-usage}

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseUsage_prediction"
>title="予測される使用状況"
>abstract="予測は過去6～7か月の使用状況に基づき、毎週金曜日に週単位で生成されます。ライセンス使用状況の予測は過去の使用状況に基づく概算です。お客様は、組織における実際の使用状況を理解し、その使用状況がアドビとの組織のライセンスの範囲を超えないようにする責任があります。使用量を減らすには、サンドボックスとデータセットに対するデータセットまたは偽名プロファイルのデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-lifecycle/ui/dataset-expiration.html?lang=ja" text="データセットの有効期限の自動化"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_licenseusage_prediction"
>title="予測される使用状況"
>abstract="予測は過去6～7か月の使用状況に基づき、毎月15日に生成されます。ライセンス使用状況の予測は過去の使用状況に基づく概算です。お客様は、組織における実際の使用状況を理解し、その使用状況がアドビとの組織のライセンスの範囲を超えないようにする責任があります。使用量を減らすには、サンドボックスとデータセットに対するデータセットまたは偽名プロファイルのデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-lifecycle/ui/dataset-expiration.html?lang=ja" text="データセットの有効期限の自動化"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

正確で最新の使用状況予測を使用して、ライセンスリソースをプロアクティブに管理し、最適化します。 [!UICONTROL Predicted Usage]列では、購入したすべての製品のすべての本番および開発サンドボックスでのサンドボックスレベルでの将来のライセンス版使用量が予測されます。予測は毎週更新され、最新の使用状況データに基づいて 6 週間の予測が提供されます。 各予測には、情報に基づいた計画をサポートするために、下限と上限の両方が含まれています。

>[!IMPORTANT]
>
>予測は毎週金曜日に更新されます。 更新日は、情報アイコン (![この情報アイコン) に表示されます。](../images/license-usage/info-icon.png))を列タイトルの上にドラッグします。

[!UICONTROL Product]テーブルの下の[!UICONTROL Core products]タブから製品の資格使用状況の概要表示。

![[!UICONTROL License usage] [!UICONTROL Product]タブ、製品と予測使用量の列が強調表示されます。](../images/license-usage/product-predicted-usage.png)

>[!NOTE]
>
>ライセンス使用状況の予測は過去の使用状況に基づく概算です。お客様は、組織の実際の使用状況を理解し、Adobeで組織のライセンスの範囲外に使用状況が使用されないようにする責任を負います。

予測される使用状況の割合は、次のように決定されます。

- 下限と上限が大きく異なる場合は、範囲として表示されます（例：32% ～ 35%）。
- 下限と上限がほぼ同じでゼロでない場合は、近似値（例：約 34%）として表示されます。
- 下限と上限がほぼ同じでゼロの場合、正確に 0% と表示されます。

>[!NOTE]
>
>このコンテキストの「ほぼ同一」とは、値が統計的に小数点以下 2 桁まで有意であることを意味します（例えば、下限が 0.342、上限が 0.344 の場合、どちらも 34% に丸められます）。

予測使用量機能は、次の指標をサポートしています。

- [!UICONTROL Addressable audience]
- [!UICONTROL Businessperson profiles]
- [!UICONTROL Compute hours]
- [!UICONTROL Customer Journey Audience number of rows]
- [!UICONTROL Engageable profiles]
- [!UICONTROL Total Data Volume]

## 使用可能な指標 {#available-metrics}

>[!IMPORTANT]
>
>8 月 20 日以降、「[!UICONTROL Average Profile Richness]」と「[!UICONTROL Total Storage]」の資格を持つお客様には、代わりにライセンス使用状況ダッシュボードに &quot;[!UICONTROL Total Data Volume]&quot; と表示されます。 顧客の使用権限に変更はなく、トラッキング指標の簡略化のみでした。 [!UICONTROL Total Data Volume] は、エンゲージメント および パーソナライズ機能 ワークフローの Real-時間 Customer プロフィール で使用可能なデータを表します。 このシンプル化された指標により、リアルタイム顧客プロファイルの使用に関する管理と測定が向上しました。 この変更に関する詳細については、Adobeの担当者に問い合わせることをお勧めします。

ライセンス使用状況ダッシュボードでは、組織内の複数の製品に適用できるいくつかの一意の指標についてレポートします。 使用できる指標は次のとおりです。

| 指標 | 説明 |
|---|---|
| [!UICONTROL Audience Activation Size] | 1 年間に任意のファイルベースの宛先に対してアクティベートされたプロファイルの合計サイズ。メモ : これには、ストリーミング宛先を通じて送信したプロファイルは含まれません。 |
| [!UICONTROL Addressable Audience] | 組織が関与する権利がある Real-時間 Customer プロフィール のユーザー プロファイルのセット (直接識別可能なプロファイルと仮名プロファイルの両方を含む)。 これらのプロファイルには、属性、動作、およびセグメントメンバーシップデータを含めることができます。 プロフィールボリュームは、Adobe Experience Platform のデフォルトの決定論的IDグラフを使用して計算され、共有機能と見なされます。 |
| [!UICONTROL Adhoc Query Service Users Packs] | 承認済み同時クエリサービスユーザーの使用権限を追加するアドオン。パックごとに同時クエリサービスユーザー 5 人と同時実行アドホッククエリ 1 つが追加されます。追加のアドホッククエリユーザーパックのライセンスは複数購入可能です。 |
| [!UICONTROL Average profile richness] | **非推奨** - 任意の時点でハブプロフィールサービスに保存されているすべての本番データの合計を、承認されたビジネスパーソンプロファイルの数の5倍で割ったものです。 [!UICONTROL Average profile richness] は共有機能です。 |
| [!UICONTROL CJA Rows Available] | Customer Journey Analytics 内で分析に使用できるデータの 1 日あたりの平均行数。 |
| [!UICONTROL Computed Attributes] | プロフィール属性に変換され、Person プロフィールに含めることができるエクスペリエンスイベントに基づいて集計されたプロファイル行動データ。 |
| [!UICONTROL Consumer Audience] | 販売注文で「消費者オーディエンス」として識別された個人プロファイルの数。 |
| [!UICONTROL Data Export Size] | データセットのアクティベーションを通じて 1 年間に送信されたデータの量。 |
| [!UICONTROL Data Exports] | 1 年間にアドビ以外のソリューションに (直接または間接的に) 書き出すことができるデータセットの合計サイズ。 |
| [!UICONTROL Data Lake Storage] | Adobe Experience Platform 内の分析データストアの使用量。 |
| [!UICONTROL Engageable Audience] | Journey Optimizer のオーサリング、決定、配信、実験、またはオーケストレーション機能を使用して過去 12 か月間に関与しようとした Real-時間 Customer プロフィール の個人プロファイルのグループ。 |
| [!UICONTROL Look-alike Audiences] | コンシューマLook類似オーディエンスは、既存のコンシューマオーディエンスをモデル化して、類似した属性または動作を持つ個人プロファイルを識別することによって生成されるオーディエンスです。 |
| [!UICONTROL Number of AMM Models] | 投資に基づいて指定した結果を測定 / 予測するために使用される機械学習モデル (Adobe Mix Modeler に組み込み済み) の数。 |
| [!UICONTROL Number of Sandboxes] | Adobe Experience Platform にアクセスしてデータと操作を分離する Adobe オンデマンドサービスのインスタンス内での論理分離の数。 |
| [!UICONTROL Profile Richness No of Packs] | 追加のプロファイルリッチネスパックごとに、許可される合計データ量がプロファイルあたり 25 KB 増加します。 |
| [!UICONTROL Query Service Compute Hours] | バッチクエリの実行時に、クエリサービスエンジンがデータレイクに対してデータの読み取り、処理、書き戻しを行うために必要な時間の測定値。 |
| [!UICONTROL Streaming Segmentation No of Packs] | パックでは、ストリーミングフローを通じて新しいデータをセグメント化サービスに入力すると、ユーザープロファイルのセグメントメンバーシップが更新されます。セグメントメンバーシップは、過去の行動を考慮せずに、現在のユーザープロファイル属性と現在のイベントの値に基づいて評価されます。ストリーミングセグメント化は共有機能です。 |
| [!UICONTROL Total Data Volume] | エンゲージメントワークフローで使用できる Real-時間 Customer プロフィール で使用できるデータの合計量。 データ音量合計、次の式を使用して計算されます: **合計 データ音量 = アドレス可能なオーディエンス×平均プロフィールリッチネス**。 この指標は、プロフィール ストアにのみ格納されたデータを反映し、データ レイク ストレージは除外します。 プロファイルベースのエンゲージメントに関連するデータのより焦点を絞った表示を提供します。 詳細については、合計 データ 音量に関する [よく寄せられる質問](../../landing/license-usage-and-guardrails/total-data-volume.md) を参照してください。 |
| [!UICONTROL Total Volume of Data Egress] | Adobe Experience Platform から サードパーティ のデータウェアハウスにエクスポートされたデータの年間累積量。 |

<!-- |  [!UICONTROL Sandbox No of Packs] |  A logical separation within your instance of any Adobe On-demand Service that accesses Adobe Experience Platform isolating data and operations | -->

>[!TIP]
>
>販売注文でライセンス版権利を確認して、「ストレージ許容量」などの指標を計算できます。<br>例えば<ul><li>ストレージ許容量 = 契約内の「許可済みプロファイル」の数 X 平均プロフィールリッチネス</li></ul>

これらのメトリックの可用性と、これらの各メトリックの具体的な定義は、組織が購入したライセンスによって異なります。 各指標の詳細な定義については、該当する製品説明のドキュメントを参照してください。

| ライセンス | 製品説明 |
| --- | --- |
| <ul><li>ADOBE EXPERIENCE PLATFORM:OD LITE</li><li>ADOBE EXPERIENCE PLATFORM:OD STANDARD</li><li>ADOBE EXPERIENCE PLATFORM:OD 重い</li></ul> | [Adobe Experience Platform](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-experience-platform.html) |
| <ul><li>ADOBE EXPERIENCE PLATFORM:OD</li></ul> | [Experience Platform、アプリケーション サービス、およびインテリジェント サービス](https://helpx.adobe.com/jp/legal/product-descriptions/exp-platform-app-svcs.html) |
| <ul><li>RT カスタマーデータプラットフォーム:OD</li><li>RT カスタマーデータプラットフォーム:OD PRFL から 10M</li><li>RT カスタマーデータプラットフォーム:OD PRFL から 50M</li></ul> | [Adobe Systems Real-時間 Customer データ Platform](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) |
| <ul><li>AEP :OD ACTIVATION</li><li>AEP:OD ACTIVATION PRFL TO 10M</li><li>AEP:OD ACTIVATION PRFL TO 50M</li></ul> | [Adobe Experience Platform Activation](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-experience-platform0.html) |
| <ul><li>AEP:OD INTELLIGENCE</li></ul> | [Adobe Experience Platform インテリジェンス &#x200B;](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-experience-platform-intelligence---product-description.html) |
| <ul><li>JOURNEY OPTIMIZER:OD</li><li>JOURNEY OPTIMIZERPRIME:OD</li><li>JOURNEY OPTIMIZERULTIMATE:OD</li><li>UNP AJO プライムスターター:OD</li><li>UNP AJO ULTIMATE スターター :OD</li><li>UNP Real-Time CDP:OD プロファイルオーケストレーション</li></ul> | [Adobe Journey Optimizer](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-journey-optimizer.html) |

>[!WARNING]
>
>ライセンス使用状況ダッシュボードは、組織にプロビジョニングされた最新のライセンスに関してのみレポートします。 組織用にプロビジョニングされた最新のライセンス版が上記の表に表示されない場合は、ライセンス版使用状況ダッシュボードが正しく表示されない可能性があります。 1 つの組織での追加ライセンスと複数のライセンスのサポートは、今後のリリースで予定されています。

## 次の手順

このドキュメントを読むと、ライセンス使用状況ダッシュボードを見つけ、購入した各製品、すべての実稼動または開発用サンドボックス、特定のサンドボックスの使用状況指標を表示できるようになります。 組織が購入したライセンスに基づいて、組織で使用可能な指標に関する詳細を確認できます。

Experience Platform UI で使用できるその他の機能について詳しくは、[Experience Platform UI ガイド &#x200B;](../../landing/ui-guide.md) を参照してください。
