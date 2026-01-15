---
title: The Trade Desk - CRM 接続
description: CRM データに基づくオーディエンスのターゲティングおよび抑制のために、プロファイルを Trade Desk アカウントに対してアクティブ化します。
last-substantial-update: 2025-01-16T00:00:00Z
exl-id: e09eaede-5525-4a51-a0e6-00ed5fdc662b
source-git-commit: b9713d5155f89ee895d9fb623088eda77b931d89
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 15%

---

# [!DNL Trade Desk] - CRM 接続

>[!IMPORTANT]
>
>EUID（European Unified ID）のリリースにより、[宛先カタログ](/help/destinations/catalog/overview.md)に 2 つの [!DNL The Trade Desk - CRM] 宛先が表示されるようになりました。
>
>* EU でデータをソースにする場合は、**[!DNL The Trade Desk - CRM (EU)]** の宛先を使用してください。
>* APAC または NAMER 地域でデータをソースにする場合は、**[!DNL The Trade Desk - CRM (NAMER & APAC)]** の宛先を使用してください。
>
>この宛先コネクタとドキュメントページは、*[!DNL Trade Desk]* チームが作成および管理します。 お問い合わせや更新のリクエストについては、[!DNL Trade Desk] 担当者にお問い合わせください。

## 概要 {#overview}

CRM データに基づくオーディエンスのターゲティングおよび抑制のために、[!DNL Trade Desk] アカウントに対してプロファイルをアクティブ化する方法について説明します。

このコネクタは、[!DNL The Trade Desk] のファーストパーティエンドポイントにデータを送信します。 Adobe Experience Platformと [!DNL The Trade Desk] の統合では、[!DNL The Trade Desk] サードパーティのエンドポイントへのデータの書き出しはサポートされていません。

[!DNL The Trade Desk(TTD)] は、メールアドレスのアップロードファイルをいつでも直接処理したり、生の（ハッシュ化され [!DNL The Trade Desk] いない）メールを保存したりしません。

>[!TIP]
>
>CRM 宛先 [!DNL The Trade Desk]、メールやハッシュ化されたメールアドレスなどの CRM データマッピングに使用します。 Cookie とデバイス ID のマッピングには、Adobe Experience Platform カタログの [ その他の Trade Desk 宛先 ](/help/destinations/catalog/advertising/tradedesk.md) を使用します。

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>The Trade Desk に対してオーディエンスをアクティブ化する前に、[!DNL Trade Desk] アカウントマネージャーに連絡して CRM オンボーディング契約に署名する必要があります。 [!DNL The Trade Desk] では、UID2/EUID を使用できるようになります。また、他の詳細を共有して、宛先の設定に役立てます。

## ID の一致要件 {#id-matching-requirements}

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件に従う必要があります。 詳しくは、[ID 名前空間の概要 ](/help/identity-service/features/namespaces.md) を参照してください。

## サポートされている ID {#supported-identities}

[!DNL The Trade Desk] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ID の一致要件の節の手順に従って、プレーンテキストとハッシュ化されたメールアドレスに適切な名前空間をそれぞれ使用します。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | メールアドレス（クリアテキスト） | ソース ID がメール名前空間または属性の場合は、`email` をターゲット ID として入力します。 |
| Email_LC_SHA256 | メールアドレスは、SHA256 を使用してハッシュ化し、小文字で区切る必要があります。 この設定は、後で変更することはできません。 | ソース ID が Email_LC_SHA256 名前空間または属性の場合は、`hashed_email` をターゲット ID として入力します。 |

{style="table-layout:auto"}

## メールハッシュ要件 {#hashing-requirements}

メールアドレスは、Adobe Experience Platformに取り込む前にハッシュ化したり、生のメールアドレスを使用したりできます。

Experience Platformでのメールアドレスの取り込みについては、[ バッチ取り込みの概要 ](/help/ingestion/batch-ingestion/overview.md) を参照してください。

メールアドレスを自分でハッシュ化することを選択する場合は、次の要件に必ず従ってください。

* 先頭と末尾のスペースを削除します。
* ASCII 文字をすべて小文字に変換します。
* メ `gmail.com` ルアドレスのユーザー名部分から、以下の文字を削除します。
   * ピリオド（. （ASCII コード 46））。 例えば、`jane.doe@gmail.com` を `janedoe@gmail.com` に正規化します。
   * プラス記号（+ （ASCII コード 43））とそれに続くすべての文字。 例えば、`janedoe+home@gmail.com` を `janedoe@gmail.com` に正規化します。

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | Trade Desk 宛先で使用される識別子（メールまたはハッシュ化されたメール）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Daily Batch]** | オーディエンスの評価に基づいてExperience Platform内でプロファイルを更新すると、プロファイル（ID）は 1 日 1 回ダウンストリームの宛先プラットフォームで更新されます。 詳しくは、[ バッチエクスポート ](/help/destinations/destination-types.md#file-based) を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

### 宛先に対する認証 {#authenticate}

CRM 宛先 [!DNL The Trade Desk]、毎日のバッチファイルアップロードであり、ユーザーによる認証を必要としません。

### 宛先の詳細の入力 {#fill-in-details}

オーディエンスデータを宛先に送信または有効化する前に、独自の宛先プラットフォームへの接続を設定する必要があります。 この宛先を[設定](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=ja)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Account Type]**: **[!UICONTROL Existing Account]** オプションを選択してください。
* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Advertiser ID]**：お使いの [!DNL Trade Desk Advertiser ID]。[!DNL Trade Desk] アカウントマネージャーで共有するか、[!DNL Advertiser Preferences] UI の [!DNL Trade Desk] にあるアセットです。

![ 宛先の詳細を入力する方法を示すExperience Platform UI のスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/configuredestination2.png)

宛先に接続する場合、データガバナンスポリシーの設定は完全にオプションです。 詳しくは、Experience Platform[ データガバナンスの概要 ](/help/data-governance/policies/overview.md) を参照してください。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!CONTEXTUALHELP]
>id="platform_destinations_required_mappings_ttdg"
>title="事前設定済みのマッピングセット"
>abstract="これら 4 つのマッピングセットは事前に設定されています。 Trade Desk に対してデータをアクティブ化する際、アクティブ化されたオーディエンスに対して選定されたプロファイルには、必ずしも 4 つの ID すべてがプロファイルに存在している必要はありません。これは、この宛先が、ここに示すいずれかのターゲット ID で動作するためです。"

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

宛先に対してオーディエンスをアクティブ化する手順については、[ プロファイル書き出しのバッチ宛先に対するオーディエンスデータのアクティブ化 ](/help/destinations/ui/activate-batch-profile-destinations.md) を参照してください。

**[!UICONTROL Scheduling]** ページでは、書き出す各オーディエンスのスケジュールとファイル名を設定できます。 スケジュールの設定は必須ですが、ファイル名の設定はオプションです。

![ オーディエンスのアクティベーションをスケジュールするためのExperience Platform UI のスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/schedulesegment1.png)

>[!NOTE]
>
>CRM 宛先に対してアクティブ化 [!DNL The Trade Desk] れたすべてのオーディエンスは、毎日の頻度と完全なファイル書き出しに自動的に設定されます。

![ オーディエンスのアクティベーションをスケジュールするためのExperience Platform UI のスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/schedulesegment2.png)

**[!UICONTROL Mapping]** ページでは、ソース列から属性または ID 名前空間を選択し、ターゲット列にマッピングする必要があります。

![ オーディエンスのアクティベーションをマッピングするためのExperience Platform UI のスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/mappingsegment1.png)

以下に、オーディエンスを CRM 宛先に対してアクティブ化する際の、正しい ID マッピング [!DNL The Trade Desk] 例を示します。

>[!IMPORTANT]
>
> CRM 宛先 [!DNL The Trade Desk]、生のメールアドレスとハッシュ化されたメールアドレスを、同じアクティベーションフローの ID として受け入れません。 生のメールアドレスとハッシュ化されたメールアドレスに対して別々のアクティベーションフローを作成します。

ソースフィールドを選択中：

* データ取り込み時に生のメールアドレスを使用する場合は、ソース ID として `Email` 名前空間または属性を選択します。
* Experience Platformへのデータ取り込み時に顧客のメールアドレスをハッシュ化した場合は、`Email_LC_SHA256` 名前空間または属性をソース ID として選択します。

ターゲットフィールドを選択：

* ソース名前空間または属性が `email` 定されている場合に、ターゲット ID として `Email` を入力します。
* ソース名前空間または属性が `hashed_email` 定されている場合に、ターゲット ID として `Email_LC_SHA256` を入力します。

## データの書き出しを検証 {#validate}

データがExperience Platformから [!DNL The Trade Desk] に正しく書き出されていることを検証するには、[!DNL The Trade Desk] Data Management Platform （DMP）内のAdobe 1PD データタイルでオーディエンスを見つけてください。 [!DNL Trade Desk] UI 内で対応する ID を見つける手順は次のとおりです。

1. まず、「**[!UICONTROL Data]**」タブを選択し、「**[!UICONTROL First-Party]**」セクションを確認します。
2. ページを下にスクロールすると、「**[!UICONTROL Imported Data]**」の下に **[!UICONTROL Adobe 1PD Tile]** が表示されます。
3. **[!UICONTROL Adobe 1PD]**タイルをクリックすると、広告主の [!DNL Trade Desk] しい宛先に対してアクティブ化されたすべてのオーディエンスが一覧表示されます。 検索機能を使用することもできます。
4. Experience Platformのセグメント ID #が、[!DNL Trade Desk] UI でセグメント名として表示されます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
