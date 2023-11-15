---
title: 新しい API へのpinterestの宛先の移行。 お客様のアクションが必要です。
description: Pinterestは、Real-Time CDPのPinterestの宛先で現在使用されている v4 広告主 API を廃止します。 pinterestキャンペーンを中断することなく、新しい API にシームレスに移行するために、アクション項目を理解します。
hide: true
hidefromtoc: true
source-git-commit: 10bf63677c66366c226d647b1174093c1704a8b9
workflow-type: tm+mt
source-wordcount: '713'
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

はい。Adobeがアップグレードを完了したら（11 月 17 日を予定）、Adobe Experience PlatformのPinterest広告主アカウントでPinterestに再認証する必要があります。 以下の詳細な手順を参照してください。

### pinterestへの再認証 {#reauthenticate}

1. に移動します。 **[!UICONTROL 宛先/アカウント]** 画面上のフィルターを使用して、Pinterestの宛先のみをフィルタリングします。
   ![pinterestアカウントのみをフィルター](/help/destinations/assets/catalog/advertising/pinterest-migration/filter-pinterest-acconts-only.png)
2. 次の日： **（新規）Pinterest** リンク先で、3 つのドット記号を選択し、「 」を選択します。 **[!UICONTROL 詳細を編集]**.
   ![詳細を編集を選択](/help/destinations/assets/catalog/advertising/pinterest-migration/edit-details-pinterest.png)
3. 選択 **[!UICONTROL OAuth を再接続]** pinterestアカウントにログインします。
   ![「 OAuth を再接続」を選択します。](/help/destinations/assets/catalog/advertising/pinterest-migration/reconnect-oauth-pinterest.png)
4. Adobeに、 **[!UICONTROL （新規）Pinterest]** 宛先。

### 既存のフローを古い宛先に対して無効にし、新しい宛先へのフローを有効にする {#disable-old-enable-new-flows}

次に、古いカードへの既存のフローを手動で無効にし、新しいカードへのフローを有効にする必要があります。

>[!IMPORTANT]
>
>再認証後、Adobeに問い合わせて、2 番目の手順を実行します。 この手順を手動で実行する場合は、次の手順に従います。

1. に移動します。 **[!UICONTROL 宛先/参照]** 画面上のフィルターを使用して、 **[!UICONTROL （新規）Pinterest]** および **[!UICONTROL （廃止） Pinterest]** の宛先のみ。
   ![pinterestのデータフローを「参照」タブでのみフィルタリングする](/help/destinations/assets/catalog/advertising/pinterest-migration/filter-pinterest-browse.png)
2. ハイパーリンク接続名（上のスクリーンショットの例では「Loyalty campaign」）を選択し、 **[!UICONTROL 有効にする]** 切り替える **オフ** 古い接続と **オン** 新しい接続に対して。
   ![新しい接続の場合はオン、古い接続の場合はオフに切り替えます。](/help/destinations/assets/catalog/advertising/pinterest-migration/enable-disable-toggle.png)
3. 古いデータフローと新しいデータフローのアクティブ化されたオーディエンスリストを比較し、新しいフローで見つからない古いフローに新しいオーディエンスがないことを確認します。

キャンペーンに中断は生じませんが、すべてが期待どおりに動作することをPinterest UI で確認するようにしてください。

## 高度なタイムラインをご紹介いただけますか？

はい、以下を参照してください。

**11 月 16 日まで**：新しい宛先の準備が整い、2 つのPinterestカードがカタログに並んで表示され、現在のPinterestカードへの既存のすべてのデータフローが新しい宛先にコピーされます。

![新旧のPinterestの宛先を並べて表示](/help/destinations/assets/catalog/advertising/pinterest-migration/pinterest-two-cards-side-by-side.png)

>[!IMPORTANT]
>
>11 月 16 日以降、従来のPinterestの宛先は **[!UICONTROL 廃止]**. <span class="preview">11 月 17 日以降に（廃止予定の）Pinterestの宛先に対してデータフローに加えた変更は、すべて次のとおりです。 *not* が新しいPinterest宛先に自動的に引き継がれます。 </span>
>例えば、次のようにします。 *推奨しない* 11 月 16 日以降に、古い宛先に対する新しいオーディエンスをアクティブ化する場合 そうすれば、 [通常のアクティベーション手順](/help/destinations/ui/activate-segment-streaming-destinations.md) 顧客アクションを実行した後で、オーディエンスを新しい宛先に追加する場合。

**12 月 15 日まで**: <span class="preview">顧客アクション</span>. 新しいカードがPinterestに接続されるように、Pinterestに再認証する必要があります（上記の手順を参照）。 これを完了したら、お問い合わせください。

古いカードのPinterestへのデータフローを無効にし、新しいカードのデータフローを有効にする必要があります。 UI で手動で実行するか、Adobeに問い合わせて実行してください。

## その他の注意事項

新しい宛先カードでデータフローを有効にし、古い宛先カードでデータフローを無効にした後は、キャンペーンやAdobe Real-Time CDPからのオーディエンス内の認定済みプロファイルの数に混乱が生じることはありません。
