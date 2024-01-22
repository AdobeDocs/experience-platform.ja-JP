---
title: 新しい API へのpinterestの宛先の移行。 お客様のアクションが必要です。
description: Pinterestは、Real-Time CDPのPinterestの宛先で現在使用されている v4 広告主 API を廃止します。 pinterestキャンペーンを中断することなく、新しい API にシームレスに移行するために、アクション項目を理解します。
hide: true
hidefromtoc: true
exl-id: c965235c-4208-4c28-9ac5-eb4c0061515d
source-git-commit: 3968c8e2a0ebd2084a7047fb41e2b85c5da7a6e7
workflow-type: tm+mt
source-wordcount: '672'
ht-degree: 0%

---

# Pinterestの宛先を新しい API にアップグレードしました。 2024 年 1 月 19 日までに必要となるお客様のアクション。

>[!IMPORTANT]
>
>このページの顧客アクション項目は、新しい **[!UICONTROL Pinterest]** の宛先が、最新のPinterest API を使用して、宛先カタログに追加されました。

## 何が起きているのでしょうか？

Pinterestは、 [Pinterestの宛先](/help/destinations/catalog/advertising/pinterest.md) Real-Time CDPで Adobeは、 [v5 広告主 API](https://developers.pinterest.com/docs/getting-started/migration/). このページを読んで、Pinterestキャンペーンを中断することなく新しい API にシームレスに移行するための操作項目を理解してください。

## なぜ通知を受けるのか？

お客様の組織が、Pinterestに対してオーディエンスをアクティブ化するアクティブなデータフローを持っていると識別しました。

## 計画は？

Adobeは、Pinterest API v5 を利用する新しいPinterest宛先カードをリリースします。このカードは、新しい接続で既存のデータフローを保持します。

## アクティブ化されたオーディエンスを機能させ続けるには、何かをおこなう必要がありますか？

はい。2024 年 1 月 18 日より前に、Real-Time CDPでPinterestの広告主アカウントを使用して、新しいPinterestの宛先に対する認証をおこなう必要があります。 以下の詳細な手順を参照してください。

### pinterestへの再認証 {#reauthenticate}

1. に移動します。 **[!UICONTROL 宛先/アカウント]** 画面上のフィルターを使用して、Pinterestの宛先のみをフィルタリングします。
   ![pinterestアカウントのみをフィルター](/help/destinations/assets/catalog/advertising/pinterest-migration/filter-pinterest-acconts-only.png)
2. 次の日： **Pinterest** リンク先で、3 つのドット記号を選択し、「 」を選択します。 **[!UICONTROL 詳細を編集]**.
   ![詳細を編集を選択](/help/destinations/assets/catalog/advertising/pinterest-migration/edit-details-pinterest.png)
3. 選択 **[!UICONTROL OAuth を再接続]** pinterestアカウントにログインします。
   ![「 OAuth を再接続」を選択します。](/help/destinations/assets/catalog/advertising/pinterest-migration/reconnect-oauth-pinterest.png)
4. 以下のセクションのアクション項目に移動します。

### 新しい宛先へのフローの有効化 {#disable-old-enable-new-flows}

次に、新しいカードへのデータフローを有効にする必要があります **[!UICONTROL （新規）Pinterest]**.

1. に移動します。 **[!UICONTROL 宛先/参照]** 画面上のフィルターを使用して、 **[!UICONTROL Pinterest]** 宛先のみ。
   ![pinterestのデータフローを「参照」タブでのみフィルタリングする](/help/destinations/assets/catalog/advertising/pinterest-migration/filter-pinterest-browse.png)
2. ハイパーリンクされた接続名（上のスクリーンショットの例では「Loyalty campaign」）を **[!UICONTROL Pinterest]** 宛先に移動し、 **[!UICONTROL 有効にする]** 切り替える **オン**.
   ![新しい接続の場合はオン、古い接続の場合はオフに切り替えます。](/help/destinations/assets/catalog/advertising/pinterest-migration/enable-disable-toggle-new-destination.png)

<!--

While no disruption to your campaigns is expected, remember to check in the Pinterest UI that everything works as expected.

-->

## 高度なタイムラインをご紹介いただけますか？

はい、以下を参照してください。

**2023 年 11 月 16 日まで**：新しい宛先の準備が整い、Pinterestが古い v4 API のサポートを停止するまで、2 枚のPinterestカードがカタログに並んで表示されます。 現在のPinterestカードに対する既存のすべてのデータフローは、新しい宛先にコピーされます。

![新旧のPinterestの宛先を並べて表示](/help/destinations/assets/catalog/advertising/pinterest-migration/pinterest-two-cards-side-by-side.png)

>[!IMPORTANT]
>
>2023 年 11 月 16 日以降、従来のPinterestの宛先が **[!UICONTROL 廃止]**. <span class="preview">11 月 17 日以降に（廃止予定の）Pinterestの宛先に対してデータフローに加えた変更は、すべて次のとおりです。 *not* が新しいPinterest宛先に自動的に引き継がれます。 </span>
>例えば、次のようにします。 *推奨しない* 11 月 16 日以降に、古い宛先に対する新しいオーディエンスをアクティブ化する場合 そうすれば、 [通常のアクティベーション手順](/help/destinations/ui/activate-segment-streaming-destinations.md) 顧客アクションを実行した後で、オーディエンスを新しい宛先に追加する場合。

**2023 年 12 月 15 日まで**: <span class="preview">顧客アクション 1</span>. 新しいカードがPinterestに接続されるように、Pinterestに再認証する必要があります。 完全な手順を次の場所に表示： [この節](#reauthenticate).

<span class="preview">顧客アクション 2</span>次に、古いカードでPinterestへのデータフローを無効にし、新しいカードでデータフローを有効にする必要があります。 完全な手順を次の場所に表示： [この節](#disable-old-enable-new-flows).

<!--

>[!IMPORTANT]
>
>After December 15th, 2023, Adobe does not guarantee the integrity of dataflows to the old **[!UICONTROL (Deprecating) Pinterest]** destination.

-->

**2024 年 1 月 19 日以降**: <span class="preview">Pinterestは V4 広告主 API へのアクセスをオフにしました。 新しい宛先にアップグレードしていないReal-Time CDPのお客様は、Pinterestの宛先へのデータフローが失敗することになります。 [pinterestへの再認証](#reauthenticate) および [データフローを有効にする](#disable-old-enable-new-flows) をアップグレードした宛先に追加して、Pinterestへのキャンペーンを再開します。</span>.

## その他の注意事項

新しい宛先カードでデータフローを有効にし、古い宛先カードでデータフローを無効にした後は、キャンペーンやAdobe Real-Time CDPからのオーディエンス内の認定済みプロファイルの数に混乱が生じることはありません。
