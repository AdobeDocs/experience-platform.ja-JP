---
keywords: Experience Platform;ユーザーインターフェイス;UI;カスタマイズ;ライセンス使用状況ダッシュボード;ダッシュボード;ライセンス使用状況;使用権限;使用
title: ライセンス使用状況ダッシュボード ガイド
description: Adobe Experience Platformには、組織のライセンス使用状況に関する重要な情報を表示できるダッシュボードが用意されています。
type: Documentation
exl-id: 143d16bb-7dc3-47ab-9b93-9c16683b9f3f
source-git-commit: d3a1d4a65d1e5810bbc37fa9d3d230557bec39ee
workflow-type: tm+mt
source-wordcount: '873'
ht-degree: 17%

---

# ライセンス使用状況ダッシュボード {#license-usage-dashboard}

Adobe Experience Platform ユーザーインターフェイス（UI）には、毎日のスナップショットで取得した、組織のライセンス使用状況に関する重要な情報を表示できるダッシュボードが用意されています。このガイドでは、UI でのライセンス使用状況ダッシュボードへのアクセスと操作の方法を説明し、ダッシュボードに表示されるビジュアライゼーションに関する詳細を提供します。

Platform UI の一般的な概要については、 [Experience PlatformUI ガイド](../../landing/ui-guide.md).

## ライセンス使用状況ダッシュボードデータ

ライセンス使用状況ダッシュボードには、Experience Platformに関する組織のライセンス関連データのスナップショットが表示されます。 ダッシュボードのデータは、スナップショットが作成された特定の時点とまったく同じ内容を表示します。つまり、スナップショットはデータの近似やサンプルではなく、ダッシュボードはリアルタイムで更新されません。

>[!NOTE]
>
>スナップショットが作成された後にデータに加えられた変更や更新は、次のスナップショットが作成されるまでダッシュボードに反映されません。

## ライセンス使用状況ダッシュボードの確認

Platform UI 内でライセンス使用状況ダッシュボードに移動するには、「 」を選択します。 **[!UICONTROL ライセンスの使用]** をクリックします。 これにより、 **[!UICONTROL 概要]** タブに表示されるダッシュボード。

>[!NOTE]
>
>ライセンス使用状況ダッシュボードは、デフォルトでは有効になっていません。 ダッシュボードを表示するには、「ライセンス使用状況ダッシュボードの表示」権限が必要です。 ライセンス使用状況ダッシュボードを表示するためのアクセス権限の付与手順については、 [ダッシュボード権限ガイド](../permissions.md).

![](../images/license-usage/dashboard-overview.png)

### サンドボックスを選択

ダッシュボードに表示するサンドボックスを選択するには、次のいずれかを選択します。 [!UICONTROL 実稼動] または [!UICONTROL 開発]. 選択したサンドボックスは、サンドボックス名の横にあるラジオボタンで示されます。

サンドボックスの使用状況レポートは、同じタイプのすべてのサンドボックスの累積的なものです。 つまり、選択 [!UICONTROL 実稼動] または [!UICONTROL 開発] は、それぞれすべての実稼動サンドボックスまたは開発サンドボックスの使用状況レポートを提供します。

![](../images/license-usage/select-sandbox.png)

>[!WARNING]
>
>ライセンス使用状況ダッシュボードを表示する権限は、サンドボックスレベルで指定する必要があります。 つまり、ダッシュボードを表示する権限は、個々のサンドボックスに追加する必要があります。 この制限は、今後のリリースで対処される予定です。 それまでの間、次の回避策を使用できます。
>
>1. Adobe Admin Consoleで製品プロファイルを作成します。
>2. 「サンドボックス」カテゴリの「権限」で、ライセンス使用状況ダッシュボードに表示するすべてのサンドボックスを追加します。
>3. 「ユーザーダッシュボード権限」カテゴリで、「ライセンス使用状況ダッシュボードの表示」権限を追加します。


### 日付範囲を選択

サンドボックスを選択した後、日付範囲ドロップダウンを使用して、ダッシュボードに表示する期間を選択できます。 過去 30 日間のデフォルト値を含む、複数のオプションを使用できます。

![](../images/license-usage/select-date-range.png)

また、 **[!UICONTROL カスタム日付]** をクリックして、表示する期間を選択します。

![](../images/license-usage/select-custom-date.png)

## ウィジェット

ライセンス使用状況ダッシュボードは、組織のライセンス使用状況に関する重要な情報を提供する読み取り専用の指標を表示するウィジェットで構成されています。 表示される指標は、組織の特定のライセンスに応じて異なります ( [利用可能な指標](#available-metrics) 」の節を参照してください )。

各ウィジェットには、組織の実際の数と、組織のライセンスで使用可能な合計を比較した線グラフが表示され、合計使用量の割合が示されます。

![](../images/license-usage/widgets.png)

## 使用可能な指標

ライセンス使用状況ダッシュボードは、4 つの主要指標に関するレポートを作成し、その後のリリースで追加される指標が増えます。 使用可能な指標は次のとおりです。

* [!UICONTROL アドレス可能なオーディエンス]
* [!UICONTROL 平均プロファイル充実度]
* [!UICONTROL セグメント化率ごとにスキャンされたデータ]
* [!UICONTROL 合計消費済みストレージ]

これらの指標の可用性と各指標の具体的な定義は、お客様の組織が購入したライセンスによって異なります。各指標の詳細な定義については、該当する製品説明ドキュメントを参照してください。

| ライセンス | 製品の説明 |
|---|---|
| <ul><li>Adobe Experience Platform:OD LITE</li><li>Adobe Experience Platform:OD STANDARD</li><li>Adobe Experience Platform:OD HEAVY</li></ul> | [Adobe Experience Platform](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform.html) |
| <ul><li>Adobe Experience Platform:OD</li></ul> | [Experience Platform、アプリサービスおよびインテリジェントサービス](https://helpx.adobe.com/legal/product-descriptions/exp-platform-app-svcs.html) |
| <ul><li>RT 顧客データプラットフォーム：OD</li><li>RT 顧客データプラットフォーム：OD PRFL ～ 10M</li><li>RT 顧客データプラットフォーム：OD PRFL ～ 50M</li></ul> | [Real-time Customer Data Platform](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform.html) |
| <ul><li>AEP:OD のアクティベーション</li><li>AEP:OD アクティベーション PRFL から 10M</li><li>AEP:OD アクティベーション（最大 50M）</li></ul> | [Adobe Experience Platform Activation](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-experience-platform0.html) |
| <ul><li>AEP:OD インテリジェンス</li></ul> | [Adobe Experience Platform Intelligence](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform-intelligence---product-description.html) |
| <ul><li>Journey Optimizer SELECT:OD</li><li>Journey Optimizer PRIME:OD</li><li>Journey Optimizer ULTIMATE:OD</li><li>UNP AJO PRIME STARTER:OD</li><li>UNP AJO ULTIMATE STARTER:OD</li><li>UNP RTCDP:OD プロファイルオーケストレーション</li></ul> | [Adobe Journey Optimizer](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-journey-optimizer.html) |

>[!WARNING]
>
>ライセンス使用状況ダッシュボードは、組織用にプロビジョニングされた最新のライセンスに関するレポートのみ表示されます。 お客様の組織でプロビジョニングされた最新のライセンスが上記の表に表示されない場合は、ライセンス使用状況ダッシュボードが正しく表示されないことがあります。 1 つの組織で追加のライセンスと複数のライセンスをサポートする予定は、今後のリリースです。

## 次の手順

このドキュメントを読むと、ライセンス使用状況ダッシュボードを探し、表示するサンドボックスを選択できます。 また、組織が購入したライセンスに基づいて、組織で使用可能な指標の詳細も確認できます。

Experience PlatformUI で使用できるその他の機能について詳しくは、 [Platform UI ガイド](../../landing/ui-guide.md).
