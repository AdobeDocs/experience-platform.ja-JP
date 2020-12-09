---
keywords: Experience Platform;user interface;UI;customization;license usage dashboard;dashboard;license usage;entitlement;consumption
title: ライセンス使用ダッシュボード
description: 'このガイドでは、Adobe Experience PlatformUIで使用できるライセンスの使用ダッシュボードについて説明します。 '
topic: guide
type: Documentation
translation-type: tm+mt
source-git-commit: 63758450276d47e7e0eddeb047779222cb80a3e2
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 2%

---


# （アルファ） [!UICONTROL ライセンス使用ダッシュボード] {#license-usage-dashboard}

>[!IMPORTANT]
>
>このドキュメントで説明されているダッシュボード機能は、現在アルファベットで表示されており、すべてのユーザーが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

Adobe Experience Platformユーザーインターフェイス(UI)は、毎日のスナップショットでキャプチャされた、組織のライセンスの使用に関する重要な情報を表示できるダッシュボードを提供します。 このガイドでは、UIのライセンス使用ダッシュボードにアクセスして操作する方法と、ダッシュボードに表示されるビジュアライゼーションに関する詳細を説明します。

プラットフォームUIの一般的な概要については、 [Experience PlatformUIガイドを参照してください](ui-guide.md)。

## ライセンス使用ダッシュボードデータ

ライセンスの使用ダッシュボードには、Experience Platformのための組織のライセンス関連データのスナップショットが表示されます。 ダッシュボード内のデータは、スナップショットが作成された特定の時点で表示されるとおりに表示されます。 つまり、スナップショットはデータの近似やサンプルではなく、ダッシュボードはリアルタイムで更新されません。

>[!NOTE]
>
>スナップショットが作成された後にデータに対して行われた変更や更新は、次のスナップショットが作成されるまでダッシュボードに反映されません。

## ライセンス使用ダッシュボードの詳細

プラットフォームUI内のライセンスの使用ダッシュボードに移動するには、左側のナビゲーションバーの **[!UICONTROL 「ライセンスの使用]** 」を選択します。 ダッシュボードが表示された「 **[!UICONTROL 概要]** 」タブが開きます。

![](images/license-usage-dashboard/dashboard-overview.png)

### サンドボックスを選択

ダッシュボードで表示するSandboxを選択するには、 [!UICONTROL Production] （実稼働環境）または [!UICONTROL Development（開発環境）のいずれかを選択します]。 選択したサンドボックスは、サンドボックス名の横のラジオボタンで示されます。

>[!NOTE]
>
>サンドボックスの消費レポートは、同じタイプのすべてのサンドボックスに対して累積的に行われます。 つまり、「 [!UICONTROL 実稼働] 」または「 [!UICONTROL 開発] 」を選択すると、それぞれ実稼働用サンドボックスと開発用サンドボックスのすべてに関するレポートが作成されます。

![](images/license-usage-dashboard/select-sandbox.png)

### 日付範囲の選択

サンドボックスを選択した後、日付範囲ドロップダウンを使用して、ダッシュボードに表示する期間を選択できます。 次の3つのオプションを使用できます。 [!UICONTROL 過去30日間]、 [!UICONTROL 過去90日間]、 [!UICONTROL 過去12か月間]。 デフォルトでは、過去30日間が選択されています。

![](images/license-usage-dashboard/select-date-range.png)

### ウィジェットと指標

ライセンスの使用ダッシュボードはウィジェットで構成され、組織のライセンスの使用に関する重要な情報を提供する読み取り専用の指標が表示されます。 これらのウィジェットの詳細については、このガイドの利用可能なウィジェットの節を参照してください。

## 利用可能なウィジェット {#available-widgets}

Experience Platformは現在、ライセンスの使用状況を視覚化するために使用できる1つのウィジェットを提供していますが、近日中にさらに多くのウィジェットがリリースされます。

### [!UICONTROL アドレス可能なオーディエンス] {#addressable-audiences}

アドレス **[!UICONTROL 指定可能なオーディエンス]** (Addressable Widget)は、システム生成のマージポリシーを適用して、決定論的（プライベート）グラフアルゴリズムを使用して現在のすべてのオーディエンスセットを組み合わせた後、プロファイルストア内のデータの合計数を測定します。 この指標の計算に使用されるマージ・ポリシーは、プラットフォームによって生成され、編集することも、別のマージ・ポリシーを選択することもできません。

![](images/license-usage-dashboard/addressable-audiences.png)

## その他のダッシュボード

Platform UIには、Experience Platform内のデータのスナップショットを表示するための追加のダッシュボードが用意されています。 これらのダッシュボードには、リアルタイム顧客プロファイルとセグメントが含まれます。 これらのダッシュボードの詳細については、次のリンクから選択してください。

* [[!DNL Profile] dashboard](../profile/ui/profile-dashboard.md)
* [セグメントダッシュボード](../segmentation/ui/segment-dashboard.md)

## 次の手順

このドキュメントに従うことで、ライセンス使用ダッシュボードを見つけ、表示用のサンドボックスを選択できるようになります。 利用可能なウィジェットに表示される指標も理解する必要があります。 Experience PlatformUIについて詳しくは、『 [プラットフォームUIガイド](ui-guide.md)』を参照してください。