---
title: Snap Inc 接続
description: Snapchat Ads Platform に接続し、Experience Platformからオーディエンスを書き出す方法を説明します。
exl-id: 1f0f2dc0-5f3d-424b-9b22-b1a14ac30039
source-git-commit: 9a80a9b49b1983e8e488d11b114c02130b045686
workflow-type: tm+mt
source-wordcount: '1063'
ht-degree: 26%

---

# Snap Inc 接続

## 概要 {#overview}

[Snapchat 広告 ](https://forbusiness.snapchat.com/) は、規模や業界に関係なく、あらゆるビジネス向けに作られています。 ビジネスにとって最も重要な人々の行動を刺激するフルスクリーンのデジタル広告で、Snapchatters の日常会話の一部になります。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、*Snap Inc* チームによって作成および管理されます。 お問い合わせや更新のリクエストについては、*dev-support@snap.comまで直接ご連絡ください*

## ユースケース {#use-cases}

この宛先を使用すると、マーケターは、Experience Platformで作成したユーザーオーディエンスを Snapchat 広告にインポートし、広告のターゲティングに使用できます。

## 前提条件 {#prerequisites}

この宛先を使用するには、Snapchat 広告アカウントが必要です。 作成方法については、このドキュメントを参照してください。

[Snapchat Advertisingの概要 ](https://businesshelp.snapchat.com/s/article/overview?language=en_US)

## 制限事項 {#limitations}

* Snap Inc は、特定のオーディエンスセグメントに対して複数の ID をサポートしていません。 セグメントをアクティブ化する際は、1 つの ID のみをマッピングしてください。
* Snap Inc は、セグメント名の変更をサポートしていません。 セグメントの名前を変更するには、セグメントをアクティベート解除し、名前を変更してから、アクティベートする必要があります。
* オーディエンスセグメントのメンバーの保持期間は定義できません。 すべてのメンバーにはライフタイムリテンションがあり、削除されるまでオーディエンスに残ります。

## サポートされている ID {#supported-identities}

*Snap Inc* の宛先では、以下の表に示す ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

*Snap Inc* の宛先に送信されるすべての識別子は、SHA-256 形式でハッシュ化する必要があります。 宛先に送信する前にプレーンテキスト識別子をハッシュ化するには、宛先のターゲット識別子をマッピングする際に、「**[!UICONTROL 変換を適用]**」オプションをオンにします。

>[!WARNING]
> 
> ハッシュ化されていない ID は Snap Inc の宛先に受け入れられず、送信するとエラーが発生する可能性があります。


>[!IMPORTANT]
> 
> Snap Inc の宛先は複数の ID をサポートしていません。 ID を 1 つだけ選択してください。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メールアドレス | SHA-256 でハッシュ化されたメールアドレス | メールアドレスをターゲット ID フィールド *emailAddress* にマッピングします。 |
| 電話番号 | SHA-256 でハッシュ化された電話番号 | メールアドレスをターゲット ID フィールド *phoneNumber* にマッピングします。 |
| GAID | SHA-256 でハッシュ化されたGoogle Advertising ID | Google Advertising ID をターゲット ID フィールド *gaid* にマッピングします。 |
| IDFA | SHA-256 でハッシュ化されたApple Advertising ID | Apple Advertising ID をターゲット ID フィールド *idfa* にマッピングします。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |
| [!DNL Federated Audience Composition] | ✓ | [Federated Audience Composition](https://experienceleague.adobe.com/en/docs/federated-audience-composition/using/start/audiences) を通じてExperience Platformにインポートされたオーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | Snap Inc 宛先で使用される識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## Snap Inc.への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、次の手順に従います。

1. Adobe Experience Platformの宛先カタログから *Snap Inc* の宛先を見つけて、「**設定**」を選択します。
2. **[!UICONTROL 宛先に接続]** を選択します。 次の画面にリダイレクトされます：
   ![ 認証画面 1](/help/destinations/assets/catalog/advertising/snapchat-ads/auth1.png)
3. Snapchat の認証情報を入力し、「**ログイン**」を選択します。
4. Adobe Experience Platformがアクセスできる Snapchat データが表示されます。 「**続行**」を選択して、接続プロセスを続行します。

![ 認証画面 2](/help/destinations/assets/catalog/advertising/snapchat-ads/auth2.png)

「続行」を選択した後、Adobe Experience Platformにリダイレクトされるまで待ちます。

### 宛先の詳細の入力 {#destination-details}

![ 宛先の詳細 ](/help/destinations/assets/catalog/advertising/snapchat-ads/destinationdetails.png)

宛先の詳細を設定するには、必須フィールドに入力し、「**[!UICONTROL 次へ]**」を選択します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**：オーディエンスの読み込み先となる広告アカウントに関連付けられている広告アカウント ID。 見つける方法の詳細については、[Snapchat ビジネスヘルプセンターのこのドキュメント ](https://businesshelp.snapchat.com/s/article/biz-acct-id?language=en_US) を参照してください。

>[!IMPORTANT]
> 
>Snapchat 広告アカウント ID が正しくないか無効な場合、オーディエンスのアクティベーションが失敗します。 適切な広告アカウント ID を入力したことを再確認してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

## データの書き出しを検証する {#exported-data}

*Snap Inc* 宛先に対してオーディエンスをアクティブ化すると、Snap Ads Manager の [**オーディエンス** セクション ](https://businesshelp.snapchat.com/s/article/audience-sharing) でオーディエンスを表示できるようになります。 このセクションに移動するには、次の手順に従います。

1. [Snap Ads Manager](https://ads.snapchat.com/) にログインします
2. 画面の左上隅にあるプルダウンメニューから「**オーディエンス**」を選択します。 Adobe Experience Platformでアクティブ化したオーディエンスがオーディエンスライブラリに表示されます。

![オーディエンス](/help/destinations/assets/catalog/advertising/snapchat-ads/audiences.png)

Adobe オーディエンスが最初に Snap Inc.に対してアクティブ化されると、最初は空のオーディエンスとして表示されることに注意してください。 これは、Adobe Experience Platformがオーディエンスを評価するまで、メンバーデータを Snap Inc に書き出さないためです。 Experience Platformでのオーディエンスの評価方法について詳しくは、[ セグメント化サービスの概要 ](https://experienceleague.adobe.com/docs/experience-platform/segmentation/home.html#evaluate-segments) を参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
