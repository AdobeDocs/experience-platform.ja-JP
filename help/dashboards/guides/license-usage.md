---
keywords: Experience Platform；ユーザーインターフェイス；UI；カスタマイズ；ライセンス使用ダッシュボード；ダッシュボード；ライセンス使用；権利付与；消費
title: ライセンス使用状況ダッシュボード
description: Adobe Experience Platformには、組織のライセンス使用状況に関する重要な情報を表示できるダッシュボードが用意されています。
type: Documentation
exl-id: 143d16bb-7dc3-47ab-9b93-9c16683b9f3f
source-git-commit: 47c4113d45b0101a761fa7d703013609e8729dbb
workflow-type: tm+mt
source-wordcount: '926'
ht-degree: 4%

---

# ライセンス使用状況ダッシュボード {#license-usage-dashboard}

Adobe Experience Platformユーザーインターフェイス(UI)は、毎日のスナップショットでキャプチャされた、組織のライセンス使用状況に関する重要な情報を表示できるダッシュボードを提供します。 このガイドでは、UIでのライセンス使用状況ダッシュボードへのアクセス方法と操作方法の概要と、ダッシュボードに表示されるビジュアライゼーションに関する詳細を説明します。

Platform UIの一般的な概要については、『[Experience PlatformUIガイド](../../landing/ui-guide.md)』を参照してください。

## ライセンス使用状況ダッシュボードデータ

ライセンス使用状況ダッシュボードには、Experience Platformに関する組織のライセンス関連データのスナップショットが表示されます。 ダッシュボードのデータは、スナップショットが作成された特定の時点で表示されたとおりに表示されます。 つまり、スナップショットはデータの近似やサンプルではなく、ダッシュボードはリアルタイムで更新されません。

>[!NOTE]
>
>スナップショットが作成された後にデータに加えられた変更や更新は、次のスナップショットが作成されるまでダッシュボードに反映されません。

## ライセンス使用状況ダッシュボードの詳細

Platform UI内のライセンス使用状況ダッシュボードに移動するには、左側のレールで「**[!UICONTROL ライセンス使用状況]**」を選択します。 「**[!UICONTROL 概要]**」タブが開き、ダッシュボードが表示されます。

>[!NOTE]
>
>ライセンス使用状況ダッシュボードは、デフォルトでは有効になっていません。 ダッシュボードを表示するには、「ライセンス使用状況ダッシュボードの表示」権限が必要です。 ライセンス使用ダッシュボードを表示するためのアクセス権限の付与手順については、[ダッシュボード権限ガイド](../permissions.md)を参照してください。

![](../images/license-usage/dashboard-overview.png)

### サンドボックスの選択

ダッシュボードに表示するサンドボックスを選択するには、「[!UICONTROL 実稼動]」または「[!UICONTROL 開発]」を選択します。 選択したサンドボックスは、サンドボックス名の横にラジオボタンで示されます。

サンドボックスの使用状況レポートは、同じタイプのすべてのサンドボックスに関して累積的です。 つまり、「[!UICONTROL 実稼動]」または「[!UICONTROL 開発]」を選択すると、すべての実稼動サンドボックスと開発サンドボックスの使用状況レポートが表示されます。

![](../images/license-usage/select-sandbox.png)

>[!WARNING]
>
>ライセンス使用状況ダッシュボードを表示する権限は、サンドボックスレベルで指定する必要があります。 つまり、ダッシュボードを表示する権限は、個々のサンドボックスに追加する必要があります。 この制限は、今後のリリースで対処される予定です。 その間、次の回避策を使用できます。
>
>1. Adobe Admin Consoleで製品プロファイルを作成します。
>2. 「サンドボックス」カテゴリの「権限」で、ライセンス使用ダッシュボードに表示するすべてのサンドボックスを追加します。
>3. 「ユーザーダッシュボードの権限」カテゴリで、「ライセンス使用状況ダッシュボードを表示」権限を追加します。


### 日付範囲の選択

サンドボックスを選択した後、日付範囲ドロップダウンを使用して、ダッシュボードに表示する期間を選択できます。 過去30日間のデフォルト値を含む、複数のオプションを使用できます。

![](../images/license-usage/select-date-range.png)

また、「**[!UICONTROL カスタム日付]**」を選択して、表示する期間を選択することもできます。

![](../images/license-usage/select-custom-date.png)

## ウィジェット

ライセンス使用状況ダッシュボードはウィジェットで構成され、組織のライセンス使用状況に関する重要な情報を提供する読み取り専用の指標が表示されます。 表示される指標は、組織固有のライセンスによって異なります（詳しくは、[使用可能な指標](#available-metrics)の節を参照してください）。

各ウィジェットには、組織の実際の数と、組織のライセンスで使用可能な合計を比較した折れ線グラフが表示され、合計使用量の割合が示されます。

![](../images/license-usage/widgets.png)

## 使用可能な指標

ライセンス使用状況ダッシュボードは、4つの主要指標に関するレポートを作成し、以降のリリースではさらに多くの指標を追加する必要があります。 使用可能な指標を以下に示します。

>[!NOTE]
>
>現在、利用可能な指標の3つはベータ版です。

* [!UICONTROL アドレス可能なオーディエンス]
* [!UICONTROL プロファイルのリッチさの平均] （ベータ版）
* [!UICONTROL セグメント化率ごとにスキャンされたデータ] （ベータ版）
* [!UICONTROL 総消費ストレージ] （ベータ版）

>[!WARNING]
>
>[!UICONTROL 合計消費済みストレージ]指標の既知の制限：バッチデータを削除する場合、そのバッチは、データ回復の使用例をサポートするために、7日間ソフト削除状態に置かれます。 7日後、バッチはハード削除状態に移動されます。 合計消費済みストレージのレポートには、バッチがハード削除状態になるまで、トレンドグラフに対する変更は反映されません。 この問題は、今後のリリースで解決される予定です。

これらの指標の使用可否と各指標の定義は、組織が購入したライセンスによって異なります。 各指標の詳細な定義については、適切な製品説明ドキュメントを参照してください。

| ライセンス | 製品の説明 |
|---|---|
| <ul><li>Adobe Experience Platform:OD LITE</li><li>Adobe Experience Platform:OD STANDARD</li><li>Adobe Experience Platform:OD HEAVY</li></ul> | [Adobe Experience Platform](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform.html) |
| <ul><li>Adobe Experience Platform:OD</li></ul> | [Experience Platform、アプリサービスおよびインテリジェントサービス](https://helpx.adobe.com/legal/product-descriptions/exp-platform-app-svcs.html) |
| <ul><li>RT顧客データプラットフォーム：OD[RTかんすうでーたぷらふぉーと：OD]</li><li>RT顧客データプラットフォーム：OD PRFL(10M)</li><li>RT顧客データプラットフォーム：OD PRFL(50M)</li></ul> | [リアルタイム顧客データプラットフォーム](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform.html) |
| <ul><li>AEP:ODのアクティベーション</li><li>AEP:ODアクティベーションPRFLから10M</li><li>AEP:ODアクティベーションPRFL（最大50M）</li></ul> | [Adobe Experience Platform Activation](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform0.html) |
| <ul><li>AEP:ODインテリジェンス</li></ul> | [Adobe Experience Platform Intelligence](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform-intelligence---product-description.html) |

>[!WARNING]
>
>ライセンス使用状況ダッシュボードは、組織にプロビジョニングされた最新のライセンスのみを報告します。 お客様の組織にプロビジョニングされた最新のライセンスが上記の表に表示されない場合、ライセンス使用状況ダッシュボードが正しく表示されないことがあります。 1つの組織で追加のライセンスと複数のライセンスがサポートされる予定は、今後のリリースです。

## 次の手順

このドキュメントを読むと、ライセンス使用状況ダッシュボードを探し、表示するサンドボックスを選択できます。 また、組織が購入したライセンスに基づいて、組織で使用可能な指標の詳細も確認できます。

Experience PlatformUIで使用できるその他の機能について詳しくは、『[Platform UIガイド](../../landing/ui-guide.md)』を参照してください。
