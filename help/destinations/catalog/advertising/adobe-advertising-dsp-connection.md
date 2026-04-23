---
title: Adobe Advertising DSPとの連携
description: 複数のID タイプを使用して、認証済みファーストパーティオーディエンスと未認証のファーストパーティオーディエンスをAdobe Advertising Demand-Side Platform（DSP）で共有する方法について説明します。
feature: Destinations
exl-id: 0ff80d38-993f-4609-bf2a-01a3e6cfe10b
source-git-commit: 36871289743f384207bb149df6e5e1af14d4d371
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 16%

---

# Adobe Advertising DSPとの連携

## 概要 {#overview}

Adobe Advertising Demand-Side Platform（DSP）の宛先では、認証済みオーディエンスと未認証のファーストパーティオーディエンスの両方を、DSP アカウントまたはアカウント内の特定の広告主と共有できます。

この宛先を使用すると、顧客は次のいずれかのIDまたはIDと1st パーティオーディエンスを共有できます。

* DSPでのターゲティング用に[!DNL LiveRamp RampID]または[!DNL Unified ID 2.0] （UID2.0）に変換されるハッシュ化メール ID

* Experience Cloud ID （ECID）とAdobe Advertisingのサードパーティ Cookie

* モバイル広告ID （MAID）:

   * [!DNL Google] デバイスの[!DNL Android] Advertising ID （GAID）

   * [!DNL Apple iOS] デバイスの広告主（IDFA）の識別子

この接続は、ハッシュ化された電子メールアドレスのみをサポートする[従来のAdobe Advertising DSP接続](adobe-advertising-cloud-dsp-connection-legacy.md)に取って代わります。

>[!IMPORTANT]
>
>このページはAdobe Advertising [!DNL DSP] チームによって作成されました。 お問い合わせやアップデートのリクエストについては、`adcloud_support@adobe.com`から直接Advertising サポートにお問い合わせください。

## ユースケース {#use-cases}

この宛先により、広告主はCookieを使用せずに、ブラウザーをまたいでオーディエンスにリーチできます。

広告主は、認証済みのファーストパーティ ID （[!DNL RampID]や[!DNL UID2.0]など）または未認証ID （CookieやMAIDなど）でセグメントを共有することができます。

## 前提条件 {#prerequisites}

* [!DNL RampID activation]の場合、[!DNL DSP]のアカウントレベルおよびキャンペーンレベルの設定で[!DNL LiveRamp RampID]とのオーディエンス共有を有効にし、顧客データを[!DNL RampIDs]に変換してターゲットを絞ったセグメントを作成します。 Adobe アカウントチームがこの設定を実行します。 [!DNL RampID]は[!DNL DSP]と[!DNL LiveRamp]のパートナーシップを通じて利用でき、使用するために独自の[!DNL LiveRamp] メンバーシップは必要ありません。

* オーディエンス ID:

   * [!DNL RampID]および[!DNL UID2.0]の場合、プロファイルにはハッシュ化されたメール IDが含まれている必要があります。

   * Cookieの場合は、[!DNL Web SDK] データストリームまたは[!DNL Experience Cloud ID Service]のいずれかを使用してCookie同期プロセスを設定します。 以下の「[Cookieを共有するためのID同期の設定](#cookie-sync)」を参照してください。

   * MAIDを持つプロファイルの場合：

      * GAIDごとに、IdentityMap列に値`GAID`を含めます。

      * 各IDFAについて、IdentityMap列に値`IDFA`を含めます。

* Experience Platform アカウントのExperience Cloud Organization ID。 Adobe [!DNL Real-Time Customer Data Platform] （[!DNL Real-Time CDP]）のユーザープロファイルページでIDを確認できます。

* キャンペーンのアクティベーション用のオーディエンスを受け取るDSP[[!DNL Real-Time CDP] の](https://experienceleague.adobe.com/en/docs/advertising/dsp/audiences/sources/source-manage) ソース。 Adobe アカウントチームは、Experience Cloud組織IDを使用してソースを作成します。

* [!DNL DSP] ソースが[[!DNL Real-Time CDP]  [!DNL DSP]に作成されたときに生成される、](https://experienceleague.adobe.com/en/docs/advertising/dsp/audiences/sources/source-manage) アカウントまたは広告主のソースキー。 [!DNL DSP] アカウントチームがこのキーを共有します。 以下に説明するように、Experience Platform内でAdvertising DSPの宛先への宛先接続を作成します。

### Cookieを共有するためのID同期の設定 {#cookie-sync}

IDの同期は、サードパーティ Cookieを共有するための前提条件です。 [!DNL Web SDK] データストリームまたは[!DNL Experience Cloud ID Service]を使用してCookie同期プロセスを設定します。 サードパーティ CookieのID処理について詳しくは、[&#x200B; サードパーティ Cookie統合に依存するAdvertisingの宛先](/help/destinations/how-destinations-work/identity-handling.md#third-party-cookie-destinations)を参照してください。

**サードパーティ IDを[!DNL Web SDK]**&#x200B;と同期する

[!DNL Experience Platform Web SDK]を使用している場合は、詳細設定で「[!UICONTROL Third Party ID Sync]」オプションを設定して、データストリームでサードパーティ IDの同期を有効にします。 手順については、データストリームのドキュメントの[詳細オプションの設定](/help/datastreams/configure.md#advanced-options)を参照してください。

**サードパーティ IDを[!DNL Experience Cloud ID Service]**&#x200B;と同期できるようにします

[!DNL Experience Platform]個のタグを[!DNL Experience Cloud ID Service]と共に使用している場合は、[Experience Cloud ID サービス拡張機能](/help/tags/extensions/client/id-service/overview.md)を使用してサードパーティ IDの同期を設定します。 これにより、特定のECIDに一致するAdobe Advertising Cookieを、[!DNL Real-Time CDP]からオーディエンスをアクティブ化するときに利用できるようになります。

## サポートされている ID {#supported-identities}

Adobe Advertising DSPの宛先は、次の表に示すIDのアクティベーションをサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
| --------------- | ----------- | -------------- |
| `email_lc_sha256` | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Experience Platformは、プレーンテキストとSHA256 ハッシュ化されたメールアドレスの両方をサポートしています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをオンにして、Experience Platformがアクティベーション時にデータを自動的にハッシュします。 |
| `ECID` | Experience Cloudの1st パーティ Cookie | cookie ベースのセグメントの作成に必要です。 |
| `adcloud` | Adobe Advertisingのサードパーティ Cookie | cookie ベースのセグメントの作成に必要です。 |
| `GAID` | [!DNL Android] デバイス ID | [!DNL Android] デバイスをターゲットにする場合は必須です。 |
| `IDFA` | [!DNL iOS] デバイス ID | [!DNL iOS] デバイスをターゲットにする場合は必須です。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}

オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
| -------------------- | --------- | ----------- | --------- |
| [人物オーディエンス &#x200B;](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス &#x200B;](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [&#x200B; データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しタイプと頻度については、次の表を参照してください。

| 項目 | タイプ | メモ |
| ---- | ---- | ----- |
| 書き出しタイプ | **[!UICONTROL Audience export]** | 選択したIDを持つオーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいてExperience Platformでプロファイルが更新されると、コネクターは更新をダウンストリームの宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、Experience Platformの&#x200B;**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

宛先に接続するには、[Experience Platform ユーザーインターフェイスを使用して宛先接続](/help/destinations/ui/connect-destination.md)を作成する手順に従います。 宛先設定ワークフローで、以下のサブセクションに記載されているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に接続するには、[!UICONTROL Connection type] セクションに次のパラメーターを指定し、**[!UICONTROL Connect to destination]**&#x200B;を選択します。

* **[!UICONTROL Account or Advertiser Key]**：この[!UICONTROL Source Key]は、[[!DNL Real-Time CDP]  ソースがDSP ユーザーインターフェイス &#x200B;](https://experienceleague.adobe.com/en/docs/advertising/dsp/audiences/sources/source-manage)で作成されたときに生成されます。 Adobe アカウントチームは、ソースを作成した後、このキーを共有します。

アカウントまたは広告主キーのフィールドを表示する接続タイプ セクションの![&#x200B; スクリーンショット。](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/authenticate-destination.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。

![名前と説明の入力を示す宛先詳細フィールドのスクリーンショット。](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/destination-details.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!CONTEXTUALHELP]
>id="platform_destinations_required_mappings_adcloud_dsp"
>title="事前設定済みのマッピングセット"
>abstract="ECIDと[!DNL adcloud] Cookieの2つのマッピングセットが事前に設定されています。 データをAdobe Advertising DSPにアクティベートする場合、アクティベートされたオーディエンスに適格なプロファイルに少なくともECID IDが関連付けられており、宛先に正常にエクスポートされる必要があります。"
>additional-url="https://experienceleague.adobe.com/en/docs/experience-platform/destinations/catalog/advertising/adobe-advertising-dsp-connection#preconfigured-mappings" text="詳しくは、事前設定されたマッピングを参照してください。"

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* IDをエクスポートするには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![&#x200B; ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

この宛先のID マッピングは、部分的に事前設定されています。 以下の事前設定済みのマッピングを確認し、含めるオプションのIDを追加します。

### 事前設定済みマッピング {#preconfigured-mappings}

次のID マッピングは、オーディエンスのアクティブ化ワークフロー中に&#x200B;**事前設定され、自動的に入力されます**。

* **`ECID`** （Experience Cloud ID）
* **`adcloud`** （Adobe Advertising サードパーティ Cookie）

Cookie ID、ハッシュ化された電子メール、IDFA、およびGAID オプションを示すID マッピング セクションの![&#x200B; スクリーンショット。](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/identity-mapping.png)

これらのマッピングはグレー表示され、読み取り専用です。 この手順で設定する必要はありません。 必要に応じて、次のマッピングを追加できます。

* **`email_lc_sha256`** （ハッシュ化された電子メール）
* **IDFA** （[!DNL Apple iOS] デバイス ID）
* **GAID** （[!DNL Android] デバイス ID）

続行するには、**[!UICONTROL Next]**&#x200B;を選択してください。

>[!IMPORTANT]
>
>Cookie ベースの書き出しを成功させるには、**ECIDが必要です。ECIDのない**&#x200B;個のプロファイルは、Cookie ベースのセグメントには含まれません。 [!DNL RampID]または[!DNL UID2.0]を使用する認証済みオーディエンスセグメントの場合、プロファイルにはハッシュ化されたメール IDが含まれている必要があります。

手順については、[属性とIDのマッピング &#x200B;](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping)を参照してください。

## データの書き出しを検証する {#exported-data}

オーディエンスデータがAdobe Advertisingと共有されていることを確認するには、次の点を確認します。

* [!DNL Real-Time CDP]宛先のデータ フローが成功しました。

* DSPでは、オーディエンスは、**[!UICONTROL Audiences]** > **[!UICONTROL All Audiences]**&#x200B;またはプレースメント設定の&#x200B;**[!UICONTROL Audience Targeting]** セクション内からオーディエンスを作成または編集する際に使用できます。 オーディエンスは、[!UICONTROL Adobe Segments] フォルダーの下の[!UICONTROL Real-Time CDP] タブに表示されます。

読み込まれたオーディエンスセグメントを含む![&#x200B; フォルダーが表示されているDSP Audiences インターフェイスの[!DNL Real-Time CDP] スクリーンショット。「Adobe セグメント」タブに表示されています。](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/segments-in-dsp.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform]がデータガバナンスを適用する方法について詳しくは、[&#x200B; データガバナンスの概要](/help/data-governance/home.md)を参照してください。
