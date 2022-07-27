---
keywords: 電子メール；電子メール；電子メールの宛先；salesforce;api salesforce marketing cloud の宛先
title: (API)SalesforceMarketing Cloud接続
description: SalesforceMarketing Cloud（旧称 ExactTarget）の宛先を使用すると、アカウントデータを書き出し、SalesforceMarketing Cloud内でビジネスニーズに合わせてアクティブ化できます。
source-git-commit: ce7b28ce31c652965a6eaad81348e330bd38e9ac
workflow-type: tm+mt
source-wordcount: '1869'
ht-degree: 6%

---


# [!DNL (API) Salesforce Marketing Cloud] 接続

## 概要 {#overview}

[[!DNL Salesforce Marketing Cloud]](https://www.salesforce.com/products/marketing-cloud/overview/) （旧称：ExactTarget）は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです。

>[!IMPORTANT]
> 
> この接続と他の接続の違いに注意してください [SalesforceMarketing Cloud接続](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/email-marketing/salesforce-marketing-cloud.html?lang=en) 「電子メールマーケティングカタログ」セクション内に存在する 他の SalesforceMarketing Cloud接続では、指定した格納場所にファイルを書き出すことができますが、これは API ベースのストリーミング接続です。

この [!DNL Adobe Experience Platform] [宛先](/help/destinations/home.md) は [Salesforce 連絡先更新 REST API](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/updateContacts.html)：新しい Salesforce セグメント内でアクティブ化した後、ビジネスニーズに合わせて連絡先を追加/連絡先データを更新できます。

SalesforceMarketing Cloudは、Salesforce REST API と通信するための認証メカニズムとして、クライアント資格情報付きの OAuth 2 を使用します。 Salesforce インスタンスに対する認証手順は、以下のとおりです。 [宛先に対する認証](#authenticate) 」セクションに入力します。

## ユースケース {#use-cases}

Adobe Experience Platformのお客様が Salesforce のMarketing Cloud先をいつどのように使用するかをより深く理解できるように、この宛先を使用して解決できる使用例を以下に示します。

### マーケティングキャンペーン用の連絡先へのメール送信 {#use-case-send-emails}

レンタルホームプラットフォームの販売部門は、ターゲットとなる顧客オーディエンスにマーケティングメールを放送したいと考えています。 プラットフォームのマーケティングチームは、新しい連絡先を追加したり、既存の連絡先を更新したりできます *（およびその電子メールアドレス）* Adobe Experience Platformを通じて、独自のオフラインデータからセグメントを作成し、SalesforceMarketing Cloudに送信します。これにより、マーケティングキャンペーン電子メールの送信に使用できます。

## 前提条件 {#prerequisites}

### Experience Platformの前提条件 {#prerequisites-in-experience-platform}

Salesforce の宛先に対してデータをアクティブ化する前に、Marketing Cloud先に [スキーマ](/help/xdm/schema/composition.md), a [データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=en)、および [セグメント](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=en) 次で作成： [!DNL Experience Platform].

### Salesforce CRM の前提条件 {#prerequisites-destination}

Platform から SalesforceMarketing Cloudアカウントにデータを書き出すための、Salesforce の次の前提条件に注意してください。

#### Salesforce アカウントが必要です {#prerequisites-account}

Salesforce に移動 [裁判](https://www.salesforce.com/in/form/signup/freetrial-sales/) Salesforce アカウントを登録および作成するページ（まだ存在しない場合）。

#### Salesforce 内にカスタムフィールドを作成 {#prerequisites-custom-field}

タイプのカスタム属性を作成 `Text Area Long` どのExperience Platformが SalesforceMarketing Cloud内でセグメントのステータスを更新する際に使用するか。

SalesforceMarketing Cloudドキュメントを参照： [カスタムフィールドの作成](https://help.salesforce.com/s/articleView?id=mc_cab_create_an_attribute.htm&amp;type=5&amp;language=en_US) 追加のガイダンスが必要な場合は、を参照してください。

>[!IMPORTANT]
>
> SalesforceMarketing Cloudアカウント内の「メール人口統計」属性セットの下にカスタム属性を作成してください。

>[!NOTE]
>
> * 各オブジェクトで許可されるカスタム属性の数は、Salesforce エディションによって異なります。 Salesforce のドキュメントを参照してください。 [オブジェクトごとに許可されるカスタムフィールド](https://help.salesforce.com/s/articleView?id=sf.custom_field_allocations.htm&amp;type=5) 追加のガイダンスが必要な場合は、を参照してください。
> * Salesforce 内でこの制限に達した場合、新しい mappingId を使用する前に、Experience Platform内の古いセグメントに対するセグメントのステータスを保存するために使用されたカスタム属性を Salesforce から削除する必要があります。


詳しくは、 Adobe Experience Platformのドキュメントを参照してください。 [セグメントメンバーシップの詳細スキーマフィールドグループ](/help/xdm/field-groups/profile/segmentation.md) セグメントのステータスに関するガイダンスが必要な場合。

#### Salesforce 認証情報の収集 {#gather-credentials}

Salesforce の宛先に対する認証を行う前に、以下の項目をメモしておきますMarketing Cloud。

| 認証情報 | 説明 | 例 |
| --- | --- | --- |
| <ul><li>SalesforceMarketing Cloudプレフィックス</li></ul> | 詳しくは、 [SalesforceMarketing Cloudドメインプレフィックス](https://help.salesforce.com/s/articleView?id=sf.domain_name_setting_login_policy.htm&amp;type=5) を参照してください。 | <ul><li>ドメインが以下の場合は、強調表示された値が必要です。<br> <i>`mcq4jrssqdlyc4lph19nnqgzzs84`.login.exacttarget.com</i></li></ul> |
| <ul><li>クライアント ID</li><li>クライアント秘密鍵</li></ul> | 詳しくは、 [Salesforce ドキュメント](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/access-token-s2s.html) 追加のガイダンスが必要な場合は、を参照してください。 | <ul><li>r23kxxxxxxxxx0z05xxxxxxx</li><li>ipxxxxxxxxT4xxxxxxxx</li></ul> |

## サポートされる ID {#supported-identities}

SalesforceMarketing Cloudは、以下の表で説明する ID のアクティベーションをサポートしています。 詳細情報： [id](/help/identity-service/namespaces.md).

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| contactKey | Salesforce 連絡先キー。 詳しくは、 [Salesforce ドキュメント](https://help.salesforce.com/s/articleView?id=sf.mc_cab_contact_builder_best_practices.htm&amp;type=5) 追加のガイダンスが必要な場合は、を参照してください。 | 必須 |

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：（電子メールアドレス、電話番号、姓）。「プロファイル属性を選択」画面で選択します。 [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションに記載されているフィールドに入力します。

![カタログ](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/catalog.png)

### 宛先に対する認証 {#authenticate}

宛先を認証するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

![SalesforceMarketing Cloudの認証方法を示すサンプルスクリーンショット](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/authenticate-destination.png)

* **[!UICONTROL サブドメイン]**:SalesforceMarketing Cloudドメインプレフィックス。 例えば、ドメインが *`mcq4jrssqdlyc4lph19nnqgzzs84`.login.exacttarget.com*&#x200B;ハイライト表示された値が必要です。
* **[!UICONTROL クライアント ID]**:Salesforce クライアント ID。
* **[!UICONTROL クライアント秘密鍵]**:Salesforce クライアント秘密鍵。

指定した詳細が有効な場合、UI に **接続済み** ステータスに緑色のチェックマークが付いている場合は、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。 UI でフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。

![SalesforceMarketing Cloudの詳細を入力する方法を示すサンプルスクリーンショット](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/destination-details.png)

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL 顧客名]**:任意の値を指定できますが、値は必須です。 そうしないと、宛先のアクティベーションが失敗します。

### アラートの有効化 {#enable-alerts}

アラートを有効にして、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の指定が完了したら、 **[!UICONTROL 次へ]**.

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [ストリーミングセグメントの書き出し先に対するプロファイルとセグメントのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platformから Salesforce のMarketing Cloud先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。 マッピングは、Platform アカウント内の Experience Data Model(XDM) スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成することで構成されます。 XDM フィールドを Salesforce 宛先フィールドに正しくマッピングするには、次の手順に従います。Marketing Cloud

属性マッピングのリスト。 [Salesforce REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_composite_upsert_example.htm?q=contacts) は以下に示されます。 宛先は、 [Salesforce 検索属性セット定義 REST API](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/retrieveAttributeSetDefinitions.html) ：連絡先の Salesforce 内で定義され、お使いのアカウントに固有の属性を取得します。

>[!IMPORTANT]
> 
> 属性名は Salesforce アカウントに従いますが、 `contactKey` および `personalEmail.address` は必須です。

1. マッピングの手順で、 **[!UICONTROL 新しいマッピングを追加]**に値を指定しない場合、新しいマッピング行が画面に表示されます。
   ![新しいマッピングを追加](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/add-new-mapping.png)

1. ソースフィールドを選択ウィンドウで、ソースフィールドを選択する際に、 **[!UICONTROL 属性を選択]** 」カテゴリに追加し、必要なマッピングを追加します。
   ![ソースマッピング](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/source-mapping.png)

1. ターゲットフィールドを選択ウィンドウで、ターゲットフィールドを選択し、 **[!UICONTROL ID 名前空間を選択]** 」カテゴリに追加し、必要なマッピングを追加します。
   ![ターゲットマッピング](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/target-mapping.png)

1. カスタム属性をマッピングするには、ターゲットフィールドウィンドウを選択し、ターゲットフィールドを選択して、 **[!UICONTROL 属性を選択]** > **メール人口統計** カテゴリ。 次に、目的のターゲット属性名を指定し、必要なマッピングを追加します。
   ![ターゲットマッピング](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/target-mapping-custom.png)

1. 例えば、XDM プロファイルスキーマと [!DNL Salesforce Marketing Cloud] インスタンス：

   |  | XDM プロファイルスキーマ | [!DNL Salesforce Marketing Cloud] インスタンス | 必須 |
   |---|---|---|---|
   | 属性 | <ul><li>person.name.firstName</code></li><li>personalEmail.address</code></li></ul> | <ul><li>メール人口統計。名</code></li><li>メールアドレス。メールアドレス</code></li></ul> | <ul><li>-</li><li>○</code></li></ul> |
   | ID | <ul><li>contactKey</code></li></ul> | <ul><li>salesforceContactKey</code></li></ul> | ○ |

1. これらのマッピングの使用例を次に示します。
   ![ターゲットマッピングの LastName](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/mappings.png)

### セグメントのエクスポートと例をスケジュール {#schedule-segment-export-example}

実行時に [セグメントの書き出しをスケジュール](/help/destinations/ui/activate-segment-streaming-destinations.md#scheduling) 手順 Platform セグメントを Salesforce のカスタム属性に手動でマッピングする必要があります。

これをおこなうには、各セグメントを選択し、Salesforce から対応するカスタム属性を **[!UICONTROL マッピング ID]** フィールドに入力します。

>[!IMPORTANT]
>
> マッピング ID に使用する値は、「Email Demographics」属性セットの下で Salesforce 内で作成されたカスタム属性の名前と完全に一致する必要があります。

次に例を示します。
![セグメントの書き出しをスケジュール](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/schedule-segment-export.png)

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. 選択 **[!UICONTROL 宛先]** > **[!UICONTROL 参照]** をクリックして、宛先のリストに移動します。

   ![宛先を参照](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/browse-destinations.png)

1. 宛先を選択し、ステータスが「 **[!UICONTROL 有効]**.

   ![宛先のデータフローの実行](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/destination-dataflow-run.png)

1. 次に切り替え： **[!DNL Activation data]** 」タブをクリックし、セグメント名を選択します。

   ![宛先のアクティベーションデータ](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/destinations-activation-data.png)

1. セグメント概要を監視し、プロファイルの数がセグメント内で作成された数に一致していることを確認します。

   ![セグメント](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/segment.png)

1. SalesforceMarketing CloudWeb サイトにログインします。 次に、 **[!DNL Audience Builder]** > **[!DNL Contact Builder]** > **[!DNL All contacts]** > **[!DNL Email]** ページを開き、セグメントのプロファイルが追加されたかどうかを確認します。

   ![Salesforce 連絡先](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/contacts.png)

1. プロファイルが更新されているかどうかを確認するには、 **[!DNL Email]** ページで、セグメントのプロファイルの属性値が更新されているかどうかを確認します。

   ![Salesforce 連絡先](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/contact-detail.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](/help/data-governance/home.md).

## エラーとトラブルシューティング {#errors-and-troubleshooting}

### イベントを SalesforceMarketing Cloudにプッシュ中に不明なエラーが発生しました {#unknown-errors}

データフローの実行をチェックする際に、以下のエラーメッセージが表示される場合は、で指定したマッピング ID を確認します。 [!DNL Salesforce CRM] は、Platform セグメントが有効で、内に存在する [!DNL Salesforce CRM].
![エラー](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/error.png)

## その他のリソース {#additional-resources}

* [Salesforce 開発者ポータル](https://developer.salesforce.com/)

### 制限 {#limits}

* Salesforce が特定の [レート制限](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/rate-limiting.html).
* 詳しくは、 [レート制限エラー](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/rate-limiting-errors.html) ページを使用して、発生する可能性のあるエラーを確認します。
* 詳しくは、 [SalesforceMarketing Cloudエンゲージメント価格](https://www.salesforce.com/editions-pricing/marketing-cloud/email/) ページ *Full Edition 比較グラフのダウンロード* プランで課せられた制限の詳細を示す pdf 形式
* この [API の概要](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/apis-overview.html) ページの詳細に関する追加の制限。
* これらの詳細を照合する KB 項目が利用可能です [ここ](https://salesforce.stackexchange.com/questions/205898/marketing-cloud-api-limits#:~:text=Day%2FHour%2FMinute%20Limit&amp;text=We%20recommend%20a%20limit%20of,per%20minute%20for%20SOAP%20calls.&amp;text=As%20has%20been%20added%20in,interacting%20with%20the%20REST%2DAPI).

