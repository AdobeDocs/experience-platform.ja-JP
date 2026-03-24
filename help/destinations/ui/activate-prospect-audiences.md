---
title: 見込み客のオーディエンスを宛先に対してアクティブ化する
type: Tutorial
description: 見込み顧客オーディエンスを配信先で活用する方法を学ぶ
exl-id: 3e034a14-09d0-4b08-b171-5afb62ae4b62
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 15%

---

# 見込み客オーディエンスをアクティベート

>[!AVAILABILITY]
>
>この機能は、[!DNL Real-Time CDP] PrimeとUltimate パッケージを購入したお客様が利用できます。 詳しくは、アドビ担当者にお問い合わせください。

この記事では、[見込み客オーディエンス ](/help/segmentation/types/prospect-audiences.md)を[!DNL Adobe Experience Platform]から好みの宛先にエクスポートするために必要なワークフローについて説明します。

## サポートされる宛先 {#supported-destinations}

**[!UICONTROL Connections]** > **[!UICONTROL Destinations]**&#x200B;に移動し、「**[!UICONTROL Catalog]**」タブを選択します。 **[!UICONTROL Data types]** フィルターを使用して&#x200B;**[!UICONTROL Prospects]**&#x200B;を選択し、見込み客オーディエンスのアクティブ化をサポートする宛先を表示します。 現在、見込み客オーディエンスの書き出しは、クラウドストレージの宛先でのみ使用できます。

![見込み客オーディエンスをサポートする宛先。](/help/destinations/assets/ui/activate-prospect-audiences/data-types-filter.png)

## 前提条件 {#prerequisites}

* 下流の宛先にアクティブ化する前に、まず[見込み客プロファイル ](/help/profile/ui/prospect-profile.md)を取り込み、[見込み客オーディエンス ](/help/segmentation/types/prospect-audiences.md)を作成する必要があります。
* 宛先に対して見込み顧客オーディエンスをアクティブ化するには、宛先に正常に接続している必要があります。 まだ実行していない場合は、[宛先カタログ ](../catalog/overview.md)に移動し、サポートされている宛先を参照して、使用する宛先を設定します。 詳しくは、[宛先への接続](./connect-destination.md)に関するUI チュートリアルを参照してください。

### 必要な権限 {#permissions}

見込み顧客オーディエンスをアクティブ化するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Activate Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

見込み顧客オーディエンスをアクティブ化するために必要な権限を持っていることを確認するには、宛先カタログを参照します。 宛先に&#x200B;**[!UICONTROL Activate]** コントロールがある場合は、適切な権限を持っています。

## 宛先の選択 {#select-destination}

データセットを書き出すことができる宛先を選択するには、次の手順に従います。

1. **[!UICONTROL Connections > Destinations]**&#x200B;に移動し、「**[!UICONTROL Catalog]**」タブを選択します。

   ![カタログコントロールがハイライト表示された「宛先カタログ」タブ](/help/destinations/assets/ui/export-datasets/catalog-tab.png)

2. データセットの書き出し先に対応するカードで&#x200B;**[!UICONTROL Activate]**&#x200B;を選択します。

>[!TIP]
>
>プロファイルオーディエンスを書き出すことができる宛先は、カードの右上隅に、下に強調表示されている宛先と同様のアイコンで示されます。または、データ型フィルターを使用して、見込み客オーディエンスを書き出すことができる宛先のみを表示できます（ページ [で上に示すように）。](#supported-destinations)

ハイライト表示されたプロファイルオーディエンスを書き出すことができる![Amazon S3の宛先ページ。](/help/destinations/assets/ui/activate-prospect-audiences/amazon-s3-icon-activate-prospect-audiences.png)

1. **[!UICONTROL Data type Prospects]**&#x200B;を選択し、データセットを書き出す宛先接続を選択してから、**[!UICONTROL Next]**&#x200B;を選択します。

>[!TIP]
> 
>見込み顧客オーディエンスをアクティブ化する新しい宛先を設定する場合は、**[!UICONTROL Configure new destination]**&#x200B;を選択して、[宛先に接続](/help/destinations/ui/connect-destination.md) ワークフローをトリガーします。

![見込客コントロールがハイライト表示された宛先アクティベーションのワークフロー。](/help/destinations/assets/ui/activate-prospect-audiences/activate-prospects-highlighted.png)

1. 次のセクションに進み、[書き出すプロファイルオーディエンス ](#select-profile-audiences)を選択します。

## 見込み客オーディエンスの選択 {#select-prospect-audiences}

見込み顧客オーディエンス名の左側にあるチェックボックスを使用して、宛先に書き出すオーディエンスを選択し、**[!UICONTROL Next]**&#x200B;を選択します。

>[!NOTE]
>
>このビューには見込み客オーディエンスのみが表示され、他のオーディエンスは表示されません。

![ データセットの書き出しワークフローで、書き出す見込み顧客オーディエンスを選択できる「オーディエンスを選択」ステップが表示されます。](/help/destinations/assets/ui/activate-prospect-audiences/select-prospect-audiences.png)

## スケジュール設定と次のステップ {#scheduling-and-next-steps}

見込み顧客オーディエンスを書き出すための残りのアクティベーションワークフローについては、ファイルベースの宛先へのデータのアクティベーションに関するチュートリアルを参照してください。 [ オーディエンスの書き出しスケジュール手順](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)から続行します。

>[!NOTE]
>
>スケジュール設定の手順では、見込み客オーディエンスをアクティブ化するワークフローでは、[完全なファイルの書き出しのみが可能です](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files)。 増分ファイルの書き出しはサポートされていません。

<!--

Note that we will need to add links to other destination types here as more destinations become supported 

-->

## パートナーデータサポートを通じて達成されるその他のユースケース {#other-use-cases}

[!DNL Real-Time CDP]のパートナーデータのサポートを通じて有効化されたその他のユースケースを確認します。

* [信頼できるデータパートナーからの属性でファーストパーティプロファイルを補完し、データ基盤を改善し、顧客ベースに関する新しいインサイトを得て、オーディエンスの最適化を改善します。](/help/rtcdp/partner-data/supplement-first-party-profiles.md)
* [!DNL Real-Time CDP]でサードパーティのデータ サポートを使用して、[ データパートナーからの見込み客プロファイルでプロファイル基盤を拡大し、新規顧客の獲得やリーチのために顧客とエンゲージします](/help/rtcdp/partner-data/prospecting.md)。
* [利用者が認証したり、自社との以前の履歴を持ったりすることなく、訪問中にパートナーが支援する認知度を利用して、オンサイト体験をパーソナライズします](/help/rtcdp/partner-data/onsite-personalization.md)。
