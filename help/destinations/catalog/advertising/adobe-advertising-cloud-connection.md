---
title: Adobe Advertising DSP接続
description: 複数の ID タイプを使用して、認証済みおよび未認証のファーストパーティオーディエンスを Adobe Advertising Cloud Demand-Side Platform（DSP）と共有する方法について説明します。
feature: Destinations
source-git-commit: 5513e95637c1016caeb6abe699e1807cc234ed40
workflow-type: tm+mt
source-wordcount: '1342'
ht-degree: 16%

---


# Adobe Advertising DSP接続

## 概要 {#overview}

Adobe Advertising Cloud Demand-Side Platform（DSP）の宛先を使用すると、認証済みオーディエンスと未認証のファーストパーティオーディエンスの両方を、DSP アカウントまたはアカウント内の特定の広告主と共有できます。

この宛先を使用すると、お客様は、次の ID の一部またはすべてを使用してファーストパーティオーディエンスを共有できます。

* ハッシュ化されたメール ID。DSPでターゲティングするために [!DNL LiveRamp RampID] または [!DNL Unified ID 2.0] （UID2.0）に変換されます

* Experience Cloud ID （ECID）およびAdobe Advertisingのサードパーティ cookie

* モバイル広告 ID （MAID）:

   * [!DNL Google] デバイス用の [!DNL Android] Advertising ID （GAID）

   * [!DNL Apple iOS] デバイスの広告主（IDFA）の識別子

この接続は、ハッシュ化されたメールアドレスのみをサポートする [&#x200B; 従来の Adobe Advertising Cloud DSP 接続 &#x200B;](adobe-advertising-cloud-connection-legacy.md) に代わるものです。

>[!IMPORTANT]
>
>このページは、Adobe Advertising [!DNL DSP] チームによって作成されました。 お問い合わせや更新のリクエストについては、Advertising Cloud サポート （`adcloud_support@adobe.com`）にお問い合わせください。

## ユースケース {#use-cases}

この宛先を使用すると、広告主は、cookie があるブラウザーとないブラウザーをまたいでオーディエンスにリーチできます。

広告主は、認証済みのファーストパーティ識別子（[!DNL RampID] や [!DNL UID2.0] など）または未認証の ID （Cookie や MAID など）でセグメントを共有する選択ができます。

## 前提条件 {#prerequisites}

* [!DNL RampID activation] しくは、アカウントレベルとキャンペーンレベルの設定を [!DNL DSP] 定して、[!DNL LiveRamp RampID] とのオーディエンス共有を有効にします。これにより、顧客データが [!DNL RampIDs] に変換され、ターゲティング可能なセグメントが作成されます。 Adobe アカウントチームがこの設定を実行します。 [!DNL RampID] は、[!DNL DSP] と [!DNL LiveRamp] の間のパートナーシップを通じて利用でき、独自の [!DNL LiveRamp] メンバーシップを使用する必要はありません。

* オーディエンス ID:

   * [!DNL RampID] および [!DNL UID2.0] の場合、プロファイルには、ハッシュ化されたメール ID が含まれている必要があります。

   * Cookie の場合、[!DNL Web SDK] データストリームまたは [!DNL Experience Cloud ID Service] で cookie 同期プロセスを設定します。 以下の [Cookie を共有するための ID 同期の設定 &#x200B;](#cookie-sync) を参照してください。

   * MAID を持つプロファイルの場合：

      * 各 GAID に対して、IdentityMap 列に値 `GAID` を含めます。

      * 各 IDFA に対して、IdentityMap 列に `IDFA` 値を含めます。

* Experience Platform アカウントのExperience Cloud組織 ID。 お使いの ID は、Adobe Real-Time Customer Data Platform（Real-Time CDP）のユーザープロファイルページで確認できます。

* Campaign アクティベーション用のオーディエンスを受け取る [DSPの &#x200B;](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html)Real-Time CDP ソース」。 Adobe アカウントチームは、Experience Cloud組織 ID を使用してソースを作成します。

* [!DNL DSP] アカウントまたは広告主のソースキー。[Real-Time CDP ソースが  [!DNL DSP]](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html) で作成されたときに生成されます。 [!DNL DSP] アカウントチームがこのキーを共有します。 以下に説明するように、Experience Platform内でこれを使用して、Advertising Cloud DSP の宛先への宛先接続を作成します。

### Cookie を共有するための ID 同期の設定 {#cookie-sync}

ID 同期は、サードパーティ cookie を共有するための前提条件です。 [!DNL Web SDK] データストリームまたは [!DNL Experience Cloud ID Service] ークフローを使用して、cookie 同期プロセスを設定します。 サードパーティ cookie の ID 処理について詳しくは、[&#x200B; サードパーティ cookie の統合に基づくAdvertisingの宛先 &#x200B;](/help/destinations/how-destinations-work/identity-handling.md#third-party-cookie-destinations) を参照してください。

**[!DNL Web SDK]** とのサードパーティ ID 同期を有効にする

[!DNL Experience Platform Web SDK] を使用している場合は、詳細設定で [!UICONTROL Third Party ID Sync] オプションを設定して、データストリームでサードパーティ ID の同期を有効にします。 手順については、データストリームに関するドキュメントの [&#x200B; 詳細オプションの設定 &#x200B;](/help/datastreams/configure.md#advanced-options) を参照してください。

**[!DNL Experience Cloud ID Service]** とのサードパーティ ID 同期を有効にする

[!DNL Experience Platform] で [!DNL Experience Cloud ID Service] タグを使用している場合は、[Experience Cloud ID サービス拡張機能 &#x200B;](/help/tags/extensions/client/id-service/overview.md) を使用してサードパーティ ID 同期を設定します。 これにより、Real-Time CDPからオーディエンスをアクティベートする際に、指定された ECID に一致したAdobe Advertising Cookie を使用できるようになります。

## サポートされている ID {#supported-identities}

Adobe Advertising Cloud DSP 宛先は、以下の表で説明する ID のアクティベーションをサポートします。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
| --------------- | ----------- | -------------- |
| `email_lc_sha256` | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Experience Platformは、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方をサポートしています。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にExperience Platformでデータを自動的にハッシュ化するように設定します。 |
| `ECID` | Experience Cloudのファーストパーティ cookie | cookie ベースのセグメントを作成する際に必要です。 |
| `Everesttech cookie` | Adobe Advertisingのサードパーティ Cookie | cookie ベースのセグメントを作成する際に必要です。 |
| `GAID` | [!DNL Android] デバイス ID | [!DNL Android] デバイスのターゲティングに必要。 |
| `IDFA` | [!DNL iOS] デバイス ID | [!DNL iOS] デバイスのターゲティングに必要。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの接触チャネル | ○ | このカテゴリには、[!DNL Segmentation Service] を通じて生成されたオーディエンス以外のすべてのオーディエンスの接触チャネルが含まれます。 [&#x200B; 様々なオーディエンスのオリジン &#x200B;](/help/segmentation/ui/audience-portal.md#customize) について確認する。 次に例を示します。 <ul><li> csv ファイルからExperience Platformへのカスタムアップロードオーディエンス [&#x200B; 読み込み &#x200B;](../../../segmentation/ui/audience-portal.md#import-audience)</li><li> 類似オーディエンス、 </li><li> 連合オーディエンス、 </li><li> Adobe Journey Optimizerなど、他のExperience Platform アプリで生成されたオーディエンス。 </li><li> その他。 </li></ul> |

{style="table-layout:auto"}

オーディエンスデータタイプでサポートされるオーディエンス：

| オーディエンスデータタイプ | サポートあり | 説明 | ユースケース |
| -------------------- | --------- | ----------- | --------- |
| [&#x200B; 人物オーディエンス &#x200B;](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルに基づき、マーケティングキャンペーンの対象となる人物のグループを指定できます。 | 頻繁な購入、買い物かごの放棄 |
| [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) | × | アカウントベースのマーケティング戦略では、特定の組織内の個人をターゲットに設定します。 | B2B マーケティング |
| [&#x200B; 見込み客オーディエンス &#x200B;](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないものの、ターゲットオーディエンスと特性を共有する個人をターゲットに設定します。 | サードパーティデータを使用した予測 |
| [&#x200B; データセットの書き出し &#x200B;](/help/catalog/datasets/overview.md) | × | Adobe Experience Platform Data Lake に保存された構造化データのコレクション。 | レポート、データサイエンスワークフロー |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | メモ |
| ---- | ---- | ----- |
| 書き出しタイプ | **[!UICONTROL Audience export]** | 選択した識別子でオーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンスの評価に基づいてExperience Platform内でプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、Experience Platformの **[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

宛先に接続するには、Experience Platform ユーザーインターフェイスを使用して [&#x200B; 宛先接続の作成 &#x200B;](/help/destinations/ui/connect-destination.md) の手順に従います。 宛先設定ワークフローで、以下のサブセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に接続するには、「[!UICONTROL Connection type]」セクションで次のパラメーターを指定し、「**[!UICONTROL Connect to destination]**」を選択します。

* **[!UICONTROL Account or Advertiser Key]**：この [!UICONTROL Source Key] は、[Real-Time CDP ソースがDSP ユーザーインターフェイス &#x200B;](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html) で作成されたときに生成されます。 Adobe アカウントチームがソースを作成すると、このキーが共有されます。

![&#x200B; 「アカウント」または「広告主キー」フィールドを示す「接続タイプ」セクションのスクリーンショット。](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/authenticate-destination.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。

![&#x200B; 名前と説明の入力を示す宛先詳細フィールドのスクリーンショット。](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/destination-details.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* ID を書き出すには、**[!UICONTROL View Identity Graph]** アクセス制御権限 [&#x200B; が必要 &#x200B;](/help/access-control/home.md#permissions) す。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

Adobe Advertising DSPに送信する ID を選択できます。 デフォルトでは、広告主の cookie 識別子が選択されます。 [!UICONTROL Hashed Email]、[!UICONTROL IDFA]、[!UICONTROL GAID] の追加も選択できます。

手順については、[&#x200B; 属性と ID のマッピング &#x200B;](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) を参照してください。

![Cookie 識別子、ハッシュ化されたメール、IDFA および GAID の各オプションを示す、ID マッピングセクションのスクリーンショット。](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/identity-mapping.png)

## データの書き出しを検証する {#exported-data}

オーディエンスデータが Advertising Cloud と共有されたことを確認するには、次の点を確認します。

* [!DNL Real-Time CDP] 宛先のデータフローが成功しました。

* DSPでは、オーディエンスは、**[!UICONTROL Audiences]**/**[!UICONTROL All Audiences]** またはプレースメント設定の **[!UICONTROL Audience Targeting]** セクション内からオーディエンスを作成または編集する際に使用できます。 オーディエンスは、[!UICONTROL Adobe Segments] フォルダーの下の「[!UICONTROL Real-Time CDP]」タブに表示されます。

![DSP オーディエンス設定のReal-Time CDP オーディエンス &#x200B;](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/segments-in-dsp.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[&#x200B; データガバナンスの概要 &#x200B;](/help/data-governance/home.md) を参照してください。
