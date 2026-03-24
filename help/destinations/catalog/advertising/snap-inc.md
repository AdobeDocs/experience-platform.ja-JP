---
title: Snap Inc接続
description: Snapchat Ads Platformに接続し、Experience Platformからオーディエンスを書き出す方法について説明します。
exl-id: 1f0f2dc0-5f3d-424b-9b22-b1a14ac30039
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '1141'
ht-degree: 21%

---

# Snap Inc接続

## 概要 {#overview}

[Snapchat広告](https://forbusiness.snapchat.com/)は、規模や業界を問わず、あらゆるビジネスに対して作成されます。 フルスクリーンのデジタル広告を通じて、自社にとって最も重要な人々の行動を促すことで、Snapchattersの日常会話に参加できるようになります。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメント ページは、*Snap Inc* チームによって作成および管理されています。 問い合わせや更新のリクエストについては、*dev-support@snap.com*&#x200B;から直接お問い合わせください

## ユースケース {#use-cases}

この宛先を使用すると、マーケターはExperience Platformで作成したユーザーオーディエンスをSnapchat Adsに読み込み、広告のターゲティングに使用できます。

## 前提条件 {#prerequisites}

この宛先を使用するには、Snapchat広告アカウントが必要です。 作成方法について詳しくは、このドキュメントを参照してください。

[Snapchat Advertisingの基本を学ぶ](https://businesshelp.snapchat.com/s/article/overview?language=en_US)

## 制限事項 {#limitations}

* Snap Incは、特定のオーディエンスセグメントに対して複数のIDをサポートしていません。 セグメントをアクティブ化する際は、1つのIDのみをマッピングしてください。
* Snap Incは、セグメント名の変更をサポートしていません。 セグメントの名前を変更するには、セグメントを非アクティブ化、名前を変更してからアクティブ化する必要があります。
* オーディエンスセグメントのメンバーの保持期間を定義することはできません。 すべての会員は生涯リテンション権を持ち、削除されるまでオーディエンスに属します。

## サポートされている ID {#supported-identities}

*Snap Inc*&#x200B;宛先は、次の表に示すIDのアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

*Snap Inc*&#x200B;宛先に送信されるすべての識別子は、SHA-256形式でハッシュ化する必要があります。 プレーンテキスト識別子を宛先に送信する前にハッシュ化するには、宛先のターゲット識別子をマッピングする際に&#x200B;**[!UICONTROL Apply transformation]** オプションをオンにします。

>[!WARNING]
>
> ハッシュ化されていないIDはSnap Incの宛先に受け入れられず、送信するとエラーが発生する可能性があります。


>[!IMPORTANT]
>
> Snap Incの宛先は、複数のIDをサポートしていません。 IDを1つだけ選択してください。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メールアドレス | SHA-256 ハッシュ化されたメールアドレス | メールアドレスをターゲット ID フィールド *emailAddress*&#x200B;にマッピングします。 |
| 電話番号 | SHA-256 ハッシュ化された電話番号 | メールアドレスをターゲット ID フィールド *phoneNumber*&#x200B;にマッピングします。 |
| GAID | SHA-256 ハッシュ化されたGoogle Advertising ID | Google Advertising IDをターゲット ID フィールド *gaid*&#x200B;にマッピングします。 |
| IDFA | SHA-256 ハッシュ化されたApple Advertising ID | Apple Advertising IDをターゲット ID フィールド *idfa*&#x200B;にマッピングします。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |
| [!DNL Federated Audience Composition] | ○ | [Federated Audience Composition](https://experienceleague.adobe.com/en/docs/federated-audience-composition/using/start/audiences)を通じてExperience Platformに読み込まれたオーディエンス。 |

{style="table-layout:auto"}



オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス ](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス ](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [ データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | Snap Incの宛先で使用されている識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## Snap Inc.への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

### 宛先に対する認証 {#authenticate}

宛先に対して認証を行うには、次の手順に従います。

1. *の宛先カタログから* Snap Inc[!DNL Adobe Experience Platform]の宛先を見つけ、**セットアップ**&#x200B;を選択します。
2. 「**[!UICONTROL Connect to destination]**」を選択します。次の画面にリダイレクトされます。
   ![認証画面1](/help/destinations/assets/catalog/advertising/snapchat-ads/auth1.png)
3. Snapchatの資格情報を入力し、**ログイン**&#x200B;を選択します。
4. [!DNL Adobe Experience Platform]がアクセスできるSnapchat データが表示されます。 接続プロセスを続行するには、**続行**&#x200B;を選択します。

![認証画面2](/help/destinations/assets/catalog/advertising/snapchat-ads/auth2.png)

続行を選択した後、[!DNL Adobe Experience Platform]にリダイレクトされるまで待ちます。

### 宛先の詳細の入力 {#destination-details}

![宛先の詳細](/help/destinations/assets/catalog/advertising/snapchat-ads/destinationdetails.png)

宛先の詳細を設定するには、必須フィールドに入力し、**[!UICONTROL Next]**&#x200B;を選択します。

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。
* **[!UICONTROL Account ID]**: オーディエンスを読み込む広告アカウントに関連付けられている広告アカウント ID。 これを見つける方法について詳しくは、[Snapchat Business ヘルプセンター](https://businesshelp.snapchat.com/s/article/biz-acct-id?language=en_US)に関するこのドキュメントを参照してください。

>[!IMPORTANT]
>
>誤ったSnapchat広告アカウント IDまたは無効なアカウント IDを入力すると、オーディエンスのアクティブ化が失敗します。 正しい広告アカウント IDを入力したことを再度確認してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![ ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

## データの書き出しを検証する {#exported-data}

*Snap Inc*&#x200B;宛先に対してオーディエンスをアクティブ化すると、Snap Ads Managerの&#x200B;[**オーディエンス** セクション ](https://businesshelp.snapchat.com/s/article/audience-sharing)にオーディエンスが表示されます。 このセクションに移動するには、次の手順に従います。

1. [Snap Ads Manager](https://ads.snapchat.com/)にログインします
2. 画面の左上隅にあるプルダウンメニューから「**オーディエンス**」を選択します。 [!DNL Adobe Experience Platform]でアクティブ化したオーディエンスは、オーディエンスライブラリに表示されます。

![オーディエンス](/help/destinations/assets/catalog/advertising/snapchat-ads/audiences.png)

Adobe オーディエンスがSnap Incに最初にアクティベートされると、最初は空のオーディエンスとして表示されます。 これは、[!DNL Adobe Experience Platform]がオーディエンスを評価するまでSnap Incにメンバーデータを書き出さないためです。 Experience Platformでのオーディエンスの評価方法について詳しくは、[ セグメント化サービスの概要](https://experienceleague.adobe.com/docs/experience-platform/segmentation/home.html?lang=ja#evaluate-segments)を参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
