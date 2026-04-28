---
title: Rokt
description: Adobe Experience PlatformのオーディエンスをRoktに接続し、よりスマートなターゲティング、抑制、パーソナライズを通じてキャンペーンのパフォーマンスを向上させる方法をご紹介します。
source-git-commit: a281a7c961b8576105913feb7a7f8258c975e875
workflow-type: tm+mt
source-wordcount: '1235'
ht-degree: 25%

---


# [!DNL Rokt] 接続 {#rokt-destination}

## 概要 {#overview}

[[!DNL Rokt]](https://www.rokt.com)は、AIを活用したリアルタイムの意思決定により、e コマースの価値を引き出し、各トランザクションモーメントをより適切™ものにします。 パーソナライズされた体験を提供し、広告主と購買意欲の高い顧客をつなぎます。 [!DNL Adobe Experience Platform]人のオーディエンスを[!DNL Rokt]に接続し、よりスマートなターゲティング、抑制、パーソナライズを通じてキャンペーンのパフォーマンスを向上させます。 適切な顧客にタイミングよくリーチし、無駄な支出を削減。

>[!IMPORTANT]
>
>宛先コネクタとドキュメント ページは、[!DNL Rokt] チームによって作成および管理されます。 問い合わせや更新のリクエストについては、[!DNL Rokt] アカウントマネージャーにお問い合わせいただくか、`support@rokt.com`にお問い合わせください。

## ユースケース {#use-cases}

次のユースケースは、[!DNL Experience Platform]人の顧客が[!DNL Rokt]宛先をどのように使用できるかを示しています。

### ユースケース #1：リターゲティング {#use-case-1}

サイトやアプリにアクセスしたものの、コンバージョンに至らなかった優良顧客とリエンゲージメントする。 特定の製品カテゴリを閲覧したユーザーやチェックアウトフローを放棄したユーザーを含む、[!DNL Experience Platform]でオーディエンスを構築します。 その後、そのオーディエンスを[!DNL Rokt]にプッシュし、パートナーサイトで購入時にパーソナライズされたオファーを提供します。 [!DNL Rokt]は、顧客が別の場所で購入を完了した直後に、トランザクションの瞬間に動作します。 リターゲティングされたオーディエンスは、購入意思がピークに達した時点でリーチされるため、従来のディスプレイリターゲティングよりもコンバージョン率が高くなります。

### ユースケース #2：抑制リスト {#use-case-2}

特定の[!DNL Rokt]件のオファーを受け取るべきではないオーディエンスを除外することで、無駄な支出や無関係なエクスペリエンスを防ぎます。 抑制の一般的なユースケースには、最近のコンバーター、アクティブなプロモーションのロイヤルティメンバー、マーケティングをオプトアウトしたユーザーを除外することが含まれます。 例えば、過去30日以内に購入した顧客を除外できます。 これらの抑制オーディエンスを[!DNL Experience Platform]から[!DNL Rokt]までリアルタイムで同期します。 これにより、施策の焦点は、新規ユーザーや再エンゲージ可能なユーザーに絞ることができます。 これにより、ROIが向上し、顧客体験を保護できます。

## 前提条件 {#prerequisites}

[!DNL Adobe Experience Platform]で[!DNL Rokt]宛先を設定する前に、**[!DNL Rokt]アカウント マネージャー**&#x200B;から次の資格情報を取得する必要があります。

* **API キー**: [宛先接続を認証する](#authenticate)際に、これを&#x200B;**[!UICONTROL Username]**&#x200B;として使用します。
* **API シークレット**: [宛先接続を認証する](#authenticate)際に、これを&#x200B;**[!UICONTROL Password]**&#x200B;として使用します。

[!DNL Rokt] アカウント マネージャーは、セットアップの前に、これらの資格情報を[!DNL Rokt] プラットフォームでプロビジョニングします。 まだ受け取っていない場合は、アカウントマネージャーにお問い合わせください。

## サポートされている ID {#supported-identities}

[!DNL Rokt] では、以下の表で説明する ID のアクティベーションをサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | プレーンテキストのメールアドレス | 推奨： [!DNL Rokt]でのプロファイルの照合に使用されます。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | プレーンテキストとSHA256 ハッシュ化されたメールアドレスの両方がサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションを選択すると、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| 電話 | プレーンテキストの電話番号 | [!DNL Rokt]でのプロファイルの照合に使用されます。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | プレーンテキストとSHA256 ハッシュ化された電話番号の両方がサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションを選択すると、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| GAID | [!DNL Google] Advertising ID | ソース IDがGAID名前空間である場合は、GAID ターゲット IDを選択します。 |
| IDFA | 広告主向けの[!DNL Apple] ID | ソース IDがIDFA名前空間の場合は、IDFA ターゲット IDを選択します。 |
| aepProfileId | [!DNL Adobe Experience Platform] プロファイル ID | プロファイル ID （`xdm:_id`）をフォールバック IDとしてマッピングします。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | [!DNL Experience Platform] [[!DNL Segmentation Service]](/help/segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルから[!DNL Experience Platform]に](/help/segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他の[!DNL Experience Platform] アプリで生成されたオーディエンス， </li><li> その他。 </li></ul> |

{style="table-layout:auto"}

オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス &#x200B;](/help/segmentation/types/people-audiences.md) | ○ | 最適な属性を選択できます。 これを使用して、マーケティングキャンペーンの特定のグループをターゲティングします。 | 買い物客やカートの放棄が多い |
| [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス &#x200B;](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [&#x200B; データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | [!DNL Rokt]宛先で使用されている識別子（電子メール、電話、モバイル広告IDなど）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。 オーディエンス評価に基づいて[!DNL Experience Platform]でプロファイルが更新されるとすぐに、コネクターは更新をダウンストリームの[!DNL Rokt]に送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](/help/destinations/ui/connect-destination.md)の手順に従ってください。 宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証を行うには、必須フィールドに入力し、**[!UICONTROL Connect to destination]**&#x200B;を選択します。

* **[!UICONTROL Username]**: [!DNL Rokt] アカウントマネージャーから提供されたAPI キー。
* **[!UICONTROL Password]**: [!DNL Rokt] アカウントマネージャーから提供されたAPI シークレット。

  ![&#x200B; アカウントの詳細、認証フィールド、宛先の詳細が入力された[!DNL Experience Platform]の[!DNL Rokt]宛先設定画面。](/help/destinations/assets/catalog/advertising/rokt/aep-configure-destination.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。 UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前（「[!DNL Rokt] - リターゲティングオーディエンス」など）。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。 アラートについて詳しくは、[UI を使用した宛先アラートの購読](/help/destinations/ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![&#x200B; ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

[!DNL Rokt]宛先は、[!DNL Experience Platform]から[!DNL Rokt]個のID フィールドへのID名前空間のマッピングをサポートしています。 オーディエンスを正常にアクティブ化するには、少なくとも1つのIDをマッピングする必要があります。 推奨されるマッピングを以下の表に示します。

| ソースフィールド | ターゲットフィールド | 注意点 |
|---|---|---|
| `IdentityMap: Email` | `Identity: email` | 推奨 |
| `IdentityMap: Email_LC_SHA256` | `Identity: emailSha256` | 推奨 |
| `IdentityMap: Phone` | `Identity: phone` | オプション |
| `IdentityMap: Phone_SHA256` | `Identity: phoneSha256` | オプション |
| `IdentityMap: GAID` | `Identity: gaid` | オプション |
| `IdentityMap: IDFA` | `Identity: idfa` | オプション |
| `xdm: _id` | `Identity: aepProfileId` | オプション |

{style="table-layout:auto"}

フルマッピングの例を次に示します。

![&#x200B; ソースとターゲット ID フィールドが設定された[!DNL Experience Platform]の[!DNL Rokt]宛先アクティベーション ワークフローのマッピング ステップ。](/help/destinations/assets/catalog/advertising/rokt/aep-identity-mapping.png)

>[!NOTE]
>
>[!DNL Rokt]で一致率を最大化するには、少なくとも1つの電子メールベース ID マッピング （`email`または`emailSha256`）を強くお勧めします。

### オーディエンススケジュールの設定 {#audience-schedule}

マッピング手順が完了したら、選択した各オーディエンスのオーディエンススケジュールを設定します。 オーディエンスの同期を開始するタイミングの&#x200B;**[!UICONTROL Start date]**&#x200B;と、**[!UICONTROL Mapping ID]** （[!DNL Rokt]内でこのオーディエンスを識別するために使用されるラベル）を指定します。 [!DNL Experience Platform] オーディエンス名または、ユーザーと[!DNL Rokt] アカウントマネージャーがオーディエンスを識別するのに役立つ任意の説明文字列を使用できます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

* [開発者用ドキュメント [!DNL Rokt]件](https://docs.rokt.com)
* [Adobe Experience Platformの宛先の概要](/help/destinations/home.md)
