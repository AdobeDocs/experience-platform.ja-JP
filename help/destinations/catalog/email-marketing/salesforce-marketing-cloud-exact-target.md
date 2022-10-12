---
keywords: 電子メール；電子メール；電子メールの宛先；salesforce;api salesforce marketing cloud の宛先
title: (API)SalesforceMarketing Cloud接続
description: SalesforceMarketing Cloud（旧称 ExactTarget）の宛先を使用すると、アカウントデータを書き出し、SalesforceMarketing Cloud内でビジネスニーズに合わせてアクティブ化できます。
exl-id: 0cf068e6-8a0a-4292-a7ec-c40508846e27
source-git-commit: 2c778fe087a04261c79b9daf8579666cd0794eb4
workflow-type: tm+mt
source-wordcount: '1912'
ht-degree: 5%

---

# [!DNL (API) Salesforce Marketing Cloud] 接続

## 概要 {#overview}

[[!DNL Salesforce Marketing Cloud]](https://www.salesforce.com/products/marketing-cloud/overview/) ( 旧称： [!DNL ExactTarget]) は、訪問者や顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです。

>[!IMPORTANT]
>
>この接続と他の接続の違いに注意してください [[!DNL Salesforce Marketing Cloud] 接続](/help/destinations/catalog/email-marketing/salesforce-marketing-cloud.md) 「電子メールマーケティングカタログ」セクション内に存在する 他の SalesforceMarketing Cloud接続では、指定した格納場所にファイルを書き出すことができますが、これは API ベースのストリーミング接続です。

この [!DNL Adobe Experience Platform] [宛先](/help/destinations/home.md) は [!DNL Salesforce Marketing Cloud] [連絡先を更新](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/updateContacts.html) 新しい内でアクティブ化した後、ビジネスニーズに合わせて連絡先を追加/連絡先データを更新できる API [!DNL Salesforce Marketing Cloud] セグメント。

[!DNL Salesforce Marketing Cloud] は、OAuth 2 とクライアント資格情報を使用し、 [!DNL Salesforce Marketing Cloud] API への認証手順 [!DNL Salesforce Marketing Cloud] インスタンスは、 [宛先に対する認証](#authenticate) 」セクションに入力します。

## ユースケース {#use-cases}

をいつどのように使用するかをより深く理解するのに役立ちます。 [!DNL Salesforce Marketing Cloud] の宛先について、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### マーケティングキャンペーン用の連絡先へのメール送信 {#use-case-send-emails}

レンタルホームプラットフォームの販売部門は、ターゲットとなる顧客オーディエンスにマーケティングメールを放送したいと考えています。 プラットフォームのマーケティングチームは、新しい連絡先を追加したり、既存の連絡先を更新したりできます *（およびその電子メールアドレス）* Adobe Experience Platformを通じて、独自のオフラインデータからセグメントを作成し、それらのセグメントをに送信する [!DNL Salesforce Marketing Cloud]：マーケティングキャンペーンの電子メールの送信に使用できます。

## 前提条件 {#prerequisites}

### Experience Platformの前提条件 {#prerequisites-in-experience-platform}

に対してデータをアクティブ化する前に [!DNL Salesforce Marketing Cloud] 宛先の [スキーマ](/help/xdm/schema/composition.md), a [データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=en)、および [セグメント](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=en) 次で作成： [!DNL Experience Platform].

### の前提条件 [!DNL Salesforce Marketing Cloud] {#prerequisites-destination}

Platform からにデータを書き出すための次の前提条件に注意してください。 [!DNL Salesforce Marketing Cloud] アカウント：

#### 次の条件を満たすには、 [!DNL Salesforce Marketing Cloud] アカウント {#prerequisites-account}

次のページに移動： [!DNL Salesforce Account Executive] 購読する [!DNL Salesforce Marketing Cloud Account Engagement] まだお持ちでない場合は、製品を使用します。

#### 内にカスタムフィールドを作成 [!DNL Salesforce Marketing Cloud] {#prerequisites-custom-field}

タイプのカスタム属性を作成する必要があります `Text Area Long`:Experience Platformは、 [!DNL Salesforce Marketing Cloud]. 宛先に対してセグメントをアクティブ化するワークフローで、 **[セグメントスケジュール](#schedule-segment-export-example)** 手順では、カスタム属性を **[!UICONTROL マッピング ID]** アクティブ化する各セグメントに対して。

詳しくは、 [!DNL Salesforce Marketing Cloud] ～に関する文書 [カスタムフィールドの作成](https://help.salesforce.com/s/articleView?id=mc_cab_create_an_attribute.htm&amp;type=5&amp;language=en_US) 追加のガイダンスが必要な場合は、を参照してください。

>[!IMPORTANT]
>
> カスタム属性は、 `Email Demographics` 属性セットを [!DNL Salesforce Marketing Cloud] アカウント

詳しくは、 Adobe Experience Platformのドキュメントを参照してください。 [セグメントメンバーシップの詳細スキーマフィールドグループ](/help/xdm/field-groups/profile/segmentation.md) セグメントのステータスに関するガイダンスが必要な場合。

#### Salesforce 認証情報の収集 {#gather-credentials}

を認証する前に、以下の項目をメモしておきます。 [!DNL Salesforce Marketing Cloud] 宛先。

| 認証情報 | 説明 | 例 |
| --- | --- | --- |
| <ul><li>[!DNL Salesforce Marketing Cloud] 先頭の文字列</li></ul> | 詳しくは、 [[!DNL Salesforce Marketing Cloud domain prefix]](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/your-subdomain-tenant-specific-endpoints.html) を参照してください。 | <ul><li>ドメインが以下の場合は、強調表示された値が必要です。<br> <i>`mcq4jrssqdlyc4lph19nnqgzzs84`.login.exacttarget.com</i></li></ul> |
| <ul><li>クライアント ID</li><li>クライアント秘密鍵</li></ul> | 詳しくは、 [!DNL Salesforce Marketing Cloud] [ドキュメント](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/access-token-s2s.html) 追加のガイダンスが必要な場合は、を参照してください。 | <ul><li>r23kxxxxxxxxx0z05xxxxxxx</li><li>ipxxxxxxxxT4xxxxxxxx</li></ul> |

{style=&quot;table-layout:auto&quot;}

## の制限 [!DNL Salesforce Marketing Cloud] {#limits}

* Salesforce が特定の [レート制限](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/rate-limiting.html).
   * 詳しくは、 [!DNL Salesforce Marketing Cloud] [ドキュメント](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/rate-limiting-errors.html) を使用して、実行中に発生する可能性のある制限事項に対処し、エラーを減らします。
   * 詳しくは、 [[!DNL Salesforce Marketing Cloud] エンゲージメントの価格](https://www.salesforce.com/editions-pricing/marketing-cloud/email/) ページ *Full Edition 比較グラフのダウンロード* プランで課せられた制限の詳細を示す pdf 形式
   * この [API の概要](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/apis-overview.html) ページの詳細に関する追加の制限。
   * 参照 [ここ](https://salesforce.stackexchange.com/questions/205898/marketing-cloud-api-limits) を参照してください。
* の数 *オブジェクトごとに許可されるカスタムフィールド* Salesforce エディションに応じて異なります。
   * 詳しくは、 [!DNL Salesforce] [ドキュメント](https://help.salesforce.com/s/articleView?id=sf.custom_field_allocations.htm&amp;type=5) を参照してください。
   * が、 *オブジェクトごとに許可されるカスタムフィールド* 範囲 [!DNL Salesforce Marketing Cloud] 次が必要です：
      * 新しいカスタムフィールドを [!DNL Salesforce Marketing Cloud].
      * Platform で、これらの古いカスタムフィールド名を **[!UICONTROL マッピング ID]** 期間中 [セグメントスケジュール](#schedule-segment-export-example) 手順

## サポートされる ID {#supported-identities}

[!DNL Salesforce Marketing Cloud] では、以下の表で説明する id のアクティブ化をサポートしています。 詳細情報： [id](/help/identity-service/namespaces.md).

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| contactKey | [!DNL Salesforce Marketing Cloud] 連絡先キー。 詳しくは、 [!DNL Salesforce Marketing Cloud] [ドキュメント](https://help.salesforce.com/s/articleView?id=sf.mc_cab_contact_builder_best_practices.htm&amp;type=5) 追加のガイダンスが必要な場合は、を参照してください。 | 必須 |

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li>セグメントのすべてのメンバーを、必要なスキーマフィールドと共に書き出します *( 例：メールアドレス、電話番号、姓*（フィールドマッピングに従います）</li><li> 各セグメントのステータス ( [!DNL Salesforce Marketing Cloud] は、 **[!UICONTROL マッピング ID]** 期間中に指定された値 [セグメントスケジュール](#schedule-segment-export-example) 手順</li></ul> |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションに記載されているフィールドに入力します。

内 **[!UICONTROL 宛先]** > **[!UICONTROL カタログ]**、を検索します。 [!DNL (API) Salesforce Marketing Cloud]. または、 **[!UICONTROL 電子メールマーケティング]** カテゴリ。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**.
![SalesforceMarketing Cloudへの認証方法を示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/authenticate-destination.png)

* **[!UICONTROL サブドメイン]**:お使いの [!DNL Salesforce Marketing Cloud] ドメインプレフィックス。 例えば、ドメインが *`mcq4jrssqdlyc4lph19nnqgzzs84`.login.exacttarget.com*&#x200B;ハイライト表示された値が必要です。
* **[!UICONTROL クライアント ID]**:お使いの [!DNL Salesforce Marketing Cloud] クライアント ID。
* **[!UICONTROL クライアント秘密鍵]**:お使いの [!DNL Salesforce Marketing Cloud] クライアント秘密鍵。

指定した詳細が有効な場合、UI に **[!UICONTROL 接続済み]** ステータスに緑色のチェックマークが付いている場合は、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。 UI でフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![宛先の詳細を示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/destination-details.png)

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。

### アラートの有効化 {#enable-alerts}

アラートを有効にして、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の指定が完了したら、 **[!UICONTROL 次へ]**.

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
>
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [ストリーミングセグメントの書き出し先に対するプロファイルとセグメントのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platformからにオーディエンスデータを正しく送信するには [!DNL Salesforce Marketing Cloud] の宛先に移動する場合は、フィールドマッピングの手順を実行する必要があります。 マッピングは、Platform アカウント内の Experience Data Model(XDM) スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成することで構成されます。 XDM フィールドを [!DNL Salesforce Marketing Cloud] 宛先フィールドには、次の手順に従います。

>[!IMPORTANT]
>
>属性名は [!DNL Salesforce Marketing Cloud] アカウント、両方の `contactKey` および `personalEmail.address` は必須です。

1. 内 **[!UICONTROL マッピング]** ステップ、選択 **[!UICONTROL 新しいマッピングを追加]**. 画面に新しいマッピング行が表示されます。
   ![「新しいマッピングを追加」の Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/add-new-mapping.png)

1. 内 **[!UICONTROL ソースフィールドを選択]** ウィンドウで、 **[!UICONTROL 属性を選択]** カテゴリと選択 `contactKey`.
   ![ソースマッピング用の Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/source-mapping.png)

1. 内 **[!UICONTROL ターゲットフィールドを選択]** ウィンドウで、ソースフィールドをマッピングするターゲットフィールドのタイプを選択します。
   * **[!UICONTROL ID 名前空間を選択]**:このオプションを選択して、ソースフィールドをリストから id 名前空間にマップします。
      ![salesforceContactKey のターゲットマッピングを示すプラットフォーム UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/target-mapping.png)

   * XDM プロファイルスキーマと [!DNL Salesforce Marketing Cloud] インスタンス： |XDM プロファイルスキーマ|[!DNL Salesforce Marketing Cloud] インスタンス|必須| |—|—|—| |`contactKey`|`salesforceContactKey`|はい |

   * **[!UICONTROL カスタム属性を選択]**:このオプションを選択して、ソースフィールドを、 **[!UICONTROL 属性名]** フィールドに入力します。 参照： [!DNL Salesforce Marketing Cloud] [ドキュメント](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/updateContacts.html) を参照してください。 また、宛先では [Salesforce 検索属性セット定義 REST API](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/retrieveAttributeSetDefinitions.html) ：連絡先の Salesforce 内で定義され、お使いのアカウントに固有の属性を取得します。
      ![Target マッピングを示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/target-mapping-custom.png)

   * 例えば、更新する値に応じて、XDM プロファイルスキーマと [!DNL Salesforce Marketing Cloud] インスタンス： |XDM プロファイルスキーマ|[!DNL Salesforce Marketing Cloud] インスタンス| |—|—| |`person.name.firstName`|`Email Demographics.First Name`| |`personalEmail.address`|`Email Addresses.Email Address`|

   * これらのマッピングの使用例を次に示します。
      ![Target マッピングを示した Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/mappings.png)

### セグメントのエクスポートと例をスケジュール {#schedule-segment-export-example}

実行時に [セグメントの書き出しをスケジュール](/help/destinations/ui/activate-segment-streaming-destinations.md#scheduling) 手順に従い、Platform セグメントを [カスタム属性](#prerequisites-custom-field) Salesforce 内。

これをおこなうには、各セグメントを選択し、Salesforce から対応するカスタム属性を **[!UICONTROL マッピング ID]** フィールドに入力します。

>[!IMPORTANT]
>
>マッピング ID に使用する値は、「Email Demographics」属性セットの下で Salesforce 内で作成されたカスタム属性の名前と完全に一致する必要があります。

次に例を示します。
![スケジュールセグメントの書き出しを示した、Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/schedule-segment-export.png)

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. 選択 **[!UICONTROL 宛先]** > **[!UICONTROL 参照]** をクリックして、宛先のリストに移動します。
   ![参照先を示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/browse-destinations.png)

1. 宛先を選択し、ステータスが「 **[!UICONTROL 有効]**.
   ![宛先のデータフロー実行を示した Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/destination-dataflow-run.png)

1. 次に切り替え： **[!DNL Activation data]** 」タブをクリックし、セグメント名を選択します。
   ![宛先のアクティベーションデータを示した、プラットフォーム UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/destinations-activation-data.png)

1. セグメント概要を監視し、プロファイルの数がセグメント内で作成された数に一致していることを確認します。
   ![セグメントを示す Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/segment.png)

1. にログインします。 [[!DNL Salesforce Marketing Cloud]](https://mc.exacttarget.com/) web サイト。 次に、 **[!DNL Audience Builder]** > **[!DNL Contact Builder]** > **[!DNL All contacts]** > **[!DNL Email]** ページを開き、セグメントのプロファイルが追加されたかどうかを確認します。
   ![SalesforceMarketing CloudUI のスクリーンショットに、セグメントで使用されているプロファイルを含む連絡先ページが表示されています。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/contacts.png)

1. プロファイルが更新されているかどうかを確認するには、 **[!UICONTROL 電子メール]** ページを開き、セグメントのプロファイルの属性値が更新されたかどうかを確認します。 成功した場合は、各セグメントのステータスが [!DNL Salesforce Marketing Cloud] は、 **[!UICONTROL マッピング ID]** 指定された値 [セグメントスケジュール](#schedule-segment-export-example) 手順
   ![SalesforceMarketing CloudUI のスクリーンショットに、選択した連絡先メールページが表示され、セグメントのステータスが更新されました。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/contact-detail.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](/help/data-governance/home.md).

## エラーとトラブルシューティング {#errors-and-troubleshooting}

### イベントを SalesforceMarketing Cloudにプッシュ中に不明なエラーが発生しました {#unknown-errors}

データフローの実行をチェックする際に、次のエラーメッセージが表示される場合があります。 `Unknown errors encountered while pushing events to the destination. Please contact the administrator and try again.`

![エラーを示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/error.png)

このエラーを修正するには、 **[!UICONTROL マッピング ID]** 指定した [!DNL Salesforce Marketing Cloud] は、Platform セグメントが有効で、内に存在する [!DNL Salesforce Marketing Cloud].

## その他のリソース {#additional-resources}

* [[!DNL Salesforce Marketing Cloud] API](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/apis-overview.html)
