---
title: 見込み客のオーディエンスを宛先に対してアクティブ化する
type: Tutorial
description: 宛先への見込み客オーディエンスをアクティブ化する方法を学ぶ
exl-id: 3e034a14-09d0-4b08-b171-5afb62ae4b62
source-git-commit: e7c0551276d31d6809ace096c00e0dc2665090e6
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 25%

---

# 見込み客オーディエンスをアクティベート

>[!AVAILABILITY]
>
>この機能は、Real-Time CDP Prime および Ultimate パッケージを購入したお客様が利用できます。詳しくは、Adobe担当者にお問い合わせください。

この記事では、Adobe Experience Platformから目的の宛先に [ 見込み客オーディエンス ](/help/segmentation/types/prospect-audiences.md) を書き出すために必要なワークフローについて説明します。

## サポートされる宛先 {#supported-destinations}

**[!UICONTROL 接続]**／**[!UICONTROL 宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。**[!UICONTROL データタイプ]** フィルターを使用し、**[!UICONTROL 見込み客]** を選択して、見込み客オーディエンスのアクティブ化をサポートする宛先を表示します。 現在、見込み客オーディエンスの書き出しは、クラウドストレージの宛先でのみ使用できます。

![ 見込み客オーディエンスをサポートする宛先。](/help/destinations/assets/ui/activate-prospect-audiences/data-types-filter.png)

## 前提条件 {#prerequisites}

* 最初に [ 見込み客プロファイル ](/help/profile/ui/prospect-profile.md) を取り込み、[ 見込み客オーディエンス ](/help/segmentation/types/prospect-audiences.md) を作成してから、ダウンストリームの宛先に対してアクティブ化する必要があります。
* 宛先への見込み客オーディエンスをアクティブ化するには、正常に宛先に接続する必要があります。 まだ接続していない場合は、[ 宛先カタログ ](../catalog/overview.md) に移動し、サポートされている宛先を参照し、使用する宛先を設定します。 詳しくは、[ 宛先への接続 ](./connect-destination.md) に関する UI チュートリアルを参照してください。

### 必要な権限 {#permissions}

見込み客オーディエンスをアクティブ化するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先のアクティブ化]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

見込み客オーディエンスをアクティブ化するために必要な権限があることを確認するには、宛先カタログを参照します。 宛先に **[!UICONTROL アクティブ化]** コントロールがある場合、適切な権限を持っています。

## 宛先の選択 {#select-destination}

データセットを書き出すことができる宛先を選択するには、次の手順に従います。

1. **[!UICONTROL 接続／宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![カタログコントロールがハイライト表示された「宛先カタログ」タブ](/help/destinations/assets/ui/export-datasets/catalog-tab.png)

2. データセットを書き出す宛先に対応するカードで「**[!UICONTROL アクティブ化]**」を選択します。

>[!TIP]
>
>プロファイルオーディエンスを書き出せる宛先は、カードの右上隅にアイコンが表示されます。これは、以下でハイライト表示されている宛先と同様です。または、データタイプフィルターを使用して、見込み客オーディエンスを書き出せる宛先のみを表示することもできます [ ページの上部に表示されます ](#supported-destinations)。

![ ハイライト表示されたプロファイルオーディエンスを書き出すことができるAmazon S3 の宛先ページ ](/help/destinations/assets/ui/activate-prospect-audiences/amazon-s3-icon-activate-prospect-audiences.png)

1. **[!UICONTROL データタイプ見込み客]** を選択し、その後データセットを書き出す宛先接続を選択して、「**[!UICONTROL 次へ]**」を選択します。

>[!TIP]
> 
>見込み客オーディエンスをアクティブ化する新しい宛先を設定する場合は、「**[!UICONTROL 新しい宛先を設定]**」を選択して [ 宛先に接続 ](/help/destinations/ui/connect-destination.md) ワークフローをトリガーにします。

![ 見込み客コントロールがハイライト表示された宛先アクティベーションワークフロー ](/help/destinations/assets/ui/activate-prospect-audiences/activate-prospects-highlighted.png)

1. 次の節に進んで、書き出し用に [ プロファイルオーディエンスを選択 ](#select-profile-audiences) します。

## 見込み客オーディエンスを選択 {#select-prospect-audiences}

見込み客オーディエンス名の左側にあるチェックボックスを使用して、宛先に書き出すオーディエンスを選択し、「**[!UICONTROL 次へ]**」を選択します。 このビューには、見込み客オーディエンスのみが表示され、他のオーディエンスは表示されません。

![ 書き出す見込み客オーディエンスを選択できる「オーディエンスを選択」ステップが表示されているデータセット書き出しワークフロー ](/help/destinations/assets/ui/activate-prospect-audiences/select-prospect-audiences.png)

## スケジュール設定と次の手順

見込み客オーディエンスを書き出すための残りのアクティベーションワークフローについては、ファイルベースの宛先へのデータのアクティブ化に関するチュートリアルを参照してください。 [ オーディエンスの書き出しをスケジュール手順 ](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling) から続行します。

>[!NOTE]
>
>スケジュール設定ステップでは、見込み客オーディエンスをアクティブ化するワークフローでは [ 完全なファイルを書き出し ](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files) のみを実行できます。 増分ファイル書き出しはサポートされていません。

<!--

Note that we will need to add links to other destination types here as more destinations become supported 

-->

## パートナーデータサポートを通じて達成されるその他のユースケース {#other-use-cases}

Real-Time CDP のパートナーデータサポートを通じて達成されるその他のユースケースを調べます。

* [信頼できるデータパートナーからの属性でファーストパーティプロファイルを補完し、データ基盤を改善し、顧客ベースに関する新しいインサイトを得て、オーディエンスの最適化を改善します。](/help/rtcdp/partner-data/supplement-first-party-profiles.md)
* Real-Time CDP のサードパーティデータのサポートを使用して、[データパートナーの見込み客プロファイルでプロファイルベースを拡張し、新規顧客の獲得またはリーチのために見込み客との関わりを深めます](/help/rtcdp/partner-data/prospecting.md)。
* [ パートナー支援の認識を活用することで ](/help/rtcdp/partner-data/onsite-personalization.md) ユーザーがブランドを認証したり過去の履歴を持ったりすることなく、訪問中にオンサイトエクスペリエンスをパーソナライズできます。
