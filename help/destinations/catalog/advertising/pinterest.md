---
title: Pinterest Customer List 接続
description: カスタマーリスト、サイトにアクセスした人、Pinterestでコンテンツにアクセスした人などから、オーディエンスを作成できます。
exl-id: e601f75f-0d40-4cd0-93ca-54d7439f1db7
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 28%

---

# [!DNL Pinterest Customer List] 接続

## 概要 {#overview}

カスタマーリスト、サイトにアクセスした人、Pinterestでコンテンツにアクセスした人などから、オーディエンスを作成できます。

>[!IMPORTANT]
>
>この宛先はPinterest チームによって作成されました。 お問い合わせやアップデートのご依頼は、https://help.pinterest.com/en/contactまで直接お問い合わせください。

## 前提条件 {#prerequisites}

* オーディエンスを追加する広告主アカウントにアクセスできるPinterest アカウントで認証する必要があります。 広告主アカウントの共有に関する詳細については、[こちら](https://help.pinterest.com/en/business/article/share-and-manage-access-to-your-ad-accounts)を参照してください。 具体的には、ユーザーには「オーディエンス」アクセスレベルが必要です。
* 顧客リスト ID形式の詳細については、[こちら](https://help.pinterest.com/en/business/article/audience-targeting)を参照してください。

## サポートされている ID {#supported-identities}

[!DNL Pinterest Customer List]宛先は、次の表に示すIDのアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html#getting-started) についての詳細情報。

宛先アクティベーションワークフローの[&#x200B; マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping)で、目的のIDをターゲットフィールド *pinterest_audience*&#x200B;にマッピングします。 IDは、Pinterestへのデータ取り込み時に区別され、決定されます。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] | *GAID*&#x200B;のソース ID名前空間をターゲット ID フィールド *pinterest_audience*&#x200B;にマッピングします。 |
| IDFA | [!DNL Apple ID for Advertisers] | *IDFA*&#x200B;のソース ID名前空間をターゲット ID フィールド *pinterest_audience*&#x200B;にマッピングします。 |
| EMAIL | メールアドレス（クリアテキストまたはSHA256 アルゴリズムでハッシュ化） | プレーンテキストとSHA256 ハッシュ化された電子メールアドレスの両方が[!DNL Adobe Experience Platform]でサポートされています。 <br> *Email*&#x200B;または&#x200B;*Email_LC_SHA256*&#x200B;のソース ID名前空間をターゲット ID フィールド *pinterest_audience*&#x200B;にマッピングします。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | × | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

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
| 書き出しタイプ | **[!UICONTROL Audience export]** | Pinterest カスタマーリストの宛先で使用されているID （名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## ユースケース {#use-cases}

[!DNL Pinterest Customer List]宛先を使用する方法とタイミングをより理解しやすくするために、[!DNL Adobe Experience Platform]のお客様がこの宛先を使用して解決できるユースケースの例を次に示します。

### ユースケース 1 {#use-case-1}

カスタマーリスト、サイトにアクセスした人、Pinterestでコンテンツにアクセスした人などから、オーディエンスを作成できます。

## 宛先に接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)する場合は、次の情報を入力する必要があります。

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。
* **[!UICONTROL Ad Account ID]**:Pinterest広告主ID。

### 認証情報を更新 {#refresh-authentication-credentials}

Pinterest トークンは30日ごとに有効期限が切れます。 トークンの有効期限は、**[!UICONTROL Account expiration date]** タブまたは&#x200B;**[[!UICONTROL Accounts]](../../ui/destinations-workspace.md#accounts)** タブの&#x200B;**[[!UICONTROL Browse]](../../ui/destinations-workspace.md#browse)**&#x200B;列から監視できます。

トークンの有効期限が切れると、宛先へのデータ書き出しが機能しなくなります。 この問題を回避するには、次の手順を実行して再認証します。

1. **[!UICONTROL Destinations]** > **[!UICONTROL Accounts]**&#x200B;に移動します
2. （オプション）ページで使用可能なフィルターを使用して、Pinterest アカウントのみを表示します。
   ![Pinterest アカウントのみを表示するフィルター](/help/destinations/assets/catalog/advertising/pinterest-customer-list/refresh-oauth-filters.png)
3. 更新するアカウントを選択し、省略記号を選択して&#x200B;**[!UICONTROL Edit details]**&#x200B;を選択します。
   ![詳細を編集コントロールを選択](/help/destinations/assets/catalog/advertising/pinterest-customer-list/refresh-oauth-edit-details.png)
4. モーダルウィンドウで、**[!UICONTROL Reconnect OAuth]**&#x200B;を選択し、Pinterestの資格情報で再認証します。
   OAuthの再接続オプションを含む![&#x200B; モーダルウィンドウ &#x200B;](/help/destinations/assets/catalog/advertising/pinterest-customer-list/reconnect-oauth-control.png)

>[!SUCCESS]
>
>認証情報が更新され、有効期限が30日にリセットされます。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![&#x200B; ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。

## その他のリソース {#additional-resources}

詳しくは、[Pinterest ヘルプセンターページ &#x200B;](https://help.pinterest.com/en/business/article/audience-targeting)を参照してください。

+++ 変更ログを表示


| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2023年11月 | 機能とドキュメントの更新 | [!DNL Real-Time CDP]のPinterestの宛先で、v5 Advertiser APIが使用されるようになりました。 |

{style="table-layout:auto"}


+++
