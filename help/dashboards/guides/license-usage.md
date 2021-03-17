---
keywords: Experience Platform；ユーザインターフェイス；UI；カスタマイズ；ライセンス使用ダッシュボード;ダッシュボード；ライセンス使用；エンタイトルメント；コンシューム
title: ライセンスの使用ダッシュボード
description: Adobe Experience Platformは、組織のライセンスの使用に関する重要な情報を表示できるダッシュボードを提供します。
topic: ガイド
type: ドキュメント
translation-type: tm+mt
source-git-commit: 6baf1fbff20a02cd599d9ad9102d56db5a9004c3
workflow-type: tm+mt
source-wordcount: '660'
ht-degree: 3%

---


# （ベータ版）ライセンス使用ダッシュボード{#license-usage-dashboard}

>[!IMPORTANT]
>
>このドキュメントで概要を説明しているダッシュボード機能は、現在ベータ版であり、一部のユーザーはご利用いただけません。 ドキュメントと機能は変更される場合があります。

Adobe Experience Platformユーザーインターフェイス(UI)は、毎日のスナップショットでキャプチャされた、組織のライセンスの使用に関する重要な情報を表示できるダッシュボードを提供します。 このガイドでは、UIのライセンス使用ダッシュボードにアクセスして操作する方法と、ダッシュボードに表示されるビジュアライゼーションに関する詳細を説明します。

プラットフォームUIの一般的な概要については、[Experience PlatformUIガイド](../../landing/ui-guide.md)を参照してください。

## ライセンス使用ダッシュボードデータ

ライセンスの使用ダッシュボードには、Experience Platformのための組織のライセンス関連データのスナップショットが表示されます。 ダッシュボード内のデータは、スナップショットが作成された特定の時点で表示されるとおりに表示されます。 つまり、スナップショットはデータの近似やサンプルではなく、ダッシュボードはリアルタイムで更新されません。

>[!NOTE]
>
>スナップショットが作成された後にデータに対して行われた変更や更新は、次のスナップショットが作成されるまでダッシュボードに反映されません。

## ライセンス使用ダッシュボードの詳細

プラットフォームUI内のライセンスの使用ダッシュボードに移動するには、左のナビゲーションバーの「**[!UICONTROL ライセンスの使用]**」を選択します。 これは、「**[!UICONTROL 概要]**」タブにダッシュボードが表示された状態で開きます。

![](../images/license-usage/dashboard-overview.png)

### サンドボックスを選択

ダッシュボード内の表示に対するサンドボックスを選択するには、「[!UICONTROL 実稼働環境]」または「[!UICONTROL 開発環境]」を選択します。 選択したサンドボックスは、サンドボックス名の横のラジオボタンで示されます。

>[!NOTE]
>
>サンドボックスの消費レポートは、同じタイプのすべてのサンドボックスに対して累積的に行われます。 つまり、「[!UICONTROL 実稼動]」または「[!UICONTROL 開発]」を選択すると、すべての実稼働用サンドボックスと開発用サンドボックスに対して、それぞれ使用状況レポートが表示されます。

![](../images/license-usage/select-sandbox.png)

### 日付範囲の選択

サンドボックスを選択した後、日付範囲ドロップダウンを使用して、ダッシュボードに表示する期間を選択できます。 次の3つのオプションを使用できます。[!UICONTROL 過去30日間]、[!UICONTROL 過去90日間]、[!UICONTROL 過去12か月間]。 デフォルトでは、過去30日間が選択されています。

![](../images/license-usage/select-date-range.png)

## ウィジェット

ライセンスの使用ダッシュボードはウィジェットで構成され、組織のライセンスの使用に関する重要な情報を提供する読み取り専用の指標が表示されます。 表示される指標は、組織のライセンスによって異なります（詳しくは、[使用可能な指標](#available-metrics)のセクションを参照してください）。

各ウィジェットには、組織の実際の数値と、組織のライセンスで利用可能な合計とを比較した折れ線グラフが表示され、合計使用率の割合が示されます。

![](../images/license-usage/widgets.png)

## 使用可能な指標

現在、ライセンスの使用ダッシュボードには4つの指標があります。

* [!UICONTROL アドレス可能なオーディエンス] (プロファイル数で測定)
* [!UICONTROL 平均的なプロファイルの豊かさ]
* [!UICONTROL 総消費ストレージ]
* [!UICONTROL セグメント化率ごとにスキャンされるデータ]

これらの各指標の定義は、組織が購入したライセンスによって異なります。 各指標の詳細な定義については、該当する「製品の詳細」ドキュメントを参照してください。

| ライセンス | 製品の説明 |
|---|---|
| <ul><li>Adobe Experience Platform:OD LITE</li><li>Adobe Experience Platform:OD標準</li><li>Adobe Experience Platform:OD HEAVY</li></ul> | [Adobe Experience Platform](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform.html) |
| <ul><li>Adobe Experience Platform:OD</li></ul> | [Experience Platform、App Services、インテリジェントサービス](https://helpx.adobe.com/legal/product-descriptions/exp-platform-app-svcs.html) |
| <ul><li>RT顧客データプラットフォーム：OD</li><li>RT顧客データプラットフォーム：OD PRFL ～ 10M</li><li>RT顧客データプラットフォーム：OD PRFL ～ 50M</li></ul> | [リアルタイム顧客データプラットフォーム](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform.html) |
| <ul><li>AEP:ODアクティベーション</li><li>AEP:ODアクティベーションPRFL ～ 10M</li><li>AEP:ODアクティベーションPRFL最大50M</li></ul> | [Adobe Experience Platformアクティベーション](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform0.html) |
| <ul><li>AEP:ODインテリジェンス</li></ul> | [Adobe Experience Platform情報](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform-intelligence---product-description.html) |

## 次の手順

このドキュメントを読むと、ライセンス使用ダッシュボードを特定し、表示用のサンドボックスを選択できるようになります。 また、購入したライセンスに基づいて、組織で利用可能な指標に関する詳細情報も確認できます。

Experience PlatformUIで使用できるその他の機能の詳細については、『[Platform UI guide](../../landing/ui-guide.md)』を参照してください。