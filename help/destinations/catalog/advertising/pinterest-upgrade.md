---
title: 新しい API へのpinterestの宛先の移行。 お客様のアクションが必要です。
description: Pinterestは、Real-Time CDPのPinterestの宛先で現在使用されている v4 広告主 API を廃止します。 pinterestキャンペーンを中断することなく、新しい API にシームレスに移行するために、アクション項目を理解します。
hide: true
hidefromtoc: true
source-git-commit: dbbdb62c996466499b70990decba58ecaf1be901
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 0%

---

# Pinterestの宛先を新しい API にアップグレードしました。 2023 年 12 月 15 日までに必要となるお客様のアクション

## 何が起きているのでしょうか？

Pinterestは、現在 [Pinterestの宛先](/help/destinations/catalog/advertising/pinterest.md) Real-Time CDPで AdobeはPinterestで動作中で、 [v5 広告主 API](https://developers.pinterest.com/docs/getting-started/migration/). このページを読んで、Pinterestキャンペーンを中断することなく新しい API にシームレスに移行するための操作項目を理解してください。

## なぜ通知を受けるのか？

お客様の組織が、Pinterestに対してオーディエンスをアクティブ化するアクティブなデータフローを持っていると識別しました。

## 計画は？

Adobeは、Pinterest API v5 を利用する新しいPinterest宛先カードをリリースします。このカードは、新しい接続で既存のデータフローを保持します。

## アクティブ化されたオーディエンスを機能させ続けるには、何かをおこなう必要がありますか？

はい。Adobeがアップグレードを完了し、新しいPinterestの宛先をリリースしたら、Real-Time CDPのPinterest広告主アカウントでPinterestに再認証する必要があります。 以下の詳細な手順を参照してください。

### pinterestへの再認証 {#reauthenticate}

1. に移動します。 **[!UICONTROL 宛先/アカウント]** 画面上のフィルターを使用して、Pinterestの宛先のみをフィルタリングします。
   ![pinterestアカウントのみをフィルター](/help/destinations/assets/catalog/advertising/pinterest-migration/filter-pinterest-acconts-only.png)
2. 次の日： **（新規）Pinterest** リンク先で、3 つのドット記号を選択し、「 」を選択します。 **[!UICONTROL 詳細を編集]**.
   ![詳細を編集を選択](/help/destinations/assets/catalog/advertising/pinterest-migration/edit-details-pinterest.png)
3. 選択 **[!UICONTROL OAuth を再接続]** pinterestアカウントにログインします。
   ![「 OAuth を再接続」を選択します。](/help/destinations/assets/catalog/advertising/pinterest-migration/reconnect-oauth-pinterest.png)
4. 以下のセクションのアクション項目に移動します。

### 既存のフローを古い宛先に対して無効にし、新しい宛先へのフローを有効にする {#disable-old-enable-new-flows}

その後、古い宛先カードへの既存のフローを手動で無効にする必要があります **[!UICONTROL （廃止） Pinterest]** 新しいカードへのフローを有効にします。 **[!UICONTROL （新規）Pinterest]**.

1. に移動します。 **[!UICONTROL 宛先/参照]** 画面上のフィルターを使用して、 **[!UICONTROL （新規）Pinterest]** および **[!UICONTROL （廃止） Pinterest]** の宛先のみ。
   ![pinterestのデータフローを「参照」タブでのみフィルタリングする](/help/destinations/assets/catalog/advertising/pinterest-migration/filter-pinterest-browse.png)
2. ハイパーリンクされた接続名（上のスクリーンショットの例では「Loyalty campaign」）を **[!UICONTROL （廃止） Pinterest]** 宛先に移動し、 **[!UICONTROL 有効にする]** 切り替える **オフ**.
   ![新しい接続の場合はオン、古い接続の場合はオフに切り替えます。](/help/destinations/assets/catalog/advertising/pinterest-migration/enable-disable-toggle-old-destination.png)
3. ハイパーリンクされた接続名（上のスクリーンショットの例では「Loyalty campaign」）を **[!UICONTROL （新規）Pinterest]** 宛先に移動し、 **[!UICONTROL 有効にする]** 切り替える **オン**.
   ![新しい接続の場合はオン、古い接続の場合はオフに切り替えます。](/help/destinations/assets/catalog/advertising/pinterest-migration/enable-disable-toggle-new-destination.png)
4. 古いデータフローと新しいデータフローのアクティブ化されたオーディエンスリストを比較し、新しいフローで見つからない古いフローに新しいオーディエンスがないことを確認します。

キャンペーンに中断は生じませんが、すべてが期待どおりに動作することをPinterest UI で確認するようにしてください。

## 高度なタイムラインをご紹介いただけますか？

はい、以下を参照してください。

**2023 年 11 月 16 日まで**：新しい宛先の準備が整い、2 つのPinterestカードがカタログに並んで表示され、現在のPinterestカードへの既存のすべてのデータフローが新しい宛先にコピーされます。

![新旧のPinterestの宛先を並べて表示](/help/destinations/assets/catalog/advertising/pinterest-migration/pinterest-two-cards-side-by-side.png)

>[!IMPORTANT]
>
>2023 年 11 月 16 日以降、従来のPinterestの宛先が **[!UICONTROL 廃止]**. <span class="preview">11 月 17 日以降に（廃止予定の）Pinterestの宛先に対してデータフローに加えた変更は、すべて次のとおりです。 *not* が新しいPinterest宛先に自動的に引き継がれます。 </span>
>例えば、次のようにします。 *推奨しない* 11 月 16 日以降に、古い宛先に対する新しいオーディエンスをアクティブ化する場合 そうすれば、 [通常のアクティベーション手順](/help/destinations/ui/activate-segment-streaming-destinations.md) 顧客アクションを実行した後で、オーディエンスを新しい宛先に追加する場合。

**2023 年 12 月 15 日まで**: <span class="preview">顧客アクション 1</span>. 新しいカードがPinterestに接続されるように、Pinterestに再認証する必要があります。 完全な手順を次の場所に表示： [この節](#reauthenticate).

<span class="preview">顧客アクション 2</span>次に、古いカードでPinterestへのデータフローを無効にし、新しいカードでデータフローを有効にする必要があります。 完全な手順を次の場所に表示： [この節](#disable-old-enable-new-flows).

>[!IMPORTANT]
>
>2023 年 12 月 15 日以降、Adobeでは、古い **[!UICONTROL （廃止） Pinterest]** 宛先。

## その他の注意事項

新しい宛先カードでデータフローを有効にし、古い宛先カードでデータフローを無効にした後は、キャンペーンやAdobe Real-Time CDPからのオーディエンス内の認定済みプロファイルの数に混乱が生じることはありません。
