---
title: Pinterestの移行先を新しいAPIに移行します。 顧客の行動が必要です。
description: Pinterestは、現在Real-Time CDPのPinterest宛先で使用されているv4 Advertiser APIを非推奨（廃止予定）にしています。 Pinterestのキャンペーンを中断することなく、新しいAPIにシームレスに移行するためのアクション項目を把握できます。
hide: true
hidefromtoc: true
exl-id: c965235c-4208-4c28-9ac5-eb4c0061515d
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 0%

---

# Pinterestの宛先を新しいAPIにアップグレードします。 2024年1月18日までに必要な顧客対応。

>[!IMPORTANT]
>
>このページの顧客行動項目は、お客様の組織が2023年11月16日（PT）より前にPinterestにデータを書き出すデータフローを設定している場合、最新のPinterest APIを使用した新しい&#x200B;**[!UICONTROL Pinterest]**&#x200B;宛先が宛先カタログに追加された日付に適用されます。

## 何が起きているのでしょうか？ {#what-is-happening}

Pinterestは、[の](/help/destinations/catalog/advertising/pinterest.md)Pinterest destination[!DNL Real-Time CDP]で使用されていたv4広告主APIを廃止しました。 Adobeは、[v5 アドバタイザーAPI](https://developers.pinterest.com/docs/getting-started/migration/)を使用するように宛先を更新しました。 Pinterestのキャンペーンを中断することなく、新しいAPIにシームレスに移行するためのアクション項目について説明します。

## なぜ通知されるのか？ {#why-notified}

Pinterestにオーディエンスをアクティブ化するためのアクティブなデータフローを保有していることを確認しました。

## プランは何ですか？ {#what-is-the-plan}

Adobeは、Pinterest API v5を活用した新しいPinterest destination cardをリリースしました。これにより、既存のデータフローが新しい接続に保持されます。

## アクティベートされたオーディエンスを機能させるためには、何か必要がありますか？ {#action-required}

はい、2024年1月18日（PT）より前に、Pinterest広告主アカウントを使用して新しいPinterestの宛先に対して[!DNL Real-Time CDP]で認証する必要があります。 以下の詳細な手順を参照してください。

### Pinterestへの再認証 {#reauthenticate}

1. **[!UICONTROL Destinations > Accounts]**に移動し、画面のフィルターを使用して、Pinterestの宛先のみをフィルタリングします。
   ![Pinterest アカウントのみをフィルタリング ](/help/destinations/assets/catalog/advertising/pinterest-migration/filter-pinterest-acconts-only.png)
2. **Pinterest**&#x200B;の宛先で、3つのドット記号…を選択し、**[!UICONTROL Edit details]**を選択します。
   ![詳細を編集を選択](/help/destinations/assets/catalog/advertising/pinterest-migration/edit-details-pinterest.png)
3. **[!UICONTROL Reconnect OAuth]**を選択し、Pinterest アカウントにログインします。
   ![OAuthの再接続を選択](/help/destinations/assets/catalog/advertising/pinterest-migration/reconnect-oauth-pinterest.png)
4. 以下のセクションのアクション項目に進みます

### 新しい宛先へのフローを有効にする {#disable-old-enable-new-flows}

次に、新しい&#x200B;**[!UICONTROL Pinterest]** カードへのデータフローを有効にする必要があります。

1. **[!UICONTROL Destinations > Browse]**&#x200B;に移動し、画面のフィルターを使用して、**[!UICONTROL Pinterest]**宛先のみをフィルタリングします。
   ![参照タブでのみPinterest データフローをフィルタリング ](/help/destinations/assets/catalog/advertising/pinterest-migration/filter-pinterest-browse.png)
2. ハイパーリンクされた接続名（上記のスクリーンショットの例ではロイヤルティキャンペーン）を&#x200B;**[!UICONTROL Pinterest]**&#x200B;の宛先に選択し、**[!UICONTROL Enable]** トグルを&#x200B;**の**に切り替えます。
   ![新しい接続を有効にし、古い接続を無効にする](/help/destinations/assets/catalog/advertising/pinterest-migration/enable-disable-toggle-new-destination.png)

<!--

While no disruption to your campaigns is expected, remember to check in the Pinterest UI that everything works as expected.

-->

## 概要のタイムラインを共有できますか？ {#high-level-timelines}

はい、以下を参照してください。

**2023年11月16日**：新しい宛先の準備ができました。Pinterestが古いv4 APIのサポートを停止するまで、カタログに2つのPinterest カードが並べて表示されます。 現在のPinterest カードへの既存のデータフローはすべて、新しい宛先にコピーされます。

![古いPinterestと新しいの宛先を並べて表示](/help/destinations/assets/catalog/advertising/pinterest-migration/pinterest-two-cards-side-by-side.png)

<!--

>[!IMPORTANT]
>
>After November 16th, 2023 the legacy Pinterest destination is marked **[!UICONTROL Deprecating]**. <span class="preview">Any changes that you make to dataflows to the (Deprecating) Pinterest destination after November 16th will *not* be automatically carried over to the new Pinterest destination. </span>
>For example, we *do not recommend* that you activate new audiences to the old destination after November 16th. If you do that, you will then have to follow the [regular activation steps](/help/destinations/ui/activate-segment-streaming-destinations.md) to add the audience to the new destination once the customer actions are taken.

-->

**2023年12月15日**: <span class="preview">お客様のアクション 1</span>。 新しいカードがPinterestに接続されるように、Pinterestに再認証する必要があります。 完全な手順については、[このセクション ](#reauthenticate)を参照してください。

<span class="preview">顧客アクション 2</span>。次に、新しいカードでデータフローを有効にする必要があります。 完全な手順については、[このセクション ](#disable-old-enable-new-flows)を参照してください。

<!--

>[!IMPORTANT]
>
>After December 15th, 2023, Adobe does not guarantee the integrity of dataflows to the old **[!UICONTROL (Deprecating) Pinterest]** destination.

-->

**2024年1月18日以降**: <span class="preview">PinterestはV4広告主APIへのアクセスをオフにしました。 新しい宛先にアップグレードしていない[!DNL Real-Time CDP]のお客様は、Pinterest宛先へのデータフローが失敗したことが確認できるようになりました。 [Pinterestに再認証](#reauthenticate)し、[ アップグレードされた宛先へのデータフロー](#disable-old-enable-new-flows)を有効にして、Pinterestへのキャンペーンを再開します。</span>

<!--

## Other items to note

After you enable the dataflows on the new destination card and disable the dataflows on the old destination cards, you should see no disruption in your campaigns or in the numbers of qualified profiles in the audiences coming in from Adobe Real-Time CDP.

-->
