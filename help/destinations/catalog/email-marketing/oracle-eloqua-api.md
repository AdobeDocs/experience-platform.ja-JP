---
title: （API）Oracle Eloqua 接続
description: （API）Oracleの Eloqua 宛先を使用すると、アカウントデータを書き出し、Oracleの Eloqua 内でビジネスニーズに合わせてアクティブ化できます。
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 97ff41a2-2edd-4608-9557-6b28e74c4480
source-git-commit: 5aefa362d7a7d93c12f9997d56311127e548497e
workflow-type: tm+mt
source-wordcount: '2033'
ht-degree: 30%

---


# [!DNL (API) Oracle Eloqua] 接続

マーケター [[!DNL Oracle Eloqua]](https://www.oracle.com/cx/marketing/automation/) キャンペーンを計画および実行しながら、パーソナライズされたカスタマーエクスペリエンスを見込み客に提供できます。 統合されたリード管理と簡単なキャンペーン作成により、マーケターがバイヤージャーニーで適切なオーディエンスを適切なタイミングで惹きつけ、メール、ディスプレイ検索、ビデオ、モバイルなどのあらゆるチャネルでオーディエンスにリーチするようにエレガントに拡張できます。 セールスチームは、より速い速度でより多くの取引をクローズし、リアルタイムインサイトを通じてマーケティングの ROI を向上させることができます。

この [!DNL Adobe Experience Platform][ 宛先 ](/help/destinations/home.md) は、[!DNL Oracle Eloqua] REST API の [ 連絡先を更新 ](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/op-api-rest-1.0-data-contact-id-put.html) 操作を活用し、オーディエンス内の **ID を更新** して [!DNL Oracle Eloqua] を更新できます。

[!DNL Oracle Eloqua] は [ 基本認証 ](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/Authentication_Basic.html) を使用して [!DNL Oracle Eloqua] REST API と通信します。 [!DNL Oracle Eloqua] インスタンスを認証する手順は、さらに下の[宛先に対する認証](#authenticate)の節にあります。

## ユースケース {#use-cases}

オンラインプラットフォームのマーケティング部門は、キュレーションされたオーディエンスにメールベースのマーケティングキャンペーンをブロードキャストしたいと考えています。 プラットフォームのマーケティングチームは、Adobe Experience Platformを使用して既存のリード情報を更新し、独自のオフラインデータからオーディエンスを作成し、これらのオーディエンスを [!DNL Oracle Eloqua] に送信できます。マーケティングキャンペーンのメール送信に使用できます。

## 前提条件 {#prerequisites}

### Experience Platform の前提条件 {#prerequisites-in-experience-platform}

[!DNL Oracle Eloqua] 宛先へのデータをアクティブ化する前に、[スキーマ](/help/xdm/schema/composition.md)、[データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html)および[セグメント](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html)を [!DNL Experience Platform] で作成する必要があります。

オーディエンスのステータスに関するガイダンスが必要な場合は、[ オーディエンスメンバーシップの詳細スキーマフィールドグループ ](/help/xdm/field-groups/profile/segmentation.md) に関するExperience Platformドキュメントを参照してください。

### [!DNL Oracle Eloqua] 前提条件 {#prerequisites-destination}

Platform から [!DNL Oracle Eloqua] アカウントにデータを書き出すには、[!DNL Oracle Eloqua] アカウントが必要です。

さらに、[!DNL Oracle Eloqua] インスタンスには少なくとも *「詳細設定ユーザー – マーケティング権限」* が必要です。 詳しくは、*セキュリティで保護されたユーザーアクセス [ ページの「セキュリティグループ ](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-user/Help/SecurityOverview/SecuredUserAccess.htm) の節を参照して* ださい。 アクセスは、[!DNL Oracle Eloqua] API を呼び出す際に宛先がプログラムで [ ベース URL を決定 ](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/DeterminingBaseURL.html) するために必要です。

#### [!DNL Oracle Eloqua] 資格情報の収集 {#gather-credentials}

[!DNL Oracle Eloqua] の宛先に対して認証を行う前に、以下の項目をメモしておきます。

| 資格情報 | 説明 |
| --- | --- |
| `Company Name` | [!DNL Oracle Eloqua] アカウントに関連付けられた会社名。 <br> 後で `Company Name` と [!DNL Oracle Eloqua] `Username` を、[ 宛先への認証 ](#authenticate) 時に **[!UICONTROL Username]** として使用される連結文字列として使用します。 |
| `Username` | [!DNL Oracle Eloqua] アカウントのユーザー名。 |
| `Password` | [!DNL Oracle Eloqua] アカウントのパスワード。 |
| `Pod` | [!DNL Oracle Eloqua] では、それぞれ一意のドメイン名を持つ複数のデータセンターをサポートしています。 現在、こ [!DNL Oracle Eloqua] らを「ポッド」と呼び、p01、p02、p03、p04、p06、p07、p08 の合計 7 つがあります。 使用しているポッドを取得するには、[!DNL Oracle Eloqua] にログインし、正常にログインしたらブラウザーに URL をメモします。 例えば、ブラウザーの URL が `secure.p01.eloqua.com` の場合、`pod` は `p01` です。 詳しくは、[POD の決定 ](https://community.oracle.com/topliners/discussion/4470225/determining-your-pod-number-for-oracle-eloqua) ページを参照してください。 |

詳しくは、[ へのログイン  [!DNL Oracle Eloqua]](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-user/Help/Administration/Tasks/SigningInToEloqua.htm#Signing) を参照してください。

## ガードレール {#guardrails}

>[!NOTE]
>
>* カスタム連絡先フィールド [!DNL Oracle Eloqua]、**[!UICONTROL セグメントの選択]** 手順で選択したオーディエンスの名前を使用して自動的に作成されます。

* [!DNL Oracle Eloqua] には、最大 250 個のカスタム連絡先フィールドという制限があります。
* 新しいオーディエンスを書き出す前に、Platform オーディエンスの数と [!DNL Oracle Eloqua] 内の既存のオーディエンスの数がこの制限を超えないようにしてください。
* この制限を超えると、Experience Platformでエラーが発生します。 これは、[!DNL Oracle Eloqua] API がリクエストを検証できず、- *400：検証エラーが発生しました* – 問題を説明するエラーメッセージと応答するからです。
* 上記の上限に達した場合、さらにセグメントを書き出す前に、既存のマッピングを宛先から削除し、[!DNL Oracle Eloqua] アカウントの対応するカスタム連絡先フィールドを削除する必要があります。

* 追加の制限について詳しくは、[[!DNL Oracle Eloqua]  連絡先フィールドの作成 ](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-user/Help/ContactFields/Tasks/CreatingContactFields.htm) ページを参照してください。

## サポートされる ID {#supported-identities}

[!DNL Oracle Eloqua] では、以下の表で説明する ID の更新をサポートしています。[ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 必須 |
|---|---|---|
| `EloquaId` | 連絡先の一意の ID。 | ○ |

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li>セグメントのすべてのメンバーを、フィールドマッピングに従って、必要なスキーマフィールドと共に書き出します&#x200B;*（例：メールアドレス、電話番号、姓）*。</li><li> Platform で選択した各オーディエンスについて、対応する [!DNL Oracle Eloqua] セグメントのステータスが、Platform のオーディエンスステータスで更新されます。</li></ul> |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | <ul><li>ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。</li></ul> |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL 宛先]**／**[!UICONTROL カタログ]**&#x200B;内で [!DNL (API) Oracle Eloqua] を検索します。または、「**[!UICONTROL メールマーケティング]**」カテゴリの下に配置することもできます。

### 宛先に対する認証 {#authenticate}

>[!CONTEXTUALHELP]
>id="platform_destinations_apioracleeloqua_companyname_username"
>title="会社名\ユーザー名"
>abstract="このフィールドに、会社名と Oracle Eloqua のユーザー名を `{COMPANY_NAME}\{USERNAME}` の形式で入力します"

以下の必須のフィールドに入力します。詳しくは、[Gather [!DNL Oracle Eloqua] credentials](#gather-credentials) の節を参照してください。
* **[!UICONTROL パスワード]**:[!DNL Oracle Eloqua] アカウントのパスワード。
* **[!UICONTROL Username]**:[!DNL Oracle Eloqua] の会社名と [!DNL Oracle Eloqua] のユーザー名を連結した文字列です。<br> 連結された値は、`{COMPANY_NAME}\{USERNAME}` の形式になります。<br> 注意：中括弧やスペースを使用せず、`\` を保持します。 <br> 例えば、[!DNL Oracle Eloqua] の会社名が `MyCompany` で [!DNL Oracle Eloqua] ユーザー名が `Username` の場合、「**[!UICONTROL ユーザー名]**」フィールドで使用する連結された値は `MyCompany\Username` です。

宛先を認証するには、「 **[!UICONTROL 宛先に接続]**」を選択します。
![認証方法を示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/oracle-eloqua-api/authenticate-destination.png)

指定した詳細が有効な場合、UI で&#x200B;**[!UICONTROL 接続済み]**&#x200B;ステータスに緑色のチェックマークが付きます。その後、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_apioracleeloqua_pod"
>title="ポッド"
>abstract="ポッド番号を見つけるには、Oracle Eloqua にログインします。正常にログインしたら、ブラウザーに表示される URL をメモします。 "

<!-- >additional-url="https://support.oracle.com/knowledge/Oracle%20Cloud/2307176_1.html" text="Oracle Knowledge base - find out your Pod number" -->

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![宛先の詳細を示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/oracle-eloqua-api/destination-details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL ポッド]**：現在の `pod` を取得するには、[!DNL Oracle Eloqua] にログインし、正常にログインしたらブラウザーに URL をメモします。 例えば、ブラウザーの URL が `secure.p01.eloqua.com` の場合、選択する必要がある `pod` の値は `p01` です。 追加のガイダンスについては、[Gather [!DNL Oracle Eloqua] credentials](#gather-credentials) の節を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platform から [!DNL Oracle Eloqua] 宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。マッピングは、Platform アカウント内の Experience Data Model （XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。

XDM フィールドを [!DNL Oracle Eloqua] の宛先フィールドにマッピングするには、次の手順に従います。

1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。画面に新しいマッピング行が表示されます。
1. **[!UICONTROL ソースフィールドを選択]** ウィンドウで、**[!UICONTROL 属性を選択]** カテゴリを選択して XDM 属性を選択するか、**[!UICONTROL ID 名前空間を選択]** を選択して ID を選択します。
1. **[!UICONTROL ターゲットフィールドを選択]** ウィンドウで、**[!UICONTROL ID 名前空間を選択]** を選択して ID を選択するか、**[!UICONTROL カスタム属性を選択]** を選択して **[!UICONTROL 属性名]** フィールドに目的の属性名を入力します。 指定する属性名は、[!DNL Oracle Eloqua] の既存の連絡先属性と一致する必要があります。 [!DNL Oracle Eloqua] で使用できる正確な属性名については、[[!DNL create a contact]](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/op-api-rest-1.0-data-contact-post.html) を参照してください。

   * これらの手順を繰り返して、XDM プロファイルスキーマと [!DNL Oracle Eloqua] の間に必要な属性マッピングと必要な属性マッピングの両方を追加します。

     | ソースフィールド | ターゲットフィールド | 必須 |
     |---|---|---|
     | `IdentityMap: Eid` | `Identity: EloquaId` | ○ |
     | `xdm: personalEmail.address` | `Attribute: emailAddress` | ○ |
     | `xdm: personName.firstName` | `Attribute: firstName` | |
     | `xdm: personName.lastName` | `Attribute: lastName` | |
     | `xdm: workAddress.street1` | `Attribute: address1` | |
     | `xdm: workAddress.street2` | `Attribute: address2` | |
     | `xdm: workAddress.street3` | `Attribute: address3` | |
     | `xdm: workAddress.postalCode` | `Attribute: postalCode` | |
     | `xdm: workAddress.country` | `Attribute: country` | |
     | `xdm: workAddress.city` | `Attribute: city` | |

   * 上記のマッピングの例を以下に示します。
     ![ 属性マッピングを含む Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/oracle-eloqua-api/mappings.png)

>[!IMPORTANT]
>
>* **[!UICONTROL ターゲットフィールド]** で指定する属性には、リクエスト本文を形成するので、[[!DNL Create a contact]](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/op-api-rest-1.0-data-contact-post.html) で指定されたとおりに正確に名前を付ける必要があります。
>* **[!UICONTROL Source フィールドで指定された属性は]** そのような制限には従いません。 必要に応じてマッピングできますが、[!DNL Oracle Eloqua] にプッシュした際にデータ形式が正しくない場合は、エラーが発生します。 例えば、**[!UICONTROL Source フィールド]** ID 名前空間 `contact key` な `ABC ID` をマッピングできます。 **[!UICONTROL ターゲットフィールド]** へ：ID 値が [!DNL Oracle Eloqua] で受け入れられる形式と一致することを確認した後、`EloquaId` します。
>* ID に対応する属性を更新するには、`EloquaID` マッピングが必須です。
>* `emailAddress` マッピングが必要です。 ない場合、次に示すように、API はエラーをスローします。
>
>```json
>{
>     "type":"ObjectValidationError",
>     "container":{
>           "type":"ObjectKey",
>           "objectType":"Contact"
>     },
>     "property":"emailAddress",
>     "requirement":{
>           "type":"EmailAddressRequirement"
>     },
>     "value":"<null>"
>}
>```

宛先接続のマッピングの指定を終えたら「**[!UICONTROL 次へ]**」を選択します。

>[!NOTE]
>
>宛先は、連絡先フィールド情報を [!DNL Oracle Eloqua] に送信する際に、各実行で選択されたオーディエンス名に自動的に一意の ID を付加します。 これにより、オーディエンス名に対応する連絡先フィールド名が重複するのを防ぐことができます。 オーディエンス名を使用して作成されたカスタム連絡先フィールドを含む [!DNL Oracle Eloqua] スタム連絡先詳細ページのスクリーンショットの例については、[ データ書き出しの検証 ](#exported-data) の節を参照してください。

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. **[!UICONTROL 宛先]**/**[!UICONTROL 参照]** を選択し、宛先のリストに移動します。
1. 次に、宛先を選択し、**[!UICONTROL アクティベーションデータ]** タブに切り替えて、オーディエンス名を選択します。
   ![宛先のアクティベーションデータを示した Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/oracle-eloqua-api/destinations-activation-data.png)

1. オーディエンスの概要を監視し、プロファイルの数がセグメント内の数と一致していることを確認します。
   ![セグメントを示す Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/oracle-eloqua-api/segment.png)

1. [!DNL Oracle Eloqua] Web サイトにログインして、**[!UICONTROL 連絡先の概要]** ページに移動し、オーディエンスのプロファイルが追加されたかどうかを確認します。 オーディエンスのステータスを表示するには、**[!UICONTROL 連絡先の詳細]** ページにドリルダウンし、選択したオーディエンス名をプレフィックスとして持つ連絡先フィールドが作成されたかどうかを確認します。

![ オーディエンス名で作成されたカスタム連絡先フィールドを含む連絡先詳細ページを示すOracle Eloqua UI のスクリーンショット。](../../assets/catalog/email-marketing/oracle-eloqua-api/contact.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

宛先を作成すると、次のいずれかのエラーメッセージが表示される場合があります：`400: There was a validation error` または `400 BAD_REQUEST`。 これは、[ ガードレール ](#guardrails) の節で説明しているように、カスタムの連絡先フィールドの上限である 250 を超えた場合に発生します。 このエラーを修正するには、[!DNL Oracle Eloqua] のカスタム連絡先フィールド制限を超えていないことを確認してください。
![ エラーを示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/oracle-eloqua-api/error.png)

ステータスコードとエラーコードの包括的なリストと説明については、[[!DNL Oracle Eloqua] HTTP ステータスコード ](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/APIRequests_HTTPStatusCodes.html) ページおよび [[!DNL Oracle Eloqua]  検証エラー ](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/APIRequests_HTTPValidationErrors.html) ページを参照してください。

## その他のリソース {#additional-resources}

詳しくは、[!DNL Oracle Eloqua] のドキュメントを参照してください。

* [Eloqua マーケティング自動処理のOracle](https://docs.oracle.com/en/cloud/saas/marketing/eloqua.html)
* [Eloqua Marketing CloudサービスOracle用 REST API](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/rest-endpoints.html)

### 変更ログ

この節では、この宛先コネクタに対する機能の概要と重要なドキュメントの更新について説明します。

+++ 変更ログを表示

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2023年4月 | ドキュメントの更新 | <ul><li>[ ユースケース ](#use-cases) の節を更新し、この宛先を使用することで、お客様にメリットが得られるタイミングをより明確に例を示しました。</li> <li>[ マッピング ](#mapping-considerations-example) の節を更新し、必須マッピングとオプションマッピングの両方の明確な例を追加しました。</li> <li>「[ 宛先への接続 ](#connect)」の節を更新し、[!DNL Oracle Eloqua] ーザーの会社名と [!DNL Oracle Eloqua] ユーザー名を使用して **[!UICONTROL ユーザー名]** フィールドの連結された値を作成する方法の例を追加しました。 （PLATIR-28343）</li><li>「**[!UICONTROL ポッド ](#gather-credentials) の選択に関するガイダンスを使用して、「[ 収集  [!DNL Oracle Eloqua]  資格情報 ](#destination-details) と [ 宛先の詳細の入力]** の節 [!DNL Oracle Eloqua] 更新しました。 *「Pod」* 値は、API 呼び出しのベース URL を作成するために宛先で使用されます。 また、[[!DNL Oracle Eloqua]  前提条件 ](#prerequisites-destination) の節が更新され、[!DNL Oracle Eloqua] インスタンスの必須の *「セキュリティグループ」として* 「詳細設定ユーザー – マーケティング権限」 *を割り当てる際のガイダンスが追加されました*</li></ul> |
| 2023年3月 | 初回リリース | 宛先の初回リリースとドキュメントの公開。 |

{style="table-layout:auto"}

+++