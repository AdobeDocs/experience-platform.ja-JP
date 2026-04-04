---
title: RainFocus参加者プロファイル
description: RainFocus Attendee Profiles destination connectorを使用して、オーディエンスプロファイルをRainFocus Global Attendee Profileと同期する方法を説明します。
last-substantial-update: 2024-12-17T00:00:00Z
exl-id: 27c3848c-411a-4305-a5d5-00b145b95287
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1073'
ht-degree: 30%

---

# RainFocus参加者プロファイル {#rainfocus-destination}

## 概要 {#overview}

[!DNL RainFocus Attendee Profiles]宛先を使用して、[!DNL Adobe Experience Platform]から[!DNL RainFocus] プラットフォームに顧客プロファイルをストリーミングし、参加者プロファイルを作成および更新します。

>[!IMPORTANT]
>
>宛先コネクタとドキュメント ページは、[!DNL RainFocus] チームによって作成および管理されます。 お問い合わせやアップデートのリクエストについては、`clientcare@rainfocus.com`から直接お問い合わせいただくか、RainFocus [ ヘルプセンター](https://help.rainfocus.com/hc/en-us)をご覧ください。

## ユースケース {#use-cases}

RainFocusの宛先を使用する方法とタイミングをより正確に把握するために、この宛先を使用して[!DNL Adobe Experience Platform]のお客様が解決できるユースケースの例を次に示します。

### ユースケース #1 {#use-case-1}

ある大規模なエンタープライズテクノロジー企業が、今後の世界的な博覧会のオープン登録を予定しており、登録プロセスを合理化するために、顧客プロファイルを[!DNL RainFocus]にプッシュしたいと考えています。

### ユースケース #2 {#use-case-2}

金融機関は、新規顧客と既存顧客をターゲットとした一連のロードショーを主催する予定です。 [!DNL Adobe Experience Platform]のターゲット顧客に対する一連のオーディエンスセグメントがあります。 [!DNL RainFocus]宛先コネクタを使用すると、それらのプロファイルを簡単に[!DNL RainFocus]に送信してアクティブ化できます。

## 前提条件 {#prerequisites}

[!DNL RainFocus]宛先を使用する前に、次の前提条件を満たしていることを確認してください。

* OAuth （グローバル）を使用して[!DNL RainFocus] API プロファイルを作成します。
   * **参加者ストア** エンドポイントを有効にする必要があります。
   * **クライアント ID**&#x200B;と&#x200B;**クライアントシークレット**&#x200B;を生成する必要があります。

また、プロファイルをに送信するRainFocus **イベントコード**&#x200B;識別子も必要です。

## サポートされている ID {#supported-identities}

[!DNL RainFocus]は、次の表に示すIDのアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | プレーンテキストとSHA256 ハッシュ化された電子メールアドレスの両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |

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
| 書き出しタイプ | **[!UICONTROL Profile-based]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証を行うには、必須フィールドに入力し、**[!UICONTROL Connect to destination]**&#x200B;を選択します。

![RainFocus Destination Connector](/help/destinations/assets/catalog/marketing-automation/rainfocus/rainfocus-destination-authentication.png)の認証の詳細を提供する

* **[!UICONTROL Client ID]**: RainFocus API プロファイルが提供する[!DNL Client ID]を入力します。
* **[!UICONTROL Client secret]**: RainFocus API プロファイルが提供する[!DNL Client Secret]を入力します。
* **[!UICONTROL Environment]**：接続するRainFocus環境（例：`dev`、`prod`）を指定します。
* **[!UICONTROL Org ID]**: RainFocusのインスタンスに一意の`orgid`を指定します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![RainFocus Destination Connector](/help/destinations/assets/catalog/marketing-automation/rainfocus/rainfocus-configure-destination-details.png)に接続の詳細を提供する

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。
* **[!UICONTROL Event ID]**: プロファイルの送信先となる[!DNL RainFocus] イベントコード ID。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に対してオーディエンスをアクティブ化する手順については、[ ストリーミング宛先に対するオーディエンスのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

ユースケースに応じて、次のターゲット ID名前空間をマッピングする必要があります。

* **メール**&#x200B;は、**ターゲットフィールド/ID名前空間を選択/メール**&#x200B;を使用して、ターゲットフィールドとしてマッピングする必要があります

![ プロファイルとID フィールドのマッピング方法](/help/destinations/assets/catalog/marketing-automation/rainfocus/rainfocus-destination-mapping.png)

追加のプロファイルフィールドをマッピングすることをお勧めします。これにより、[!DNL RainFocus]の参加者プロファイルが完全に入力されます。 次のターゲットフィールドは、[!DNL RainFocus]から利用できます。

| ターゲットフィールド | 説明 |
|------------|-------------|
| `address1` | 住所の最初の行 |
| `address2` | 住所の2行目（該当する場合） |
| `city` | 市名 |
| `companyname` | 会社の名前 |
| `countryid` | 国のISO 3166-1 alpha-2国コード識別子 |
| `email` | メールアドレス |
| `firstname` | 人物の名前 |
| `lastname` | ユーザーの姓 |
| `jobtitle` | 人物の役職名 |
| `phone` | 電話番号 |
| `state` | 州または州のFIPS状態アルファコード |
| `zip` | 郵便番号または郵便番号 |

{style="table-layout:auto"}

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

プロファイルのセットが[!DNL RainFocus]に送信されたら、[!DNL RainFocus]でログインしているAPI プロファイルを使用して、プロファイルが正常に取り込まれたことを検証します。

![RainFocusのAPI プロファイルでログを表示](/help/destinations/assets/catalog/marketing-automation/rainfocus/rainfocus-destination-api-profile.png)

![ プロファイルが正常に取り込まれたことを検証します](/help/destinations/assets/catalog/marketing-automation/rainfocus/rainfocus-destination-api-logging.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

* [RainFocus ストリーミング Source コネクタ ](https://experienceleague.adobe.com/ja/docs/experience-platform/sources/connectors/analytics/rainfocus)
