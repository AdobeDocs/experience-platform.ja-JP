---
keywords: Experience Platform;ユーザーインターフェイス;UI;カスタマイズ;ライセンス使用状況ダッシュボード;ダッシュボード;ライセンス使用状況;使用権限;使用
title: ライセンス使用状況ダッシュボード
description: Adobe Experience Platformには、組織のライセンス使用状況に関する重要な情報を表示できるダッシュボードが用意されています。
type: Documentation
exl-id: 143d16bb-7dc3-47ab-9b93-9c16683b9f3f
source-git-commit: 7332b39b0e213632e595dc52eda390aa0b9a24ec
workflow-type: tm+mt
source-wordcount: '3483'
ht-degree: 17%

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
>abstract="計算時間は、バッチクエリの実行時に、クエリサービスエンジンがデータの読み取り、処理、書き込みに費やした時間を測定します。<br> ライセンス量に達する可能性があります。 使用状況を評価または削減するには、クエリ/ログに移動してクエリ履歴を確認します。 クエリワークスペースへのアクセス権がない場合は、管理者にお問い合わせください。"
>additional-url="https://experience.adobe.com/#/platform/query/log.html" text="クエリログワークスペース"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_predictedusage_addressableaudience"
>title="予測されるアドレス可能なオーディエンス"
>abstract="アドレス可能なオーディエンスは、組織が関与する資格のあるリアルタイム顧客プロファイルの一連のユーザープロファイルです。 これには、直接識別可能なプロファイルと偽名プロファイルの両方が含まれます。<br> ライセンス量に達する可能性があります。 使用を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_predictedusage_engageableprofiles"
>title="予測されるエンゲージ可能なプロファイル"
>abstract="エンゲージメント可能なプロファイルは、過去 12 か月以内に組織がJourney Optimizerを使用してエンゲージメントを試みた、リアルタイム顧客プロファイルのユーザープロファイルです。<br> ライセンス量に達する可能性があります。 使用を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_predictedusage_businesspersonprofile"
>title="予測されるビジネスユーザーのプロファイル"
>abstract="ビジネスユーザープロファイルは、B2B コンテキストの個人を表す、リアルタイム顧客プロファイル内のレコードです。<br> ライセンス量に達する可能性があります。 使用を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_predictedusage_corehours"
>title="予測されるコア時間"
>abstract="コア時間は、Experience Platform サービス全体で消費される処理時間を表します。<br> ライセンス量に達する可能性があります。 使用を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_predictedusage_totaldatavolume"
>title="予測される合計データボリューム"
>abstract="合計データボリュームは、エンゲージメントワークフローおよびパーソナライゼーションワークフローで使用するために、リアルタイム顧客プロファイルで利用可能なデータの量です。<br> ライセンス量に達する可能性があります。 使用を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_predictedusage_cjaRowsAvailable"
>title="予測される使用可能な CJA 行数"
>abstract="使用可能なCJA行は、Customer Journey Analyticsで分析に使用できるデータの 1 日当たりの平均行を表します。<br> ライセンス量に達する可能性があります。 使用を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_exceededusage_addressableaudience"
>title="予測されるアドレス可能なオーディエンス"
>abstract="アドレス可能なオーディエンスは、組織が関与する資格のあるリアルタイム顧客プロファイルの一連のユーザープロファイルです。 これには、直接識別可能なプロファイルと偽名プロファイルの両方が含まれます。<br> 使用状況がライセンス金額を超えています。 使用を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_exceededusage_engageableprofiles"
>title="予測されるエンゲージ可能なプロファイル"
>abstract="エンゲージメント可能なプロファイルは、過去 12 か月以内に組織がJourney Optimizerを使用してエンゲージメントを試みた、リアルタイム顧客プロファイルのユーザープロファイルです。<br> 使用状況がライセンス金額を超えています。 使用を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_exceededusage_businesspersonprofile"
>title="予測されるビジネスユーザーのプロファイル"
>abstract="ビジネスユーザープロファイルは、B2B コンテキストの個人を表す、リアルタイム顧客プロファイル内のレコードです。<br> 使用状況がライセンス金額を超えています。 使用を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_exceededusage_corehours"
>title="予測されるコア時間"
>abstract="コア時間は、Experience Platform サービス全体で消費される処理時間を表します。<br> 使用状況がライセンス金額を超えています。 使用を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_exceededusage_totaldatavolume"
>title="予測される合計データボリューム"
>abstract="合計データボリュームは、エンゲージメントワークフローおよびパーソナライゼーションワークフローで使用するために、リアルタイム顧客プロファイルで利用可能なデータの量です。<br> 使用状況がライセンス金額を超えています。 使用を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseusage_exceededusage_cjaRowsAvailable"
>title="予測される使用可能な CJA 行数"
>abstract="使用可能なCJA行は、Customer Journey Analyticsで分析に使用できるデータの 1 日当たりの平均行を表します。<br> 使用状況がライセンス金額を超えています。 使用を減らすには、データセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja" text="エクスペリエンスイベントの有効期限"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

Adobe Experience Platformの [!UICONTROL  ライセンスの使用状況 ] ダッシュボードを使用して、組織のライセンスの使用状況に関する重要な情報を表示できます。 ここに表示される情報は、Platform インスタンスの毎日のスナップショット中にキャプチャされます。

ライセンス使用状況レポートを使用すると、ライセンス使用指標に対して高い精度でレポートを作成できます。 ダッシュボードには、購入した各製品（および関連するアドオン）の使用状況指標、すべての実稼動サンドボックスまたは開発サンドボックスでの指標の統合使用状況、特定のサンドボックスからの使用状況指標が表示されます。 使用状況指標を使用して追跡できるExperience Platform アプリケーションは、Real-Time Customer Data Platform、Adobe Journey Optimizer、Customer Journey Analyticsです。

このガイドでは、UI でのライセンス使用状況ダッシュボードへのアクセス方法と操作方法の概要を説明し、ダッシュボードに表示されるビジュアライゼーションの詳細を説明します。

Platform UI の一般的な概要については、[Experience Platform UI ガイド ](../../landing/ui-guide.md) を参照してください。

## [!UICONTROL  ライセンス使用状況 ] ダッシュボードデータ

[!UICONTROL  ライセンスの使用状況 ] ダッシュボードには、購入したすべてのExperience Platform製品とそれらの製品のアドオンのリストが表示されます。 このダッシュボードから、関連する任意のサンドボックスをまたいで、Experience Platformに関する組織のライセンス関連データのスナップショットを見つけることができます。

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

[!UICONTROL  ライセンスの使用状況 ] ダッシュボードには、「**コア製品**」と「**アドオン** の 2 つの異なるテーブルが表示されます。

- **[!UICONTROL コア製品 ] テーブル**：このテーブルには、組織でライセンスされている主なAdobe Experience Platform製品が一覧表示されます。 各コア製品には、サンドボックスレベルで独自の指標、使用状況トラッキングおよびドリルスルービューがあります。 これらのコア製品は、トラッキングの主要指標を提供し、これらの指標には任意のアドオンが含まれます。

- **[!UICONTROL アドオン ] テーブル**：このテーブルには、コア製品でサポートされている指標にライセンス量が結合される追加製品が一覧表示されます。 アドオンには、個別の指標はありませんが、関連付けられたコア製品の使用状況追跡を強化します。

| 列の名前 | 説明 |
|---|---|
| **[!UICONTROL 製品]** | お客様の組織でライセンスされたAdobe ソリューション。 |
| **[!UICONTROL プライマリ指標]** | その製品内でのトラッキングに使用される主な指標。 |
| **[!UICONTROL 許可の金額]** | ライセンス契約で合意された、プライマリ指標の最大量に対する契約値。 |
| **[!UICONTROL 用途]** | 使用されたプライマリ指標の量。 この値は、実稼動または開発のすべてのサンドボックスにおける、その指標の合計使用量を提供します。 |
| **[!UICONTROL 使用方法 %]** | ライセンス量に応じて使用される主要指標の割合。 |
| **[!UICONTROL 予測の使用]** | ライセンス量に応じた主要指標の予測使用割合。 |

>[!NOTE]
>
>アドオンのライセンス額は、コア製品の [!UICONTROL  ライセンス額 ] に含まれています。 例えば、5 つのサンドボックスのパックを 1 組をアドオンとして購入した場合、金額はベース製品の金額に追加されます。 アドオンテーブルには、アドオンに固有の [!UICONTROL  ライセンス額 ] が表示されますが、実際の使用状況は基本製品を通じて追跡されます。

各製品は多数の指標を追跡できるので、表は各製品の主要指標を示しています。

### 予測される使用状況 {#predicted-usage}

>[!CONTEXTUALHELP]
>id="platform_dashboards_licenseUsage_prediction"
>title="予測される使用状況"
>abstract="予測は過去 6～7 か月間の使用状況に基づいており、毎週金曜日に毎週生成されます。 ライセンス使用状況の予測は過去の使用状況に基づく概算です。お客様は、組織における実際の使用状況を理解し、その使用状況がアドビとの組織のライセンスの範囲を超えないようにする責任があります。使用量を減らすには、サンドボックスとデータセットのデータセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-lifecycle/ui/dataset-expiration.html?lang=ja" text="データセットの有効期限の自動化"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

>[!CONTEXTUALHELP]
>id="platform_licenseusage_prediction"
>title="予測される使用状況"
>abstract="予測は過去6～7か月の使用状況に基づき、毎月15日に生成されます。ライセンス使用状況の予測は過去の使用状況に基づく概算です。お客様は、組織における実際の使用状況を理解し、その使用状況がアドビとの組織のライセンスの範囲を超えないようにする責任があります。使用量を減らすには、サンドボックスとデータセットのデータセットまたは偽名プロファイルデータの有効期限を設定します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-lifecycle/ui/dataset-expiration.html?lang=ja" text="データセットの有効期限の自動化"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html?lang=ja" text="偽名プロファイルデータの有効期限"

正確で最新の使用予測を使用して、ライセンスリソースをプロアクティブに管理および最適化します。 [!UICONTROL  予測使用状況 ] 列は、購入したすべての製品に関する、すべての実稼動および開発用サンドボックスにわたる、サンドボックスレベルでの将来のライセンス使用状況を予測します。 予測は毎週更新され、最新の使用状況データに基づいて 6 週間の予測が提供されるようになりました。 各予測には、情報に基づいた計画をサポートするための下限と上限の両方が含まれています。

>[!IMPORTANT]
>
>予測は毎週金曜日に更新されます。 更新日は、情報アイコン（![ この情報アイコン](../images/license-usage/info-icon.png)）を選択します。

製品の使用権限の概要を確認するには、「[!UICONTROL  コア製品 ]」テーブルから製品を選択します。

![ 製品と予測される使用状況列がハイライト表示された [!UICONTROL  ライセンス使用状況 ] [!UICONTROL  概要 ]。](../images/license-usage/product-predicted-usage.png)

「概要」タブが表示されます。 [!UICONTROL  概要 ] タブと [!UICONTROL  詳細 ] タブで使用できる詳細な予測を使用して、情報に基づいた意思決定を確保し、効率的なライセンス使用を実現できます。

>[!NOTE]
>
>ライセンス使用状況の予測は過去の使用状況に基づく概算です。お客様は、組織の実際の使用状況を理解し、Adobeで組織のライセンスの範囲外に使用状況が使用されないようにする責任を負います。

![ 予測された使用状況列がハイライト表示された Platform 製品の概要ビュー。](../images/license-usage/summary-predicted-usage.png)

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

## [!UICONTROL  概要 ] タブ {#summary-tab}

製品ライセンスの使用状況に関するその他の指標と詳細なインサイトを表示するには、リストから製品名を選択します。 その製品の [!UICONTROL  概要 ] ビューが表示されます。 使用可能なすべての指標が「[!UICONTROL  概要 ]」タブに表示されます。 使用できる指標は、ライセンスを取得した製品によって異なります。 このビューには **すべての実稼動または開発用サンドボックスのすべての指標の統合ビュー** が表示されます。 実稼動用サンドボックスと開発用サンドボックスの両方に対して、同じレベルの分析が提供されます。

![ その製品で使用可能なすべての指標を表示する Platform 製品の概要ビュー ](../images/license-usage/summary-tab.png)

「概要」タブで、テーブルには [!UICONTROL  指標 ] 列が含まれます。 これらの人間が読み取れる説明は、そのタイプのサンドボックスに使用されるすべての指標を示します。

### サンドボックスを選択 {#select-sandbox}

実稼動用サンドボックスと開発用サンドボックスのタイプ間で表示を変更するには、[!UICONTROL  実稼動用サンドボックス ] または [!UICONTROL  開発用サンドボックス ] を選択します。 選択したサンドボックスタイプは、サンドボックス名の横のラジオボタンで示されます。

サンドボックスの消費レポートは、同じタイプのすべてのサンドボックスに対して累積的です。 つまり、「実稼動 [!UICONTROL  または  開発 ] を選択すると、それぞれ、すべての実稼動サンドボックスまたは開発用サンドボックスの消費レポートが表示されます。

![ 実稼動用サンドボックスと開発用サンドボックスがハイライト表示された Platform 製品の概要ビュー ](../images/license-usage/summary-tab-sandboxes.png)

>[!WARNING]
>
>ライセンス使用状況ダッシュボードを表示する権限は、サンドボックスレベルで指定する必要があります。 個々のサンドボックスに権限を追加して、ダッシュボード内で表示します。 この制限は、今後のリリースで対処される予定です。 それまでの間、次の回避策を使用できます。
>
>1. Adobe Admin Consoleで製品プロファイルを作成します。
>2. サンドボックス カテゴリの権限で、表示するすべてのサンドボックスをライセンス使用状況ダッシュボードに追加します。
>3. ユーザーダッシュボード権限カテゴリで、「ライセンス使用状況ダッシュボードを表示」権限を追加します。

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

>[!IMPORTANT]
>
>8 月 20 日以降、「[!UICONTROL  平均プロファイル充実度 ]」および「[!UICONTROL  合計ストレージ ]」の資格を持つお客様には、ライセンス使用状況ダッシュボードに「[!UICONTROL  合計データ量 ]」が表示されるようになりました。 顧客の使用権限に変更はなく、トラッキング指標の簡略化のみでした。 [!UICONTROL  合計データ量 ] は、エンゲージメントワークフローおよびパーソナライゼーションワークフローについて、リアルタイム顧客プロファイルで利用できるデータを表します。 このシンプル化された指標により、リアルタイム顧客プロファイルの使用に関する管理と測定が向上しました。 この変更に関する詳細については、Adobeの担当者に問い合わせることをお勧めします。

ライセンス使用状況ダッシュボードでは、組織内の複数の製品に適用できるいくつかの一意の指標についてレポートします。 使用できる指標は次のとおりです。

| 指標 | 説明 |
|---|---|
| [!UICONTROL Audience Activation サイズ ] | 1 年間に任意のファイルベースの宛先に対してアクティブ化されたプロファイルの合計サイズ。 注意：これには、ストリーミング宛先を介して送信されるプロファイルは含まれません。 |
| [!UICONTROL  アドレス可能なオーディエンス ] | 直接識別可能なプロファイルと偽名プロファイルの両方を含む、組織が関与する資格のあるリアルタイム顧客プロファイルのユーザープロファイルのセット。 これらのプロファイルには、属性、ビヘイビアー、セグメントメンバーシップデータを含めることができます。 プロファイルボリュームは、Adobe Experience Platformのデフォルトの決定論的 ID グラフを使用して計算され、共有機能と見なされます。 |
| [!UICONTROL  アドホッククエリサービスユーザーパック ] | 承認された同時クエリサービスユーザーの使用権限を増やすアドオン。5 人の同時クエリサービスユーザーと、パックごとに 1 つの同時に実行されるアドホッククエリを追加できます。 複数の追加アドホック クエリ ユーザーパックがライセンスされている可能性があります。 |
| [!UICONTROL  平均プロファイル充実度 ] | **非推奨** – 任意の時点でハブプロファイルサービス内に保存されたすべての実稼動データの合計を、許可されたビジネスユーザープロファイルの数の 5 倍で割った値です。 [!UICONTROL  平均的なプロファイル充実度 ] は共有機能です。 |
| [!UICONTROL  使用可能なCJA行 ] | Customer Journey Analytics内で分析に使用できる 1 日あたりの平均データ行数。 |
| [!UICONTROL  計算属性 ] | プロファイル属性に変換され、ユーザープロファイルに含めることができる、エクスペリエンスイベントに基づく集計プロファイル行動データ。 |
| [!UICONTROL  消費者オーディエンス ] | 販売注文で「消費者オーディエンス」として識別されたユーザープロファイルの数。 |
| [!UICONTROL  データの書き出しサイズ ] | 1 年にデータセットのアクティベーションを通じて送信されるデータの量。 |
| [!UICONTROL  データの書き出し ] | 1 年に（直接または間接的に）Adobe以外のソリューションに書き出すことができるデータセットの合計サイズ。 |
| [!UICONTROL Data Lake ストレージ ] | Adobe Experience Platform内で使用される分析データストアの量です。 |
| [!UICONTROL  エンゲージメント可能なオーディエンス ] | Journey Optimizerのオーサリング、意思決定、配信、実験またはオーケストレーションの機能を使用して、過去 12 か月以内に関与を試みたリアルタイム顧客プロファイルのユーザーグループ。 |
| [!UICONTROL  類似オーディエンス ] | 消費者の類似オーディエンスは、既存の消費者オーディエンスをモデリングして類似の属性や行動を持つユーザープロファイルを識別することで生成されるオーディエンスです。 |
| [!UICONTROL AMM モデルの数 ] | 投資に基づいて指定された成果を測定または予測するために使用される機械学習モデル（Adobe Mix Modelerで構築）のカウント。 |
| [!UICONTROL  サンドボックスの数 ] | データと操作を分離してAdobe Experience PlatformにアクセスするAdobe オンデマンドサービスのインスタンス内の論理分離の数。 |
| [!UICONTROL  プロファイル充実度パック数 ] | 追加のプロファイル充実度パックごとに、プロファイルごとに認証済みの合計データボリュームが 25 KB 増加します。 |
| [!UICONTROL  クエリサービス計算時間 ] | バッチクエリの実行時に、クエリサービスエンジンがデータの読み取り、処理およびデータレイクへの書き込みに要した時間の測定値。 |
| [!UICONTROL  ストリーミングセグメント化パック数 ] | パックは、新しいデータがストリーミングフローを通じてセグメント化サービスに入ると、個人プロファイルのセグメントメンバーシップを更新します。 セグメントメンバーシップは、過去の行動を考慮せずに、現在の人物プロファイル属性と現在のイベントの値に基づいて評価されます。 ストリーミングセグメント化は共有機能です。 |
| [!UICONTROL  合計データ量 ] | リアルタイム顧客プロファイルがエンゲージメントワークフローで使用できるデータの総量。 詳しくは、[ 合計データ量に関するよくある質問 ](../../landing/license-usage-and-guardrails/total-data-volume.md) を参照してください。 |
| [!UICONTROL  データエグレスの合計量 ] | Adobe Experience Platformからサードパーティのデータウェアハウスに書き出されたデータの年間累積量です。 |

<!-- Approval needed on my revision above.
Original PM version: | [!UICONTROL Total Volume of Data Egress] | The cumulative annual amount of data processed from third-party data warehouses. | -->

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

Platform UI で使用できるその他の機能について詳しくは、[Experience Platform UI ガイド ](../../landing/ui-guide.md) を参照してください。
