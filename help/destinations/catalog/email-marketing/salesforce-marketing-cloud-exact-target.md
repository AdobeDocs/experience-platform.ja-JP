---
title: （API）Salesforce Marketing Cloud 接続
description: Salesforce Marketing Cloud（旧称 ExactTarget）の宛先を使用すると、アカウントデータを書き出し、Salesforce Marketing Cloud内でビジネスニーズに合わせてアクティブ化できます。
exl-id: 0cf068e6-8a0a-4292-a7ec-c40508846e27
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '2916'
ht-degree: 20%

---

# [!DNL (API) Salesforce Marketing Cloud] 接続

## 概要 {#overview}

[[!DNL (API) Salesforce Marketing Cloud]](https://www.salesforce.com/products/marketing-cloud/engagement/) （旧称 [!DNL ExactTarget]）は、訪問者および顧客がエクスペリエンスをパーソナライズするためのジャーニーを構築し、カスタマイズできるデジタルマーケティングスイートです。

>[!IMPORTANT]
>
> この接続と、メールマーケティングカタログ セクション内に存在するその他の [[!DNL Salesforce Marketing Cloud]  接続 ](/help/destinations/catalog/email-marketing/salesforce-marketing-cloud.md) の違いに注意してください。 もう 1 つのSalesforce Marketing Cloud接続は、指定したストレージの場所にファイルを書き出すことができますが、これは API ベースのストリーミング接続です。

**B2B** マーケティングに重点を置いた [!DNL Salesforce Marketing Cloud Account Engagement] ースに比べて、[!DNL (API) Salesforce Marketing Cloud] の宛先は、トランザクションの意思決定サイクルが短い **B2C** のユースケースに最適です。 連絡先を優先順位付けおよびセグメント化してマーケティングキャンペーンを調整および改善するために、ターゲットオーディエンスの行動を表すより大きなデータセットを統合できます（特に、[!DNL Salesforce] 外のデータセットから）。 *なお、Experience Platformには [[!DNL Salesforce Marketing Cloud Account Engagement]](/help/destinations/catalog/email-marketing/salesforce-marketing-cloud-account-engagement.md) への接続もあります。*

この [!DNL Adobe Experience Platform] [ 宛先 ](/help/destinations/home.md) は、[!DNL Salesforce Marketing Cloud] [ 連絡先の更新 ](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/updateContacts.html) API を使用しています。この API では、新しい [!DNL Salesforce Marketing Cloud] セグメント内でアクティブ化した後に、ビジネスニーズに合わせて連絡先を **追加して、連絡先データを更新** することができます。

[!DNL Salesforce Marketing Cloud] は、[!DNL Salesforce Marketing Cloud] API と通信するための認証メカニズムとして、クライアント資格情報を含む OAuth 2 を使用します。 [!DNL Salesforce Marketing Cloud] インスタンスを認証する手順は、さらに下の[宛先に対する認証](#authenticate)の節にあります。

## ユースケース {#use-cases}

[!DNL (API) Salesforce Marketing Cloud] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### マーケティングキャンペーンの連絡先へのメールの送信 {#use-case-send-emails}

ホームレンタルプラットフォームの販売部門は、ターゲットとなる顧客オーディエンスにマーケティングメールをブロードキャストしたいと考えています。 プラットフォームのマーケティングチームは、Adobe Experience Platformを使用して新しい連絡先の追加や既存の連絡先 *（およびメールアドレス）の更新を行い* 独自のオフラインデータからオーディエンスを作成し、これらのオーディエンスを [!DNL Salesforce Marketing Cloud] に送信できます。このオーディエンスを使用して、マーケティングキャンペーンのメールを送信できます。

## 前提条件 {#prerequisites}

### Experience Platformの前提条件 {#prerequisites-in-experience-platform}

[!DNL (API) Salesforce Marketing Cloud] 宛先へのデータをアクティブ化する前に、[スキーマ](/help/xdm/schema/composition.md)、[データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja)および[セグメント](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=ja)を [!DNL Experience Platform] で作成する必要があります。

### [!DNL (API) Salesforce Marketing Cloud] の前提条件 {#prerequisites-destination}

Experience Platformから [!DNL Salesforce Marketing Cloud] アカウントにデータを書き出すには、次の前提条件に注意してください。

#### [!DNL Salesforce Marketing Cloud] アカウントが必要です {#prerequisites-account}

[[!DNL Marketing Cloud Engagement]](https://www.salesforce.com/products/marketing-cloud/engagement/) 製品のサブスクリプションを持つ [!DNL Salesforce Marketing Cloud] アカウントは続行する必要があります。

[!DNL Salesforce Marketing Cloud] アカウントをお持ちでない場合や、アカウントに [!DNL Marketing Cloud Engagement] 製品のサブスクリプションがない場合は、[[!DNL Salesforce]  サポート ](https://www.salesforce.com/company/contact-us/?d=cta-glob-footer-10) にお問い合わせください。

#### [!DNL Salesforce Marketing Cloud] 内での属性の作成 {#prerequisites-attribute}

[!DNL (API) Salesforce Marketing Cloud] の宛先に対してオーディエンスをアクティブ化する場合、**[!UICONTROL オーディエンススケジュール]** 手順で、アクティブ化された各オーディエンスの **[マッピング ID](#schedule-segment-export-example)** フィールドに値を入力する必要があります。

[!DNL Salesforce] では、Experience Platformから受信するオーディエンスを正しく読み取って解釈し、[!DNL Salesforce Marketing Cloud] 内でオーディエンスステータスを更新するためにこの値が必要です。 オーディエンスのステータスに関するガイダンスが必要な場合は、[ オーディエンスメンバーシップの詳細スキーマフィールドグループ ](/help/xdm/field-groups/profile/segmentation.md) に関するExperience Platform ドキュメントを参照してください。

Experience Platformから [!DNL Salesforce] にアクティブ化するオーディエンスごとに、[!DNL Salesforce Marketing Cloud] 内の [!DNL Email Demographics] データ拡張機能にリンクされた `Text` タイプの属性が必要です。 [!DNL Salesforce Marketing Cloud] [!DNL Contact Builder] を使用して属性を作成します。 属性の作成に関するガイダンスが必要な場合は、[!DNL Salesforce Marketing Cloud] ドキュメントを参照して [ 属性の作成 ](https://help.salesforce.com/s/articleView?id=mc_cab_create_an_attribute.htm&amp;type=5&amp;language=en_US) を確認してください。

属性フィールド名は、**[!UICONTROL マッピング]** 手順の [!DNL (API) Salesforce Marketing Cloud] ターゲットフィールドに使用されます。 ビジネス要件に応じて、最大 4,000 文字のフィールド文字を定義できます。 属性タイプについて詳しくは、[!DNL Salesforce Marketing Cloud] [ データ拡張機能データタイプ ](https://help.salesforce.com/s/articleView?id=sf.mc_es_data_extension_data_types.htm&amp;type=5) のドキュメントページを参照してください。

属性を追加する [!DNL Salesforce Marketing Cloud] のデータデザイナー画面の例を次に示します。
![Salesforce Marketing Cloud UI データデザイナー ](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/salesforce-data-designer.png)

[!DNL Email Demographics] データ拡張機能内のオーディエンスステータスに対応する属性を含む [!DNL Salesforce Marketing Cloud] [!DNL Email Data] 属性グループのビューを以下に示します。
![Salesforce Marketing Cloud UI メールデータ属性グループ。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/salesforce-email-demographics-fields.png)

[!DNL (API) Salesforce Marketing Cloud] の宛先では、[!DNL Salesforce Marketing Cloud] [!DNL Search Attribute-Set Definitions REST] [API](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/retrieveAttributeSetDefinitions.html) を使用して、[!DNL Salesforce Marketing Cloud] 内で定義されたデータ拡張機能とそのリンク属性を動的に取得します。

これらは、**[!UICONTROL 宛先に対してオーディエンスをアクティブ化]** するためにワークフローで [ マッピング ](#mapping-considerations-example) を設定した場合、[ ターゲットフィールド ](#activate) 選択ウィンドウに表示されます。

>[!IMPORTANT]
>
> [!DNL Salesforce Marketing Cloud] 内で、アクティブ化された各Experience Platform セグメントに対して、**[!UICONTROL マッピング ID]** 内で指定された値と完全に一致する **[!UICONTROL フィールド名]** を持つ属性を作成する必要があります。 例えば、以下のスクリーンショットは、`salesforce_mc_segment_1` という名前の属性を示しています。 この宛先に対してオーディエンスをアクティブ化する場合、`salesforce_mc_segment_1` を **[!UICONTROL マッピング ID]** として追加し、Experience Platformのオーディエンスオーディエンスをこの属性に入力します。

[!DNL Salesforce Marketing Cloud] での属性の作成例を次に示します。
![ 属性を示すSalesforce Marketing Cloud UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/salesforce-custom-field.png)

>[!TIP]
>
> * 属性を作成する際は、フィールド名に空白文字を含めないでください。 代わりに、アンダースコア `(_)` 文字を区切り文字として使用します。
> * Experience Platform オーディエンスに使用する属性と [!DNL Salesforce Marketing Cloud] 内の他の属性を区別するために、Adobe セグメントに使用する属性の識別可能なプレフィックスまたはサフィックスを含めることができます。 例えば、`test_segment` の代わりに、`Adobe_test_segment` または `test_segment_Adobe` を使用します。
> * [!DNL Salesforce Marketing Cloud] で既に他の属性を作成している場合は、Experience Platform セグメントと同じ名前を使用して、[!DNL Salesforce Marketing Cloud] でオーディエンスを簡単に識別できます。

#### [!DNL Salesforce Marketing Cloud] 内でのユーザーの役割と権限の割り当て {#prerequisites-roles-permissions}

[!DNL Salesforce Marketing Cloud] はユースケースに応じてカスタムの役割をサポートするので、[!DNL Salesforce Marketing Cloud] 内の属性を更新するには、関連する役割をユーザーに割り当てる必要があります。 ユーザーに割り当てられた役割の例を次に示します。
![ 選択したユーザーのSalesforce Marketing Cloud UI で、ユーザーに割り当てられているロールを表示します。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/salesforce-edit-roles.png)

[!DNL Salesforce Marketing Cloud] ユーザーが割り当てられている役割に応じて、更新しようとしているフィールドにリンクされている [!DNL Salesforce Marketing Cloud] データ拡張機能に権限を割り当てる必要もあります。

この宛先は `[!DNL data extension]` へのアクセスが必要なため、許可する必要があります。 例えば、`Email` [!DNL data extension] の場合、以下に示すように、を許可する必要があります。

![ 許可された権限を持つメールデータ拡張機能を示すSalesforce Marketing Cloud UI。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/salesforce-permisions-list.png)

アクセスレベルを制限するために、詳細な権限を使用して個々のアクセスを上書きすることもできます。
![ 詳細な権限を持つメールデータ拡張機能を示すSalesforce Marketing Cloud UI。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/sales-email-attribute-set-permission.png)

詳しいガイダンスについては、[[!DNL Marketing Cloud Roles]](https://help.salesforce.com/s/articleView?language=en_US&amp;id=sf.mc_overview_marketing_cloud_roles.htm&amp;type=5) ページと [[!DNL Marketing Cloud Roles and Permissions]](https://help.salesforce.com/s/articleView?language=en_US&amp;id=sf.mc_overview_roles.htm&amp;type=5) ページを参照してください。

#### [!DNL Salesforce Marketing Cloud] 資格情報の収集 {#gather-credentials}

[!DNL (API) Salesforce Marketing Cloud] の宛先に対して認証を行う前に、以下の項目をメモしておきます。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| サブドメイン | [!DNL Salesforce Marketing Cloud] インターフェイスからこの値を取得する方法については、[[!DNL Salesforce Marketing Cloud domain prefix]](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/your-subdomain-tenant-specific-endpoints.html) を参照してください。 | [!DNL Salesforce Marketing Cloud] ドメインが <br> の場合 *`mcq4jrssqdlyc4lph19nnqgzzs84`.login.exacttarget.com*、<br> 値として `mcq4jrssqdlyc4lph19nnqgzzs84` を指定する必要があります。 |
| クライアント ID | [!DNL Salesforce Marketing Cloud] インターフェイスからこの値を取得する方法については、[!DNL Salesforce Marketing Cloud] [ ドキュメント ](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/access-token-s2s.html) を参照してください。 | r23kxxxxxxxx0z05xxxxxx |
| クライアント秘密鍵 | [!DNL Salesforce Marketing Cloud] インターフェイスからこの値を取得する方法については、[!DNL Salesforce Marketing Cloud] [ ドキュメント ](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/access-token-s2s.html) を参照してください。 | ipxxxxxxxxxT4xxxxxxxxxxx |

{style="table-layout:auto"}

### ガードレール {#guardrails}

* Salesforceには一定の [ レート制限 ](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/rate-limiting.html) があります。
   * 発生する可能性のある制限に対処し、実行中のエラーを減らすには、[!DNL Salesforce Marketing Cloud] [ ドキュメント ](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/rate-limiting-errors.html) を参照してください。
   * プランによって課せられる制限の詳細については、[[!DNL Salesforce Marketing Cloud]  エンゲージメントの価格 ](https://www.salesforce.com/editions-pricing/marketing-cloud/email/) ページを参照して *フルエディションの比較表をダウンロード* してください。
   * [API の概要 ](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/apis-overview.html) ページでは、追加の制限について詳しく説明しています。
   * これらの詳細を照合するページについては、[ こちら ](https://salesforce.stackexchange.com/questions/205898/marketing-cloud-api-limits) を参照してください。
* *オブジェクトごとに許可されたカスタムフィールド* の数は、Salesforce Edition によって異なります。
   * 詳しくは、[!DNL Salesforce] [ ドキュメント ](https://help.salesforce.com/s/articleView?id=sf.custom_field_allocations.htm&amp;type=5) を参照してください。
   * *オブジェクトごとに許可されるカスタムフィールド* に定義されている制限に達した場合は、[!DNL Salesforce Marketing Cloud] 内で次の操作を行う必要があります
      * [!DNL Salesforce Marketing Cloud] に新しい属性を追加する前に、古い属性を削除します。
      * [ オーディエンスのスケジュール設定 ](#schedule-segment-export-example) 手順で、**[!UICONTROL マッピング ID]** の値としてこれらの古い属性名を使用する、Experience Platformの宛先でアクティブ化されたオーディエンスを更新または削除します。

## サポートされている ID {#supported-identities}

[!DNL (API) Salesforce Marketing Cloud] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| contactKey | [!DNL Salesforce Marketing Cloud] 連絡先キー。 追加のガイダンスが必要な場合は、[!DNL Salesforce Marketing Cloud] [ ドキュメント ](https://help.salesforce.com/s/articleView?id=sf.mc_cab_contact_builder_best_practices.htm&amp;type=5) を参照してください。 | 必須 |

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | X | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li>セグメントのすべてのメンバーを、フィールドマッピングに従って、必要なスキーマフィールドと共に書き出します&#x200B;*（例：メールアドレス、電話番号、姓）*。</li><li> [!DNL Salesforce Marketing Cloud] の各セグメントのステータスは、[ オーディエンススケジュール ](#schedule-segment-export-example) 手順で提供された **[!UICONTROL マッピング ID]** 値に基づいて、Experience Platformの対応するオーディエンスステータスとともに更新されます。</li></ul> |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
> 宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]** 内で、[!DNL (API) Salesforce Marketing Cloud] を検索します。 または、**[!UICONTROL メールマーケティング]** カテゴリの下に配置することもできます。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、以下の必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。 詳しくは、[Gather [!DNL Salesforce Marketing Cloud] credentials](#gather-credentials) の節を参照してください。

| [!DNL (API) Salesforce Marketing Cloud] の宛先 | [!DNL Salesforce Marketing Cloud] |
| --- | --- |
| **[!UICONTROL サブドメイン]** | [!DNL Salesforce Marketing Cloud] ドメインのプレフィックス。 <br> 例えば、ドメインが <br> の場合 *`mcq4jrssqdlyc4lph19nnqgzzs84`.login.exacttarget.com*、を値と <br> て指定する必要 `mcq4jrssqdlyc4lph19nnqgzzs84` あります。 |
| **[!UICONTROL クライアント ID]** | [!DNL Salesforce Marketing Cloud] の `Client ID`。 |
| **[!UICONTROL クライアント秘密鍵]** | [!DNL Salesforce Marketing Cloud] の `Client Secret`。 |

![Salesforce Marketing Cloudへの認証方法を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/authenticate-destination.png)

指定した詳細が有効な場合、UI に **[!UICONTROL 接続済み]** ステータスと緑色のチェックマークが表示され、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![ 宛先の詳細を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/destination-details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
> * データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
> * *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platform から [!DNL (API) Salesforce Marketing Cloud] 宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。マッピングは、Experience Platform アカウント内の Experience Data Model （XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。

XDM フィールドを [!DNL (API) Salesforce Marketing Cloud] の宛先フィールドに正しくマッピングするには、次の手順に従います。

>[!IMPORTANT]
>
> * 属性名は [!DNL Salesforce Marketing Cloud] アカウントに従ったものですが、`contactKey` と `personalEmail.address` の両方のマッピングが必須です。
>
> * [!DNL Salesforce Marketing Cloud] API との統合には、Experience PlatformがSalesforceから取得できる属性の数のページネーション制限が適用されます。 つまり、**[!UICONTROL マッピング]** 手順の間、ターゲットフィールドスキーマにSalesforce アカウントから最大 2,000 個の属性を表示できます。

1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。画面に新しいマッピング行が表示されます。
   ![ 「新しいマッピングを追加」のExperience Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/add-new-mapping.png)
1. **[!UICONTROL ソースフィールドを選択]** ウィンドウで、**[!UICONTROL 属性を選択]** カテゴリを選択して XDM 属性を選択するか、**[!UICONTROL ID 名前空間を選択]** を選択して ID を選択します。
1. **[!UICONTROL ターゲットフィールドを選択]** ウィンドウで、「**[!UICONTROL ID 名前空間を選択]**」を選択して ID を選択するか、「**[!UICONTROL 属性を選択]**」カテゴリを選択して、必要に応じて表示されるデータ拡張機能から属性を選択します。 [!DNL (API) Salesforce Marketing Cloud] の宛先では、[!DNL Salesforce Marketing Cloud] [!DNL Search Attribute-Set Definitions REST] [API](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/retrieveAttributeSetDefinitions.html) を使用して、[!DNL Salesforce Marketing Cloud] 内で定義されたデータ拡張機能とそのリンク属性を動的に取得します。 これらは **[!UICONTROL オーディエンスのアクティブ化]** ワークフローで [ マッピング ](#mapping-considerations-example) を設定したときに、[ ターゲットフィールド ](#activate) ポップアップに表示されます。

   * これらの手順を繰り返して、XDM プロファイルスキーマと [!DNL (API) Salesforce Marketing Cloud] の間に次のマッピングを追加します。

     | ソースフィールド | ターゲットフィールド | 必須 |
     |---|---|---|
     | `IdentityMap: contactKey` | `Identity: salesforceContactKey` | `Mandatory` |
     | `xdm: personalEmail.address` | [!DNL Salesforce Marketing Cloud] [!DNL Email Addresses] データ拡張機能から `Attribute: Email Address` きます。 | `Mandatory` （新しい連絡先を追加する場合） |
     | `xdm: person.name.firstName` | 目的の [!DNL Salesforce Marketing Cloud] データ拡張機能から `Attribute: First Name` きます。 | - |

   * これらのマッピングの使用例を次に示します。

     ![ ターゲットマッピングを示したExperience Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/mappings.png)

宛先接続のマッピングの指定が完了したら、「**[!UICONTROL 次へ]**」を選択します。

### オーディエンスの書き出しのスケジュールと例 {#schedule-segment-export-example}

[ オーディエンスの書き出しをスケジュール ](/help/destinations/ui/activate-segment-streaming-destinations.md#scheduling) 手順を実行する際は、Experience Platform オーディエンスを [!DNL Salesforce Marketing Cloud] で [ 属性 ](#prerequisites-attribute) に手動でマッピングする必要があります。

これを行うには、各セグメントを選択し、[!DNL Salesforce Marketing Cloud] の属性の名前を「[!DNL (API) Salesforce Marketing Cloud] ーザー **[!UICONTROL マッピング ID]**」フィールドに入力します。 [!DNL Salesforce Marketing Cloud] で属性を作成する際のガイダンスとベストプラクティスについては、[ 内での属性の作成  [!DNL Salesforce Marketing Cloud]](#prerequisites-custom-field) の節を参照してください。

例えば、[!DNL Salesforce Marketing Cloud] 属性が `salesforce_mc_segment_1` の場合、[!DNL (API) Salesforce Marketing Cloud] **[!UICONTROL マッピング ID]** にこの値を指定し、Experience Platformのオーディエンスオーディエンスをこの属性に入力します。

[!DNL Salesforce Marketing Cloud] の属性例を以下に示します。
![ 属性を示すSalesforce Marketing Cloud UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/salesforce-custom-field.png)

[!DNL (API) Salesforce Marketing Cloud] マッピング ID **[!UICONTROL の場所を示す例を以下に示し]** す。

![ オーディエンスの書き出しのスケジュールを示したExperience Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/schedule-segment-export.png)

[!DNL (API) Salesforce Marketing Cloud] に示すように、**[!UICONTROL マッピング ID]** は、**[!UICONTROL フィールド名]** 内で指定された値 [!DNL Salesforce Marketing Cloud] 完全に一致する必要があります。

アクティブ化された各Experience Platform セグメントに対して、このセクションを繰り返します。

上記の画像に基づく典型的な例は、です。

| [!DNL (API) Salesforce Marketing Cloud] セグメント名 | [!DNL Salesforce Marketing Cloud] **[!UICONTROL フィールド名]** | [!DNL (API) Salesforce Marketing Cloud] **[!UICONTROL マッピング ID]** |
| --- | --- | --- |
| salesforce mc オーディエンス 1 | `salesforce_mc_segment_1` | `salesforce_mc_segment_1` |
| salesforce mc オーディエンス 2 | `salesforce_mc_segment_2` | `salesforce_mc_segment_2` |

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. **[!UICONTROL 宛先]**／**[!UICONTROL 参照]** を選択して、宛先のリストに移動します。
   ![ 宛先の参照を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/browse-destinations.png)

1. 宛先を選択し、ステータスが「 **[!UICONTROL 有効]**」であることを確認します。
   ![ 宛先のデータフロー実行を示したExperience Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/destination-dataflow-run.png)

1. 「**[!DNL Activation data]**」タブに切り替えて、オーディエンス名を選択します。
   ![ 宛先のアクティベーションデータを示したExperience Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/destinations-activation-data.png)

1. オーディエンスの概要を監視し、プロファイルの数がセグメント内で作成された数と一致していることを確認します。
   ![ セグメントを示すExperience Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/segment.png)

1. [[!DNL Salesforce Marketing Cloud]](https://mc.exacttarget.com/) web サイトにログインします。 次に、**[!DNL Audience Builder]** / **[!DNL Contact Builder]** / **[!DNL All contacts]** / **[!DNL Email]** ページに移動し、オーディエンスのプロファイルが追加されたかどうかを確認します。
   ![ セグメントで使用されるプロファイルを含む連絡先ページを示すSalesforce Marketing Cloud UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/contacts.png)

1. プロファイルが更新されたかどうかを確認するには、**[!UICONTROL メール]** ページに移動し、オーディエンスのプロファイルの属性値が更新されているかどうかを確認します。 成功すると、[!DNL Salesforce Marketing Cloud] の各オーディエンスステータスが、[ オーディエンスのスケジュール設定 ](#schedule-segment-export-example) 手順で提供された **[!UICONTROL マッピング ID]** 値に基づいて、Experience Platformの対応するオーディエンスステータスで更新されたことがわかります。
   ![ 選択した連絡先のメールページと更新されたオーディエンスのステータスを示すSalesforce Marketing Cloud UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/contact-detail.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

### イベントをSalesforce Marketing Cloudにプッシュする際に不明なエラーが発生しました {#unknown-errors}

* データフローの実行を確認すると、次のエラーメッセージが表示される場合があります。`Unknown errors encountered while pushing events to the destination. Please contact the administrator and try again.`
  ![ エラーを示すExperience Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-exact-target/error.png)

   * このエラーを修正するには、アクティベーションワークフローで指定した **[!UICONTROL マッピング ID]** が [!DNL (API) Salesforce Marketing Cloud] の宛先に対して [!DNL Salesforce Marketing Cloud] で作成した属性の名前と完全に一致することを確認します。 詳しくは、[ 内で属性を作成  [!DNL Salesforce Marketing Cloud]](#prerequisites-custom-field) の節を参照してください。

* セグメントをアクティブ化すると、次のエラーメッセージが表示される場合があります。`The client's IP address is unauthorized for this account. Allowlist the client's IP address...`
   * このエラーを修正するには、[!DNL Salesforce Marketing Cloud] アカウント管理者に連絡して、[Experience Platformの IP アドレス ](/help/destinations/catalog/streaming/ip-address-allow-list.md) を [!DNL Salesforce Marketing Cloud] アカウントの信頼できる IP 範囲に追加してください。 追加のガイダンスが必要な場合は、Marketing Cloudの許可リストに含める [!DNL Salesforce Marketing Cloud] [IP アドレス ](https://help.salesforce.com/s/articleView?id=sf.mc_es_ip_addresses_for_inclusion.htm&amp;type=5) ドキュメントを参照してください。

## その他のリソース {#additional-resources}

* [!DNL Salesforce Marketing Cloud] [API](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/apis-overview.html)
* 指定 [!DNL Salesforce Marketing Cloud] た情報を使用して連絡先を更新する方法を説明する [ ドキュメント ](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/updateContacts.html)。

### 変更ログ {#changelog}

この節では、この宛先コネクタに対する機能の概要と重要なドキュメントの更新について説明します。

+++ 変更ログを表示

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2023年10月 | ドキュメントの更新 | <ul><li>[ （API）Salesforce Marketing Cloudの前提条件 ](#prerequisites-destination) の節を更新し、通常はドキュメント全体で属性グループへの不要な参照を削除しました。</li> <li>オーディエンスステータスの属性を [!DNL Email Demographics] データ拡張機能内の [!DNL Salesforce Marketing Cloud] 内にのみ作成する必要があることを示すために、ドキュメントを更新しました。</li> <li>[ マッピングの考慮事項と例 ](#mapping-considerations-example) セクション内のマッピングテーブルを更新しました。`Email Addresses` データ拡張機能内の `Email Address` 属性のマッピングは必須とマークされています。この要件は、重要とマークされたコールアウトで言及されていましたが、テーブルからは省略されました。</li></ul> |
| 2023年4月 | ドキュメントの更新 | <ul><li>[ （API）Salesforce Marketing Cloudの前提条件 ](#prerequisites-destination) の節のステートメントと参照リンクを修正して、この宛先を使用する [!DNL Salesforce Marketing Cloud Engagement] めの必須のサブスクリプションであることを呼び出しました。 この節では、前に、ユーザーが続行するにはMarketing Cloud **アカウント** エンゲージメントのサブスクリプションが必要であるという誤った呼び出しがありました。</li> <li>[ 役割と権限 ](#prerequisites) をこの宛先が機能するように [!DNL Salesforce] ユーザーに割り当てるため、[ 前提条件 ](#prerequisites-roles-permissions) の節を追加しました。 （PLATIR-26299）</li></ul> |
| 2023年2月 | ドキュメントの更新 | [ （API）Salesforce Marketing Cloudの前提条件 ](#prerequisites-destination) の節を更新して、この宛先を使用するための必須のサブスクリプションで [!DNL Salesforce Marketing Cloud Engagement] ることを呼び出す参照リンクを含めました。 |
| 2023年2月 | 機能の更新 | 宛先の設定が正しくないと、不正な形式の JSON がSalesforceに送信される問題を修正しました。 これにより、一部のユーザーには、アクティベーションで多数の ID が失敗していました。 （PLATIR-26299） |
| 2023年1月 | ドキュメントの更新 | <ul><li>[ の前提条件  [!DNL Salesforce]](#prerequisites-destination) の節を更新して、[!DNL Salesforce] 側で属性を作成する必要があることを明記しました。 この節では、その方法と、[!DNL Salesforce] での属性の命名に関するベストプラクティスについて詳しく説明します。 （PLATIR-25602）</li><li>[ オーディエンススケジュール ](#schedule-segment-export-example) ステップで、アクティブ化された各オーディエンスのマッピング ID を使用する方法に関する明確な手順を追加しました。 （PLATIR-25602）</li></ul> |
| 2022年10月 | 初回リリース | 宛先の初回リリースとドキュメントの公開。 |

{style="table-layout:auto"}

+++
