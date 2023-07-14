---
keywords: crm;CRM;crm 宛先;Microsoft Dynamics 365;Microsoft Dynamics 365 crm 宛先
title: Microsoft Dynamics 365 接続
description: Microsoft Dynamics 365 の宛先を使用すると、アカウントデータを書き出し、Microsoft Dynamics 365 内でビジネスニーズに合わせてアクティブ化できます。
last-substantial-update: 2022-11-08T00:00:00Z
exl-id: 49bb5c95-f4b7-42e1-9aae-45143bbb1d73
source-git-commit: 416f4ff28ae5ca7ee3235f020ce012352d6002c7
workflow-type: tm+mt
source-wordcount: '1927'
ht-degree: 77%

---

# [!DNL Microsoft Dynamics 365] 接続

## 概要 {#overview}

[[!DNL Microsoft Dynamics 365]](https://dynamics.microsoft.com/ja-jp/) は、クラウドベースのビジネスアプリケーションプラットフォームです。エンタープライズリソースプランニング（ERP）と顧客関係管理（CRM）を生産性アプリケーションと AI ツールと組み合わせて、エンドツーエンドのスムーズで制御可能な運用、優れた成長の可能性およびコスト削減を実現します。

この [!DNL Adobe Experience Platform] [宛先](/help/destinations/home.md) は [[!DNL Contact Entity Reference API]](https://docs.microsoft.com/ja-jp/dynamics365/customerengagement/on-premises/developer/entities/contact?view=op-9-1)：オーディエンス内の id をに更新できます。 [!DNL Dynamics 365].

[!DNL Dynamics 365] は、認証付与を使用する OAuth 2 を認証メカニズムとして使用して、[!DNL Contact Entity Reference API] と通信します。[!DNL Dynamics 365] インスタンスを認証する手順は、さらに下の[宛先に対する認証](#authenticate)の節にあります。

## ユースケース {#use-cases}

マーケターは、Adobe Experience Platform プロファイルの属性に基づいて、ユーザーにパーソナライズされたエクスペリエンスを提供できます。 オフラインデータからオーディエンスを構築し、そのオーディエンスをに送信できます。 [!DNL Dynamics 365]:Adobe Experience Platformでオーディエンスとプロファイルが更新されるとすぐにユーザーのフィードに表示されます。

## 前提条件 {#prerequisites}

### Experience Platform の前提条件 {#prerequisites-in-experience-platform}

[!DNL Dynamics 365] 宛先へのデータをアクティブ化する前に、[スキーマ](/help/xdm/schema/composition.md)、[データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja)および[セグメント](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=ja)を [!DNL Experience Platform] で作成する必要があります。

詳しくは、Adobeのドキュメントを参照してください。 [セグメントメンバーシップの詳細スキーマフィールドグループ](/help/xdm/field-groups/profile/segmentation.md) オーディエンスのステータスに関するガイダンスが必要な場合は、を参照してください。

### [!DNL Microsoft Dynamics 365] 前提条件 {#prerequisites-destination}

Platform から [!DNL Dynamics 365] アカウントにデータを書き出すには、[!DNL Dynamics 365] で次の前提条件に注意してください。

#### [!DNL Microsoft Dynamics 365] アカウントが必要です {#prerequisites-account}

アカウントをまだお持ちでない場合は、[!DNL Dynamics 365] [体験版](https://dynamics.microsoft.com/ja-jp/dynamics-365-free-trial/)ページに移動し、アカウントを登録して作成してください。

#### [!DNL Dynamics 365] 内にフィールドを作成 {#prerequisites-custom-field}

タイプのカスタムフィールドの作成 `Simple` フィールドのデータ型を次のように設定 `Single Line of Text` どのExperience Platformが、 [!DNL Dynamics 365].
追加のガイダンスが必要な場合は、[!DNL Dynamics 365] ドキュメントを参照して[フィールド（属性）を作成](https://docs.microsoft.com/ja-jp/dynamics365/customerengagement/on-premises/customize/create-edit-fields?view=op-9-1)してください。

[!DNL Dynamics 365] 内の設定例を次に示します。
![カスタムフィールドを示す Dynamics 365 UI のスクリーンショット。](../../assets/catalog/crm/microsoft-dynamics-365/dynamics-365-fields.png)

#### Azure Active Directory 内でのアプリケーションとアプリケーションユーザーの登録 {#prerequisites-app-user}

[!DNL Dynamics 365] をリソースにアクセスできるようにするには、[!DNL Azure Account] を使用して [[!DNL Azure Active Directory]](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/howto-create-service-principal-portal#register-an-application-with-azure-ad-and-create-a-service-principal) にログインし、以下を作成する必要があります。
* [!DNL Azure Active Directory] アプリケーション
* サービスプリンシパル
* アプリケーション秘密鍵

また、[!DNL Azure Active Directory] で[アプリケーションユーザーを作成](https://docs.microsoft.com/ja-jp/power-platform/admin/manage-application-users#create-an-application-user)し、新しく作成したアプリケーションに関連付ける必要があります。

#### [!DNL Dynamics 365] 資格情報の収集 {#gather-credentials}

[!DNL Dynamics 365] CRM 宛先に対して認証を行う前に、以下の項目をメモしておきます。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| `Client ID` | お使いの [!DNL Azure Active Directory] アプリケーションの [!DNL Dynamics 365] クライアント ID。詳しくは、[[!DNL Dynamics 365] ドキュメント](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-and-app-id-values-for-signing-in)を参照してください。 | `ababbaba-abab-baba-acac-acacacacacac` |
| `Client Secret` | お使いの [!DNL Azure Active Directory] アプリケーションの [!DNL Dynamics 365] クライアント秘密鍵。[[!DNL Dynamics 365] ドキュメント](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/howto-create-service-principal-portal#authentication-two-options)内のオプション #2 を使用することになります。 | 説明のための `abcde~abcdefghijklmnopqrstuvwxyz12345678`。 |
| `Tenant ID` | お使いの [!DNL Azure Active Directory] アプリケーションの [!DNL Dynamics 365] テナント ID。詳しくは、[[!DNL Dynamics 365] ドキュメント](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-and-app-id-values-for-signing-in)を参照してください。 | `1234567-aaaa-12ab-ba21-1234567890` |
| `Region` | 環境 URL に関連付けられたMicrosoft地域です。<br>詳しくは、[[!DNL Dynamics 365] ドキュメント](https://learn.microsoft.com/en-us/power-platform/admin/new-datacenter-regions)を参照してください。 | ドメインが次のような場合、 [宛先](#authenticate).<br> *org57771b33.`crm`.dynamics.com*<br>  次に例を示します。会社が北米 (NAM) 地域でプロビジョニングされている場合、URL は次のようになります。 `crm.dynamics.com` 次を選択します。 `crm`. 会社がカナダ (CAN) 地域でプロビジョニングされている場合、URL は次のようになります。 `crm3.dynamics.com` 次を選択します。 `crm3`. |
| `Environment URL` | 詳しくは、[[!DNL Dynamics 365] ドキュメント](https://docs.microsoft.com/ja-jp/dynamics365/customerengagement/on-premises/developer/org-service/discover-url-organization-organization-service?view=op-9-1)を参照してください。 | お使いの [!DNL Dynamics 365] ドメインが以下のようになっている場合は、ハイライト表示された値が必要です。<br> *`org57771b33`.crm.dynamics.com* |

{style="table-layout:auto"}

## ガードレール {#guardrails}

[リクエストの制限と割り当て](https://docs.microsoft.com/ja-jp/power-platform/admin/api-request-limits-allocations)ページでは、[!DNL Dynamics 365] ライセンスに関連する [!DNL Dynamics 365] API 制限を詳細に説明しています。データとペイロードがこれらの制約内にあることを確認する必要があります。

## サポートされる ID {#supported-identities}

[!DNL Dynamics 365] では、以下の表で説明する ID の更新をサポートしています。[ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 例 | 説明 | 注意点 |
|---|---|---|---|
| `contactId` | 7eb682f1-ca75-e511-80d4-00155d2a68d1 | 連絡先の一意の ID。 | **必須**。詳しくは、[[!DNL Dynamics 365] ドキュメント](https://docs.microsoft.com/ja-jp/dynamics365/customerengagement/on-premises/developer/entities/contact?view=op-9-1)を参照してください。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li>セグメントのすべてのメンバーを、フィールドマッピングに従って、必要なスキーマフィールドと共に書き出します&#x200B;*（例：メールアドレス、電話番号、姓）*。</li><li> での各オーディエンスのステータス [!DNL Dynamics 365] は、 **[!UICONTROL マッピング ID]** 期間中に指定された値 [オーディエンススケジュール](#schedule-segment-export-example) 手順</li></ul> |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | <ul><li>ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 [ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。</li></ul> |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL 宛先]**／**[!UICONTROL カタログ]**&#x200B;内で [!DNL Dynamics 365] を検索します。または、**[!UICONTROL CRM]** カテゴリの下に配置することもできます。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、「 **[!UICONTROL 宛先に接続]**」を選択します。
![認証方法を示す Platform UI のスクリーンショット。](../../assets/catalog/crm/microsoft-dynamics-365/authenticate-destination.png)

以下の必須のフィールドに入力します。詳しくは、[Dynamics 365 資格情報の収集](#gather-credentials)の節を参照してください。
* **[!UICONTROL クライアント ID]**：お使いの [!DNL Azure Active Directory] アプリケーションの [!DNL Dynamics 365] クライアント ID 。
* **[!UICONTROL テナント ID]**：お使いの [!DNL Azure Active Directory] アプリケーションの [!DNL Dynamics 365] テナント ID 。
* **[!UICONTROL クライアント秘密鍵]**：お使いの [!DNL Azure Active Directory] アプリケーションの [!DNL Dynamics 365] クライアント秘密鍵。
* **[!UICONTROL 地域]**:お使いの [[!DNL Dynamics 365]](https://learn.microsoft.com/en-us/power-platform/admin/new-datacenter-regions) 地域。 次に例を示します。会社が北米 (NAM) 地域でプロビジョニングされている場合、URL は次のようになります。 `crm.dynamics.com` 次を選択します。 `crm`. 会社がカナダ (CAN) 地域でプロビジョニングされている場合、URL は次のようになります。 `crm3.dynamics.com` 次を選択します。 `crm3`.
* **[!UICONTROL 環境 URL]**：お使いの [!DNL Dynamics 365] 環境 URL。

指定した詳細が有効な場合、UI で&#x200B;**[!UICONTROL 接続済み]**&#x200B;ステータスに緑色のチェックマークが付きます。その後、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![宛先の詳細を示す Platform UI のスクリーンショット。](../../assets/catalog/crm/microsoft-dynamics-365/destination-details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対するオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [ストリーミングオーディエンスの書き出し先に対するプロファイルとオーディエンスのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platform から [!DNL Dynamics 365] 宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。マッピングは、Platform アカウント内の Experience Data Model（XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。 XDM フィールドを [!DNL Dynamics 365] 宛先フィールドに正しくマッピングするには、次の手順に従います。

1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。画面に新しいマッピング行が表示されます。
   ![「新しいマッピングを追加」の Platform UI のスクリーンショットの例。](../../assets/catalog/crm/microsoft-dynamics-365/add-new-mapping.png)

1. **[!UICONTROL ソースフィールドを選択]**&#x200B;ウィンドウで、「**[!UICONTROL ID 名前空間カテゴリを選択]**」を選択します。`contactId`
   ![ソースマッピング用の Platform UI のスクリーンショットの例。](../../assets/catalog/crm/microsoft-dynamics-365/source-mapping.png)

1. **[!UICONTROL ターゲットフィールドを選択]**&#x200B;ウィンドウで、ソースフィールドにマッピングするターゲットフィールドのタイプを選択します。
   * **[!UICONTROL ID 名前空間を選択]**：このオプションを選択して、ソースフィールドをリストから ID 名前空間にマッピングします。
     ![contactId のターゲットマッピングを示す Platform UI のスクリーンショット。](../../assets/catalog/crm/microsoft-dynamics-365/target-mapping-contactid.png)

   * XDM プロファイルスキーマと [!DNL Dynamics 365] インスタンスの間の次のマッピングを追加：
|XDM プロファイルスキーマ|[!DNL Dynamics 365] インスタンス| 必須|
|—|—|—| 
|`contactId`|`contactId`| はい |

   * **[!UICONTROL カスタム属性を選択]**：このオプションを選択して、「**[!UICONTROL 属性名]**」フィールドに定義するカスタム属性にマッピングするソースフィールドを選択します。サポートされる属性の包括的なリストについては、[[!DNL Dynamics 365] ドキュメント](https://docs.microsoft.com/ja-jp/dynamics365/customerengagement/on-premises/developer/entities/contact?view=op-9-1#entity-properties)を参照してください。
     ![LastName のターゲットマッピングを示す Platform UI のスクリーンショット。](../../assets/catalog/crm/microsoft-dynamics-365/target-mapping-lastname.png)

     >[!IMPORTANT]
     >
     >[!DNL Dynamics 365] [日付またはタイムスタンプ](https://docs.microsoft.com/ja-jp/power-apps/developer/data-platform/webapi/reference/timestampdatemapping?view=dataverse-latest)ターゲットフィールドでマッピングされた日付またはタイムスタンプのソースフィールドがある場合、マッピングされた値が空でないことを確認します。 渡された値が空の場合、*`Bad request reported while pushing events to the destination. Please contact the administrator and try again.`* エラーメッセージが表示され、データは更新されません。 （これは [!DNL Dynamics 365] の制限です。）

   * 例えば、更新する値に応じて、XDM プロファイルスキーマと [!DNL Dynamics 365] インスタンスの間に次のようにマッピングを追加します。
|XDM プロファイルスキーマ|[!DNL Dynamics 365] インスタンス|
|—|—| 
|`person.name.firstName`|`FirstName`| 
|`person.name.lastName`|`LastName`| 
|`personalEmail.address`|`Email`|

   * これらのマッピングの使用例を次に示します。
     ![ターゲットマッピングを示した Platform UI のスクリーンショットの例。](../../assets/catalog/crm/microsoft-dynamics-365/mappings.png)

### オーディエンスのエクスポートと例をスケジュール {#schedule-segment-export-example}

内 [[!UICONTROL オーディエンスの書き出しをスケジュール]](/help/destinations/ui/activate-segment-streaming-destinations.md#scheduling) アクティベーションワークフローの手順では、Platform オーディエンスを、 [!DNL Dynamics 365].

これを行うには、各セグメントを選択し、対応するカスタムフィールド属性を [!DNL Dynamics 365] の「**[!UICONTROL マッピング ID]**」フィールドから入力します。

>[!IMPORTANT]
>
>**[!UICONTROL マッピング ID]** に使用する値は、[!DNL Dynamics 365] 内で作成されたカスタムフィールド属性の名前と正確に一致している必要があります。カスタムフィールド属性を見つけるガイダンスが必要な場合は、[[!DNL Dynamics 365] ドキュメント](https://docs.microsoft.com/ja-jp/dynamics365/customerengagement/on-premises/customize/create-edit-fields?view=op-9-1) を参照してください。

次に例を示します。
![スケジュールオーディエンスの書き出しを示した、Platform UI のスクリーンショットの例。](../../assets/catalog/crm/microsoft-dynamics-365/schedule-segment-export.png)

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. **[!UICONTROL 宛先]**／**[!UICONTROL 参照]** を選択して、宛先のリストに移動します。
   ![宛先の参照を示す Platform UI のスクリーンショット。](../../assets/catalog/crm/microsoft-dynamics-365/browse-destinations.png)

1. 宛先を選択し、ステータスが「 **[!UICONTROL 有効]**」であることを確認します。
   ![宛先のデータフロー実行を示した Platform UI のスクリーンショット。](../../assets/catalog/crm/microsoft-dynamics-365/destination-dataflow-run.png)

1. 次に切り替え： **[!DNL Activation data]** 」タブをクリックし、オーディエンス名を選択します。
   ![宛先のアクティベーションデータを示した Platform UI のスクリーンショットの例。](../../assets/catalog/crm/microsoft-dynamics-365/destinations-activation-data.png)

1. オーディエンスの概要を監視し、プロファイルの数がセグメント内で作成された数に対応していることを確認します。
   ![セグメントを示す Platform UI のスクリーンショットの例。](../../assets/catalog/crm/microsoft-dynamics-365/segment.png)

1. にログインします。 [!DNL Dynamics 365] web サイトに移動し、 [!DNL Customers] > [!DNL Contacts] ページを開き、オーディエンスからのプロファイルが追加されたかどうかを確認します。 各オーディエンスのステータスは、 [!DNL Dynamics 365] は、 **[!UICONTROL マッピング ID]** 期間中に指定された値 [オーディエンススケジュール](#schedule-segment-export-example) 手順
   ![Dynamics 365 UI のスクリーンショットに、オーディエンスのステータスが更新された連絡先ページが表示されています。](../../assets/catalog/crm/microsoft-dynamics-365/contacts.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

### イベントを宛先にプッシュする際に不明なエラーが発生しました {#unknown-errors}

データフローの実行を確認する際に、次のエラーメッセージが表示される場合。`Bad request reported while pushing events to the destination. Please contact the administrator and try again.`

![無効なリクエストエラーを示す Platform UI のスクリーンショット。](../../assets/catalog/crm/microsoft-dynamics-365/error.png)

このエラーを修正するには、 **[!UICONTROL マッピング ID]** 指定した [!DNL Dynamics 365] は、Platform オーディエンスが有効で、内に存在する場合に使用します。 [!DNL Dynamics 365].

## その他のリソース {#additional-resources}

[[!DNL Dynamics 365]  ドキュメント](https://docs.microsoft.com/ja-jp/dynamics365/)からのその他の役に立つ情報は次のとおりです。
* [IOrganizationService.Update(Entity) メソッド](https://docs.microsoft.com/ja-jp/dotnet/api/microsoft.xrm.sdk.iorganizationservice.update?view=dataverse-sdk-latest)
* [Web API を使用したテーブル行の更新と削除](https://docs.microsoft.com/ja-jp/power-apps/developer/data-platform/webapi/update-delete-entities-using-web-api#basic-update)
