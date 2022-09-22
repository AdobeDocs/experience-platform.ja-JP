---
keywords: crm;CRM;crm の宛先；salesforce crm;salesforce crm の宛先
title: Salesforce CRM 接続
description: Salesforce CRM の宛先を使用すると、アカウントデータをエクスポートし、Salesforce CRM 内でビジネスニーズに合わせてアクティブ化できます。
exl-id: bd9cb656-d742-4a18-97a2-546d4056d093
source-git-commit: b243a5f88cadc238ac3edd3bf45a54564598bbf0
workflow-type: tm+mt
source-wordcount: '2256'
ht-degree: 4%

---

# [!DNL Salesforce CRM] 接続

## 概要 {#overview}

[[!DNL Salesforce CRM]](https://www.salesforce.com/crm/) は、人気のある顧客関係管理 (CRM) プラットフォームで、次のものをサポートします。

* [リード](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_lead.htm)  — リードとは、販売する製品やサービスに関心を持つ（または興味を持たない）人または会社の名前です。
* [連絡先](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_contact.htm)  — 連絡先とは、担当者の 1 人が関係を確立し、潜在的な顧客として認定された個人です。

この [!DNL Adobe Experience Platform] [宛先](/help/destinations/home.md) は [[!DNL Salesforce composite API]](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_composite_sobjects_collections_update.htm)：前述の両方のタイプのプロファイルをサポートします。

条件 [セグメントのアクティブ化](#activate)を使用する場合は、リードまたは連絡先のいずれかを選択し、属性とセグメントデータを [!DNL Salesforce CRM].

[!DNL Salesforce CRM] は、Salesforce REST API と通信するための認証メカニズムとして、パスワード付与付きの OAuth 2 を使用しています。 への認証手順 [!DNL Salesforce CRM] インスタンスは、 [宛先に対する認証](#authenticate) 」セクションに入力します。

## ユースケース {#use-cases}

マーケターは、Adobe Experience Platformプロファイルの属性に基づいて、ユーザーにパーソナライズされたエクスペリエンスを提供できます。 オフラインデータからセグメントを作成し、Salesforce CRM に送信して、Adobe Experience Platformでセグメントとプロファイルが更新されるとすぐにユーザーのフィードに表示できます。

## 前提条件 {#prerequisites}

### Experience Platformの前提条件 {#prerequisites-in-experience-platform}

Salesforce CRM の宛先に対してデータをアクティブ化する前に、 [スキーマ](/help/xdm/schema/composition.md), a [データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=en)、および [セグメント](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=en) 次で作成： [!DNL Experience Platform].

### の前提条件 [!DNL Salesforce CRM] {#prerequisites-destination}

次の前提条件に注意してください。 [!DNL Salesforce CRM]Platform から Salesforce アカウントにデータをエクスポートするには、次の手順を実行します。

#### Salesforce アカウントが必要です {#prerequisites-account}

Salesforce に移動 [裁判](https://www.salesforce.com/in/form/signup/freetrial-sales/) Salesforce アカウントを登録および作成するページ（まだ存在しない場合）。

#### 接続されたアプリの設定 {#prerequisites-connected-app}

次に、 [連携アプリ](https://help.salesforce.com/s/articleView?id=sf.connected_app_create.htm&amp;language=en_US&amp;r=https%3A%2F%2Fhelp.salesforce.com%2F&amp;type=5) Salesforce アカウント内にまだ存在しない場合は、を選択します。

接続されたアプリ内で、 [OAuth 設定](https://help.salesforce.com/s/articleView?id=connected_app_create_api_integration.htm&amp;type=5&amp;language=en_US) が有効になっている。

また、 [スコープ](https://help.salesforce.com/s/articleView?id=connected_app_create_api_integration.htm&amp;type=5&amp;language=en_US) 以下に示す項目を選択します。

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

#### Salesforce 内にカスタムフィールドを作成 {#prerequisites-custom-field}

タイプのカスタムフィールドの作成 `Text Area Long`:Experience Platformは、 [!DNL Salesforce CRM].
Salesforce のドキュメントを参照してください： [カスタムフィールドの作成](https://help.salesforce.com/s/articleView?id=sf.adding_fields.htm&amp;type=5) 追加のガイダンスが必要な場合は、を参照してください。

>[!IMPORTANT]
>
>フィールド名に空白文字が含まれていないことを確認します。 代わりに、アンダースコアを使用します。 `(_)` 文字を区切り文字として使用します。

>[!NOTE]
>
>* Salesforce 内のオブジェクトは 25 個の外部フィールドに制限されています。詳しくは、 [カスタムフィールド属性](https://help.salesforce.com/s/articleView?id=sf.custom_field_attributes.htm&amp;type=5).
>* この制限は、いつでもアクティブなExperience Platformセグメントメンバーシップを最大 25 個までにすることを意味します。
>* Salesforce 内でこの制限に達した場合は、新しい **[!UICONTROL マッピング ID]** を使用できます。


詳しくは、 Adobe Experience Platformのドキュメントを参照してください。 [セグメントメンバーシップの詳細スキーマフィールドグループ](/help/xdm/field-groups/profile/segmentation.md) セグメントのステータスに関するガイダンスが必要な場合。

#### Salesforce 認証情報の収集 {#gather-credentials}

を認証する前に、以下の項目をメモしておきます。 [!DNL Salesforce CRM] 宛先：

| 認証情報 | 説明 | 例 |
| --- | --- | --- |
| <ul><li>Salesforce ドメインプレフィックス</li></ul> | 詳しくは、 [Salesforce ドメインプレフィックス](https://help.salesforce.com/s/articleView?id=sf.domain_name_setting_login_policy.htm&amp;type=5) を参照してください。 | <ul><li>ドメインが以下の場合は、強調表示された値が必要です。<br> <i>`d5i000000isb4eak-dev-ed`.my.salesforce.com</i></li></ul> |
| <ul><li>消費者キー</li><li>消費者の秘密鍵</li></ul> | 詳しくは、 [Salesforce ドキュメント](https://help.salesforce.com/s/articleView?id=sf.connected_app_rotate_consumer_details.htm&amp;type=5) 追加のガイダンスが必要な場合は、を参照してください。 | <ul><li>r23kxxxxxxxxx0z05xxxxxxx</code></li><li>ipxxxxxxxxT4xxxxxxxx</code></li></ul> |

### ガードレール {#guardrails}

Salesforce は、要求、レート、タイムアウトの制限を課すことで、トランザクション読み込みのバランスを取ります。 詳しくは、 [API リクエストの制限と割り当て](https://developer.salesforce.com/docs/atlas.en-us.salesforce_app_limits_cheatsheet.meta/salesforce_app_limits_cheatsheet/salesforce_app_limits_platform_api.htm) 」を参照してください。

>[!IMPORTANT]
>
>条件 [セグメントのアクティブ化](#activate) 次のいずれかを選択する必要があります。 *連絡先* または *リード* タイプ。 選択したタイプに従って、セグメントに適切なデータマッピングがあることを確認する必要があります。

## サポートされる ID {#supported-identities}

[!DNL Salesforce CRM] では、以下の表で説明する id の更新をサポートしています。 詳細情報： [id](/help/identity-service/namespaces.md).

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| `SalesforceId` | この [!DNL Salesforce CRM] セグメントを通じて書き出しまたは更新する連絡先 ID またはリード ID の識別子。 | 必須 |

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li>セグメントのすべてのメンバーを、必要なスキーマフィールドと共に書き出します *( 例：メールアドレス、電話番号、姓*（フィールドマッピングに従います）</li><li> 各セグメントのステータス ( [!DNL Salesforce CRM] は、 **[!UICONTROL マッピング ID]** 期間中に指定された値 [セグメントスケジュール](#schedule-segment-export-example) 手順</li></ul> |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | <ul><li>ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations).</li></ul> |

{style=&quot;table-layout:auto&quot;}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションに記載されているフィールドに入力します。

内 **[!UICONTROL 宛先]** > **[!UICONTROL カタログ]** 検索 [!DNL Salesforce CRM]. または、 **[!UICONTROL CRM]** カテゴリ。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

![認証方法を示す Platform UI のスクリーンショット。](../../assets/catalog/crm/salesforce/authenticate-destination.png)

* **[!UICONTROL パスワード]**:Salesforce アカウントのパスワード。
* **[!UICONTROL カスタムドメイン]**:Salesforce ドメイン。
* **[!UICONTROL クライアント ID]**:Salesforce が連携したアプリ消費者キー。
* **[!UICONTROL クライアント秘密鍵]**:Salesforce が接続したアプリ消費者秘密鍵。
* **[!UICONTROL ユーザー名]**:Salesforce アカウントのユーザ名。

指定した詳細が有効な場合、UI に **[!UICONTROL 接続済み]** ステータスに緑色のチェックマークが付いている場合は、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。 UI でフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![宛先の詳細を示す Platform UI のスクリーンショット。](../../assets/catalog/crm/salesforce/destination-details.png)

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Salesforce ID タイプ]**:選択 **[!UICONTROL 連絡先]** 書き出しまたは更新する id のタイプがの場合 *連絡先*. 選択 **[!UICONTROL リード]** 書き出しまたは更新する id のタイプがの場合 *リード*.

### アラートの有効化 {#enable-alerts}

アラートを有効にして、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の指定が完了したら、 **[!UICONTROL 次へ]**.

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
>
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [ストリーミングセグメントの書き出し先に対するプロファイルとセグメントのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platformからにオーディエンスデータを正しく送信するには [!DNL Salesforce CRM] の宛先に移動する場合は、フィールドマッピングの手順を実行する必要があります。 マッピングは、Platform アカウント内の Experience Data Model(XDM) スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成することで構成されます。 XDM フィールドを [!DNL Salesforce CRM] 宛先フィールドは、次の手順に従います。

1. 内 **[!UICONTROL マッピング]** ステップ、選択 **[!UICONTROL 新しいマッピングを追加]**に値を指定しない場合、新しいマッピング行が画面に表示されます。
   ![「新しいマッピングを追加」の Platform UI のスクリーンショットの例。](../../assets/catalog/crm/salesforce/add-new-mapping.png)

1. 内 **[!UICONTROL ソースフィールドを選択]** ウィンドウで、 **[!UICONTROL ID 名前空間を選択]** または **[!UICONTROL 属性を選択]** カテゴリと選択 `crmID`.
   ![ソースマッピング用の Platform UI のスクリーンショットの例。](../../assets/catalog/crm/salesforce/source-mapping.png)

1. 内 **[!UICONTROL ターゲットフィールドを選択]** ウィンドウで、 **[!UICONTROL ID 名前空間を選択]** カテゴリと選択 `SalesforceId`.
   ![SalesforceId のターゲットマッピングを示すプラットフォーム UI のスクリーンショット。](../../assets/catalog/crm/salesforce/target-mapping-salesforceid.png)

   * XDM プロファイルスキーマと [!DNL Salesforce CRM] インスタンス：
   | XDM プロファイルスキーマ | [!DNL Salesforce CRM] インスタンス | 必須 |
   |---|---|---|
   | `crmID` | `SalesforceId` | ○ |

   * **[!UICONTROL カスタム属性を選択]**:ソースフィールドを、 **[!UICONTROL 属性名]** フィールドに入力します。 詳しくは、 [[!DNL Salesforce CRM] ドキュメント](https://help.salesforce.com/s/articleView?id=sf.custom_field_attributes.htm&amp;type=5) を参照してください。
      ![LastName の Target マッピングを示す Platform UI のスクリーンショット。](../../assets/catalog/crm/salesforce/target-mapping-lastname.png)

   * を使用して *連絡先* セグメント内で、Salesforce のオブジェクト参照を参照してください。 [連絡先](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_contact.htm) ：更新するフィールドのマッピングを定義します。
   * 必須フィールドは、 *必須*：前述のリンクのフィールドの説明で説明されています。
   * 書き出しまたは更新するフィールドに応じて、XDM プロファイルスキーマと [!DNL Salesforce CRM] インスタンス：

   | XDM プロファイルスキーマ | [!DNL Salesforce CRM] インスタンス | 備考 |
   | --- | --- | --- |
   | `person.name.lastName` | `LastName` | `Required`。連絡先の姓（最大 80 文字）。 |
   | `person.name.firstName` | `FirstName` | 連絡先の名（40 文字まで）。 |
   | `personalEmail.address` | `Email` | 連絡先の電子メールアドレス。 |

   * これらのマッピングの使用例を次に示します。
      ![Target マッピングを示した Platform UI のスクリーンショットの例。](../../assets/catalog/crm/salesforce/mappings-contacts.png)

   * を使用して *リード* セグメント内で、Salesforce のオブジェクト参照を参照してください。 [リード](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_lead.htm) ：更新するフィールドのマッピングを定義します。
   * 必須フィールドは、 *必須*：前述のリンクのフィールドの説明で説明されています。
   * 書き出しまたは更新するフィールドに応じて、XDM プロファイルスキーマと [!DNL Salesforce CRM] インスタンス：

   | XDM プロファイルスキーマ | [!DNL Salesforce CRM] インスタンス | 備考 |
   | --- | --- | --- |
   | `person.name.lastName` | `LastName` | `Required`。連絡先の姓（最大 80 文字）。 |
   | `b2b.companyName` | `Company` | `Required`。リードの会社。 |
   | `personalEmail.address` | `Email` | 連絡先の電子メールアドレス。 |

   * これらのマッピングの使用例を次に示します。
      ![Target マッピングを示した Platform UI のスクリーンショットの例。](../../assets/catalog/crm/salesforce/mappings-leads.png)




### セグメントのエクスポートと例をスケジュール {#schedule-segment-export-example}

実行時に [セグメントの書き出しをスケジュール](/help/destinations/ui/activate-segment-streaming-destinations.md#scheduling) 手順 Platform セグメントを Salesforce のカスタムフィールド属性に手動でマッピングする必要があります。

これをおこなうには、各セグメントを選択し、Salesforce から対応するカスタムフィールド属性を **[!UICONTROL マッピング ID]** フィールドに入力します。

>[!IMPORTANT]
>
>* に使用する値 **[!UICONTROL マッピング ID]** は、Salesforce 内で作成されたカスタムフィールド属性の名前と完全に一致する必要があります。
>* Salesforce で作成したカスタムフィールド属性の名前に空白文字が使用されていないことを確認してください。


次に例を示します。
![スケジュールセグメントの書き出しを示した、Platform UI のスクリーンショットの例。](../../assets/catalog/crm/salesforce/schedule-segment-export.png)

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. 選択 **[!UICONTROL 宛先]** > **[!UICONTROL 参照]** をクリックして、宛先のリストに移動します。
   ![参照先を示す Platform UI のスクリーンショット。](../../assets/catalog/crm/salesforce/browse-destinations.png)

1. 宛先を選択し、ステータスが「 **[!UICONTROL 有効]**.
   ![宛先のデータフロー実行を示した Platform UI のスクリーンショット。](../../assets/catalog/crm/salesforce/destination-dataflow-run.png)

1. 次に切り替え： **[!UICONTROL アクティベーションデータ]** 」タブをクリックし、セグメント名を選択します。
   ![宛先のアクティベーションデータを示した、プラットフォーム UI のスクリーンショットの例。](../../assets/catalog/crm/salesforce/destinations-activation-data.png)

1. セグメント概要を監視し、プロファイルの数がセグメント内で作成された数に一致していることを確認します。
   ![セグメントを示す Platform UI のスクリーンショットの例。](../../assets/catalog/crm/salesforce/segment.png)

1. 最後に、Salesforce Web サイトにログインし、セグメントのプロファイルが追加または更新されたかどうかを検証します。
   * もし *連絡先* Platform セグメント内で、 **[!DNL Apps]** > **[!DNL Contacts]** ページ。
      ![Salesforce CRM のスクリーンショットに、セグメントのプロファイルを含む連絡先ページが表示されました。](../../assets/catalog/crm/salesforce/contacts.png)

   * を選択します。 *連絡先* フィールドが更新されたかどうかを確認します。 各セグメントのステータスは、 [!DNL Salesforce CRM] は、 **[!UICONTROL マッピング ID]** 期間中に指定された値 [セグメントスケジュール](#schedule-segment-export-example).
      ![連絡先詳細ページが表示された Salesforce CRM スクリーンショットと、更新されたセグメントステータス。](../../assets/catalog/crm/salesforce/contact-info.png)

   * もし *リード* を Platform セグメント内でクリックし、 **[!DNL Apps]** > **[!DNL Leads]** ページ。
      ![セグメントのプロファイルを含むリードページを示す Salesforce CRM のスクリーンショット。](../../assets/catalog/crm/salesforce/leads.png)

   * を選択します。 *リード* フィールドが更新されたかどうかを確認します。 各セグメントのステータスは、 [!DNL Salesforce CRM] は、 **[!UICONTROL マッピング ID]** 期間中に指定された値 [セグメントスケジュール](#schedule-segment-export-example).
      ![Salesforce CRM のスクリーンショットに、リード詳細ページと更新されたセグメントステータスが表示されています。](../../assets/catalog/crm/salesforce/lead-info.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](/help/data-governance/home.md).

## エラーとトラブルシューティング {#errors-and-troubleshooting}

### イベントを宛先にプッシュする際に不明なエラーが発生しました {#unknown-errors}

データフローの実行をチェックする際に、次のエラーメッセージが表示される場合。 `Unknown errors encountered while pushing events to the destination. Please contact the administrator and try again.`

![エラーを示す Platform UI のスクリーンショット。](../../assets/catalog/crm/salesforce/error.png)

このエラーを修正するには、 **[!UICONTROL マッピング ID]** 指定した [!DNL Salesforce CRM] は、Platform セグメントが有効で、内に存在する [!DNL Salesforce CRM].

## その他のリソース {#additional-resources}

次の役に立つ追加情報： [Salesforce 開発者ポータル](https://developer.salesforce.com/) は以下です。
* [クイックスタート](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/quickstart.htm)
* [レコードの作成](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_sobject_create.htm)
* [カスタム Recommendation オーディエンス](https://developer.salesforce.com/docs/atlas.en-us.236.0.chatterapi.meta/chatterapi/connect_resources_recommendation_audiences_list.htm)
* [複合リソースの使用](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/using_composite_resources.htm?q=composite)
* この宛先では [複数のレコードのアップサート](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_composite_sobjects_collections_update.htm) API( [単一レコードのアップサート](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_composite_upsert_example.htm?q=contacts) API 呼び出し。