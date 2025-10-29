---
title: HubSpot 接続
description: HubSpot の宛先を使用すると、HubSpot アカウントの連絡先レコードを管理できます。
last-substantial-update: 2023-09-28T00:00:00Z
exl-id: e2114bde-b7c3-43da-9f3a-919322000ef4
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1499'
ht-degree: 30%

---

# [!DNL HubSpot] 接続

[[!DNL HubSpot]](https://www.hubspot.com) は、マーケティング、セールス、コンテンツ管理、カスタマーサービスを結び付けるために必要なすべてのソフトウェア、統合、リソースを備えた CRM プラットフォームです。 データ、チーム、顧客を 1 つの CRM プラットフォームに接続できます。

この [!DNL Adobe Experience Platform][&#x200B; 宛先 &#x200B;](/help/destinations/home.md) は、[[!DNL HubSpot] Contacts API](https://developers.hubspot.com/docs/api/crm/contacts) を活用して、アクティブ化後に既存のExperience Platform オーディエンスから [!DNL HubSpot] 内の連絡先を更新します。

[!DNL HubSpot] インスタンスを認証する手順は、さらに下の[宛先に対する認証](#authenticate)の節にあります。

## ユースケース {#use-cases}

[!DNL HubSpot] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

連絡先 [!DNL HubSpot]、ビジネスとやり取りする個人に関する情報を保存します。 チームは、[!DNL HubSpot] に存在する連絡先を使用してExperience Platform オーディエンスを作成します。 これらのオーディエンスを [!DNL HubSpot] に送信すると、その情報が更新され、各連絡先には、連絡先が属するオーディエンスを示すオーディエンス名としてその値を持つプロパティが割り当てられます。

## 前提条件 {#prerequisites}

Experience Platformと [!DNL HubSpot] で設定する必要がある前提条件と、[!DNL HubSpot] の宛先を使用する前に収集する必要がある情報については、以下の節を参照してください。

### Experience Platform の前提条件 {#prerequisites-in-experience-platform}

[!DNL HubSpot] の宛先へのデータをアクティブ化する前に、[&#x200B; スキーマ &#x200B;](/help/xdm/schema/composition.md)、[&#x200B; データセット &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja) および [&#x200B; オーディエンス &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/tutorials/audiences/create-audiences.html?lang=ja) を [!DNL Experience Platform] で作成する必要があります。

オーディエンスのステータスに関するガイダンスが必要な場合は、[&#x200B; オーディエンスメンバーシップの詳細スキーマフィールドグループ &#x200B;](/help/xdm/field-groups/profile/segmentation.md) に関するExperience Platform ドキュメントを参照してください。

### [!DNL HubSpot] の宛先の前提条件 {#prerequisites-destination}

Experience Platformから [!DNL HubSpot] アカウントにデータを書き出すには、次の前提条件に注意してください。

#### [!DNL HubSpot] アカウントが必要です {#prerequisites-account}

Experience Platformから [!DNL Hubspot] アカウントにデータをエクスポートするには、[!DNL HubSpot] アカウントが必要です。 アカウントをお持ちでない場合は、[HubSpot アカウントの設定 &#x200B;](https://knowledge.hubspot.com/get-started/set-up-your-account) ページにアクセスし、ガイダンスに従ってアカウントを登録して作成してください。

#### [!DNL HubSpot] プライベートアプリアクセストークンの収集 {#gather-credentials}

[!DNL HubSpot] の宛先が `Access token` アカウント内の [!DNL HubSpot] プライベートアプリを介して API 呼び出しを行えるようにするには、[!DNL HubSpot] [!DNL HubSpot] が必要です。 `Access token` は、（宛先を認証 `Bearer token` する際の [&#x200B; として機能 &#x200B;](#authenticate) ます。

プライベートアプリがない場合は、のドキュメントに従って [&#x200B; プライベートアプリの作成  [!DNL HubSpot]](https://developers.hubspot.com/docs/api/private-apps) してください。

>[!IMPORTANT]
>
> プライベートアプリには、次の範囲を割り当てる必要があります。
> &#x200B;> `crm.objects.contacts.write`, `crm.objects.contacts.read`
> &#x200B;> `crm.schemas.contacts.write`, `crm.schemas.contacts.read`

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| `Bearer token` | `Access token` プライベートアプリの [!DNL HubSpot]。 <br>[!DNL HubSpot] を取得する `Access token` は、[!DNL HubSpot] のドキュメントに従って [&#x200B; アプリのアクセストークンを使用して API 呼び出しを行う &#x200B;](https://developers.hubspot.com/docs/api/private-apps#make-api-calls-with-your-app-s-access-token) ください。 | `pat-na1-11223344-abcde-12345-9876-1234a1b23456` |

## ガードレール {#guardrails}

[!DNL HubSpot] のプライベートアプリには、「レート制限 [&#x200B; の対象とな &#x200B;](https://developers.hubspot.com/docs/api/usage-details) ます。 プライベートアプリで実行できる呼び出しの数は、[!DNL HubSpot] アカウントのサブスクリプションと、API アドオンを購入したかどうかに基づきます。 また、[&#x200B; その他の制限 &#x200B;](https://developers.hubspot.com/docs/api/usage-details#other-limits) も参照してください。

## サポートされる ID {#supported-identities}

[!DNL HubSpot] では、以下の表で説明する ID の更新をサポートしています。[ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 例 | 説明 | 注意点 |
|---|---|---|---|
| `email` | `test@test.com` | 連絡先のメールアドレス。 | 必須 |

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出しできるすべてのオーディエンスについて説明します。

この宛先では、Experience Platform の[セグメント化サービス](../../../segmentation/home.md)で生成したすべてのオーディエンスのアクティブ化をサポートします。

この宛先では、以下の表で説明するオーディエンスのアクティブ化もサポートされています。

| オーディエンスタイプ | 説明 |
|---------|----------|
| カスタムアップロード | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | <ul><li>フィールドマッピングに従って、目的のスキーマフィールド *（例：メールアドレス、電話番号、姓）* と共に、オーディエンスのすべてのメンバーを書き出します。</li><li> さらに、[!DNL HubSpot] では、選択した各オーディエンスに対して、オーディエンス名を使用して新しいプロパティが作成され、その値はExperience Platformの対応するオーディエンスステータスになります。</li></ul> |
| 書き出し頻度 | **[!UICONTROL Streaming]** | <ul><li>ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。</li></ul> |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL Destinations]** > **[!UICONTROL Catalog]** 内で [!DNL HubSpot] を検索します。 または、**[!UICONTROL CRM]** カテゴリの下に配置することもできます。

### 宛先に対する認証 {#authenticate}

以下の必須のフィールドに入力します。詳しくは、[&#x200B; プライベートアプリアクセストークンの収集  [!DNL HubSpot]  の節を参照し &#x200B;](#gather-credentials) ください。

* **[!UICONTROL Bearer token]**:[!DNL HubSpot] プライベートアプリのアクセストークン。

宛先を認証するには、「**[!UICONTROL Connect to destination]**」を選択します。
![&#x200B; 認証方法を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/authenticate-destination.png)

指定した詳細が有効な場合、UI で **[!UICONTROL Connected]** ステータスに緑色のチェックマークが付きます。 その後、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![&#x200B; 宛先の詳細を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/destination-details.png)

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

Adobe Experience Platformから [!DNL HubSpot] の宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。 マッピングは、Experience Platform アカウント内の Experience Data Model （XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。

XDM フィールドを [!DNL HubSpot] の宛先フィールドに正しくマッピングするには、次の手順に従います。

#### `Email` ID のマッピング

`Email` ID は、この宛先に対する必須のマッピングです。 マッピングするには、次の手順に従います。

1. **[!UICONTROL Mapping]** の手順で、「**[!UICONTROL Add new mapping]**」を選択します。 これで、新しいマッピング行が画面に表示されます。
   ![&#x200B; 「新しいマッピングを追加」ボタンがハイライト表示されたExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-add-new-mapping.png)
1. **[!UICONTROL Select source field]** ウィンドウで **[!UICONTROL Select identity namespace]** を選択し、ID を選択します。
   ![ID としてマッピングするソース属性としてメールを選択するExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-select-source-identity.png)
1. **[!UICONTROL Select target field]** ウィンドウで **[!UICONTROL Select attributes]** を選択し、「`email`」を選択します。
   ![ID としてマッピングするターゲット属性としてメールを選択するExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-select-target-identity.png)

| ソースフィールド | ターゲットフィールド | 必須 |
| --- | --- | --- |
| `IdentityMap: Email` | `Identity: email` | ○ |

ID マッピングの例を以下に示します。
![&#x200B; メール ID マッピングを使用したExperience Platform UI のスクリーンショットの例。](../../assets/catalog/crm/hubspot/mapping-identities.png)

#### マッピング **オプション** 属性

XDM プロファイルスキーマと [!DNL HubSpot] アカウントの間で更新する他の属性を追加するには、次の手順を繰り返します。

1. **[!UICONTROL Mapping]** の手順で、「**[!UICONTROL Add new mapping]**」を選択します。 これで、新しいマッピング行が画面に表示されます。
   ![&#x200B; 「新しいマッピングを追加」ボタンがハイライト表示されたExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-add-new-mapping.png)
1. **[!UICONTROL Select source field]** ウィンドウで、**[!UICONTROL Select attributes]** カテゴリを選択し、XDM 属性を選択します。
   ![&#x200B; ソース属性として名を選択するExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-select-source-attribute.png)
1. **[!UICONTROL Select target field]** ウィンドウでカテゴリ **[!UICONTROL Select attributes]** 選択し、[!DNL HubSpot] アカウントから自動的に入力される属性のリストから選択します。 宛先では [[!DNL HubSpot] Properties](https://developers.hubspot.com/docs/api/crm/properties) API を使用してこの情報を取得します。 [!DNL HubSpot] デフォルトのプロパティ [&#x200B; とカスタムプロパティの両方が &#x200B;](https://knowledge.hubspot.com/contacts/hubspots-default-contact-properties) 取得され、ターゲットフィールドとして選択されます。
   ![&#x200B; ターゲット属性として名を選択するExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-select-target-attribute.png)

XDM プロファイルスキーマと [!DNL Hubspot] の間で使用できるマッピングを以下に示します。

| ソースフィールド | ターゲットフィールド |
| --- | --- |
| `xdm: person.name.firstName` | `Attribute: firstname` |
| `xdm: person.name.lastName` | `Attribute: lastname` |
| `xdm: workAddress.street1` | `Attribute: address` |
| `xdm: workAddress.city` | `Attribute: city` |
| `xdm: workAddress.country` | `Attribute: country` |

これらの属性マッピングの使用例を次に示します。
![&#x200B; 属性マッピングを含むExperience Platform UI のスクリーンショットの例。](../../assets/catalog/crm/hubspot/mapping-attributes.png)

宛先接続のマッピングの指定を終えたら「**[!UICONTROL Next]**」を選択します。

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. [!DNL HubSpot] web サイトにログインし、**[!UICONTROL Contacts]** ページに移動して、オーディエンスのステータスを確認します。 このリストは、オーディエンス名で作成されたカスタムプロパティの列を表示し、その値がオーディエンスステータスになるように設定できます。
   ![&#x200B; オーディエンス名とセルのオーディエンスのステータスを示す列ヘッダーを含む連絡先ページを示す HubSpot UI のスクリーンショット &#x200B;](../../assets/catalog/crm/hubspot/contacts.png)

1. または、個々のオーディエンスページにドリルダウンして、**[!UICONTROL Person]** ーディエンス名とオーディエンスのステータスを表示するプロパティに移動することもできます。
   ![&#x200B; オーディエンス名とオーディエンスのステータスを表示するカスタムプロパティを含む連絡先ページを示す HubSpot UI のスクリーンショット。](../../assets/catalog/crm/hubspot/contact.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

[!DNL HubSpot] ドキュメントからのその他の役に立つ情報は次のとおりです。

* [HubSpot の認証方法 &#x200B;](https://developers.hubspot.com/docs/api/intro-to-auth)
* [!DNL HubSpot] 連絡先 [&#x200B; API と &#x200B;](https://developers.hubspot.com/docs/api/crm/contacts) プロパティ [&#x200B; API の &#x200B;](https://developers.hubspot.com/docs/api/crm/properties) API リファレンス。

### 変更ログ

この節では、この宛先コネクタに対する機能の概要と重要なドキュメントの更新について説明します。

+++ 変更ログを表示

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2023年9月 | 初回リリース | 宛先の初回リリースとドキュメントの公開。 |

{style="table-layout:auto"}

+++
