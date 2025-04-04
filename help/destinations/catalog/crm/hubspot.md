---
title: HubSpot 接続
description: HubSpot の宛先を使用すると、HubSpot アカウントの連絡先レコードを管理できます。
last-substantial-update: 2023-09-28T00:00:00Z
exl-id: e2114bde-b7c3-43da-9f3a-919322000ef4
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1557'
ht-degree: 35%

---

# [!DNL HubSpot] 接続

[[!DNL HubSpot]](https://www.hubspot.com) は、マーケティング、セールス、コンテンツ管理、カスタマーサービスを結び付けるために必要なすべてのソフトウェア、統合、リソースを備えた CRM プラットフォームです。 データ、チーム、顧客を 1 つの CRM プラットフォームに接続できます。

この [!DNL Adobe Experience Platform][ 宛先 ](/help/destinations/home.md) は、[[!DNL HubSpot] Contacts API](https://developers.hubspot.com/docs/api/crm/contacts) を活用して、アクティブ化後に既存のExperience Platform オーディエンスから [!DNL HubSpot] 内の連絡先を更新します。

[!DNL HubSpot] インスタンスを認証する手順は、さらに下の[宛先に対する認証](#authenticate)の節にあります。

## ユースケース {#use-cases}

[!DNL HubSpot] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

連絡先 [!DNL HubSpot]、ビジネスとやり取りする個人に関する情報を保存します。 チームは、[!DNL HubSpot] に存在する連絡先を使用してExperience Platform オーディエンスを作成します。 これらのオーディエンスを [!DNL HubSpot] に送信すると、その情報が更新され、各連絡先には、連絡先が属するオーディエンスを示すオーディエンス名としてその値を持つプロパティが割り当てられます。

## 前提条件 {#prerequisites}

Experience Platformと [!DNL HubSpot] で設定する必要がある前提条件と、[!DNL HubSpot] の宛先を使用する前に収集する必要がある情報については、以下の節を参照してください。

### Experience Platform の前提条件 {#prerequisites-in-experience-platform}

[!DNL HubSpot] の宛先へのデータをアクティブ化する前に、[ スキーマ ](/help/xdm/schema/composition.md)、[ データセット ](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html) および [ オーディエンス ](https://experienceleague.adobe.com/docs/platform-learn/tutorials/audiences/create-audiences.html) を [!DNL Experience Platform] で作成する必要があります。

オーディエンスのステータスに関するガイダンスが必要な場合は、[ オーディエンスメンバーシップの詳細スキーマフィールドグループ ](/help/xdm/field-groups/profile/segmentation.md) に関するExperience Platform ドキュメントを参照してください。

### [!DNL HubSpot] の宛先の前提条件 {#prerequisites-destination}

Experience Platformから [!DNL HubSpot] アカウントにデータを書き出すには、次の前提条件に注意してください。

#### [!DNL HubSpot] アカウントが必要です {#prerequisites-account}

Experience Platformから [!DNL Hubspot] アカウントにデータをエクスポートするには、[!DNL HubSpot] アカウントが必要です。 アカウントをお持ちでない場合は、[HubSpot アカウントの設定 ](https://knowledge.hubspot.com/get-started/set-up-your-account) ページにアクセスし、ガイダンスに従ってアカウントを登録して作成してください。

#### [!DNL HubSpot] プライベートアプリアクセストークンの収集 {#gather-credentials}

[!DNL HubSpot] の宛先が [!DNL HubSpot] アカウント内の [!DNL HubSpot] プライベートアプリを介して API 呼び出しを行えるようにするには、[!DNL HubSpot] `Access token` が必要です。 `Access token` は、（宛先を認証 [ する際の `Bearer token` として機能 ](#authenticate) ます。

プライベートアプリがない場合は、のドキュメントに従って [ プライベートアプリの作成  [!DNL HubSpot]](https://developers.hubspot.com/docs/api/private-apps) してください。

>[!IMPORTANT]
>
> プライベートアプリには、次の範囲を割り当てる必要があります。
> `crm.objects.contacts.write`, `crm.objects.contacts.read`
> `crm.schemas.contacts.write`, `crm.schemas.contacts.read`

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| `Bearer token` | [!DNL HubSpot] プライベートアプリの `Access token`。 <br>[!DNL HubSpot] を取得する `Access token` は、[!DNL HubSpot] のドキュメントに従って [ アプリのアクセストークンを使用して API 呼び出しを行う ](https://developers.hubspot.com/docs/api/private-apps#make-api-calls-with-your-app-s-access-token) ください。 | `pat-na1-11223344-abcde-12345-9876-1234a1b23456` |

## ガードレール {#guardrails}

[!DNL HubSpot] のプライベートアプリには、「レート制限 [ の対象とな ](https://developers.hubspot.com/docs/api/usage-details) ます。 プライベートアプリで実行できる呼び出しの数は、[!DNL HubSpot] アカウントのサブスクリプションと、API アドオンを購入したかどうかに基づきます。 また、[ その他の制限 ](https://developers.hubspot.com/docs/api/usage-details#other-limits) も参照してください。

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
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li>フィールドマッピングに従って、目的のスキーマフィールド *（例：メールアドレス、電話番号、姓）* と共に、オーディエンスのすべてのメンバーを書き出します。</li><li> さらに、[!DNL HubSpot] では、選択した各オーディエンスに対して、オーディエンス名を使用して新しいプロパティが作成され、その値はExperience Platformの対応するオーディエンスステータスになります。</li></ul> |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | <ul><li>ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。</li></ul> |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL 宛先]**／**[!UICONTROL カタログ]**&#x200B;内で [!DNL HubSpot] を検索します。または、**[!UICONTROL CRM]** カテゴリの下に配置することもできます。

### 宛先に対する認証 {#authenticate}

以下の必須のフィールドに入力します。詳しくは、[ プライベートアプリアクセストークンの収集  [!DNL HubSpot]  の節を参照し ](#gather-credentials) ください。
* **[!UICONTROL ベアラートークン]**:[!DNL HubSpot] プライベートアプリのアクセストークン。

宛先を認証するには、「 **[!UICONTROL 宛先に接続]**」を選択します。
![ 認証方法を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/authenticate-destination.png)

指定した詳細が有効な場合、UI で&#x200B;**[!UICONTROL 接続済み]**&#x200B;ステータスに緑色のチェックマークが付きます。その後、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![ 宛先の詳細を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/destination-details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

Adobe Experience Platformから [!DNL HubSpot] の宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。 マッピングは、Experience Platform アカウント内の Experience Data Model （XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。

XDM フィールドを [!DNL HubSpot] の宛先フィールドに正しくマッピングするには、次の手順に従います。

#### `Email` ID のマッピング

`Email` ID は、この宛先に対する必須のマッピングです。 マッピングするには、次の手順に従います。
1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。これで、新しいマッピング行が画面に表示されます。
   ![ 「新しいマッピングを追加」ボタンがハイライト表示されたExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-add-new-mapping.png)
1. **[!UICONTROL ソースフィールドを選択]** ウィンドウで、「**[!UICONTROL ID 名前空間を選択]** を選択し、ID を選択します。
   ![ID としてマッピングするソース属性としてメールを選択するExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-select-source-identity.png)
1. **[!UICONTROL ターゲットフィールドを選択]** ウィンドウで **[!UICONTROL 属性を選択]** を選択し `email` す。
   ![ID としてマッピングするターゲット属性としてメールを選択するExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-select-target-identity.png)

| ソースフィールド | ターゲットフィールド | 必須 |
| --- | --- | --- |
| `IdentityMap: Email` | `Identity: email` | ○ |

ID マッピングの例を以下に示します。
![ メール ID マッピングを使用したExperience Platform UI のスクリーンショットの例。](../../assets/catalog/crm/hubspot/mapping-identities.png)

#### マッピング **オプション** 属性

XDM プロファイルスキーマと [!DNL HubSpot] アカウントの間で更新する他の属性を追加するには、次の手順を繰り返します。
1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。これで、新しいマッピング行が画面に表示されます。
   ![ 「新しいマッピングを追加」ボタンがハイライト表示されたExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-add-new-mapping.png)
1. **[!UICONTROL ソースフィールドを選択]** ウィンドウで、**[!UICONTROL 属性を選択]** カテゴリを選択して、XDM 属性を選択します。
   ![ ソース属性として名を選択するExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-select-source-attribute.png)
1. **[!UICONTROL ターゲットフィールドを選択]** ウィンドウで **[!UICONTROL 属性を選択]** カテゴリを選択し、[!DNL HubSpot] アカウントから自動的に入力される属性のリストから選択します。 宛先では [[!DNL HubSpot] Properties](https://developers.hubspot.com/docs/api/crm/properties) API を使用してこの情報を取得します。 [ デフォルトのプロパティ ](https://knowledge.hubspot.com/contacts/hubspots-default-contact-properties) とカスタムプロパティの両方が [!DNL HubSpot] 取得され、ターゲットフィールドとして選択されます。
   ![ ターゲット属性として名を選択するExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-select-target-attribute.png)

XDM プロファイルスキーマと [!DNL Hubspot] の間で使用できるマッピングを以下に示します。

| ソースフィールド | ターゲットフィールド |
| --- | --- |
| `xdm: person.name.firstName` | `Attribute: firstname` |
| `xdm: person.name.lastName` | `Attribute: lastname` |
| `xdm: workAddress.street1` | `Attribute: address` |
| `xdm: workAddress.city` | `Attribute: city` |
| `xdm: workAddress.country` | `Attribute: country` |

これらの属性マッピングの使用例を次に示します。
![ 属性マッピングを含むExperience Platform UI のスクリーンショットの例。](../../assets/catalog/crm/hubspot/mapping-attributes.png)

宛先接続のマッピングの指定を終えたら「**[!UICONTROL 次へ]**」を選択します。

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. [!DNL HubSpot] web サイトにログインし、**[!UICONTROL 連絡先]** ページに移動して、オーディエンスのステータスを確認します。 このリストは、オーディエンス名で作成されたカスタムプロパティの列を表示し、その値がオーディエンスステータスになるように設定できます。
   ![ オーディエンス名とセルのオーディエンスのステータスを示す列ヘッダーを含む連絡先ページを示す HubSpot UI のスクリーンショット ](../../assets/catalog/crm/hubspot/contacts.png)

1. または、個々の **[!UICONTROL ユーザー]** ページにドリルダウンして、オーディエンス名とオーディエンスのステータスを表示するプロパティに移動することもできます。
   ![ オーディエンス名とオーディエンスのステータスを表示するカスタムプロパティを含む連絡先ページを示す HubSpot UI のスクリーンショット。](../../assets/catalog/crm/hubspot/contact.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

[!DNL HubSpot] ドキュメントからのその他の役に立つ情報は次のとおりです。
* [HubSpot の認証方法 ](https://developers.hubspot.com/docs/api/intro-to-auth)
* [ 連絡先 ](https://developers.hubspot.com/docs/api/crm/contacts) API と [ プロパティ ](https://developers.hubspot.com/docs/api/crm/properties) API の [!DNL HubSpot] API リファレンス。

### 変更ログ

この節では、この宛先コネクタに対する機能の概要と重要なドキュメントの更新について説明します。

+++ 変更ログを表示

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2023年9月 | 初回リリース | 宛先の初回リリースとドキュメントの公開。 |

{style="table-layout:auto"}

+++
