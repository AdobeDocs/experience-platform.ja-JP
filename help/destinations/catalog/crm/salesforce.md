---
keywords: crm;CRM;crm の宛先；salesforce crm;salesforce crm の宛先
title: Salesforce CRM 接続
description: Salesforce CRM の宛先を使用すると、アカウントデータをエクスポートし、Salesforce CRM 内でビジネスニーズに合わせてアクティブ化できます。
exl-id: bd9cb656-d742-4a18-97a2-546d4056d093
source-git-commit: 7d8b25b25b53edd7de836fc508de15cfa2a5a3a2
workflow-type: tm+mt
source-wordcount: '1730'
ht-degree: 6%

---

# [!DNL Salesforce CRM] 接続

## 概要 {#overview}

[Salesforce CRM](https://www.salesforce.com/) は、一般的な顧客関係管理 (CRM) プラットフォームです。

この [!DNL Adobe Experience Platform] [宛先](/help/destinations/home.md) は [Salesforce REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_composite_upsert_example.htm?q=contacts)：セグメント内の id を Salesforce CRM に更新できます。

Salesforce CRM は、Salesforce REST API と通信するための認証メカニズムとして、パスワード付与付き OAuth 2 を使用します。 Salesforce CRM インスタンスに対する認証手順は、以下の通りです。 [宛先に対する認証](#authenticate) 」セクションに入力します。

## ユースケース {#use-cases}

マーケターは、Adobe Experience Platformプロファイルの属性に基づいて、ユーザーにパーソナライズされたエクスペリエンスを提供できます。 オフラインデータからセグメントを作成し、Salesforce CRM に送信して、Adobe Experience Platformでセグメントとプロファイルが更新されるとすぐにユーザーのフィードに表示できます。

## 前提条件 {#prerequisites}

### Experience Platformの前提条件 {#prerequisites-in-experience-platform}

Salesforce CRM の宛先に対してデータをアクティブ化する前に、 [スキーマ](/help/xdm/schema/composition.md), a [データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=en)、および [セグメント](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=en) 次で作成： [!DNL Experience Platform].

### Salesforce CRM の前提条件 {#prerequisites-destination}

Platform から Salesforce アカウントにデータを書き出すための、Salesforce の次の前提条件に注意してください。

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

タイプのカスタムフィールドの作成 `Text Area Long` Salesforce CRM 内でセグメントステータスを更新する際に使用するExperience Platform。
Salesforce のドキュメントを参照してください： [カスタムフィールドの作成](https://help.salesforce.com/s/articleView?id=sf.adding_fields.htm&amp;type=5) 追加のガイダンスが必要な場合は、を参照してください。

>[!IMPORTANT]
>
> フィールド名に空白文字が含まれていないことを確認します。 代わりに、アンダースコアを使用します。 `(_)` 文字を区切り文字として使用します。

>[!NOTE]
>
> * Salesforce 内のオブジェクトは 25 個の外部フィールドに制限されています。詳しくは、 [カスタムフィールド属性](https://help.salesforce.com/s/articleView?id=sf.custom_field_attributes.htm&amp;type=5).
> * この制限は、いつでもアクティブなExperience Platformセグメントメンバーシップを最大 25 個に制限できることを意味します。
> * Salesforce 内でこの制限に達した場合、新しい mappingId を使用する前に、Experience Platform内の古いセグメントに対するセグメントのステータスを保存するために使用されたカスタム属性を Salesforce から削除する必要があります。


詳しくは、 Adobe Experience Platformのドキュメントを参照してください。 [セグメントメンバーシップの詳細スキーマフィールドグループ](/help/xdm/field-groups/profile/segmentation.md) セグメントのステータスに関するガイダンスが必要な場合。

#### Salesforce 認証情報の収集 {#gather-credentials}

Salesforce CRM の宛先に対して認証を行う前に、以下の項目をメモしておきます。

| 認証情報 | 説明 | 例 |
| --- | --- | --- |
| <ul><li>Salesforce ドメインプレフィックス</li></ul> | 詳しくは、 [Salesforce ドメインプレフィックス](https://help.salesforce.com/s/articleView?id=sf.domain_name_setting_login_policy.htm&amp;type=5) を参照してください。 | <ul><li>ドメインが以下の場合は、強調表示された値が必要です。<br> <i>`d5i000000isb4eak-dev-ed`.my.salesforce.com</i></li></ul> |
| <ul><li>消費者キー</li><li>消費者の秘密鍵</li></ul> | 詳しくは、 [Salesforce ドキュメント](https://help.salesforce.com/s/articleView?id=sf.connected_app_rotate_consumer_details.htm&amp;type=5) 追加のガイダンスが必要な場合は、を参照してください。 | <ul><li>r23kxxxxxxxxx0z05xxxxxxx</li><li>ipxxxxxxxxT4xxxxxxxx</li></ul> |

## サポートされる ID {#supported-identities}

Salesforce CRM では、以下の表で説明する ID の更新をサポートしています。 詳細情報： [id](/help/identity-service/namespaces.md).

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| SalesforceId | 任意の ID のマッピングをサポートするカスタム Salesforce CRM 識別子。 | 必須。任意の [id](../../../identity-service/namespaces.md) から [!DNL Salesforce CRM] 宛先にマッピングする場合は、 `SalesforceId`. |

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li>セグメントのすべてのメンバーを、必要なスキーマフィールドと共に書き出します *( 例：メールアドレス、電話番号、姓*（フィールドマッピングに従います）</li><li> Platform セグメントのステータスは、に書き出されます。 [!DNL Salesforce CRM] で対応するカスタムフィールド属性を指定します。 [!DNL Salesforce CRM] 内 **[!UICONTROL 宛先を有効化]** > **[!UICONTROL セグメントの書き出しをスケジュール]** > **[!UICONTROL マッピング ID]** フィールドに入力します。</li></ul> |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | <ul><li>ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations).</li></ul> |

{style=&quot;table-layout:auto&quot;}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションに記載されているフィールドに入力します。

![カタログ](../../assets/catalog/crm/salesforce/catalog.png)

### 宛先に対する認証 {#authenticate}

宛先を認証するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

![Salesforce CRM への認証方法を示すサンプルスクリーンショット](../../assets/catalog/crm/salesforce/authenticate-destination.png)

* **[!UICONTROL パスワード]**:Salesforce アカウントのパスワード。
* **[!UICONTROL クライアント ID]**:Salesforce が連携したアプリ消費者キー。
* **[!UICONTROL クライアント秘密鍵]**:Salesforce が接続したアプリ消費者秘密鍵。
* **[!UICONTROL ユーザー名]**:Salesforce アカウントのユーザ名。

指定した詳細が有効な場合、UI に **接続済み** ステータスに緑色のチェックマークが付いている場合は、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。 UI でフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![Salesforce CRM の詳細を入力する方法を示すサンプルスクリーンショット](../../assets/catalog/crm/salesforce/destination-details.png)

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL カスタムドメイン]**:Salesforce ドメイン。

### アラートの有効化 {#enable-alerts}

アラートを有効にして、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の指定が完了したら、 **[!UICONTROL 次へ]**.

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [ストリーミングセグメントの書き出し先に対するプロファイルとセグメントのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platformから Salesforce CRM の宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。 マッピングは、Platform アカウント内の Experience Data Model(XDM) スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成することで構成されます。 XDM フィールドを Salesforce CRM 宛先フィールドに正しくマッピングするには、次の手順に従います。

1. マッピングの手順で、 **[!UICONTROL 新しいマッピングを追加]**&#x200B;に値を指定しない場合、新しいマッピング行が画面に表示されます。

   ![新しいマッピングを追加](../../assets/catalog/crm/salesforce/add-new-mapping.png)

1. ソースフィールドを選択ウィンドウで、ソースフィールドを選択する際に、 **[!UICONTROL 属性を選択]** 」カテゴリに追加し、必要なマッピングを追加します。

   ![ソースマッピング](../../assets/catalog/crm/salesforce/source-mapping.png)

1. ターゲットフィールドを選択ウィンドウで、ターゲットフィールドを選択し、 **[!UICONTROL ID 名前空間を選択]** 」カテゴリに追加し、必要なマッピングを追加します。

   ![SalesforceId を使用したターゲットマッピング](../../assets/catalog/crm/salesforce/target-mapping-salesforceid.png)

1. カスタム属性の場合、ターゲットフィールドを選択ウィンドウで、ターゲットフィールドを選択し、 **[!UICONTROL カスタム属性を選択]** 「 」カテゴリで、目的のターゲット属性名を指定し、必要なマッピングを追加します。

   ![LastName を使用したターゲットマッピング](../../assets/catalog/crm/salesforce/target-mapping-lastname.png)

1. 例えば、XDM プロファイルスキーマと [!DNL Salesforce CRM] インスタンス：

   |  | XDM プロファイルスキーマ | [!DNL Salesforce CRM] インスタンス | 必須 |
   |---|---|---|---|
   | 属性 | <ul><li>person.name.firstName</code></li><li>person.name.lastName</code></li><li>personalEmail.address</code></li></ul> | <ul><li>名</code></li><li>姓</code></li><li>メール</code></li></ul> |
   | ID | <ul><li>crmID</code></li></ul> | <ul><li>SalesforceId</code></li></ul> | ○ |

1. これらのマッピングの使用例を次に示します。

   ![ターゲットマッピング](../../assets/catalog/crm/salesforce/mappings.png)

### セグメントのエクスポートと例をスケジュール {#schedule-segment-export-example}

実行時に [セグメントの書き出しをスケジュール](/help/destinations/ui/activate-segment-streaming-destinations.md#scheduling) 手順 Platform セグメントを Salesforce のカスタムフィールド属性に手動でマッピングする必要があります。

これをおこなうには、各セグメントを選択し、Salesforce から対応するカスタムフィールド属性を **[!UICONTROL マッピング ID]** フィールドに入力します。

>[!IMPORTANT]
>
>* に使用する値 **[!UICONTROL マッピング ID]** は、Salesforce 内で作成されたカスタムフィールド属性の名前と完全に一致する必要があります。
>* Salesforce で作成したカスタムフィールド属性の名前に空白文字が使用されていないことを確認してください。


次に例を示します。
![セグメントの書き出しをスケジュール](../../assets/catalog/crm/salesforce/schedule-segment-export.png)

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. 選択 **[!UICONTROL 宛先]** > **[!UICONTROL 参照]** をクリックして、宛先のリストに移動します。
   ![宛先を参照](../../assets/catalog/crm/salesforce/browse-destinations.png)

1. 宛先を選択し、ステータスが「 **[!UICONTROL 有効]**.
   ![宛先のデータフローの実行](../../assets/catalog/crm/salesforce/destination-dataflow-run.png)

1. 次に切り替え： **[!DNL Activation data]** 」タブをクリックし、セグメント名を選択します。
   ![宛先のアクティベーションデータ](../../assets/catalog/crm/salesforce/destinations-activation-data.png)

1. セグメント概要を監視し、プロファイルの数がセグメント内で作成された数に一致していることを確認します。
   ![セグメント](../../assets/catalog/crm/salesforce/segment.png)

1. Salesforce Web サイトにログインし、 **[!DNL Apps]** > **[!DNL Contacts]** ページを開き、セグメントのプロファイルが追加されたかどうかを確認します。
   ![Salesforce 連絡先](../../assets/catalog/crm/salesforce/contacts.png)

1. 連絡先をクリックし、フィールドが更新されているかどうかを確認します。 Experience Platformのセグメントステータスが、 **マッピング ID** 次の期間のフィールド **[!UICONTROL 宛先を有効化]** > **[!UICONTROL セグメントの書き出しをスケジュール]** 手順
   ![Salesforce 連絡先](../../assets/catalog/crm/salesforce/contact-info.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](/help/data-governance/home.md).

## エラーとトラブルシューティング {#errors-and-troubleshooting}

### イベントを宛先にプッシュする際に不明なエラーが発生しました {#unknown-errors}

データフローの実行をチェックする際に、以下のエラーメッセージが表示される場合は、で指定したマッピング ID を確認します。 [!DNL Salesforce CRM] は、Platform セグメントが有効で、内に存在する [!DNL Salesforce CRM].
![エラー](../../assets/catalog/crm/salesforce/error.png)

## その他のリソース {#additional-resources}

次の役に立つ追加情報： [Salesforce 開発者ポータル](https://developer.salesforce.com/) は以下です。
* [レコードの作成](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_sobject_create.htm)
* [カスタム Recommendation オーディエンス](https://developer.salesforce.com/docs/atlas.en-us.236.0.chatterapi.meta/chatterapi/connect_resources_recommendation_audiences_list.htm)
* [複合リソースの使用](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/using_composite_resources.htm?q=composite)
* [クイックスタート](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/quickstart.htm)

### 制限 {#limits}

Salesforce は、要求、レート、タイムアウトの制限を課すことで、トランザクション読み込みのバランスを取ります。 詳しくは、 [API リクエストの制限と割り当て](https://developer.salesforce.com/docs/atlas.en-us.salesforce_app_limits_cheatsheet.meta/salesforce_app_limits_cheatsheet/salesforce_app_limits_platform_api.htm) 」を参照してください。
