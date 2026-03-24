---
title: TikTok 接続
description: Adobe TikTokでカスタムオーディエンスを構築し、広告施策でターゲティングするためのデータを活用。 これには、web サイトを訪問した人やコンテンツとインタラクションした人が含まれます。 AdobeとTikTok Ads Managerのリアルタイム統合を利用して、Adobe Experience PlatformからTikTokに、必要なオーディエンスを迅速かつ安全に送信できます。
last-substantial-update: 2023-03-20T00:00:00Z
exl-id: 7b12d17f-7d9a-4615-9830-92bffe3f6927
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '1211'
ht-degree: 27%

---

# TikTok 接続

## 概要 {#overview}

Adobe TikTokでカスタムオーディエンスを構築し、広告施策でターゲティングするためのデータを活用。 これには、web サイトを訪問した人やコンテンツとインタラクションした人が含まれます。 AdobeとTikTok Ads Managerのリアルタイム統合を利用して、必要なオーディエンスを[!DNL Adobe Experience Platform]からTikTokにすばやく安全にプッシュできます。 詳しくは、[TikTok ビジネス ヘルプセンター](https://ads.tiktok.com/help/article/audiences)を参照してください。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、TikTok チームによって作成および管理されます。 問い合わせや更新のリクエストについては、[https://ads.tiktok.com/help/](https://ads.tiktok.com/help/)から直接お問い合わせください。

## ユースケース {#use-cases}

TikTokの宛先を使用する方法とタイミングをより正確に把握するために、[!DNL Adobe Experience Platform]のお客様の使用例を次に示します。

### ユースケース {#use-case-1}

スポーツウェア企業が、ソーシャルメディアアカウントを通じて既存顧客にリーチしようとしている。 アパレル企業は、自社のCRMから[!DNL Adobe Experience Platform]へのメールアドレスを取り込み、オフラインデータからオーディエンスを構築し、そのオーディエンスをTikTokに送信して、顧客のソーシャルメディアのフィードに広告を表示することができます。

## 前提条件 {#prerequisites}

オーディエンスの送信先となるTikTok Ads Manager アカウントに[!DNL Admin]または[!DNL Operator] アクセスできる必要があります。 詳細については、[TikTok ヘルプセンター](https://ads.tiktok.com/help/article/add-users-tiktok-business-center)を参照してください。

TikTok Ads Manager アカウントにデータを送信する前に、[!DNL Adobe Experience Platform]の広告アカウントにアクセスする権限を`Audience Management`さんに付与する必要があります。 この権限は、[TikTok UIでAds Manager ID](#authenticate)を入力し、Experience Platform Ads Manager アカウントにリダイレクトした後に権限を付与することで付与できます。

## サポートされている ID {#supported-identities}

TikTokでは、次の表に示すIDのアクティベーションがサポートされています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | GOOGLE ADVERTISING ID | ソース IDがGAID名前空間である場合は、GAID ターゲット IDを選択します。 プレーンテキストとSHA256 ハッシュ化されたGAID値の両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| IDFA | Apple の広告主 ID | ソース IDがIDFA名前空間の場合は、IDFA ターゲット IDを選択します。 プレーンテキストとSHA256 ハッシュ化されたIDFA値の両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| 電話番号 | SHA256 アルゴリズムでハッシュ化された電話番号 | プレーンテキストとSHA256 ハッシュ化された電話番号の両方が[!DNL Adobe Experience Platform]でサポートされており、これらはE.164形式である必要があります。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| メール | SHA256 アルゴリズムでハッシュ化されたメールアドレス | プレーンテキストとSHA256 ハッシュ化された電子メールアドレスの両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |
| [!DNL Federated Audience Composition] | ○ | [Federated Audience Composition](https://experienceleague.adobe.com/ja/docs/federated-audience-composition/using/start/audiences)を通じてExperience Platformに読み込まれたオーディエンス。 |

{style="table-layout:auto"}



オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス &#x200B;](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス &#x200B;](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [&#x200B; データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | TikTokの宛先で使用されているID （名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証を行うには、[!DNL TikTok Ads Manager] アカウントにログインし、Adobeに代わってオーディエンスを管理する権限を付与するようにリダイレクトされます。

![TikTok権限の選択](/help/destinations/assets/catalog/social/tiktok/tiktok-authenticate-destination.png "権限を選択するためのTikTok UIの画像")

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![宛先接続の詳細](/help/destinations/assets/catalog/social/tiktok/tiktok-configure-destination-details.png "Experience Platform UIの画像。入力する宛先接続の詳細を表示")

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。
* **[!UICONTROL TikTok Ads Manager ID]**：あなたの[!DNL TikTok Ads Manager ID]。 この情報は[!DNL TikTok Ads manager] アカウントで確認できます。

![TikTok Ads Manager ID](/help/destinations/assets/catalog/social/tiktok/tiktok-ads-manager-ID.png "TikTok Ads Manager UIの画像。TikTok Ads Manager ID")の取得方法が表示されている

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![&#x200B; ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### ID のマッピング {#map}

以下は、オーディエンスをTikTok Ads Managerに書き出す際の正しいID マッピングの例です。

ソースフィールドの選択：

* `Email_LC_SHA256`と[!DNL Adobe Experience Platform]のプロファイルを一意に識別するソース IDとして、識別子（例：[!DNL TikTok Ads Manager]）を選択します。

ターゲットフィールドの選択：

* メール名前空間をターゲット IDとして選択します。

![ID マッピング &#x200B;](/help/destinations/assets/catalog/social/tiktok/tiktok-map-identity.png "Experience Platform UIの画像、ID マッピング ")

## 書き出したデータ {#exported-data}

[!DNL TikTok Ads Manager] アカウント（**Assets > Audiences**&#x200B;の下）を確認して、Experience Platform オーディエンスが正常にエクスポートされたかどうかを確認します。 オーディエンスは、オーディエンスタイプ `Partner Audience`として入力されます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

詳しくは、[TikTok ヘルプセンターページ &#x200B;](https://ads.tiktok.com/help/article/audiences)を参照してください。
