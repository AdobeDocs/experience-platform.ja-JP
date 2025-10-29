---
title: Salesforce Marketing Cloud アカウントのエンゲージメント
description: Salesforce Marketing Cloud Account Engagement （旧称 Pardot）の宛先を使用してアカウントデータを書き出し、ビジネスニーズに合わせてSalesforce Marketing Cloud Account Engagement 内でアクティブ化する方法を説明します。
last-substantial-update: 2023-04-14T00:00:00Z
exl-id: fca9d4f4-8717-4bfa-9992-5164ba98bea4
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1482'
ht-degree: 29%

---

# [!DNL Salesforce Marketing Cloud Account Engagement] 接続

[[!DNL Salesforce Marketing Cloud Account Engagement]](https://www.salesforce.com/products/marketing-cloud/marketing-automation/) *（旧称 [!DNL Pardot]）* の宛先を使用して、リードをキャプチャ、追跡、スコアリング、評価します。 また、電子メールドリップキャンペーンを通じて、ターゲットマーケットオーディエンスと顧客グループに向けたパイプラインのすべての段階でリードトラックを設計し、育成、スコアリング、キャンペーンセグメント化を含むリード管理を行うこともできます。

[!DNL Salesforce Marketing Cloud Engagement]B2C **マーケティングに重点を置いた** に比べ、[!DNL Marketing Cloud Account Engagement] れは、複数の部門や意思決定者が関与し、より長い販売サイクルと意思決定サイクルが必要な **B2B** ユースケースに最適です。 さらに、CRM との緊密な近接性と統合を維持して、適切な販売とマーケティングの意思決定を行います。 *Experience Platformには [!DNL Salesforce Marketing Cloud Engagement] 用の接続もあります。[[!DNL Salesforce Marketing Cloud]](/help/destinations/catalog/email-marketing/salesforce-marketing-cloud.md) ページと [[!DNL (API) Salesforce Marketing Cloud]](/help/destinations/catalog/email-marketing/salesforce-marketing-cloud-exact-target.md) ページで確認できます。*

この [!DNL Adobe Experience Platform][ 宛先 ](/help/destinations/home.md) は、新しい [[!DNL Salesforce Account Engagement API > Prospect Upsert by Email] セグメント内でアクティブ化した後、](https://developer.salesforce.com/docs/marketing/pardot/guide/prospect-v5.html#prospect-upsert-by-email)**エンドポイントを活用して、リードを** 追加または更新 [!DNL Marketing Cloud Account Engagement] します。

[!DNL Marketing Cloud Account Engagement] は、認証コードプロトコルを使用した OAuth 2 を使用して、[!DNL Account Engagement] API への認証を行います。 [!DNL Marketing Cloud Account Engagement] インスタンスを認証する手順は、さらに下の[宛先に対する認証](#authenticate)の節にあります。

## ユースケース {#use-cases}

[!DNL Marketing Cloud Account Engagement] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### マーケティングキャンペーンの連絡先へのメールの送信 {#use-case-send-emails}

オンラインプラットフォームのマーケティング部門は、B2B リードのキュレーションされたオーディエンスにメールベースのマーケティングキャンペーンをブロードキャストしたいと考えています。 プラットフォームのマーケティングチームは、Adobe Experience Platformを使用して新しいリードを追加したり、既存のリード情報を更新したりできます。また、独自のオフラインデータからオーディエンスを作成して、これらのオーディエンスを [!DNL Marketing Cloud Account Engagement] に送信できます。このオーディエンスを使用して、マーケティングキャンペーンのメールを送信できます。

## 前提条件 {#prerequisites}

Experience Platformと [!DNL Salesforce] で設定する必要がある前提条件と、[!DNL Marketing Cloud Account Engagement] の宛先を使用する前に収集する必要がある情報については、以下の節を参照してください。

### Experience Platformの前提条件 {#prerequisites-in-experience-platform}

[!DNL Marketing Cloud Account Engagement] 宛先へのデータをアクティブ化する前に、[スキーマ](/help/xdm/schema/composition.md)、[データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html)および[セグメント](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html)を [!DNL Experience Platform] で作成する必要があります。

### [!DNL Marketing Cloud Account Engagement] の前提条件 {#prerequisites-destination}

Experience Platformから [!DNL Marketing Cloud Account Engagement] アカウントにデータを書き出すには、次の前提条件に注意してください。

#### [!DNL Marketing Cloud Account Engagement] アカウントが必要です {#prerequisites-account}

[!DNL Marketing Cloud Account Engagement]Marketing Cloud Account Engagement[ 製品のサブスクリプションを持つ ](https://www.salesforce.com/products/marketing-cloud/marketing-automation/) アカウントは続行する必要があります。

[!DNL Salesforce] アカウントには、[!DNL Salesforce] `Account Engagement Administrator role` が必要です。 これは [ カスタム見込み客フィールドの作成 ](https://help.salesforce.com/s/articleView?id=sf.pardot_fields_create_custom_field.htm&type=5) に必要です。

最後に、お使いのアカウントから [[!DNL Account Engagement Lightning App]](https://help.salesforce.com/s/articleView?id=sf.pardot_lightning_enable.htm&type=5) にアクセスできるようになります。

アカウントをお持ちでない場合や、アカウントに [[!DNL Salesforce]  サブスクリプションまたは ](https://www.salesforce.com/company/contact-us/?d=cta-glob-footer-10) がない場合は、[!DNL Salesforce] サポート [!DNL Marketing Cloud Account Engagement] または [!DNL Account Engagement Administrator role] アカウント管理者にお問い合わせください。

#### [!DNL Marketing Cloud Account Engagement] 資格情報の収集 {#gather-credentials}

[!DNL Marketing Cloud Account Engagement] の宛先に対して認証を行う前に、以下の項目をメモしておきます。

| 資格情報 | 説明 |
| --- | --- |
| `Username` | [!DNL Marketing Cloud Account Engagement] アカウントのユーザー名。 |
| `Password` | [!DNL Marketing Cloud Account Engagement] アカウントのパスワード。 |
| `Account Engagement Business Unit ID` | アカウントエンゲージメント事業部門 ID を見つけるには、[!DNL Salesforce] の設定を使用します。 「設定」から、「クイック検索」ボックスに *事業単位設定* と入力します。 アカウントエンゲージメント事業部 ID は `0Uv` から始まり、18 文字の長さになります。 ビジネスユニットの設定情報にアクセスできない場合は、[!DNL Salesforce] アカウント管理者に問い合わせて、`Account Engagement Business Unit ID` を提供してもらってください。 追加のガイダンスが必要な場合は、[[!DNL Salesforce]  認証 ](https://developer.salesforce.com/docs/marketing/pardot/guide/authentication) ガイドラインページを参照してください。 |

{style="table-layout:auto"}

### ガードレール {#guardrails}

[!DNL Marketing Cloud Account Engagement] の [ レート制限 ](https://developer.salesforce.com/docs/marketing/pardot/guide/overview.html#rate-limits) を参照してください。これは、計画によって課せられる制限の詳細であり、Experience Platformの実行にも当てはまります。

>[!IMPORTANT]
>
>[!DNL Salesforce] アカウント管理者が信頼できる IP 範囲へのアクセスを制限している場合、[Experience Platformの IP を許可リストに加えるするには、管理者に連絡する必要 ](/help/destinations/catalog/streaming/ip-address-allow-list.md) あります。 追加のガイダンスが必要な場合は、[!DNL Salesforce] [ 接続されたアプリの信頼できる IP 範囲へのアクセスの制限 ](https://help.salesforce.com/s/articleView?id=sf.connected_app_edit_ip_ranges.htm&type=5) ドキュメントを参照してください。

## サポートされている ID {#supported-identities}

[!DNL Marketing Cloud Account Engagement] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | 見込み客のメールアドレス | 必須 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | <ul><li>セグメントのすべてのメンバーを、フィールドマッピングに従って、必要なスキーマフィールドと共に書き出します&#x200B;*（例：メールアドレス、電話番号、姓）*。</li><li> Experience Platformで選択した各オーディエンスについて、対応する [!DNL Salesforce Marketing Cloud Account Engagement] セグメントのステータスが、Experience Platformのオーディエンスステータスで更新されます。</li></ul> |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL Destinations]**/**[!UICONTROL Catalog]** 内で、[!DNL Salesforce Marketing Cloud Account Engagement] を検索します。 または、**[!UICONTROL Email marketing]** カテゴリの下に配置することもできます。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、「**[!UICONTROL Connect to destination]**」を選択します。 [!DNL Salesforce] ログインページに移動します。 [!DNL Marketing Cloud Account Engagement] アカウントの資格情報を入力し、「[!DNL Log In]」を選択します。

![Marketing Cloud アカウントエンゲージメントへの認証方法を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement/authenticate-destination.png)

次に、後続のウィンドウで「[!UICONTROL Allow]」を選択し、**Adobe Experience Platform** アプリに対して、[!DNL Salesforce Marketing Cloud Account Engagement] アカウントにアクセスする権限を付与します。 *これは 1 回だけ行う必要があります*。

![Salesforce アプリにMarketing Cloud Account Engagement へのアクセス権を付与するためのExperience Platform アプリのスクリーンショット確認ポップアップ。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement/allow-app.png)

指定した詳細が有効な場合、「*Salesforce Marketing Cloud アカウントエンゲージメントアカウントに正常に接続しました*」というメッセージと **[!UICONTROL Connected]** ステータスが緑色のチェックマークで表示され、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。 詳しくは、[Gather [!DNL Marketing Cloud Account Engagement] credentials](#gather-credentials) の節を参照してください。

![ 宛先の詳細を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement/destination-details.png)

| フィールド | 説明 |
| --- | --- |
| **[!UICONTROL Name]** | 今後この宛先を認識するための名前。 |
| **[!UICONTROL Description]** | 今後この宛先を識別するのに役立つ説明。 |
| **[!UICONTROL Account Engagement Business Unit ID]** | [!DNL Salesforce] の `Account Engagement Business Unit ID`。 |

{style="table-layout:auto"}

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platform から [!DNL Marketing Cloud Account Engagement] 宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。マッピングは、Experience Platform アカウント内の Experience Data Model （XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。

XDM フィールドを [!DNL Marketing Cloud Account Engagement] の宛先フィールドに正しくマッピングするには、次の手順に従います。

1. **[!UICONTROL Mapping]** の手順で、「**[!UICONTROL Add new mapping]**」を選択します。 画面に新しいマッピング行が表示されます。
1. **[!UICONTROL Select source field]** ウィンドウで、**[!UICONTROL Select attributes]** カテゴリを選択して XDM 属性を選択するか、**[!UICONTROL Select identity namespace]** を選択して ID を選択します。
1. **[!UICONTROL Select target field]** ウィンドウで、**[!UICONTROL Select identity namespace]** を選択して ID を選択するか、カテゴリ **[!UICONTROL Select custom attributes]** 選択して使用可能なスキーマの [[!DNL Prospect API fields]](https://developer.salesforce.com/docs/marketing/pardot/guide/prospect-v5.html#fields) のリストから指定します。

   * これらの手順を繰り返して、XDM プロファイルスキーマと [!DNL Marketing Cloud Account Engagement] の間のマッピングを追加します。

     | ソースフィールド | ターゲットフィールド | 必須 |
     | --- | --- | --- |
     | `IdentityMap: Email` | `Identity: email` | ○ |
     | `xdm: MailingAddress.city` | `xdm: city` | |
     | `xdm: person.name.firstName` | `Attribute: firstName` | |

   * 上記のマッピングの例を以下に示します。
     ![ ターゲットマッピングを示したExperience Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement/mappings.png)

宛先接続のマッピングの指定が完了したら、「**[!UICONTROL Next]**」を選択します。

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. 選択したオーディエンスの 1 つに移動します。 「**[!DNL Activation data]**」タブを選択します。**[!UICONTROL Mapping ID]** 列には、[!DNL Marketing Cloud Account Engagement Prospects] ページ内で生成されたカスタムフィールドの名前が表示されます。
   ![ 選択したセグメントのマッピング ID を示すExperience Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement/selected-segment-mapping-id.png)

1. [[!DNL Salesforce]](https://login.salesforce.com/) web サイトにログインします。 次に、**[!DNL Account Engagement]** / **[!DNL Prospects]** / **[!DNL Pardot Prospects]** ページに移動し、オーディエンスの見込み客が追加/更新されたかどうかを確認します。 または、[[!DNL Salesforce Pardot]](https://pi.pardot.com/) にアクセスして **[!DNL Prospects]** ページにアクセスすることもできます。
   ![ 見込み客ページを示すSalesforce UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement/prospects.png)

1. 見込み客が更新されたかどうかを確認するには、見込み客を選択し、カスタム見込み客フィールドがExperience Platform オーディエンスステータスで更新されたかどうかを確認します。
   選択した見込み客ページを示す ![Salesforce UI のスクリーンショット。カスタム見込み客フィールドがオーディエンスステータスで更新されます。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement/prospect.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

* [!DNL Marketing Cloud Account Engagement]API ドキュメント [ を ](https://developer.salesforce.com/docs/marketing/pardot/guide/overview.html) 照してください。
