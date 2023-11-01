---
title: Snap Inc 接続
description: Snapchat Ads Platform に接続し、Experience Platformからオーディエンスを書き出す方法を説明します。
exl-id: 1f0f2dc0-5f3d-424b-9b22-b1a14ac30039
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '1028'
ht-degree: 28%

---

# Snap Inc 接続

## 概要 {#overview}

[Snapchat 広告](https://forbusiness.snapchat.com/) 規模や業界に関係なく、あらゆるビジネスに対して作成されます。 Snapchatters の日常会話の一部となり、フルスクリーンのデジタル広告を使用して、ビジネスにとって最も重要な人々の行動を呼び起こします。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、 *Snap Inc* チーム。 お問い合わせや更新のご依頼は、直接お問い合わせください。 *dev-support@snap.com*

## ユースケース {#use-cases}

この宛先を使用すると、マーケターは、Experience Platformで作成されたユーザーオーディエンスを Snapchat 広告にインポートし、広告のターゲティングに使用できます。

## 前提条件 {#prerequisites}

この宛先を使用するには、Snapchat 広告アカウントが必要です。 作成方法については、このドキュメントを参照してください。

[Snapchat 広告の概要](https://businesshelp.snapchat.com/s/article/overview?language=en_US)

## 制限事項 {#limitations}

* Snap Inc は、特定のオーディエンスセグメントに対して複数の ID をサポートしていません。 セグメントをアクティブ化する際は、1 つの ID のみをマッピングしてください。
* Snap Inc はセグメント名の変更をサポートしていません。 セグメント名を変更するには、非アクティブ化して名前を変更してからアクティブ化する必要があります。
* オーディエンスセグメントのメンバーのリテンション期間を定義することはできません。 すべてのメンバーは全期間保持し、削除されるまでオーディエンスに含まれます。

## サポートされている ID {#supported-identities}

The *Snap Inc* の宛先では、以下の表で説明する id のアクティブ化がサポートされます。 [ID](/help/identity-service/namespaces.md) についての詳細情報。

に送信されたすべての識別子 *Snap Inc* の宛先は、SHA-256 形式でハッシュ化する必要があります。 プレーンテキストの識別子をハッシュ化してから宛先に送信するには、 **[!UICONTROL 変換を適用]** オプションを使用します。

>[!WARNING]
> 
> ハッシュ化されていない識別子は、Snap Inc の宛先で受け付けられず、送信するとエラーが発生する可能性があります。


>[!IMPORTANT]
> 
> Snap Inc の宛先は複数の ID をサポートしていません。 ID を 1 つだけ選択してください。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メールアドレス | SHA-256 ハッシュ化された電子メールアドレス | ターゲット ID フィールドへの電子メールアドレスのマッピング *emailAddress*. |
| 電話番号 | SHA-256 ハッシュ化された電話番号 | ターゲット ID フィールドへの電子メールアドレスのマッピング *phoneNumber*. |
| GAID | SHA-256 ハッシュ化されたGoogle Advertising ID | Google Advertising ID をターゲット ID フィールドにマッピングする *ガイド*. |
| IDFA | SHA-256 ハッシュ化されたApple Advertising ID | Apple Advertising ID をターゲット ID フィールドにマッピングする *idfa*. |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | オーディエンスのすべてのメンバーを、 *YOURDESTINATION* 宛先。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## Snap Inc に接続中 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、次の手順に従います。

1. 次を検索： *Snap Inc* Adobe Experience Platformの宛先カタログからの宛先を選択し、「 **設定**.
2. 選択 **[!UICONTROL 宛先に接続]**. 次の画面にリダイレクトされます。
   ![認証画面 1](/help/destinations/assets/catalog/advertising/snapchat-ads/auth1.png)
3. Snapchat の認証情報を入力し、[ ] を選択します。 **ログイン**.
4. Adobe Experience Platformがアクセスできる Snapchat データが表示されます。 選択 **続行** をクリックして、接続プロセスを続行します。

![認証画面 2](/help/destinations/assets/catalog/advertising/snapchat-ads/auth2.png)

「続行」を選択した後、Adobe Experience Platformにリダイレクトされるまで待ちます。

### 宛先の詳細の入力 {#destination-details}

![宛先の詳細](/help/destinations/assets/catalog/advertising/snapchat-ads/destinationdetails.png)

宛先の詳細を設定するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 次へ]**.

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**：オーディエンスのインポート先の広告アカウントに関連付けられている広告アカウント ID。 これの見つけ方について詳しくは、 [Snapchat Business ヘルプセンターに関するこのドキュメント](https://businesshelp.snapchat.com/s/article/biz-acct-id?language=en_US).

>[!IMPORTANT]
> 
>不正なまたは無効な Snapchat 広告アカウント ID を入力すると、オーディエンスのアクティベーションが失敗します。 正しい広告アカウント ID を入力したことを再確認してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

## データの書き出しを検証する {#exported-data}

オーディエンスを *Snap Inc* の宛先に設定すると、スナップ広告マネージャーの [**オーディエンス** セクション](https://businesshelp.snapchat.com/s/article/audience-sharing). このセクションに移動するには、次の手順に従います。

1. にログインします。 [スナップ広告マネージャ](https://ads.snapchat.com/)
2. 選択 **オーディエンス** 画面の左上隅にあるプルダウンメニューから、 オーディエンスライブラリに、Adobe Experience Platformでアクティブ化したオーディエンスが表示されます。

![オーディエンス](/help/destinations/assets/catalog/advertising/snapchat-ads/audiences.png)

Adobe・オーディエンスが Snap Inc に対して最初にアクティブ化されたとき、最初は空のオーディエンスとして表示されます。 これは、Adobe Experience Platformがオーディエンスを評価するまで、メンバーデータを Snap Inc に書き出さないためです。 Experience Platformでのオーディエンスの評価方法について詳しくは、 [セグメント化サービスの概要](https://experienceleague.adobe.com/docs/experience-platform/segmentation/home.html#evaluate-segments).

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
