---
keywords: crm;CRM;crm 宛先；salesforce crm;salesforce crm 宛先
title: Salesforce CRM 接続
description: Salesforce CRM 宛先を使用すると、アカウントデータをエクスポートし、Salesforce CRM 内でビジネスニーズに合わせてアクティブ化できます。
exl-id: bd9cb656-d742-4a18-97a2-546d4056d093
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '2736'
ht-degree: 16%

---

# [!DNL Salesforce CRM] 接続

## 概要 {#overview}

[[!DNL Salesforce CRM]](https://www.salesforce.com/crm/) は一般的な顧客関係管理（CRM）プラットフォームであり、以下に説明するプロファイルのタイプをサポートしています。

* [&#x200B; リード &#x200B;](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_lead.htm) - リードとは、販売する製品やサービスに興味を持つ（持たない）可能性のある人物や会社の名前です。
* [&#x200B; 連絡先 &#x200B;](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_contact.htm) – 連絡先は、担当者の 1 人が関係を確立し、潜在的な顧客として認定された個人です。

この [!DNL Adobe Experience Platform][&#x200B; 宛先 &#x200B;](/help/destinations/home.md) は [[!DNL Salesforce composite API]](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_composite_sobjects_collections_update.htm) を活用します。これは、前述の両方のタイプのプロファイルをサポートします。

[&#x200B; セグメントのアクティブ化 &#x200B;](#activate) を行う際に、リードまたは連絡先を選択し、属性とオーディエンスデータを [!DNL Salesforce CRM] に更新することができます。

[!DNL Salesforce CRM] は、Salesforce REST API と通信するための認証メカニズムとして、パスワード付与を使用した OAuth 2 を使用します。 [!DNL Salesforce CRM] インスタンスを認証する手順は、さらに下の[宛先に対する認証](#authenticate)の節にあります。

## ユースケース {#use-cases}

マーケターは、Adobe Experience Platform プロファイルの属性に基づいて、ユーザーにパーソナライズされたエクスペリエンスを提供できます。 オフラインデータからオーディエンスを作成し、これらのオーディエンスをSalesforce CRM に送信して、Adobe Experience Platformでオーディエンスとプロファイルが更新されるとすぐに CRM メンバーシップを更新できます。

## 前提条件 {#prerequisites}

### Experience Platformの前提条件 {#prerequisites-in-experience-platform}

Salesforce CRM 宛先へのデータをアクティブ化する前に、[schema](/help/xdm/schema/composition.md)、[dataset](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja) および [segments](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=ja) を [!DNL Experience Platform] で作成する必要があります。

### [!DNL Salesforce CRM] の前提条件 {#prerequisites-destination}

Experience PlatformからSalesforce アカウントにデータを書き出すには、[!DNL Salesforce CRM] で次の前提条件に注意してください。

#### [!DNL Salesforce] アカウントが必要です {#prerequisites-account}

[!DNL Salesforce] アカウントをまだお持ちでない場合は、[&#x200B; &#x200B;](https://www.salesforce.com/in/form/signup/freetrial-sales/) 体験版 [!DNL Salesforce] ページに移動し、登録して作成してください。

#### [!DNL Salesforce] 内での接続アプリケーションの設定 {#prerequisites-connected-app}

まず、[[!DNL Salesforce]  アカウント内に &#x200B;](https://help.salesforce.com/s/articleView?id=sf.connected_app_create.htm&language=en_US&r=https%3A%2F%2Fhelp.salesforce.com%2F&type=5) 接続されたアプリ [!DNL Salesforce] がない場合は、設定する必要があります。 [!DNL Salesforce CRM] は、接続されたアプリを利用して [!DNL Salesforce] に接続します。

次に、[!DNL OAuth Settings for API Integration] の [!DNL Salesforce connected app] を有効にします。 詳しくは、[[!DNL Salesforce]](https://help.salesforce.com/s/articleView?id=connected_app_create_api_integration.htm&type=5&language=en_US) ドキュメントを参照してください。

また、[&#x200B; ージに対して以下に説明する &#x200B;](https://help.salesforce.com/s/articleView?id=connected_app_create_api_integration.htm&type=5&language=en_US) スコープ [!DNL Salesforce connected app] が選択されていることを確認します。

* ``chatter_api``
* ``lightning``
* ``visualforce``
* ``content``
* ``openid``
* ``full``
* ``api``
* ``web``
* ``refresh_token``
* ``offline_access``

最後に、`password` アカウント内で [!DNL Salesforce] 付与が有効になっていることを確認します。 ガイダンスが必要な場合は、[!DNL Salesforce] [OAuth 2.0 ユーザー名 – パスワードフロー特別なシナリオ &#x200B;](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_username_password_flow.htm&type=5) に関するドキュメントを参照してください。

>[!IMPORTANT]
>
>[!DNL Salesforce] アカウント管理者が信頼できる IP 範囲へのアクセスを制限している場合、[Experience Platformの IP を許可リストに加えるするには、管理者に連絡する必要 &#x200B;](/help/destinations/catalog/streaming/ip-address-allow-list.md) あります。 追加のガイダンスが必要な場合は、[!DNL Salesforce] [&#x200B; 接続されたアプリの信頼できる IP 範囲へのアクセスの制限 &#x200B;](https://help.salesforce.com/s/articleView?id=sf.connected_app_edit_ip_ranges.htm&type=5) ドキュメントを参照してください。

#### [!DNL Salesforce] 内でのカスタムフィールドの作成 {#prerequisites-custom-field}

[!DNL Salesforce CRM] の宛先に対してオーディエンスをアクティブ化する場合、**[!UICONTROL Mapping ID]** オーディエンススケジュール **[手順で、アクティブ化された各オーディエンスの](#schedule-segment-export-example)** フィールドに値を入力する必要があります。

[!DNL Salesforce CRM] では、Experience Platformから受信するオーディエンスを正しく読み取って解釈し、[!DNL Salesforce] 内でオーディエンスステータスを更新するためにこの値が必要です。 オーディエンスのステータスに関するガイダンスが必要な場合は、[&#x200B; オーディエンスメンバーシップの詳細スキーマフィールドグループ &#x200B;](/help/xdm/field-groups/profile/segmentation.md) に関するExperience Platform ドキュメントを参照してください。

Experience Platformから [!DNL Salesforce CRM] に対してアクティブ化するオーディエンスごとに、`Text Area (Long)` 内に [!DNL Salesforce] タイプのカスタムフィールドを作成する必要があります。 ビジネス要件に応じて、256～131,072 文字の任意のサイズのフィールド文字の長さを定義できます。 カスタムフィールドタイプについて詳しくは、[!DNL Salesforce] [&#x200B; カスタムフィールドタイプ &#x200B;](https://help.salesforce.com/s/articleView?id=sf.custom_field_types.htm&type=5) のドキュメントページを参照してください。 また、フィールドの作成に関してサポートが必要な場合は、[!DNL Salesforce] のドキュメントを参照して [&#x200B; カスタムフィールドの作成 &#x200B;](https://help.salesforce.com/s/articleView?id=mc_cab_create_an_attribute.htm&type=5&language=en_US) を確認してください。

>[!IMPORTANT]
>
>フィールド名に空白文字を含めないでください。 代わりに、アンダースコア `(_)` 文字を区切り文字として使用します。
>&#x200B;>[!DNL Salesforce] 内では、アクティブ化された各Experience Platform セグメントの **[!UICONTROL Field Name]** 内で指定された値と完全に一致する **[!UICONTROL Mapping ID]** を持つカスタムフィールドを作成する必要があります。 例えば、以下のスクリーンショットは、`crm_2_seg` という名前のカスタムフィールドを示しています。 この宛先に対してオーディエンスをアクティブ化する際に、`crm_2_seg` を **[!UICONTROL Mapping ID]** として追加し、Experience Platformのオーディエンスオーディエンスをこのカスタムフィールドに入力します。

[!DNL Salesforce] でのカスタムフィールド作成の例 *手順 1 - データタイプを選択* を以下に示します。
![&#x200B; カスタムフィールドの作成を示すSalesforce UI のスクリーンショット、手順 1 - データタイプの選択 &#x200B;](../../assets/catalog/crm/salesforce/create-salesforce-custom-field-step-1.png)

[!DNL Salesforce] でのカスタムフィールド作成の例 *手順 2 - カスタムフィールドの詳細を入力* は、次のとおりです。
![&#x200B; カスタムフィールドの作成を示すSalesforce UI のスクリーンショット、手順 2 - カスタムフィールドの詳細を入力 &#x200B;](../../assets/catalog/crm/salesforce/create-salesforce-custom-field-step-2.png)

>[!TIP]
>
>* Experience Platform オーディエンスに使用されるカスタムフィールドと、[!DNL Salesforce] 内の他のカスタムフィールドを区別するために、カスタムフィールドの作成時に認識できるプレフィックスやサフィックスを含めることができます。 例えば、`test_segment` の代わりに、`Adobe_test_segment` または `test_segment_Adobe` を使用します
>* [!DNL Salesforce] で既に他のカスタムフィールドを作成している場合は、Experience Platform セグメントと同じ名前を使用して、[!DNL Salesforce] でオーディエンスを簡単に識別できます。

>[!NOTE]
>
>* Salesforceのオブジェクトは、25 個の外部フィールドに制限されます。[&#x200B; カスタムフィールド属性 &#x200B;](https://help.salesforce.com/s/articleView?id=sf.custom_field_attributes.htm&type=5) を参照してください。
>* この制限は、Experience Platform オーディエンスメンバーシップを常に最大 25 個のみアクティブにできることを意味します。
>* Salesforce内でこの制限に達した場合は、新しい **[!UICONTROL Mapping ID]** を使用する前に、Salesforce内で古いオーディエンスに対するオーディエンスステータスの保存に使用されていたExperience Platformからカスタム属性を削除する必要があります。

#### [!DNL Salesforce CRM] 資格情報の収集 {#gather-credentials}

[!DNL Salesforce CRM] の宛先に対して認証を行う前に、以下の項目をメモしておきます。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| `Username` | [!DNL Salesforce] アカウントのユーザー名。 | |
| `Password` | [!DNL Salesforce] アカウントのパスワード。 | |
| `Security Token` | [!DNL Salesforce] セキュリティトークン：後で [!DNL Salesforce] パスワードの末尾に追加して、**[!UICONTROL Password]** 宛先への認証 [&#x200B; 時に &#x200B;](#authenticate) として使用される連結文字列を作成します。<br> セキュリティトークンがない場合に [!DNL Salesforce] インターフェイスからセキュリティトークンを再生成する方法については、[&#x200B; のドキュメント &#x200B;](https://help.salesforce.com/s/articleView?id=sf.user_security_token.htm&type=5) セキュリティトークンのリセット [!DNL Salesforce] を参照してください。 |  |
| `Custom Domain` | [!DNL Salesforce] ドメインのプレフィックス。 <br> インターフェイスからこの値を取得する方法については、[[!DNL Salesforce]  ドキュメント &#x200B;](https://help.salesforce.com/s/articleView?id=sf.domain_name_setting_login_policy.htm&type=5) を参照してくださ [!DNL Salesforce]。 | [!DNL Salesforce] ドメインが <br> の場合 *`d5i000000isb4eak-dev-ed`.my.salesforce.com*,<br> 値として `d5i000000isb4eak-dev-ed` が必要です。 |
| `Client ID` | Salesforceの `Consumer Key`。 <br> [[!DNL Salesforce]  インターフェイスからこの値を取得する方法については、](https://help.salesforce.com/s/articleView?id=sf.connected_app_rotate_consumer_details.htm&type=5) ドキュメント [!DNL Salesforce] を参照してください。 | |
| `Client Secret` | Salesforceの `Consumer Secret`。 <br> [[!DNL Salesforce]  インターフェイスからこの値を取得する方法については、](https://help.salesforce.com/s/articleView?id=sf.connected_app_rotate_consumer_details.htm&type=5) ドキュメント [!DNL Salesforce] を参照してください。 | |

### ガードレール {#guardrails}

[!DNL Salesforce] は、リクエスト、レート、タイムアウトの制限を課すことにより、トランザクションの負荷を分散させます。 詳しくは、[API リクエストの制限と割り当て &#x200B;](https://developer.salesforce.com/docs/atlas.en-us.salesforce_app_limits_cheatsheet.meta/salesforce_app_limits_cheatsheet/salesforce_app_limits_platform_api.htm) を参照してください。

[!DNL Salesforce] アカウント管理者が IP 制限を適用している場合は、[&#x200B; アカウントの信頼済み IP 範囲に &#x200B;](/help/destinations/catalog/streaming/ip-address-allow-list.md)1&rbrace;Experience Platform IP アドレス &rbrace; を追加する必要があります。 [!DNL Salesforce]追加のガイダンスが必要な場合は、[!DNL Salesforce] [&#x200B; 接続されたアプリの信頼できる IP 範囲へのアクセスの制限 &#x200B;](https://help.salesforce.com/s/articleView?id=sf.connected_app_edit_ip_ranges.htm&type=5) ドキュメントを参照してください。

>[!IMPORTANT]
>
>セグメントを [&#x200B; アクティブ化 &#x200B;](#activate) する際は、*連絡先* タイプまたは *リード* タイプのいずれかを選択する必要があります。 選択したタイプに応じた適切なデータマッピングがオーディエンスにあることを確認する必要があります。

## サポートされる ID {#supported-identities}

[!DNL Salesforce CRM] では、以下の表で説明する ID の更新をサポートしています。[ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| `SalesforceId` | セグメントを通じて書き出しまたは更新する連絡先またはリード ID の [!DNL Salesforce CRM] 識別子。 | 必須 |

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | <ul><li>セグメントのすべてのメンバーを、フィールドマッピングに従って、必要なスキーマフィールドと共に書き出します&#x200B;*（例：メールアドレス、電話番号、姓）*。</li><li> [!DNL Salesforce CRM] の各オーディエンスステータスは、**[!UICONTROL Mapping ID]** オーディエンスのスケジュール設定 [&#x200B; 手順で指定された &#x200B;](#schedule-segment-export-example) 値に基づいて、Experience Platformの対応するオーディエンスステータスとともに更新されます。</li></ul> |
| 書き出し頻度 | **[!UICONTROL Streaming]** | <ul><li>ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。</li></ul> |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL Destinations]** > **[!UICONTROL Catalog]** 内で [!DNL Salesforce CRM] を検索します。 または、**[!UICONTROL CRM]** カテゴリの下に配置することもできます。

### 宛先に対する認証 {#authenticate}

宛先に対する認証を行うには、以下の必須フィールドに入力し、「**[!UICONTROL Connect to destination]**」を選択します。 詳しくは、[Gather [!DNL Salesforce CRM] credentials](#gather-credentials) の節を参照してください。

| 資格情報 | 説明 |
| --- | --- |
| **[!UICONTROL Username]** | [!DNL Salesforce] アカウントのユーザー名。 |
| **[!UICONTROL Password]** | [!DNL Salesforce] アカウントのパスワードと [!DNL Salesforce] セキュリティトークンで構成される連結文字列。<br> 連結された値は、`{PASSWORD}{TOKEN}` の形式になります。<br> 注意：中括弧やスペースは使用しないでください。<br> 例えば、[!DNL Salesforce] パスワードが `MyPa$$w0rd123` で [!DNL Salesforce] セキュリティトークンが `TOKEN12345....0000` の場合、**[!UICONTROL Password]** フィールドで使用する連結された値は `MyPa$$w0rd123TOKEN12345....0000` になります。 |
| **[!UICONTROL Custom Domain]** | [!DNL Salesforce] ドメインのプレフィックス。 <br> 例えば、ドメインが *`d5i000000isb4eak-dev-ed`.my.salesforce.com の場合*、値として `d5i000000isb4eak-dev-ed` を指定する必要があります。 |
| **[!UICONTROL Client ID]** | [!DNL Salesforce] で接続されたアプリ `Consumer Key`。 |
| **[!UICONTROL Client Secret]** | [!DNL Salesforce] で接続されたアプリ `Consumer Secret`。 |

![&#x200B; 認証方法を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/salesforce/authenticate-destination.png)

指定した詳細が有効な場合、UI で **[!UICONTROL Connected]** ステータスに緑色のチェックマークが付き、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Salesforce ID Type]**：

   * 書き出しまたは更新する ID のタイプが **[!UICONTROL Contact]** 連絡先 *の場合は、* を選択します。
   * 書き出しまたは更新する ID のタイプが **[!UICONTROL Lead]** リード *の場合は、「*」を選択します。

![&#x200B; 宛先の詳細を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/salesforce/destination-details.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platform から [!DNL Salesforce CRM] 宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。マッピングは、Experience Platform アカウント内の Experience Data Model （XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。

**[!UICONTROL Target field]** で指定する属性には、属性マッピングテーブルで説明されているとおりに正確に名前を付ける必要があります。これらの属性はリクエスト本文を形成するからです。

**[!UICONTROL Source field]** で指定された属性は、そのような制限には従いません。 必要に応じてマッピングできますが、入力データの形式が [[!DNL Salesforce]  ドキュメント &#x200B;](https://help.salesforce.com/s/articleView?id=sf.custom_field_attributes.htm&type=5) に従って有効であることを確認してください。 入力データが無効な場合、[!DNL Salesforce] への更新呼び出しは失敗し、連絡先/リードが更新されません。

XDM フィールドを [!DNL (API) Salesforce CRM] 宛先フィールドに正しくマッピングするには、次の手順に従います。

1. **[!UICONTROL Mapping]** の手順で「**[!UICONTROL Add new mapping]**」を選択すると、新しいマッピング行が画面に表示されます。
   ![&#x200B; 「新しいマッピングを追加」のExperience Platform UI のスクリーンショットの例。](../../assets/catalog/crm/salesforce/add-new-mapping.png)
1. **[!UICONTROL Select source field]** ウィンドウで、**[!UICONTROL Select attributes]** カテゴリを選択して XDM 属性を選択するか、**[!UICONTROL Select identity namespace]** を選択して ID を選択します。
1. **[!UICONTROL Select target field]** ウィンドウで、**[!UICONTROL Select identity namespace]** を選択して ID を選択するか、カテゴリ **[!UICONTROL Select custom attributes]** 選択して属性を選択するか、必要に応じて **[!UICONTROL Attribute name]** フィールドを使用して属性を定義します。 サポートされる属性について詳しくは、[[!DNL Salesforce CRM]  ドキュメント &#x200B;](https://help.salesforce.com/s/articleView?id=sf.custom_field_attributes.htm&type=5) を参照してください。
   * これらの手順を繰り返して、XDM プロファイルスキーマと [!DNL (API) Salesforce CRM] の間に次のマッピングを追加します。

   **連絡先の操作**

   * セグメント内で *連絡先* を使用している場合は、Salesforceのオブジェクト参照で [&#x200B; 連絡先 &#x200B;](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_contact.htm) の情報を参照し、更新するフィールドのマッピングを定義してください。
   * 必須フィールドは、上記のリンクのフィールドの説明で説明されている *必須* という単語を検索することで識別できます。
   * 書き出しまたは更新するフィールドに応じて、XDM プロファイルスキーマと [!DNL (API) Salesforce CRM] の間のマッピングを追加します。

     | ソースフィールド | ターゲットフィールド | メモ |
     | --- | --- | --- |
     | `IdentityMap: crmID` | `Identity: SalesforceId` | `Mandatory` |
     | `xdm: person.name.lastName` | `Attribute: LastName` | `Mandatory`。連絡先の姓（最大 80 文字）。 |
     | `xdm: person.name.firstName` | `Attribute: FirstName` | 連絡先の名（最大 40 文字）。 |
     | `xdm: personalEmail.address` | `Attribute: Email` | 連絡先のメールアドレス。 |

   * これらのマッピングの使用例を次に示します。
     ![&#x200B; ターゲットマッピングを示したExperience Platform UI のスクリーンショットの例。](../../assets/catalog/crm/salesforce/mappings-contacts.png)

   **リードの使用**

   * セグメント内で *リード* を使用している場合は、Salesforceの [&#x200B; リード &#x200B;](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_lead.htm) のオブジェクト参照を参照して、更新するフィールドのマッピングを定義します。
   * 必須フィールドは、上記のリンクのフィールドの説明で説明されている *必須* という単語を検索することで識別できます。
   * 書き出しまたは更新するフィールドに応じて、XDM プロファイルスキーマと [!DNL (API) Salesforce CRM] の間のマッピングを追加します。

     | ソースフィールド | ターゲットフィールド | メモ |
     | --- | --- | --- |
     | `IdentityMap: crmID` | `Identity: SalesforceId` | `Mandatory` |
     | `xdm: person.name.lastName` | `Attribute: LastName` | `Mandatory`。リードの姓（最大 80 文字）。 |
     | `xdm: b2b.companyName` | `Attribute: Company` | `Mandatory`。リードの会社。 |
     | `xdm: personalEmail.address` | `Attribute: Email` | リードのメールアドレス。 |

   * これらのマッピングの使用例を次に示します。
     ![&#x200B; ターゲットマッピングを示したExperience Platform UI のスクリーンショットの例。](../../assets/catalog/crm/salesforce/mappings-leads.png)

宛先接続のマッピングの指定が完了したら、「**[!UICONTROL Next]**」を選択します。

### オーディエンスの書き出しのスケジュールと例 {#schedule-segment-export-example}

[&#x200B; オーディエンスの書き出しをスケジュール &#x200B;](/help/destinations/ui/activate-segment-streaming-destinations.md#scheduling) 手順を実行する際は、Experience Platformからアクティブ化されたオーディエンスを、[!DNL Salesforce] の対応するカスタムフィールドに手動でマッピングする必要があります。

それには、各セグメントを選択し、[!DNL Salesforce] からカスタムフィールド名を「[!DNL Salesforce CRM] **[!UICONTROL Mapping ID]**」フィールドに入力します。 [&#x200B; でカスタムフィールドを作成する際のガイダンスとベストプラクティスについては、 [!DNL Salesforce]](#prerequisites-custom-field) 内でのカスタムフィールドの作成 [!DNL Salesforce] の節を参照してください。

例えば、[!DNL Salesforce] カスタムフィールドが `crm_2_seg` の場合、[!DNL Salesforce CRM] **[!UICONTROL Mapping ID]** でこの値を指定して、Experience Platformのオーディエンスオーディエンスをこのカスタムフィールドに入力します。

[!DNL Salesforce] のカスタムフィールドの例を以下に示します。
カスタムフィールドを示す ![[!DNL Salesforce] UI のスクリーンショット。](../../assets/catalog/crm/salesforce/salesforce-custom-field.png)

[!DNL Salesforce CRM] **[!UICONTROL Mapping ID]** の場所を示す例を以下に示します。
![&#x200B; オーディエンスの書き出しのスケジュールを示したExperience Platform UI のスクリーンショットの例。](../../assets/catalog/crm/salesforce/schedule-segment-export.png)

上記のように、[!DNL Salesforce] **[!UICONTROL Field Name]** は [!DNL Salesforce CRM] 内で指定された値と完全に一致 **[!UICONTROL Mapping ID]** ます。

ユースケースに応じて、アクティブ化されたすべてのオーディエンスを同じ [!DNL Salesforce] カスタムフィールドまたは **[!UICONTROL Field Name]** で異なる [!DNL Salesforce CRM] にマッピングできます。 上記の画像に基づく典型的な例は、です。

| [!DNL Salesforce CRM] セグメント名 | [!DNL Salesforce] **[!UICONTROL Field Name]** | [!DNL Salesforce CRM] **[!UICONTROL Mapping ID]** |
| --- | --- | --- |
| crm_1_seg | `crm_1_seg` | `crm_1_seg` |
| crm_2_seg | `crm_2_seg` | `crm_2_seg` |

アクティブ化された各Experience Platform セグメントに対して、このセクションを繰り返します。

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. **[!UICONTROL Destinations]**/**[!UICONTROL Browse]** を選択して、宛先のリストに移動します。
   ![&#x200B; 宛先の参照を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/salesforce/browse-destinations.png)

1. 宛先を選択し、ステータスが「**[!UICONTROL enabled]**」であることを確認します。
   ![&#x200B; 宛先のデータフロー実行を示したExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/salesforce/destination-dataflow-run.png)

1. 「**[!UICONTROL Activation data]**」タブに切り替えて、オーディエンス名を選択します。
   ![&#x200B; 宛先のアクティベーションデータを示したExperience Platform UI のスクリーンショットの例。](../../assets/catalog/crm/salesforce/destinations-activation-data.png)

1. オーディエンスの概要を監視し、プロファイルの数がセグメント内で作成された数と一致していることを確認します。
   ![&#x200B; セグメントを示すExperience Platform UI のスクリーンショットの例。](../../assets/catalog/crm/salesforce/segment.png)

1. 最後に、Salesforce web サイトにログインし、オーディエンスのプロファイルが追加されたか更新されたかを検証します。

   **連絡先の操作**

   * Experience Platform セグメント内で *連絡先* を選択した場合は、**[!DNL Apps]**/**[!DNL Contacts]** ページに移動します。
     ![&#x200B; セグメントのプロファイルを含む連絡先ページを示すSalesforce CRM のスクリーンショット。](../../assets/catalog/crm/salesforce/contacts.png)

   * *連絡先* を選択し、フィールドが更新されているかどうかを確認します。 [!DNL Salesforce CRM] の各オーディエンスステータスが、**[!UICONTROL Mapping ID]** オーディエンスのスケジュール設定 [&#x200B; の際に指定された &#x200B;](#schedule-segment-export-example) 値に基づいて、Experience Platformから対応するオーディエンスステータスに更新されたことがわかります。
     ![&#x200B; 更新されたオーディエンスのステータスを含む連絡先の詳細ページを示すSalesforce CRM のスクリーンショット。](../../assets/catalog/crm/salesforce/contact-info.png)

   **リードの使用**

   * Experience Platform セグメント内で *リード* を選択した場合は、**[!DNL Apps]**/リー **[!DNL Leads]** ページに移動します。
     ![&#x200B; セグメントのプロファイルを含むリードページを示すSalesforce CRM のスクリーンショット。](../../assets/catalog/crm/salesforce/leads.png)

   * *リード* を選択し、フィールドが更新されたかどうかを確認します。 [!DNL Salesforce CRM] の各オーディエンスステータスが、**[!UICONTROL Mapping ID]** オーディエンスのスケジュール設定 [&#x200B; の際に指定された &#x200B;](#schedule-segment-export-example) 値に基づいて、Experience Platformから対応するオーディエンスステータスに更新されたことがわかります。
     ![&#x200B; 更新されたオーディエンスステータスを含むリードの詳細ページを示すSalesforce CRM のスクリーンショット。](../../assets/catalog/crm/salesforce/lead-info.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

### イベントを宛先にプッシュする際に不明なエラーが発生しました {#unknown-errors}

* データフローの実行を確認すると、次のエラーメッセージが表示される場合があります。`Unknown errors encountered while pushing events to the destination. Please contact the administrator and try again.`
  ![&#x200B; エラーを示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/salesforce/error.png)

   * このエラーを修正するには、アクティベーションワークフローで **[!UICONTROL Mapping ID]** の宛先に指定した [!DNL Salesforce CRM] が、[!DNL Salesforce] で作成したカスタムフィールドタイプの値と完全に一致することを確認します。 詳しくは、[&#x200B; 内でのカスタムフィールドの作成  [!DNL Salesforce]](#prerequisites-custom-field) の節を参照してください。

* セグメントをアクティブ化すると、次のエラーメッセージが表示される場合があります。`The client's IP address is unauthorized for this account. Allowlist the client's IP address...`
   * このエラーを修正するには、[!DNL Salesforce] アカウント管理者に連絡して、[Experience Platformの IP アドレス &#x200B;](/help/destinations/catalog/streaming/ip-address-allow-list.md) を [!DNL Salesforce] アカウントの信頼できる IP 範囲に追加してください。 追加のガイダンスが必要な場合は、[!DNL Salesforce] [&#x200B; 接続されたアプリの信頼できる IP 範囲へのアクセスの制限 &#x200B;](https://help.salesforce.com/s/articleView?id=sf.connected_app_edit_ip_ranges.htm&type=5) ドキュメントを参照してください。

## その他のリソース {#additional-resources}

[Salesforce開発者ポータル &#x200B;](https://developer.salesforce.com/) からのその他の役に立つ情報は次のとおりです。

* [クイックスタート](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/quickstart.htm)
* [&#x200B; レコードの作成 &#x200B;](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_sobject_create.htm)
* [&#x200B; カスタム推奨オーディエンス &#x200B;](https://developer.salesforce.com/docs/atlas.en-us.236.0.chatterapi.meta/chatterapi/connect_resources_recommendation_audiences_list.htm)
* [&#x200B; 複合リソースの使用 &#x200B;](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/using_composite_resources.htm?q=composite)
* この宛先では、[Upsert Single Record](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_composite_sobjects_collections_update.htm) API 呼び出しの代わりに [Upsert Multiple Records](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_composite_upsert_example.htm?q=contacts) API を利用します。
