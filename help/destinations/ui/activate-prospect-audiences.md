---
title: 見込み客のオーディエンスを宛先に対してアクティブ化する
type: Tutorial
description: 見込み客オーディエンスを宛先に対してアクティブ化する方法を説明します。
source-git-commit: 384faa13154386ef2578da4c20ab47f171aefeda
workflow-type: tm+mt
source-wordcount: '620'
ht-degree: 28%

---


# 見込み客オーディエンスの有効化

>[!AVAILABILITY]
>
>この機能は、Real-Time CDP Prime および Ultimate パッケージを購入したお客様が利用できます。詳しくは、Adobe担当者にお問い合わせください。

この記事では、エクスポートに必要なワークフローについて説明します。 [見込み客オーディエンス](/help/segmentation/ui/prospect-audience.md) Adobe Experience Platformから目的の宛先に移動します。

## サポートされる宛先 {#supported-destinations}

**[!UICONTROL 接続]**／**[!UICONTROL 宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。以下を使用します。 **[!UICONTROL データタイプ]** フィルターと選択 **[!UICONTROL 見込み客]** を参照して、見込み客オーディエンスのアクティブ化をサポートする宛先を確認します。 現在、見込み客オーディエンスのエクスポートは、クラウドストレージの宛先でのみ使用できます。

![データセットの書き出しをサポートする宛先](/help/destinations/assets/ui/activate-prospect-audiences/data-types-filter.png)

## 前提条件 {#prerequisites}

* 最初に取り込む必要があります [見込み客プロファイル](/help/profile/ui/prospect-profile.md) を作成します。 [見込み客オーディエンス](/help/segmentation/ui/prospect-audience.md) これらをダウンストリームの宛先に対してアクティブ化する前に、次の手順を実行します。
* 見込み客オーディエンスを宛先に対してアクティブ化するには、宛先に正常に接続している必要があります。 まだ接続していない場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照し、使用する宛先を設定します。に関する UI チュートリアルをお読みください。 [宛先への接続](./connect-destination.md) を参照してください。

### 必要な権限 {#permissions}

見込み客オーディエンスをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

見込み客オーディエンスをアクティブ化するために必要な権限を持っていることを確認するには、宛先カタログを参照します。 宛先に **[!UICONTROL 有効化]** コントロールを使用する場合、適切な権限を持っています。

## 宛先の選択 {#select-destination}

データセットを書き出すことができる宛先を選択するには、次の手順に従います。

1. **[!UICONTROL 接続／宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![カタログコントロールがハイライト表示された「宛先カタログ」タブ](/help/destinations/assets/ui/export-datasets/catalog-tab.png)

2. 選択 **[!UICONTROL 有効化]** を、データセットの書き出し先の宛先に対応するカードに貼り付けます。

>[!TIP]
>
>プロファイルオーディエンスを書き出す宛先は、カードの右上隅にアイコン付きで表示されます。次に例を示します。また、データタイプフィルターを使用して、見込み客オーディエンスを書き出す宛先のみを表示できます。 [ページの上に表示される](#supported-destinations).

![ハイライト表示されたプロファイルオーディエンスをエクスポートできるAmazon S3 の宛先ページ。](/help/destinations/assets/ui/activate-prospect-audiences/amazon-s3-icon-activate-prospect-audiences.png)

1. 選択 **[!UICONTROL データタイプ見込み客]**&#x200B;に続いて、データセットの書き出し先の接続を選択し、「 」を選択します。 **[!UICONTROL 次へ]**.

>[!TIP]
> 
>見込み客オーディエンスをアクティブ化する新しい宛先を設定する場合は、「 **[!UICONTROL 新しい宛先の設定]** トリガーする [宛先に接続](/help/destinations/ui/connect-destination.md) ワークフロー。

![見込み客コントロールが強調表示された宛先のアクティブ化ワークフロー。](/help/destinations/assets/ui/activate-prospect-audiences/activate-prospects-highlighted.png)

1. 次のセクションに進み、 [プロファイルオーディエンスを選択](#select-profile-audiences) エクスポート用。

## 見込み客のオーディエンスを選択 {#select-prospect-audiences}

見込み客のオーディエンス名の左側にあるチェックボックスを使用して、宛先にエクスポートするオーディエンスを選択してから、を選択します。 **[!UICONTROL 次へ]**. このビューには見込み客のオーディエンスのみが表示され、他のオーディエンスは表示されないことに注意してください。

![エクスポートする見込み客のオーディエンスを選択するオーディエンスの選択手順を表示するデータセットエクスポートワークフロー。](/help/destinations/assets/ui/activate-prospect-audiences/select-prospect-audiences.png)

## スケジュールおよび次の手順

見込み客オーディエンスをエクスポートする残りのアクティベーションワークフローについては、ファイルベースの宛先へのデータのアクティブ化に関するチュートリアルを参照してください。 次から続行： [オーディエンスの書き出し手順をスケジュール](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling).

>[!NOTE]
>
>スケジュール設定手順では、見込み客オーディエンスをアクティブ化するワークフローで許可されるのは次の操作のみです。 [完全なファイルをエクスポート](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files). 増分ファイルの書き出しはサポートされていません。

<!--

Note that we will need to add links to other destination types here as more destinations become supported 

-->

## パートナーデータサポートを通じて達成されるその他のユースケース {#other-use-cases}

Real-Time CDP のパートナーデータサポートを通じて達成されるその他のユースケースを調べます。

* [信頼できるデータパートナーからの属性でファーストパーティプロファイルを補完し、データ基盤を改善し、顧客ベースに関する新しいインサイトを得て、オーディエンスの最適化を改善します。](/help/rtcdp/partner-data/supplement-first-party-profiles.md)
* Real-Time CDP のサードパーティデータのサポートを使用して、[データパートナーの見込み客プロファイルでプロファイルベースを拡張し、新規顧客の獲得またはリーチのために見込み客との関わりを深めます](/help/rtcdp/partner-data/prospecting.md)。
* [パートナーの支援による認識を活用して、オンサイトエクスペリエンスをパーソナライズする](/help/rtcdp/partner-data/onsite-personalization.md) ユーザーがブランドを認証していない、または以前の履歴がない訪問中に発生した場合に発生していた問題を修正しました。