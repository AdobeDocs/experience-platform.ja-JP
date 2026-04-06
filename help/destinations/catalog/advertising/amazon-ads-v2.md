---
title: Amazon Ads v2
description: Amazon Ads v2は、登録販売者、ベンダー、ブックベンダー、Kindle Direct Publishing （KDP）の作成者、アプリ開発者、代理店などに対して、広告目標の達成に役立つ様々なオプションを提供します。 Amazon Ads v2とAdobe Experience Platformの連携により、Amazon Ads製品を簡単に連携できます。
last-substantial-update: 2026-03-31T00:00:00Z
source-git-commit: 1e93c78b13159a2aed24d283e3768c670ad14097
workflow-type: tm+mt
source-wordcount: '1667'
ht-degree: 16%

---

# Amazon Ads v2との連携 {#amazon-ads-v2}

## 概要 {#overview}

[!DNL Amazon Ads v2]を使用すると、広告主は[!DNL Amazon Ads]製品にわたってオーディエンスデータを効率的に取り込み、管理、有効化、再利用できます。

>[!IMPORTANT]
>
>[!DNL Amazon Ads v2]は、すべての新しい[!DNL Amazon Ads]接続の現在の宛先です。 既存の[&#x200B; （レガシー）  [!DNL Amazon Ads]](./amazon-ads.md)接続がある場合、必要な変更を加えずに引き続き機能します。 [!DNL Amazon Ads v2]は[!DNL Ads Data Manager]に接続します。これにより、[!DNL Amazon Ads]製品でのID タイプの拡張、アドレス関連フィールド、データ共有がサポートされ、[&#x200B; （レガシー）  [!DNL Amazon Ads]](./amazon-ads.md)と比較して、ターゲティングとオーディエンスの一致率が向上します。
>
>2026年4月末以降、[!DNL Amazon Ads v2]の名前は[!DNL Amazon Ads]に変更され、レガシーカードは非表示になり、カタログに1つの宛先カードが残ります。 既存のレガシーデータフローは引き続き機能し、その日付以降は&#x200B;**[!UICONTROL Browse]** タブで管理できます。

[!DNL Amazon Ads v2]と[!DNL Adobe Experience Platform]の統合は、オーディエンスメンバーを[!DNL Amazon Ads]に取り込むための直接接続を提供します。 アップロードされたオーディエンスは、[!DNL Ads Data Manager (ADM)]内の[!DNL Amazon Ads] コンソールで利用できます。 [!DNL Ads Data Manager] コンソールを使用して、様々な[!DNL Amazon Ads]製品のデータを共有できます。

[!DNL Ads Data Manager]の詳細については、次を参照してください。

* [Ads Data Manager - コンソールの概要](https://advertising.amazon.com/API/docs/en-us/adm/1_ads-data-manager-console-overview)
* [Ads Data Manager コンソールの使用](https://advertising.amazon.com/API/docs/en-us/adm/2_ads-data-manager-console)
* [Ads Data Managerのアカウント設定](https://advertising.amazon.com/API/docs/en-us/adm/2a_ads-data-manager_account_setup)

>[!IMPORTANT]
>
>この宛先コネクタとドキュメント ページは、*[!DNL Amazon Ads]* チームによって作成および管理されています。 問い合わせや更新のリクエストについては、*`amc-support@amazon.com`.*&#x200B;から直接連絡してください。

## ユースケース {#use-cases}

[!DNL Amazon Ads v2]宛先を使用する方法とタイミングをより理解しやすくするために、[!DNL Adobe Experience Platform]のお客様がこの宛先を使用して解決できるユースケースの例を次に示します。

### オーディエンスの取り込みとアクティベーション {#activation-and-targeting}

スポーツ衣料ブランドが、[!DNL Amazon Ads]をまたいで関連性の高い広告を既存顧客に配信したいと考えています。 企業は、顧客のメールアドレスをCRMから[!DNL Adobe Experience Platform]に取り込み、ファーストパーティのオフラインデータを使用してオーディエンスを構築し、これらのオーディエンスを[!DNL Amazon Ads]宛先を通じて[!DNL Amazon Ads v2]にアクティブ化できます。 アクティベーション後は、これらのオーディエンスを利用して、[!DNL Amazon Ads]のインベントリ全体でそれらの顧客に広告をターゲティングできるため、ブランドは既知の顧客と再エンゲージし、リピート購入を促進することができます。 詳しくは、[&#x200B; データの管理](https://advertising.amazon.com/API/docs/en-us/adm/6_adm-manage-data)を参照してください。

## 前提条件 {#prerequisites}

[!DNL Amazon Ads v2]との[!DNL Adobe Experience Platform]接続を使用するには、**[!DNL Amazon Ads Data Manager]** マネージャーアカウント [を使用して](https://advertising.amazon.com/help/G69CDSR9MNSWJH95)へのアクセス権が必要です。 詳しくは、[Amazon Ads Data Managerの基本を学ぶ](https://advertising.amazon.com/API/docs/en-us/adm/1_ads-data-manager-console-overview)を参照してください。

### Amazon Ads Data Managerの利用条件 {#accept-terms}

[!DNL Amazon Ads v2]宛先を設定する前に、[!DNL Amazon Ads] アカウントにログインし、[!DNL Ads Data Manager]の利用条件に同意してください。 [!DNL Ads Data Manager]内の[!DNL Amazon Ads] コンソールに移動し、プロンプトが表示されたら条件に同意します。 利用条件に同意しない場合、[!DNL Amazon Ads]にオーディエンスは作成されません。

## サポートされている ID {#supported-identities}

[!DNL Amazon Ads v2]宛先は、次のIDのアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| `phone` | SHA256 アルゴリズムでハッシュ化された電話番号 | プレーンテキストとSHA256 ハッシュ化された電話番号の両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| `email` | SHA256 アルゴリズムでハッシュ化された電子メールアドレス（小文字） | プレーンテキストとSHA256 ハッシュ化された電子メールアドレスの両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| `firstname` | ユーザーの名 | プレーンテキストとSHA256 ハッシュ化された名前の両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| `lastname` | ユーザーの姓 | プレーンテキストとSHA256 ハッシュ化された姓の両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| `address` | ユーザーの住所 | プレーンテキストとSHA256 ハッシュ化された通りの両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| `city` | ユーザーの市区町村 | プレーンテキストとSHA256 ハッシュ化された都市の両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| `state` | ユーザーの都道府県 | プレーンテキストとSHA256 ハッシュ化された状態の両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| `zip` | ユーザーの郵便番号 | プレーンテキストとSHA256 ハッシュ化されたzip ファイルの両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| `countryCode` | ユーザーの国（2文字のISO コード） | プレーンテキスト入力をサポートしています。 |
| `experianId` | [!DNL Experian]によって割り当てられた識別子 | プレーンテキスト入力をサポートしています。 |
| `kantarId` | [!DNL Kantar]によって割り当てられた識別子 | プレーンテキスト入力をサポートしています。 |
| `liveRampId` | [!DNL LiveRamp]によって割り当てられた識別子 | プレーンテキスト入力をサポートしています。 |
| `maId` | モバイルアプリケーションによって割り当てられた識別子 | プレーンテキスト入力をサポートしています。 |
| `merkleId` | [!DNL Merkle]によって割り当てられた識別子 | プレーンテキスト入力をサポートしています。 |
| `neustarId` | [!DNL Neustar]によって割り当てられた識別子 | プレーンテキスト入力をサポートしています。 |
| `realId` | Real ID ID ID グラフによって割り当てられた識別子 | プレーンテキスト入力をサポートしています。 |
| `sambaTvId` | [!DNL Samba TV]によって割り当てられた識別子 | プレーンテキスト入力をサポートしています。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | [!DNL Experience Platform] [&#x200B; セグメント化サービス &#x200B;](/help/segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルから](/help/segmentation/ui/audience-portal.md#import-audience)に[!DNL Experience Platform]をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Experience Platform]などの他の[!DNL Adobe Journey Optimizer] アプリで生成されたオーディエンス， </li><li> その他。 </li></ul> |

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

次の表に、宛先の書き出しタイプと頻度を示します。

| 項目 | タイプ | メモ |
| ---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | [!DNL Amazon Ads]がサポートする識別子を使用して、オーディエンスのすべてのメンバーを書き出しています。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。[!DNL Experience Platform]のオーディエンスの更新はすぐに[!DNL Ads Data Manager]に送信されます。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](/help/destinations/ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証を行うには、必須フィールドに入力し、**[!UICONTROL Connect to destination]**&#x200B;を選択します。

* **[!UICONTROL Account name]**：この宛先アカウントを識別するのに役立つ名前を入力します。 これは、同じ宛先に複数の接続がある場合に特に便利です。
* **[!UICONTROL Description]** （オプション）：接続の目的や関連するビジネス コンテキストなど、アカウント間の区別に役立つ詳細を追加します。

![Amazon Ads用Experience Platformの宛先に接続ダイアログ &#x200B;](../../assets/catalog/advertising/amazon-ads/amazon-ads-v2-connect-to-destination.png)

[!DNL Amazon Ads v2] インターフェイスにリダイレクトされます。 **[!UICONTROL Allow]**&#x200B;を選択して、Amazon アカウントにログインします。

![Amazon Ads OAuth認証プロンプトがユーザーに許可を求めています](../../assets/catalog/advertising/amazon-ads/amazon-ads-v2-allow.png)

認証後、新しい接続で[!DNL Adobe Experience Platform]にリダイレクトされます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![Experience PlatformのAmazon Ads v2宛先設定フィールド &#x200B;](../../assets/catalog/advertising/amazon-ads/amazon-ads-v2-configure-destination.png)

* **[!UICONTROL Name]**：この宛先を識別する名前。
* **[!UICONTROL Description]**：この宛先の特定に役立つ説明です。
* **[!UICONTROL Manager Account]**: ドロップダウンからのターゲットマネージャーアカウント ID。
* **[!UICONTROL All audience members sent to Amazon are consented for use for Advertising]**: データ使用の同意を指定します（`GRANTED`または`DENIED`）。
* **[!UICONTROL Ads data manager Terms & Conditions]**: [!DNL Amazon Ads] Data Managerの利用条件に同意します。 詳しくは、[利用規約に同意](#accept-terms) セクションを参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](/help/destinations/ui/alerts.md)に関するガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。
>* IDをエクスポートするには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![&#x200B; ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 必須マッピング {#map}

[!DNL Amazon Ads v2]宛先では、データのアクティブ化を成功させるために次のマッピングを設定する必要があります。

| ソースフィールド | ターゲットフィールド | 説明 |
|---------|----------|---------|
| `IdentityMap: Email_LC_SHA256` または `IdentityMap: Email` | `Identity: email` | ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| `xdm: homeAddress.countryCode` | `Identity: countryCode` | ユーザーの国（2文字のISO コード） |

![Amazon Ads v2宛先のID フィールドマッピング設定](../../assets/catalog/advertising/amazon-ads/amazon-ads-v2-mapping.png)

### マッピングのベストプラクティス {#mapping-best-practices}

ファーストパーティのID （電話番号や住所など）を、パートナーが提供するIDと組み合わせます。 これにより、[!DNL Amazon Ads]はオーディエンスの照合中に複数のID シグナルを使用できるようになり、照合率が向上しました。

パートナーが提供するIDは、ソースデータに入力された場合にのみ使用します。 マッピングされたパートナーID フィールドが空であるか、特定のプロファイルに存在しない場合、オーディエンスの照合中に無視され、一致率に影響しません。

### 例 {#examples}

* `kantarId`ID データを使用して構築または強化されたオーディエンスをアクティブ化する場合は、[!DNL Kantar]を使用します。
* オーディエンスデータが`merkleId`で管理されているID ソリューションから送信される場合は、[!DNL Merkle]を使用します。
* データが`neustarId`のID解決を通じてリンクされている場合は、[!DNL Neustar]を使用します。
* `experianId`のID データを使用して強化されたオーディエンスに[!DNL Experian]を使用します。
* `liveRampId`のID解決に依存するオーディエンスをアクティブ化する場合は、[!DNL LiveRamp]を使用します。
* `sambaTvId`が提供するオーディエンスデータを操作する場合は、[!DNL Samba TV]を使用します。

これらの識別子は、通常、それぞれのパートナーによってプレーンテキスト識別子として提供され、ハッシュ化は必要ありません。

## データの書き出しを検証する {#exported-data}

アクティブ化後、**[!DNL Ads Data Manager]コンソール**&#x200B;でオーディエンスの取り込みを検証します。

**[!UICONTROL Audiences]** → **[!UICONTROL Uploaded Sources]**&#x200B;に移動します。 オーディエンスの取り込みステータス、サイズ、エラーログを確認します。 [&#x200B; ドキュメントの](https://advertising.amazon.com/API/docs/en-us/adm/6_adm-manage-data) データの管理[および](https://advertising.amazon.com/API/docs/en-us/adm/7_adm-destinations)宛先[!DNL Amazon Ads] ページでは、さらに検証ガイダンスが提供されています。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

[!DNL Amazon Ads Data Manager]の詳細については、次のリソースを参照してください。

* [Amazon Ads Data Managerの概要](https://advertising.amazon.com/API/docs/en-us/adm/1_ads-data-manager-console-overview)
