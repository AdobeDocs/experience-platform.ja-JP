---
title: SAP Commerce接続
description: SAP Commerce宛先コネクタを使用して、SAP アカウントの顧客レコードを更新します。
last-substantial-update: 2024-02-20T00:00:00Z
exl-id: 3bd1a2a7-fb56-472d-b9bd-603b94a8937e
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '2175'
ht-degree: 21%

---

# [!DNL SAP Commerce] 接続

[!DNL SAP Commerce] （旧称 [[!DNL Hybris]](https://www.sap.com/india/products/acquired-brands/what-is-hybris.html)）は、B2B および B2C 企業向けのクラウドベースの e コマースプラットフォームソリューションで、SAP カスタマーエクスペリエンス ポートフォリオの一部として使用できます。 [[!DNL SAP]  サブスクリプション請求 &#x200B;](https://www.sap.com/products/financial-management/subscription-billing.html) は、ポートフォリオの製品であり、標準化された統合によってシンプルな販売および支払いエクスペリエンスで完全なサブスクリプションライフサイクル管理を可能にします。

この [!DNL Adobe Experience Platform][&#x200B; 宛先 &#x200B;](/help/destinations/home.md) は、[[!DNL SAP Subscription Billing] customer management API](https://api.sap.com/api/BusinessPartner_APIs/path/PUT_customers-customerNumber) を使用して、アクティベーション後に既存のExperience Platform オーディエンスから [!DNL SAP Commerce] 内で顧客の詳細を更新します。

[!DNL SAP Commerce] インスタンスを認証する手順は、さらに下の[宛先に対する認証](#authenticate)の節にあります。

## ユースケース {#use-cases}

[!DNL SAP Commerce] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

顧客 [!DNL SAP Commerce]、ビジネスとやり取りする個人または組織単位に関する情報を保存します。 チームは、[!DNL SAP Commerce] に存在するお客様を使用して、Experience Platform オーディエンスを構築します。 これらのオーディエンスを [!DNL SAP Commerce] に送信すると、情報が更新され、各顧客には、顧客が属するオーディエンスを示すオーディエンス名としてその値を持つプロパティが割り当てられます。

## 前提条件 {#prerequisites}

Experience Platformと [!DNL SAP Commerce] で設定する必要がある前提条件と、[!DNL SAP Commerce] の宛先を使用する前に収集する必要がある情報については、以下の節を参照してください。

### Experience Platform の前提条件 {#prerequisites-in-experience-platform}

[!DNL SAP Commerce] の宛先へのデータをアクティブ化する前に、[&#x200B; スキーマ &#x200B;](/help/xdm/schema/composition.md)、[&#x200B; データセット &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html) および [&#x200B; オーディエンス &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/tutorials/audiences/create-audiences.html) を [!DNL Experience Platform] で作成する必要があります。

オーディエンスのステータスに関するガイダンスが必要な場合は、[&#x200B; オーディエンスメンバーシップの詳細スキーマフィールドグループ &#x200B;](/help/xdm/field-groups/profile/segmentation.md) に関するExperience Platform ドキュメントを参照してください。

### [!DNL SAP Commerce] の宛先の前提条件 {#prerequisites-destination}

Experience Platformから [!DNL SAP Commerce] アカウントにデータを書き出すには、次の前提条件に注意してください。

#### [!DNL SAP Subscription Billing] アカウントが必要です {#prerequisites-account}

Experience Platformから [!DNL SAP Commerce] アカウントにデータを書き出すには、[!DNL SAP Subscription Billing] アカウントが必要です。 有効な請求アカウントがない場合は、[!DNL SAP] アカウントマネージャーにお問い合わせください。 詳しくは、[[!DNL SAP] Platform 設定 &#x200B;](https://help.sap.com/doc/5fd179965d5145fbbe7f2a7aa1272338/latest/en-US/PlatformConfiguration.pdf) ドキュメントを参照してください。

#### サービスキーの生成 {#prerequisites-service-key}

* [!DNL SAP Commerce] サービスキーを使用すると、Experience Platformから [!DNL SAP Subscription Billing] API にアクセスできます。 サービスキーを作成するには、[!DNL SAP Commerce][&#x200B; クライアント ID とクライアント秘密鍵を使用したサービスキーの作成 &#x200B;](https://help.sap.com/docs/CLOUD_TO_CASH_OD/1216e7b79c984675b0a6f0005e351c74/87c11a0f5dc3494eaf3baa355925c030.html#create-a-service-key-with-client-id-and-client-secret) を参照してください。 [!DNL SAP Commerce] には、以下が必要です。
   * クライアント ID
   * クライアントシークレット
   * URL。 URL パターンは次のとおりです。`https://subscriptionbilling.authentication.eu10.hana.ondemand.com` この値は、後で `Region` と `Endpoint` の値を取得する際に使用されます。

+++選択すると、サービスキーの例が表示されます。

```json
{ 
    "url": "https://eu10.revenue.cloud.sap/api",
    "uaa": {
        "clientid": "XXX",
        "clientsecret": "XXX",
        "url": "https://subscriptionbilling.authentication.eu10.hana.ondemand.com",
        "identityzone": "subscriptionbilling",
        "identityzoneid": "XXX",
        "tenantid": "XXX",
        "tenantmode": "dedicated",
        "sburl": "https://internal-xsuaa.authentication.eu10.hana.ondemand.com",
        "apiurl": "https://api.authentication.eu10.hana.ondemand.com",
        "verificationkey": "XXX",
        "xsappname": "XXX",
        "subaccountid": "XXX",
        "uaadomain": "authentication.eu10.hana.ondemand.com",
        "zoneid": "XXX",
        "credential-type": "binding-secret"
    },
    "vendor": "SAP"
}
```

+++

#### [!DNL SAP Subscription Billing] でのカスタム参照の作成 {#prerequisites-custom-reference}

[!DNL SAP Subscription Billing] のExperience Platform オーディエンスステータスを更新するには、Experience Platformで選択した各オーディエンスに対するカスタム参照フィールドが必要です。

カスタム参照を作成するには、[!DNL SAP Subscription Billing] アカウントにログインし、**[マスターデータと設定]**/**[カスタム参照]** ページに移動します。 次に、「**[!UICONTROL Create]**」を選択して、Experience Platformで選択した各オーディエンスに対して新しい参照を追加します。 これらの参照フィールド名は、後続の [&#x200B; オーディエンスの書き出しをスケジュールと例 &#x200B;](#schedule-segment-export-example) 手順で必要になります。

**[!UICONTROL Reference Type]** 内にカスタム [!DNL SAP Subscription Billing] を作成する方法の例を次に示します。
![SAP サブスクリプション請求でカスタム参照を作成する場所を示す画像。](../../assets/catalog/ecommerce/sap-commerce/create-custom-reference.png)

追加のガイダンスについては、[!DNL SAP Subscription Billing] [&#x200B; カスタムリファレンス &#x200B;](https://help.sap.com/docs/CLOUD_TO_CASH_OD/80d121f216af43648e79664efe5595f7/85696a63c8d8453a934e86c9413a25cf.html?version=2023-11-27) ドキュメントを参照してください。

### 必要な資格情報の収集 {#gather-credentials}

[!DNL SAP Commerce] をExperience Platformに接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| クライアント ID | サービスキーからの `clientId` の値。 |
| クライアントシークレット | サービスキーからの `clientSecret` の値。 |
| エンドポイント | サービスキーからの `url` の値は、`https://subscriptionbilling.authentication.eu10.hana.ondemand.com` と同様です。 |
| 領域 | データセンターの場所。 領域は `url` 内に存在し、値は `eu10` または `us10` に類似しています。 例えば、`url` が `https://eu10.revenue.cloud.sap/api` の場合、`eu10` が必要です。 |

## ガードレール {#guardrails}

[!DNL SAP Cloud Management service] への API リクエストは、[&#x200B; レート制限 &#x200B;](https://help.sap.com/docs/btp/sap-business-technology-platform/account-administration-rate-limiting) の対象となります。 レート制限を超えると、`HTTP 429 Too Many Requests` 応答ステータスコードが表示されます。

## サポートされる ID {#supported-identities}

[!DNL SAP Commerce] では、以下の表で説明する ID の更新をサポートしています。[ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
| --- | --- | --- |
| `customerNumberSAP` | [!DNL SAP Commerce] アカウントに既に存在する個人または法人の顧客の顧客識別子。 | 必須 |

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出しできるすべてのオーディエンスについて説明します。

この宛先では、Experience Platform の[セグメント化サービス](../../../segmentation/home.md)で生成したすべてのオーディエンスのアクティブ化をサポートします。

この宛先では、以下の表で説明するオーディエンスのアクティブ化もサポートされています。

| オーディエンスタイプ | サポートあり | 説明 |
| ------------- | --------- | ----------- |
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | <ul><li>フィールドマッピングに従って、目的のスキーマフィールド *（例：メールアドレス、電話番号、姓）* と共に、オーディエンスのすべてのメンバーを書き出します。</li><li> Experience Platformで選択したオーディエンスごとに、対応する [!DNL SAP Commerce] の追加属性がExperience Platformのオーディエンスステータスで更新されます。</li></ul> |
| 書き出し頻度 | **[!UICONTROL Streaming]** | <ul><li>ストリーミングの宛先は常に、API ベースの接続です。オーディエンスの評価に基づいてExperience Platform内でプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。</li></ul> |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL Manage Destinations]** アクセス制御権限 [&#x200B; が必要 &#x200B;](/help/access-control/home.md#permissions) す。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL Destinations]**/**[!UICONTROL Catalog]** 内で、[!DNL SAP Commerce] を検索します。 または、**[!UICONTROL eCommerce]** カテゴリの下に配置することもできます。

### 宛先に対する認証 {#authenticate}

以下の必須のフィールドに入力します。詳しくは、[&#x200B; サービスキーの生成 &#x200B;](#prerequisites-service-key) の節を参照してください。

| フィールド | 説明 |
| --- | --- |
| **[!UICONTROL Client ID]** | サービスキーからの `clientId` の値。 |
| **[!UICONTROL Client secret]** | サービスキーからの `clientSecret` の値。 |
| **[!UICONTROL Endpoint]** | サービスキーからの `url` の値は、`https://subscriptionbilling.authentication.eu10.hana.ondemand.com` と同様です。 |
| **[!UICONTROL Region]** | データセンターの場所。 領域は `url` 内に存在し、値は `eu10` または `us10` に類似しています。 例えば、`url` が `https://eu10.revenue.cloud.sap/api` の場合、`eu10` が必要です。 |

宛先を認証するには、「**[!UICONTROL Connect to destination]**」を選択します。
![&#x200B; 宛先への認証方法を示すExperience Platform UI からの画像。](../../assets/catalog/ecommerce/sap-commerce/authenticate-destination.png)

指定した詳細が有効な場合、UI で **[!UICONTROL Connected]** ステータスに緑色のチェックマークが付きます。 その後、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![&#x200B; 認証後に入力される宛先の詳細を示すExperience Platform UI の画像 &#x200B;](../../assets/catalog/ecommerce/sap-commerce/destination-details.png)

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Type of Customer]**：オーディエンス内のエンティティに応じて ***個人*** または ***企業*** を選択します。 [!DNL SAP Subscription Billing] [&#x200B; スキーマ &#x200B;](https://api.sap.com/api/BusinessPartner_APIs/schema) は、`customerType` 属性にマッピングされる、この選択に応じて必須フィールドを切り替えます。 選択が ***企業*** の場合、個々の顧客に必要な `firstName` や `lastName` などの必須マッピングは無視され、必須マッピングに `company` ります（その逆も同様です）。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

Adobe Experience Platformから [!DNL SAP Commerce] の宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。 マッピングは、Experience Platform アカウント内の Experience Data Model （XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。 XDM フィールドを [!DNL SAP Commerce] の宛先フィールドに正しくマッピングするには、次の手順に従います。

#### `customerNumberSAP` ID のマッピング

`customerNumberSAP` ID は、この宛先に対する必須のマッピングです。 マッピングするには、次の手順に従います。

1. **[!UICONTROL Mapping]** の手順で、「**[!UICONTROL Add new mapping]**」を選択します。 これで、新しいマッピング行が画面に表示されます。
   ![&#x200B; 「新しいマッピングを追加」ボタンがハイライト表示されたExperience Platform UI のスクリーンショット。](../../assets/catalog/ecommerce/sap-commerce/mapping-add-new-mapping.png)
1. **[!UICONTROL Select source field]** ウィンドウで **[!UICONTROL Select identity namespace]** を選択し、「`customerNumberSAP`」を選択します。
   ![ID としてマッピングするソース属性としてメールを選択するExperience Platform UI のスクリーンショット。](../../assets/catalog/ecommerce/sap-commerce/mapping-select-source-identity.png)
1. **[!UICONTROL Select target field]** ウィンドウで **[!UICONTROL Select identity namespace]** を選択し、`customerNumber` ID を選択します。
   ![ID としてマッピングするターゲット属性としてメールを選択するExperience Platform UI のスクリーンショット。](../../assets/catalog/ecommerce/sap-commerce/mapping-select-target-identity.png)

| ソースフィールド | ターゲットフィールド | 必須 |
| --- | --- | --- |
| `IdentityMap: customerNumberSAP` | `Identity: customerNumber` | ○ |

ID マッピングの例を以下に示します。
![customerNumber ID マッピングの例を示すExperience Platform UI からの画像。](../../assets/catalog/ecommerce/sap-commerce/mapping-identities.png)

#### 属性のマッピング

XDM プロファイルスキーマと [!DNL SAP Subscription Billing] アカウントの間で更新する他の属性を追加するには、次の手順を繰り返します。

1. **[!UICONTROL Mapping]** の手順で、「**[!UICONTROL Add new mapping]**」を選択します。 これで、新しいマッピング行が画面に表示されます。
   ![&#x200B; 「新しいマッピングを追加」ボタンがハイライト表示されたExperience Platform UI のスクリーンショット。](../../assets/catalog/ecommerce/sap-commerce/mapping-add-new-mapping.png)
1. **[!UICONTROL Select source field]** ウィンドウで、**[!UICONTROL Select attributes]** カテゴリを選択し、XDM 属性を選択します。
   ![&#x200B; 姓をソース属性として選択するExperience Platform UI のスクリーンショット。](../../assets/catalog/ecommerce/sap-commerce/mapping-select-source-attribute.png)
1. **[!UICONTROL Select target field]** ウィンドウでカテゴリ **[!UICONTROL Select custom attributes]** 選択し、顧客 [!DNL SAP Subscription Billing] スキーマ [&#x200B; 属性のリストから &#x200B;](https://api.sap.com/api/BusinessPartner_APIs/schema) 属性の名前を入力します。
   ![lastName がターゲット属性として定義されているExperience Platform UI のスクリーンショット。](../../assets/catalog/ecommerce/sap-commerce/mapping-select-target-attribute.png)

>[!IMPORTANT]
>
> ターゲットフィールド名では大文字と小文字が区別され、[!DNL SAP Subscription Billing] の属性名と一致する必要があります。 唯一の例外は、代わりに `country` を使用する必要がある `countryCode` です。 [!DNL SAP Subscription Billing] は alpha-2 （ISO 3166）国コードをサポートしています。 値は大文字と小文字が区別され、0 ～ 3 文字にする必要があるので、定義したとおりに指定してください。指定しないと、エラー「`The country code {} does not exist`」または「`size must be between 0 and 3`」が発生します。

#### 選択 `mandatory` た顧客タイプの属性のマッピング

必須の属性マッピングは、選択した **[!UICONTROL Type of Customer]** によって異なります。 必須属性をマッピングするには、以下から選択します。

>[!BEGINTABS]

>[!TAB  個人顧客 ]

| ソースフィールド | ターゲットフィールド | 必須 |
| --- | --- | --- |
| `xdm: person.lastName` | `Attribute: lastName` | ○ |
| `xdm: workAddress.countryCode` | `Attribute: countryCode` | ○ |

>[!TAB  法人顧客 ]

| ソースフィールド | ターゲットフィールド | 必須 |
| --- | --- | --- |
| `xdm: b2b.companyName` | `Attribute: company` | ○ |
| `xdm: workAddress.countryCode` | `Attribute: countryCode` | ○ |

>[!ENDTABS]

#### 追加属性のマッピング

次に、以下に示すように、XDM プロファイルスキーマと顧客の [!DNL SAP Subscription Billing] [&#x200B; スキーマ &#x200B;](https://api.sap.com/api/BusinessPartner_APIs/schema) 属性の間に追加のマッピングを追加できます。

>[!BEGINTABS]

>[!TAB  個人顧客 ]

| ソースフィールド | ターゲットフィールド | 必須 |
| --- | --- | --- |
| `xdm: person.name.firstName` | `Attribute: firstName` | × |
| `xdm: workAddress.street1` | `Attribute: street` | × |
| `xdm: workAddress.city` | `Attribute: city` | × |

顧客が個人である、必須の属性マッピングとオプションの属性マッピングの両方の例を次に示します。
![&#x200B; 顧客が個人の、必須とオプションの両方の属性マッピングを含む例を示すExperience Platform UI の画像。](../../assets/catalog/ecommerce/sap-commerce/mapping-attributes-individual.png)

>[!TAB  法人顧客 ]

| ソースフィールド | ターゲットフィールド | 必須 |
| --- | --- | --- |
| `xdm: workAddress.street1` | `Attribute: street` | × |
| `xdm: workAddress.city` | `Attribute: city` | × |

顧客が企業である、必須の属性マッピングとオプションの属性マッピングの例を次に示します。
![&#x200B; 顧客が企業である場合の、必須の属性マッピングとオプションの属性マッピングの両方を含む例を示すExperience Platform UI からの画像。](../../assets/catalog/ecommerce/sap-commerce/mapping-attributes-corporate.png)

>[!ENDTABS]

宛先接続のマッピングの指定を終えたら「**[!UICONTROL Next]**」を選択します。

### オーディエンスの書き出しのスケジュールと例 {#schedule-segment-export-example}

[&#x200B; オーディエンスの書き出しをスケジュール &#x200B;](/help/destinations/ui/activate-segment-streaming-destinations.md#scheduling) 手順を実行する際は、Experience Platform オーディエンスを [&#x200B; で &#x200B;](#prerequisites-attribute) 属性 [!DNL SAP Subscription Billing] に手動でマッピングする必要があります。

オーディエンスの書き出しをスケジュール手順の例では、[!DNL SAP Commerce] **[!UICONTROL Mapping ID]** の場所がハイライト表示されています。
![&#x200B; マッピング ID が入力されたスケジュールオーディエンスの書き出しを示すExperience Platformの画像 &#x200B;](../../assets/catalog/ecommerce/sap-commerce/schedule-segment-export.png)

これを行うには、各セグメントを選択し、[!DNL SAP Subscription Billing] のカスタム参照の名前を [!DNL SAP Commerce] **[!UICONTROL Mapping ID]** 宛先コネクタ フィールドに入力します。 カスタム参照の作成ガイダンスについては、[&#x200B; でのカスタム参照の作成  [!DNL SAP Subscription Billing]](#prerequisites-custom-reference) の節を参照してください。

>[!IMPORTANT]
>
> カスタム参照ラベルを値として使用しないでください。
> &#x200B;>![マッピングにカスタム参照ラベル値を使用しないことを示す画像。](../../assets/catalog/ecommerce/sap-commerce/custom-reference-dont-use-label-for-mapping.png)

例えば、選択したExperience Platform オーディエンスが `sap_audience1` で、そのステータスを [!DNL SAP Subscription Billing] カスタム参照 `SAP_1` に更新する場合は、「[!DNL SAP_Commerce] **[!UICONTROL Mapping ID]**」フィールドにこの値を指定します。

**[!UICONTROL Reference Type]** の [!DNL SAP Subscription Billing] の例を次に示します。
![SAP サブスクリプション請求でカスタム参照を作成する場所を示す画像。](../../assets/catalog/ecommerce/sap-commerce/create-custom-reference.png)

オーディエンスの書き出しをスケジュール手順の例で、オーディエンスを選択し、対応する [!DNL SAP Commerce] ーディエンス **[!UICONTROL Mapping ID]** ハイライト表示したものを次に示します。
![&#x200B; マッピング ID が入力されたスケジュールオーディエンスの書き出しを示すExperience Platformの画像 &#x200B;](../../assets/catalog/ecommerce/sap-commerce/schedule-segment-export-example.png)

ここに示すように、「**[!UICONTROL Mapping ID]**」フィールド内の値は、[!DNL SAP Subscription Billing] **[!UICONTROL Reference Type]** 値と完全に一致する必要があります。

アクティブ化された各Experience Platform オーディエンスに対して、このセクションを繰り返します。

2 つのオーディエンスを選択した場合の上記の画像に基づいて、マッピングは次のようになります。

| [!DNL SAP Commerce] オーディエンス名 | [!DNL SAP Subscription Billing] **[!UICONTROL Reference Type]** | [!DNL SAP Commerce] **[!UICONTROL Mapping ID]** 値 |
| --- | --- | --- |
| sap_audience1 | `SAP_1` | `SAP_1` |
| SAP オーディエンス 2 | `SAP_2` | `SAP_2` |

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

[!DNL SAP Subscription Billing] アカウントにログインし、**[!UICONTROL Contacts]** ページに移動してオーディエンスのステータスを確認します。 このリストは、カスタム参照の列を表示し、対応するオーディエンスステータスを表示するように設定できます。
![&#x200B; 顧客の概要ページを示す SAP 登録請求の画像。オーディエンス名とセルのオーディエンスステータスを示す列ヘッダーが含まれています &#x200B;](../../assets/catalog/ecommerce/sap-commerce/customer-overview.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

使用可能なエラータイプとその応答コードのリストについては、[[!DNL SAP Subscription Billing]  エラータイプ &#x200B;](https://help.sap.com/docs/CLOUD_TO_CASH_OD/987aec876092428f88162e438acf80d6/1a6a0dd6129c48e8b235190a1b5409fa.html) のドキュメントページを参照してください。

## その他のリソース {#additional-resources}

[!DNL SAP] ドキュメントからのその他の役に立つ情報は次のとおりです。

* [&#x200B; オンボーディング SAP サブスクリプション請求 &#x200B;](https://help.sap.com/docs/CLOUD_TO_CASH_OD/1216e7b79c984675b0a6f0005e351c74/e4b8badf7d124026991e4ab6b57d2a33.html)

### 変更ログ

この節では、この宛先コネクタに対する機能の概要と重要なドキュメントの更新について説明します。

+++ 変更ログを表示

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2024年1月 | 初回リリース | 宛先の初回リリースとドキュメントの公開。 |

{style="table-layout:auto"}

+++
