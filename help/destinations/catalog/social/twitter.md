---
title: Twitter カスタムオーディエンス接続
description: Twitterで既存のフォロワーや顧客をターゲットにし、Adobe Experience Platform内に構築されたオーディエンスを活用して、関連性の高いリマーケティングキャンペーンを作成します
exl-id: fd244e58-cd94-4de7-81e4-c321eb673b65
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 33%

---

# [!DNL Twitter Custom Audiences] 接続

## 概要 {#overview}

Twitterの既存のフォロワーや顧客をターゲットにして、[!DNL Adobe Experience Platform]内に構築されたオーディエンスをアクティブ化することで、関連性の高いリマーケティングキャンペーンを作成します。

## 前提条件 {#prerequisites}

[!DNL Twitter Custom Audiences]宛先を設定する前に、必ず満たす必要がある次のTwitterの前提条件を確認してください。

1. お客様の[!DNL Twitter Ads] アカウントは広告対象である必要があります。 新しい[!DNL Twitter Ads] アカウントは、作成後の最初の2週間は広告の対象にはなりません。
2. [!DNL Twitter Audience Manager]でアクセスを許可したTwitter ユーザーアカウントでは、*[!DNL Partner Audience Manager]*&#x200B;権限が有効になっている必要があります。

## サポートされている ID {#supported-identities}

[!DNL Twitter Custom Audiences]は、次の表に示すIDのアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html#getting-started) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| device_id | IDFA/AdID/Android ID | Google Advertising ID （GAID）とApple ID for Advertisers （IDFA）は、[!DNL Adobe Experience Platform]でサポートされています。 宛先アクティベーションワークフローの[ マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping)に従って、ソーススキーマからこれらの名前空間や属性をマッピングしてください。 |
| メール | ユーザーの電子メールアドレス | プレーンテキストのメールアドレスと、SHA256でハッシュ化されたメールアドレスをこのフィールドにマッピングしてください。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 顧客の電子メールアドレスを[!DNL Adobe Experience Platform]にアップロードする前にハッシュ化する場合は、これらのIDは塩を使用せずにSHA256を使用してハッシュ化する必要があります。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | × | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

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
| 書き出しタイプ | **[!UICONTROL Audience export]** | Twitter カスタムオーディエンスの宛先で使用されている識別子を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## ユースケース {#use-cases}

[!DNL Twitter Custom Audiences]宛先を使用する方法とタイミングをより理解しやすくするために、[!DNL Adobe Experience Platform]のお客様がこの宛先を使用して解決できるユースケースの例を次に示します。

### ユースケース 1 {#use-case-1}

Twitterで[!DNL Adobe Experience Platform]内に[!DNL List Custom Audiences]として構築したオーディエンスをアクティブ化することで、Twitterで既存のフォロワーや顧客をターゲットにし、関連性の高いリマーケティングキャンペーンを作成します。

## 宛先に接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

1. 宛先カタログで[!DNL Twitter Custom Audiences]の宛先を検索し、**[!UICONTROL Set Up]**&#x200B;を選択します。
2. **[!UICONTROL Connect to destination]** を選択します。
   ![LinkedInに対する認証](/help/destinations/assets/catalog/social/twitter/authenticate-twitter-destination.png)
3. Twitterの資格情報を入力し、**ログイン**&#x200B;を選択します。

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_twitter_accountid"
>title="アカウント ID"
>abstract="Twitter 広告アカウント ID。これは、Twitter 広告設定で確認できます。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。
* **[!UICONTROL Account ID]**: [!DNL Twitter Ads] アカウント ID。 これは、[!DNL Twitter Ads]の設定で確認できます。

>[!IMPORTANT]
>
>特殊文字（+ &amp;、% : ; @ / = ?）は使用しないでください $ \n）をオーディエンス、説明、およびオーディエンスマッピング名に使用します。 Experience Platform オーディエンス名にこれらの文字が含まれている場合は、オーディエンスをTwitterの宛先にマッピングする前に、これらの文字を削除してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![ ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### マッピングに関する考慮事項 {#mapping-considerations}

オーディエンスをTwitterにマッピングする場合は、人間が読み取れるオーディエンスマッピング名を指定します。 Experience Platform セグメントに使用したのと同じ名前を使用することをお勧めします。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。

## その他のリソース {#additional-resources}

Twitterの[!DNL List Custom Audiences]の詳細については、[Twitter ドキュメント ](https://business.twitter.com/en/help/campaign-setup/campaign-targeting/custom-audiences/lists.html)を参照してください。
