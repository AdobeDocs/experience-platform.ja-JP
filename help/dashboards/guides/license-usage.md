---
keywords: Experience Platform;ユーザーインターフェイス;UI;カスタマイズ;ライセンス使用状況ダッシュボード;ダッシュボード;ライセンス使用状況;使用権限;使用
title: ライセンス使用状況ダッシュボード
description: Adobe Experience Platformには、組織のライセンス使用状況に関する重要な情報を表示できるダッシュボードが用意されています。
type: Documentation
exl-id: 143d16bb-7dc3-47ab-9b93-9c16683b9f3f
source-git-commit: 62f5ecf82df46284365e64d633c8242ac45567bc
workflow-type: tm+mt
source-wordcount: '3455'
ht-degree: 39%

---

# ライセンス使用状況ダッシュボード {#license-usage-dashboard}

>[!CONTEXTUALHELP]
>id="testy-mctestface"
>title="表示してはいけないテストダイアログ"
>abstract="{date} で表示され {name} オブジェクト。"

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

Adobe Experience Platformの [!UICONTROL  ライセンスの使用状況 ] ダッシュボードを使用して、組織のライセンスの使用状況に関する重要な情報を表示できます。 ここに表示される情報は、Experience Platform インスタンスの毎日のスナップショット中にキャプチャされます。

ライセンス使用状況レポートでは高い精度が提供されます。 ほとんどの指標は、複数の製品で共有され、製品ごとの合計ではなく、それらを使用するすべての製品の集計使用状況を反映します。 ダッシュボードでは、これらの指標をすべての実稼動サンドボックスまたは開発サンドボックスで使用する統合された使用状況と、特定のサンドボックスからの使用状況指標を提供します。 使用状況指標を使用して追跡できるExperience Platform アプリケーションは、Real-Time Customer Data Platform、Adobe Journey Optimizer、Customer Journey Analyticsです。

このガイドでは、UI でのライセンス使用状況ダッシュボードへのアクセス方法と操作方法の概要を説明し、ダッシュボードに表示されるビジュアライゼーションの詳細を説明します。

Experience Platform UI の一般的な概要については、[Experience Platform UI ガイド ](../../landing/ui-guide.md) を参照してください。

## [!UICONTROL  ライセンス使用状況 ] ダッシュボードデータ

[!UICONTROL  ライセンスの使用状況 ] ダッシュボードには、購入したすべてのExperience Platform製品とそれらの製品のアドオンのリストが表示されます。 このダッシュボードから、関連する任意のサンドボックスをまたいで、Experience Platformに関する組織のライセンス関連データのスナップショットを見つけることができます。

このダッシュボードのデータは、スナップショットが作成された特定の時点とまったく同じように表示されます。 近似やサンプルではありませんが、ダッシュボードはリアルタイムでは更新されません。

>[!NOTE]
>
>ダッシュボード内のほとんどの指標は、Experience Platform インスタンスのスナップショットに基づいて毎日更新されます。 [!UICONTROL  使用可能なCJA行 ] は例外で、毎月更新されます。 [!UICONTROL  アドホッククエリサービスユーザーパック ]、[!UICONTROL  プロファイル充実度パック数 ]、[!UICONTROL  ストリーミングセグメント化パック数 ] など、「パック」でラベル付けされた指標は、アドオン製品のライセンス使用権限を反映し、継続的な使用を追跡しません。 スナップショット後に行われた変更は、次のスナップショットが作成されるまで表示されません。

## ライセンス使用状況ダッシュボードの確認 {#explore}

Experience Platform UI 内でライセンス使用状況ダッシュボードに移動するには、左パネルで **[!UICONTROL ライセンス使用状況]** を選択します。 ダッシュボードには、「**[!UICONTROL 指標]**」と「**[!UICONTROL 製品]** の 2 つのタブがあります。

>[!NOTE]
>
>ライセンス使用状況ダッシュボードは、デフォルトでは有効になっていません。 ダッシュボードを表示するには、「ライセンス使用状況ダッシュボードを表示」権限をユーザーに付与する必要があります。 アクセス権限の付与手順については、[ ダッシュボード権限ガイド ](../permissions.md) を参照してください。

## [!UICONTROL  指標 ] タブ {#metrics-tab}

「**[!UICONTROL 指標]**」タブには、組織全体のすべてのライセンス使用状況の指標が一元的に表示されます。 ほとんどの指標は製品間で共有されるので、これらの指標に対して個別の製品ごとの分類はありません。

指標テーブルには、次の列が含まれます。

| 列の名前 | 説明 |
|---|---|
| **[!UICONTROL 指標名]** | ライセンス使用状況指標の名前。 各エントリには、説明と関連する製品のリストを表示する情報アイコン（`ⓘ`）が含まれます。 |
| **[!UICONTROL ライセンス]** | 契約で定義されているとおり、組織が使用できるユニット数。 この指標は、「製品」タブの **ライセンス量** と同じ値です。 |
| **[!UICONTROL 測定済み]** | 組織が現在使用している指標の量。 |
| **[!UICONTROL 使用方法 %]** | ライセンス値の現在使用中の割合。 |
| **[!UICONTROL 予測された使用状況 %]** | 今後 6 週間の指標の使用状況の予測範囲。 |

**[!UICONTROL 実稼動]** サンドボックスまたは **[!UICONTROL 開発]** サンドボックスの切り替えスイッチを使用して、サンドボックスによって表示される指標をフィルタリングします。

>[!NOTE]
>
>消費レポートは、サンドボックスタイプによって累積されます。 [!UICONTROL  実稼動 ] または [!UICONTROL  開発 ] を選択すると、そのタイプのすべてのサンドボックスの合計使用量が表示されます。

![ 指標、ライセンス量、使用状況データのリストが表示されているライセンス使用状況ダッシュボードの「指標」タブ ](../images/license-usage/metrics-tab.png)

>[!WARNING]
>
>ライセンス使用状況ダッシュボードを表示する権限は、サンドボックスレベルで指定する必要があります。 個々のサンドボックスに権限を追加して、ダッシュボード内で表示します。 この制限は、今後のリリースで対処される予定です。 それまでの間、次の回避策を使用できます。
>
>1. Adobe Admin Consoleで製品プロファイルを作成します。
>2. サンドボックス カテゴリの権限で、表示するすべてのサンドボックスをライセンス使用状況ダッシュボードに追加します。
>3. ユーザーダッシュボード権限カテゴリで、「ライセンス使用状況ダッシュボードを表示」権限を追加します。

### 指標の詳細の表示 {#view-metric-details}

特定の指標の使用状況詳細を表示するには、リストで指標名を選択します。 次のような指標の詳細ビューが表示されます。

- 使用状況の推移を示す履歴の折れ線グラフ
- ライセンス値と測定値の比較
- 個々のサンドボックスによる使用
- データをフィルタリングするためのサンドボックスセレクター
- CSV ダウンロード用の書き出しオプション

このビジュアライゼーションを使用すると、トレンドの追跡、各サンドボックスが全体的な使用状況にどのように貢献しているかを把握、オフライン分析用にデータを書き出すことができます。

各グラフには、データをフィルタリングするためのドロップダウンメニューが含まれています。 日付範囲ドロップダウンを使用して、ルックバック期間（デフォルト：過去 30 日間）を調整するか、サンドボックスドロップダウンを使用して、特定の実稼動用または開発用サンドボックスの使用状況を表示します。

![ 履歴使用状況グラフ、サンドボックステーブル、エクスポートボタンを含んだアドレス可能なオーディエンス指標の詳細ビュー ](../images/license-usage/metric-details-view.png)

また、**[!UICONTROL カスタム日付]** を選択して、表示する期間を選択することもできます。

![ カスタムの日付範囲オプションがハイライト表示されたライセンス使用状況ダッシュボードの「概要」タブ。](../images/license-usage/custom-date-range.png)

### CSV の書き出し {#export-metric-usage-data}

選択した指標とサンドボックスの使用履歴データを、指標の詳細表示から直接、CSV ファイルとして書き出すことができます。 **[!UICONTROL 書き出し]** アイコンを選択して、グラフのデータを表形式でダウンロードします。 書き出された CSV を使用すると、オフラインのトレンドを分析したり、チーム間で使用状況のインサイトを共有したりすることが簡単にできます。

## [!UICONTROL  製品 ] タブ {#products-tab}

**[!UICONTROL 製品]** タブには、購入した製品と関連するアドオン別にグループ化されたライセンス使用状況データが表示されます。 「[!UICONTROL  製品 ]」タブには、次の 2 つのテーブルがあります。

- **[!UICONTROL コア製品 ] テーブル**：このテーブルには、組織でライセンスされている主なAdobe Experience Platform製品が一覧表示されます。 各製品には、主要指標、使用状況のトラッキングおよび予測される使用状況が一覧表示されます。
- **[!UICONTROL アドオン ] テーブル**：ライセンス量がコア製品指標に貢献する補助項目をリストします。 アドオンには、個別の指標はありませんが、関連付けられたコア製品の使用状況追跡を強化します。

| 列の名前 | 説明 |
|---|---|
| **[!UICONTROL 製品]** | お客様の組織でライセンスされたAdobe ソリューション。 |
| **[!UICONTROL プライマリ指標]** | その製品内でのトラッキングに使用される主な指標。 |
| **[!UICONTROL 許可の金額]** | プライマリ指標の最大量に対する契約値。 |
| **[!UICONTROL 用途]** | 使用されたプライマリ指標の量。 |
| **[!UICONTROL 使用方法 %]** | ライセンス量に応じて使用される主要指標の割合。 |
| **[!UICONTROL 予測される使用方法]** | プライマリ指標の予測使用率。 |

>[!NOTE]
>
>アドオンの [!UICONTROL  ライセンス額 ] は、コア製品の合計ライセンス額に含まれています。 アドオンは個別に追跡されるのではなく、関連する製品の機能を向上させます。 例えば、5 つのサンドボックスのパックを 1 組をアドオンとして購入した場合、金額はベース製品の金額に追加されます。 アドオンテーブルには、アドオンに固有の [!UICONTROL  ライセンス額 ] が表示されますが、実際の使用状況は基本製品を通じて追跡されます。

![ コア製品とアドオンのテーブルが表示されているライセンス使用状況ダッシュボードの「製品」タブ ](../images/license-usage/products-tab.png)

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

正確で最新の使用予測を使用して、ライセンスリソースをプロアクティブに管理および最適化します。 [!UICONTROL  予測使用状況 ] 列は、購入したすべての製品に関する、すべての実稼動および開発用サンドボックスにわたる、サンドボックスレベルでの将来のライセンス使用状況を予測します。 予測は毎週更新され、最新の使用状況データに基づいて 6 週間の予測が提供されるようになりました。 各予測には、情報に基づいた計画をサポートするための下限と上限の両方が含まれています。

>[!IMPORTANT]
>
>予測は毎週金曜日に更新されます。 更新日は、情報アイコン（![ この情報アイコン](../images/license-usage/info-icon.png)）を選択します。

[!UICONTROL  コア製品 ] テーブルの「[!UICONTROL  製品 ]」タブで、製品の使用権限の概要を表示します。

![ 製品と予測される使用状況列がハイライト表示された [!UICONTROL  ライセンス使用状況 ] [!UICONTROL  「製品 ]」タブ ](../images/license-usage/product-predicted-usage.png)

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

- [!UICONTROL  アドレス可能なオーディエンス ]
- [!UICONTROL  ビジネスユーザープロファイル ]
- [!UICONTROL  時間を計算 ]
- [!UICONTROL  顧客ジャーニーオーディエンスの行数 ]
- [!UICONTROL  エンゲージメント可能なプロファイル ]
- [!UICONTROL  合計データ量 ]

## 使用可能な指標 {#available-metrics}

>[!IMPORTANT]
>
>8 月 20 日以降、「[!UICONTROL  平均プロファイル充実度 ]」および「[!UICONTROL  合計ストレージ ]」の資格を持つお客様には、ライセンス使用状況ダッシュボードに「[!UICONTROL  合計データ量 ]」が表示されるようになりました。 顧客の使用権限に変更はなく、トラッキング指標の簡略化のみでした。 [!UICONTROL  合計データ量 ] は、エンゲージメントワークフローおよびパーソナライゼーションワークフローについて、リアルタイム顧客プロファイルで利用できるデータを表します。 このシンプル化された指標により、リアルタイム顧客プロファイルの使用に関する管理と測定が向上しました。 この変更に関する詳細については、Adobeの担当者に問い合わせることをお勧めします。

ライセンス使用状況ダッシュボードでは、組織内の複数の製品に適用できるいくつかの一意の指標についてレポートします。 使用できる指標は次のとおりです。

| 指標 | 説明 |
|---|---|
| [!UICONTROL Audience Activation サイズ ] | 1 年間に任意のファイルベースの宛先に対してアクティブ化されたプロファイルの合計サイズ。メモ : これには、ストリーミング宛先を通じて送信したプロファイルは含まれません。 |
| [!UICONTROL  アドレス可能なオーディエンス ] | 直接識別可能なプロファイルと偽名プロファイルの両方を含む、組織が関与する資格のあるリアルタイム顧客プロファイルのユーザープロファイルのセット。 これらのプロファイルには、属性、ビヘイビアー、セグメントメンバーシップデータを含めることができます。 プロファイルボリュームは、Adobe Experience Platformのデフォルトの決定論的 ID グラフを使用して計算され、共有機能と見なされます。 |
| [!UICONTROL  アドホッククエリサービスユーザーパック ] | 承認済み同時クエリサービスユーザーの使用権限を追加するアドオン。パックごとに同時クエリサービスユーザー 5 人と同時実行アドホッククエリ 1 つが追加されます。追加のアドホッククエリユーザーパックのライセンスは複数購入可能です。 |
| [!UICONTROL  平均プロファイル充実度 ] | **非推奨** – 任意の時点でハブプロファイルサービス内に保存されたすべての実稼動データの合計を、許可されたビジネスユーザープロファイルの数の 5 倍で割った値です。 [!UICONTROL  平均的なプロファイル充実度 ] は共有機能です。 |
| [!UICONTROL  使用可能なCJA行 ] | Customer Journey Analytics 内で分析に使用できるデータの 1 日あたりの平均行数。 |
| [!UICONTROL  計算属性 ] | プロファイル属性に変換され、ユーザープロファイルに含めることができる、エクスペリエンスイベントに基づく集計プロファイル行動データ。 |
| [!UICONTROL  消費者オーディエンス ] | 販売注文で「消費者オーディエンス」として識別されたユーザープロファイルの数。 |
| [!UICONTROL  データの書き出しサイズ ] | データセットのアクティベーションを通じて 1 年間に送信されたデータの量。 |
| [!UICONTROL  データの書き出し ] | 1 年間にアドビ以外のソリューションに (直接または間接的に) 書き出すことができるデータセットの合計サイズ。 |
| [!UICONTROL Data Lake ストレージ ] | Adobe Experience Platform 内の分析データストアの使用量。 |
| [!UICONTROL  エンゲージメント可能なオーディエンス ] | Journey Optimizerのオーサリング、意思決定、配信、実験またはオーケストレーションの機能を使用して、過去 12 か月以内に関与を試みたリアルタイム顧客プロファイルのユーザーグループ。 |
| [!UICONTROL  類似オーディエンス ] | 消費者の類似オーディエンスは、既存の消費者オーディエンスをモデリングして類似の属性や行動を持つユーザープロファイルを識別することで生成されるオーディエンスです。 |
| [!UICONTROL AMM モデルの数 ] | 投資に基づいて指定した結果を測定 / 予測するために使用される機械学習モデル (Adobe Mix Modeler に組み込み済み) の数。 |
| [!UICONTROL  サンドボックスの数 ] | Adobe Experience Platform にアクセスしてデータと操作を分離する Adobe オンデマンドサービスのインスタンス内での論理分離の数。 |
| [!UICONTROL  プロファイル充実度パック数 ] | 追加のプロファイルリッチネスパックごとに、許可される合計データ量がプロファイルあたり 25 KB 増加します。 |
| [!UICONTROL  クエリサービス計算時間 ] | バッチクエリの実行時に、クエリサービスエンジンがデータレイクに対してデータの読み取り、処理、書き戻しを行うために必要な時間の測定値。 |
| [!UICONTROL  ストリーミングセグメント化パック数 ] | パックでは、ストリーミングフローを通じて新しいデータをセグメント化サービスに入力すると、ユーザープロファイルのセグメントメンバーシップが更新されます。セグメントメンバーシップは、過去の行動を考慮せずに、現在のユーザープロファイル属性と現在のイベントの値に基づいて評価されます。ストリーミングセグメント化は共有機能です。 |
| [!UICONTROL  合計データ量 ] | リアルタイム顧客プロファイルがエンゲージメントワークフローで使用できるデータの総量。 合計データ量は、**合計データ量= アドレス可能なオーディエンス ×平均プロファイル充実度** の式を使用して計算されます。 この指標は、プロファイルストアにのみ保存されたデータを反映し、データレイクのストレージを除外します。 プロファイルベースのエンゲージメントに関連するデータについて、より焦点を絞ったビューを提供します。 詳しくは、[ 合計データ量に関するよくある質問 ](../../landing/license-usage-and-guardrails/total-data-volume.md) を参照してください。 |
| [!UICONTROL  データエグレスの合計量 ] | Adobe Experience Platformからサードパーティのデータウェアハウスに書き出されたデータの年間累積量です。 |

<!-- |  [!UICONTROL Sandbox No of Packs] |  A logical separation within your instance of any Adobe On-demand Service that accesses Adobe Experience Platform isolating data and operations | -->

>[!TIP]
>
>販売注文でライセンスの使用権限を確認して、「ストレージ許可」などの指標を計算できます。<br> 例：<ul><li>ストレージ許可=契約内の「承認済みプロファイル」の数 X プロファイルの平均充実度</li></ul>

これらの指標の可用性と各指標の具体的な定義は、組織が購入したライセンスによって異なります。 各指標の定義について詳しくは、該当する製品説明ドキュメントを参照してください。

| ライセンス | 商品の説明 |
| --- | --- |
| <ul><li>ADOBE EXPERIENCE PLATFORM:OD LITE</li><li>ADOBE EXPERIENCE PLATFORM:OD 標準</li><li>ADOBE EXPERIENCE PLATFORM:OD ヘビー</li></ul> | [Adobe Experience Platform](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform.html) |
| <ul><li>ADOBE EXPERIENCE PLATFORM:OD</li></ul> | [Experience Platform、アプリサービス、インテリジェントサービス ](https://helpx.adobe.com/legal/product-descriptions/exp-platform-app-svcs.html) |
| <ul><li>RT CUSTOMER DATA PLATFORM:OD</li><li>RT CUSTOMER DATA PLATFORM:OD PRFL ～ 10M</li><li>RT CUSTOMER DATA PLATFORM:OD PRFL ～ 50M</li></ul> | [Adobe Real-Time Customer Data Platform](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) |
| <ul><li>AEP:OD アクティベーション</li><li>AEP:OD ACTIVATION PRFL TO 10M</li><li>AEP:OD ACTIVATION PRFL （最大 50M）</li></ul> | [Adobe Experience Platformのライセンス認証 ](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-experience-platform0.html) |
| <ul><li>AEP:OD INTELLIGENCE</li></ul> | [Adobe Experience Platform インテリジェンス ](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform-intelligence---product-description.html) |
| <ul><li>JOURNEY OPTIMIZER SELECT:OD</li><li>JOURNEY OPTIMIZERPRIME:OD</li><li>JOURNEY OPTIMIZERULTIMATE:OD</li><li>UNP AJO PRIME スターター：OD</li><li>UNP AJO ULTIMATE スターター：OD</li><li>UNP Real-Time CDP:OD プロファイルオーケストレーション</li></ul> | [Adobe Journey Optimizer](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-journey-optimizer.html) |

>[!WARNING]
>
>ライセンス使用状況ダッシュボードは、組織にプロビジョニングされた最新のライセンスに関してのみレポートします。 組織にプロビジョニングされた最新のライセンスが上の表に表示されない場合、ライセンス使用状況ダッシュボードが正しく表示されない可能性があります。 今後のリリースで、1 つの組織での追加ライセンスと複数ライセンスのサポートが予定されています。

## 次の手順

このドキュメントを読むと、ライセンス使用状況ダッシュボードを見つけ、購入した各製品、すべての実稼動または開発用サンドボックス、特定のサンドボックスの使用状況指標を表示できるようになります。 組織が購入したライセンスに基づいて、組織で使用可能な指標に関する詳細を確認できます。

Experience Platform UI で使用できるその他の機能について詳しくは、[Experience Platform UI ガイド ](../../landing/ui-guide.md) を参照してください。
