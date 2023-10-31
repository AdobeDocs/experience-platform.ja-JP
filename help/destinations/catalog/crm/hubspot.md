---
title: HubSpot 接続
description: HubSpot の宛先を使用すると、HubSpot アカウント内の連絡先レコードを管理できます。
last-substantial-update: 2023-09-28T00:00:00Z
exl-id: e2114bde-b7c3-43da-9f3a-919322000ef4
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '1604'
ht-degree: 39%

---

# [!DNL HubSpot] 接続

[[!DNL HubSpot]](https://www.hubspot.com) は、マーケティング、セールス、コンテンツ管理、顧客サービスに接続するために必要なすべてのソフトウェア、統合、リソースを含む CRM プラットフォームです。 1 つの CRM プラットフォーム上でデータ、チーム、顧客を接続できます。

この [!DNL Adobe Experience Platform] [宛先](/help/destinations/home.md) は、 [[!DNL HubSpot] 連絡先 API](https://developers.hubspot.com/docs/api/crm/contacts)，内の連絡先を更新 [!DNL HubSpot] アクティブ化後に既存のExperience Platformオーディエンスから

[!DNL HubSpot] インスタンスを認証する手順は、さらに下の[宛先に対する認証](#authenticate)の節にあります。

## ユースケース {#use-cases}

[!DNL HubSpot] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

[!DNL HubSpot] の連絡先には、ビジネスとやり取りする個人に関する情報が保存されます。 チームがに存在する連絡先を使用しています [!DNL HubSpot] を使用して、Experience Platformオーディエンスを構築します。 これらのオーディエンスをに送信した後 [!DNL HubSpot]の場合、連絡先の情報が更新され、各連絡先にプロパティが割り当てられ、その連絡先が属するオーディエンスを示すオーディエンス名がその値になります。

## 前提条件 {#prerequisites}

Experience Platformおよびで設定する必要がある前提条件については、以下の節を参照してください。 [!DNL HubSpot] また、を使用する前に収集する必要がある情報についても説明します。 [!DNL HubSpot] 宛先。

### Experience Platform の前提条件 {#prerequisites-in-experience-platform}

に対してデータをアクティブ化する前に [!DNL HubSpot] 宛先の [スキーマ](/help/xdm/schema/composition.md), a [データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja)、および [audiences](https://experienceleague.adobe.com/docs/platform-learn/tutorials/audiences/create-audiences.html?lang=en) 次で作成： [!DNL Experience Platform].

詳しくは、Experience Platformドキュメントを参照してください。 [オーディエンスメンバーシップ詳細スキーマフィールドグループ](/help/xdm/field-groups/profile/segmentation.md) オーディエンスのステータスに関するガイダンスが必要な場合は、を参照してください。

### の前提条件 [!DNL HubSpot] 宛先 {#prerequisites-destination}

Platform からにデータを書き出すための次の前提条件に注意してください。 [!DNL HubSpot] アカウント：

#### 次をお持ちの場合は、 [!DNL HubSpot] アカウント {#prerequisites-account}

Platform からにデータを書き出すには、以下を実行します。 [!DNL Hubspot] アカウントに [!DNL HubSpot] アカウント。 まだない場合は、 [HubSpot アカウントの設定](https://knowledge.hubspot.com/get-started/set-up-your-account) ページを開き、ガイダンスに従って登録し、アカウントを作成します。

#### を収集します。 [!DNL HubSpot] プライベートアプリアクセストークン {#gather-credentials}

次が必要です： [!DNL HubSpot] `Access token` 許可する [!DNL HubSpot] の宛先を使用して、 [!DNL HubSpot] 内のプライベートアプリ [!DNL HubSpot] アカウント。 The `Access token` ～の役割を果たす `Bearer token` いつ [宛先の認証](#authenticate).

プライベートアプリをお持ちでない場合は、ドキュメントに従って、 [でのプライベートアプリの作成 [!DNL HubSpot]](https://developers.hubspot.com/docs/api/private-apps).

>[!IMPORTANT]
>
> プライベートアプリには以下のスコープを割り当てる必要があります。
> `crm.objects.contacts.write`、`crm.objects.contacts.read`）を適用します
> `crm.schemas.contacts.write`、`crm.schemas.contacts.read`）を適用します

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| `Bearer token` | The `Access token` の [!DNL HubSpot] プライベートアプリ。 <br>次の手順で、 [!DNL HubSpot] `Access token` 後を追う [!DNL HubSpot] ～に関する文書 [アプリのアクセストークンで API 呼び出しをおこなう](https://developers.hubspot.com/docs/api/private-apps#make-api-calls-with-your-app-s-access-token). | `pat-na1-11223344-abcde-12345-9876-1234a1b23456` |

## ガードレール {#guardrails}

[!DNL HubSpot] 非公開アプリは次の条件を満たす [レート制限](https://developers.hubspot.com/docs/api/usage-details). プライベートアプリで実行できる呼び出しの数は、 [!DNL HubSpot] アカウントの購読と、API アドオンを購入したかどうか。 また、 [その他の制限](https://developers.hubspot.com/docs/api/usage-details#other-limits).

## サポートされる ID {#supported-identities}

[!DNL HubSpot] では、以下の表で説明する ID の更新をサポートしています。[ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 例 | 説明 | 注意点 |
|---|---|---|---|
| `email` | `test@test.com` | 連絡先の電子メールアドレス。 | 必須 |

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出しできるすべてのオーディエンスについて説明します。

この宛先では、Experience Platform の[セグメント化サービス](../../../segmentation/home.md)で生成したすべてのオーディエンスのアクティブ化をサポートします。

また、この宛先では、以下の表で説明するオーディエンスのアクティブ化もサポートされます。

| オーディエンスタイプ | 説明 |
---------|----------|
| カスタムアップロード | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li>オーディエンスのすべてのメンバーを、目的のスキーマフィールドと共に書き出します *（例：電子メールアドレス、電話番号、姓）*（フィールドマッピングに従います）。</li><li> また、新しいプロパティは [!DNL HubSpot] オーディエンス名とその値を使用すると、選択した各オーディエンスに対し、Platform からの対応するオーディエンスステータスが表示されます。</li></ul> |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | <ul><li>ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。</li></ul> |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL 宛先]**／**[!UICONTROL カタログ]**&#x200B;内で [!DNL HubSpot] を検索します。または、**[!UICONTROL CRM]** カテゴリの下に配置することもできます。

### 宛先に対する認証 {#authenticate}

以下の必須のフィールドに入力します。詳しくは、 [を収集します。 [!DNL HubSpot] プライベートアプリアクセストークン](#gather-credentials) 」の節を参照してください。
* **[!UICONTROL Bearer トークン]**: [!DNL HubSpot] プライベートアプリ。

宛先を認証するには、「 **[!UICONTROL 宛先に接続]**」を選択します。
![認証方法を示す Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/authenticate-destination.png)

指定した詳細が有効な場合、UI で&#x200B;**[!UICONTROL 接続済み]**&#x200B;ステータスに緑色のチェックマークが付きます。その後、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![宛先の詳細を示す Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/destination-details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

Adobe Experience Platformからにオーディエンスデータを正しく送信するには、以下を実行します。 [!DNL HubSpot] の宛先の場合は、フィールドマッピングの手順を実行する必要があります。 マッピングは、Platform アカウント内の Experience Data Model（XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。 

XDM フィールドを [!DNL HubSpot] 宛先フィールドには、次の手順に従います。

#### のマッピング `Email` id

The `Email` id は、この宛先での必須マッピングです。 次の手順に従って、マッピングします。
1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。これで、新しいマッピング行が画面に表示されます。
   ![新しいマッピングを追加ボタンがハイライト表示された Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-add-new-mapping.png)
1. Adobe Analytics の **[!UICONTROL ソースフィールドを選択]** ウィンドウで、 **[!UICONTROL ID 名前空間を選択]** ID を選択します。
   ![ID としてマッピングするソース属性として電子メールを選択した Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-select-source-identity.png)
1. Adobe Analytics の **[!UICONTROL ターゲットフィールドを選択]** ウィンドウで、 **[!UICONTROL 属性を選択]** を選択し、 `email`.
   ![ID としてマッピングするターゲット属性としてメールを選択した Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-select-target-identity.png)

| ソースフィールド | ターゲットフィールド | 必須 |
| --- | --- | --- |
| `IdentityMap: Email` | `Identity: email` | ○ |

ID マッピングの例を次に示します。
![電子メール ID マッピングを含む Platform UI のスクリーンショットの例。](../../assets/catalog/crm/hubspot/mapping-identities.png)

#### マッピング **オプション** 属性

XDM プロファイルスキーマと [!DNL HubSpot] アカウントは、以下の手順を繰り返します。
1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。これで、新しいマッピング行が画面に表示されます。
   ![新しいマッピングを追加ボタンがハイライト表示された Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-add-new-mapping.png)
1. Adobe Analytics の **[!UICONTROL ソースフィールドを選択]** ウィンドウで、 **[!UICONTROL 属性を選択]** カテゴリを選択し、XDM 属性を選択します。
   ![ソース属性として名を選択した Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-select-source-attribute.png)
1. Adobe Analytics の **[!UICONTROL ターゲットフィールドを選択]** ウィンドウ：選択 **[!UICONTROL 属性を選択]** カテゴリを選択し、属性のリストから選択します。属性のリストは、 [!DNL HubSpot] アカウント。 宛先は、 [[!DNL HubSpot] プロパティ](https://developers.hubspot.com/docs/api/crm/properties) この情報を取得する API です。 両方 [!DNL HubSpot] [デフォルトのプロパティ](https://knowledge.hubspot.com/contacts/hubspots-default-contact-properties) およびすべてのカスタムプロパティがターゲットフィールドとして取得され、選択できます。
   ![ターゲット属性として名を選択した Platform UI のスクリーンショット。](../../assets/catalog/crm/hubspot/mapping-select-target-attribute.png)

XDM プロファイルスキーマとの使用可能なマッピングの一部 [!DNL Hubspot] を次に示します。

| ソースフィールド | ターゲットフィールド |
| --- | --- |
| `xdm: person.name.firstName` | `Attribute: firstname` |
| `xdm: person.name.lastName` | `Attribute: lastname` |
| `xdm: workAddress.street1` | `Attribute: address` |
| `xdm: workAddress.city` | `Attribute: city` |
| `xdm: workAddress.country` | `Attribute: country` |

これらの属性のマッピングを使用する例を次に示します。
![属性マッピングを使用した Platform UI のスクリーンショットの例。](../../assets/catalog/crm/hubspot/mapping-attributes.png)

宛先接続のマッピングの指定が完了したら、「 」を選択します。 **[!UICONTROL 次へ]**.

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. にログインします。 [!DNL HubSpot] web サイトに移動し、 **[!UICONTROL 連絡先]** ページを開いて、オーディエンスのステータスを確認します。 このリストは、オーディエンス名を使用して作成されたカスタムプロパティの列を表示するように設定でき、その値がオーディエンスのステータスになります。
   ![オーディエンス名とセルのオーディエンスステータスを示す列ヘッダーを持つ連絡先ページを示す HubSpot UI スクリーンショット](../../assets/catalog/crm/hubspot/contacts.png)

1. 別の方法として、個人にドリルダウンすることもできます。 **[!UICONTROL 人物]** ページを開き、オーディエンス名とオーディエンスのステータスを表示しているプロパティに移動します。
   ![HubSpot UI のスクリーンショットに、オーディエンス名とオーディエンスのステータスを示すカスタムプロパティを含む連絡先ページが表示されています。](../../assets/catalog/crm/hubspot/contact.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

[!DNL HubSpot] ドキュメントからのその他の役に立つ情報は次のとおりです。
* [HubSpot での認証方法](https://developers.hubspot.com/docs/api/intro-to-auth)
* [!DNL HubSpot] の API リファレンス [連絡先](https://developers.hubspot.com/docs/api/crm/contacts) および [プロパティ](https://developers.hubspot.com/docs/api/crm/properties) API

### 変更ログ

この節では、この宛先コネクタに対する機能の概要と重要なドキュメントの更新について説明します。

+++ 変更ログを表示

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2023年9月 | 初回リリース | 宛先の初回リリースとドキュメントの公開。 |

{style="table-layout:auto"}

+++
