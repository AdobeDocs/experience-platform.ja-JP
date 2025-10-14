---
title: 新しい API へのPinterest宛先の移行。 顧客側で必要な対応です。
description: Pinterestは、Real-Time CDPのPinterest宛先で現在使用されている v4 広告主 API を非推奨（廃止予定）にしています。 pinterest キャンペーンを中断することなく新しい API にシームレスに移行するには、アクション項目を理解します。
hide: true
hidefromtoc: true
exl-id: c965235c-4208-4c28-9ac5-eb4c0061515d
source-git-commit: e3341ec6f62844858ecda7dd4db70d085f0bf217
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 0%

---

# Pinterestの宛先を新しい API にアップグレードします。 2024 年 1 月 18 日までに必要な顧客対応。

>[!IMPORTANT]
>
>2023 年 11 月 16 日（PT）より前に、最新のPinterest API を使用した新しい **[!UICONTROL Pinterest]** の宛先が宛先カタログに追加された日付でPinterestにデータを書き出すデータフローを設定している場合、このページのカスタマーアクション項目は該当します。

## 何が起きているのでしょうか？

Pinterestは、Real-Time CDPの [Pinterest destination](/help/destinations/catalog/advertising/pinterest.md) で使用されていた v4 advertiser API を非推奨（廃止予定）にしました。 Adobeは、[v5 広告主 API](https://developers.pinterest.com/docs/getting-started/migration/) を使用するように宛先を更新しました。 このページでは、Pinterest キャンペーンを中断することなく新しい API にシームレスに移行するためのアクション項目について説明します。

## なぜ私に通知されるのですか。

pinterestに対してオーディエンスをアクティブ化するアクティブなデータフローがあるものと識別しました。

## 計画は何ですか？

Adobeは、Pinterest API v5 を活用し、既存のデータフローを新しい接続に保持する新しいPinterest宛先カードをリリースしました。

## アクティブ化されたオーディエンスを機能させるには、何か作業が必要ですか？

はい、2024 年 1 月 18 日（PT）より前に、Real-Time CDPのPinterest広告主アカウントを使用して、新しいPinterestの宛先に対して認証を行う必要があります。 詳しくは、以下の手順を参照してください。

### pinterestへの再認証 {#reauthenticate}

1. **[!UICONTROL 宛先/アカウント]** に移動し、画面のフィルターを使用して、Pinterestの宛先のみをフィルタリングします。
   ![Pinterest アカウントのみをフィルタリング &#x200B;](/help/destinations/assets/catalog/advertising/pinterest-migration/filter-pinterest-acconts-only.png)
2. **Pinterest** の宛先で、3 つのドット記号…を選択し、「**[!UICONTROL 詳細を編集]**」を選択します。
   ![&#x200B; 詳細を編集を選択 &#x200B;](/help/destinations/assets/catalog/advertising/pinterest-migration/edit-details-pinterest.png)
3. 「**[!UICONTROL OAuth に再接続]**」を選択し、Pinterest アカウントにログインします。
   ![&#x200B; 「OAuth に再接続」を選択します &#x200B;](/help/destinations/assets/catalog/advertising/pinterest-migration/reconnect-oauth-pinterest.png)
4. 下のセクションのアクション アイテムに移動します

### 新しい宛先へのフローの有効化 {#disable-old-enable-new-flows}

次に、新しい **[!UICONTROL Pinterest]** カードへのデータフローを有効にする必要があります。

1. **[!UICONTROL 宛先/参照]** に移動し、画面上のフィルターを使用して、**[!UICONTROL Pinterest]** の宛先のみをフィルタリングします。
   ![&#x200B; 「参照」タブでのPinterest データフローのフィルタリングのみ &#x200B;](/help/destinations/assets/catalog/advertising/pinterest-migration/filter-pinterest-browse.png)
2. ハイパーリンクされた接続名（上記のスクリーンショットの例ではロイヤルティキャンペーン）を **[!UICONTROL Pinterest]** の宛先に選択し、**[!UICONTROL 有効]** 切り替え **オン** に切り替えます。
   ![&#x200B; 新しい接続にはオン、古い接続にはオフを切り替えます &#x200B;](/help/destinations/assets/catalog/advertising/pinterest-migration/enable-disable-toggle-new-destination.png)

<!--

While no disruption to your campaigns is expected, remember to check in the Pinterest UI that everything works as expected.

-->

## 高水準のタイムラインを共有できますか？

はい、以下を参照してください。

**2023 年 11 月 16 日（PT）まで**：新しい宛先の準備が整い、Pinterestが古い v4 API のサポートを停止するまで、2 つのPinterest カードがカタログ内に並んで表示されます。 現在のPinterest カードに対する既存のデータフローはすべて、新しい宛先にコピーされます。

![&#x200B; 古い宛先と新しいPinterestの宛先を並べて &#x200B;](/help/destinations/assets/catalog/advertising/pinterest-migration/pinterest-two-cards-side-by-side.png)

<!--

>[!IMPORTANT]
>
>After November 16th, 2023 the legacy Pinterest destination is marked **[!UICONTROL Deprecating]**. <span class="preview">Any changes that you make to dataflows to the (Deprecating) Pinterest destination after November 16th will *not* be automatically carried over to the new Pinterest destination. </span>
>For example, we *do not recommend* that you activate new audiences to the old destination after November 16th. If you do that, you will then have to follow the [regular activation steps](/help/destinations/ui/activate-segment-streaming-destinations.md) to add the audience to the new destination once the customer actions are taken.

-->

**2023 年 12 月 15 日（PT）までに**:<span class="preview"> 顧客行動 1</span>。 新しいカードがPinterestに接続されるように、Pinterestへの再認証が必要です。 完全な手順については、[&#x200B; この節 &#x200B;](#reauthenticate) を参照してください。

<span class="preview"> 顧客のアクション 2</span>。次に、新しいカードでデータフローを有効にする必要があります。 完全な手順については、[&#x200B; この節 &#x200B;](#disable-old-enable-new-flows) を参照してください。

<!--

>[!IMPORTANT]
>
>After December 15th, 2023, Adobe does not guarantee the integrity of dataflows to the old **[!UICONTROL (Deprecating) Pinterest]** destination.

-->

**2024 年 1 月 18 日（PT）以降**: <span class="preview">Pinterestは、V4 広告主 API へのアクセスを無効にしました。 Real-Time CDPのお客様が新しい宛先にアップグレードしていない場合、Pinterest宛先へのデータフローが失敗するようになりました。 [Pinterestへの再認証 &#x200B;](#reauthenticate) およびアップグレードされた宛先への [&#x200B; データフローの有効化 &#x200B;](#disable-old-enable-new-flows) を行って、Pinterestに対するキャンペーンを再開します。</span>

<!--

## Other items to note

After you enable the dataflows on the new destination card and disable the dataflows on the old destination cards, you should see no disruption in your campaigns or in the numbers of qualified profiles in the audiences coming in from Adobe Real-Time CDP.

-->
