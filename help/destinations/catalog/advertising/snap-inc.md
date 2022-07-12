---
title: （ベータ版）Snap Inc 接続
description: Snapchat Ads Platform に接続し、Experience Platformからオーディエンスセグメントを書き出す方法を説明します。
source-git-commit: 734d66cc881ab1b691c13ef446331d0c51851cf9
workflow-type: tm+mt
source-wordcount: '1012'
ht-degree: 6%

---


# （ベータ版）Snap Inc

## 概要 {#overview}

[Snapchat 広告](https://forbusiness.snapchat.com/) 規模や業界に関係なく、あらゆるビジネスに対して作成されます。 Snapchatters の日常会話の一部となり、フルスクリーンのデジタル広告を使用して、ビジネスにとって最も重要な人々の行動を喚起します。

>[!IMPORTANT]
>
>このドキュメントページは、 *Snap Inc* チーム。 現在はベータ版の製品であり、機能は変更される場合があります。 お問い合わせや更新のご依頼は、直接 *dev-support@snap.com*

## ユースケース {#use-cases}

この宛先を使用すると、マーケターは、Experience Platformで作成したユーザーセグメントを Snapchat 広告にインポートし、広告のターゲティングに使用できます。

## 前提条件 {#prerequisites}

この宛先を使用するには、Snapchat 広告アカウントが必要です。 作成方法については、このドキュメントを参照してください。

[Snapchat 広告の概要](https://businesshelp.snapchat.com/s/article/overview?language=en_US)

## 制限事項 {#limitations}

* Snap Inc は、特定のオーディエンスセグメントに対して複数の ID をサポートしていません。 セグメントをアクティブ化する際は、1 つの ID のみをマッピングしてください。
* Snap Inc はセグメント名の変更をサポートしていません。 セグメント名を変更するには、非アクティブ化して名前を変更してからアクティブ化する必要があります。
* オーディエンスセグメントのメンバーのリテンション期間を定義することはできません。 すべてのメンバーは全期間を保持し、削除されるまでセグメントに含まれます。

## サポートされる ID {#supported-identities}

この *Snap Inc* の宛先では、以下の表で説明する id のアクティブ化がサポートされます。 詳細情報： [id](/help/identity-service/namespaces.md).

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

{style=&quot;table-layout:auto&quot;}

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | セグメント（オーディエンス）のすべてのメンバーを、 *宛先* 宛先。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## Snap Inc に接続中 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、次の手順に従います。

1. 次を検索： *Snap Inc* Adobe Experience Platformの宛先カタログからの宛先を選択し、「 **設定**.
2. 選択 **[!UICONTROL 宛先に接続]**. 次の画面にリダイレクトされます。
   ![認証画面 1](/help/destinations/assets/catalog/advertising/snapchat-ads/auth1.png)
3. Snapchat の認証情報を入力し、 **ログイン**.
4. Adobe Experience Platformがアクセスできる Snapchat データが表示されます。 選択 **続行** をクリックして、接続プロセスを続行します。

![認証画面 2](/help/destinations/assets/catalog/advertising/snapchat-ads/auth2.png)

「続行」を選択した後、Adobe Experience Platformにリダイレクトされるまで待ちます。

### 宛先の詳細を入力 {#destination-details}

![宛先の詳細](/help/destinations/assets/catalog/advertising/snapchat-ads/destinationdetails.png)

宛先の詳細を設定するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 次へ]**.

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**:セグメントのインポート先の広告アカウントに関連付けられている広告アカウント ID。 これの見つけ方について詳しくは、 [Snapchat Business ヘルプセンターに関するこのドキュメント](https://businesshelp.snapchat.com/s/article/biz-acct-id?language=en_US).

>[!IMPORTANT]
> 
>不正なまたは無効な Snapchat 広告アカウント ID を入力すると、セグメントのアクティベーションが失敗します。 正しい広告アカウント ID を入力したことを再確認してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にして、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の指定が完了したら、 **[!UICONTROL 次へ]**.

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [ストリーミングセグメントの書き出し先に対するプロファイルとセグメントのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

## データの書き出しを検証する {#exported-data}

セグメントを *Snap Inc* 宛先に設定すると、Snap Ads Manager の [**オーディエンス** セクション](https://businesshelp.snapchat.com/s/article/audience-sharing). このセクションに移動するには、次の手順に従います。

1. にログインします。 [スナップ広告マネージャ](https://ads.snapchat.com/)
2. 選択 **オーディエンス** 画面の左上隅にあるプルダウンメニューから、 オーディエンスライブラリに、Adobe Experience Platformでアクティブ化したセグメントが表示されます。

![オーディエンス](/help/destinations/assets/catalog/advertising/snapchat-ads/audiences.png)

Adobeセグメントが Snap Inc に対して最初にアクティブ化されると、最初は空のオーディエンスとして表示されます。 これは、Adobe Experience Platformがセグメントを評価するまで、メンバデータを Snap Inc に書き出さないためです。 Experience Platformでのセグメントの評価方法について詳しくは、 [セグメント化サービスの概要](https://experienceleague.adobe.com/docs/experience-platform/segmentation/home.html?lang=en#evaluate-segments).

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](/help/data-governance/home.md).