---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy Service API ガイド付録
description: このドキュメントでは、Privacy Service APIの操作に関する詳細を説明します。
role: Developer
exl-id: 7099e002-b802-486e-8863-0630d66e330f
source-git-commit: 36871289743f384207bb149df6e5e1af14d4d371
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 46%

---

# Privacy Service API ガイド付録

以下の節では、Adobe Experience Platform Privacy Service APIの操作に関する追加情報を示します。

## 標準 ID 名前空間 {#standard-namespaces}

[!DNL Privacy Service]に送信されるすべてのIDは、特定のID名前空間で指定する必要があります。 ID 名前空間は [Adobe Experience Platform ID サービス](../../identity-service/home.md)のコンポーネントで、ID が関連付けられているコンテキストを示します。

次の表は、[!DNL Experience Platform]が利用できる一般的に使用される事前定義済みのID タイプと、関連する`namespace`値の概要を示しています。

| ID タイプ | `namespace` | `namespaceId` |
| --- | --- | --- |
| メール | `Email` | `6` |
| Phone | `Phone` | `7` |
| ADOBE ADVERTISING ID | `AdCloud` | `411` |
| Adobe Audience Manager UUID | `CORE` | `0` |
| Adobe Experience Cloud ID | `ECID` | `4` |
| Adobe Target ID | `TNTID` | `9` |
| 広告主向けの[!DNL Apple] ID | `IDFA` | `20915` |
| [!DNL Google]広告ID | `GAID` | `20914` |
| [!DNL Windows]件の援助 | `WAID` | `8` |

{style="table-layout:auto"}

>[!NOTE]
>
>各ID タイプには`namespaceId`整数値も含まれており、IDの`namespace` プロパティを「namespaceId」に設定する場合、`type`文字列の代わりに使用できます。 詳しくは、[名前空間修飾子](#namespace-qualifiers)の節を参照してください。

`idnamespace/identities` APIの[!DNL Identity Service] エンドポイントに対してGET リクエストを行うことで、組織で使用されているID名前空間のリストを取得できます。 詳しくは、[ID サービス開発者ガイド](../../identity-service/api/getting-started.md)を参照してください。

## 名前空間修飾子 {#namespace-qualifiers}

`namespace` APIで[!DNL Privacy Service]値を指定する場合、**名前空間修飾子**&#x200B;を対応する`type` パラメーターに含める必要があります。 使用できる様々な名前空間修飾子の概要を次の表に示します。

| 修飾子 | 定義 |
| --------- | ---------- |
| `standard` | 個々の組織データセット（メールアドレス、電話番号など）に関連付けられていない、グローバルに定義された標準名前空間の 1 つです。名前空間 ID が指定されています。 |
| `custom` | 組織のコンテキストで作成された一意の名前空間で、[!DNL Experience Cloud]間で共有されていません。 この値は、検索の対象となるわかりやすい名前（「name」フィールド）を表します。名前空間 ID が指定されています。 |
| `integrationCode` | 「custom」に似ていますが、特に、検索対象となるデータソースの統合コードとして定義されています。名前空間 ID が指定されています。 |
| `namespaceId` | 値が、名前空間サービスを通じて作成またはマッピングされた名前空間の実際の ID であることを示します。 |
| `unregistered` | 名前空間サービスで定義されておらず、「現状のまま」解釈されるフリーフォーム文字列です。この種の名前空間を処理するアプリケーションは、名前空間をチェックし、会社のコンテキストおよびデータセットに適している場合は処理します。名前空間 IDが指定されていません。 |
| `analytics` | 名前空間サービスではなく、[!DNL Analytics]内で内部マッピングされるカスタム名前空間。 これは、名前空間 ID なしで、元のリクエストで指定されたとおりに直接渡されます。 |
| `target` | カスタム名前空間は、名前空間サービスではなく、[!DNL Target]によって内部で理解されました。 これは、名前空間 ID なしで、元のリクエストで指定されたとおりに直接渡されます。 |

{style="table-layout:auto"}

## 使用可能な製品値 {#accepted-product-values}

この節では、Privacy Service ジョブ（APIまたはUI）を作成する際に`include`属性で受け入れられる製品識別子の値を示します。 これらの値は、ジョブリクエストの`include`配列で使用します。

次の表に、サポートされている製品、UI表示名、対応するコード値を示します。

>[!NOTE]
>
>- 製品値では大文字と小文字は区別されません。一貫性を保つには、キャメルケースを使用することをお勧めします。
>- 上記の製品のみがUIおよびAPIでサポートされています。 製品が組織にプロビジョニングされていない場合、その製品は無視されるか、検証エラーが発生する可能性があります。使用権限を確認するには、Adobeの契約またはプロビジョニングドキュメントを参照してください。

| ブランド製品名 | UI表示名 | `include` の値 |
| ------------------------------------------------------ | -------------------------- | ---------------------------------------- |
| Adobe Analytics | [!UICONTROL Analytics] | `analytics` |
| Adobe Audience Manager | [!UICONTROL Audience Manager] | `audienceManager` |
| Adobe Advertising | [!UICONTROL Ad Cloud] | `adCloud` |
| Adobe Experience Platform （プロファイルストア） | [!UICONTROL Profile] | `profileService` |
| Adobe Experience Platform（データレイク） | [!UICONTROL AEP Data Lake] | `aepDataLake` |
| Adobe Campaign | [!UICONTROL Campaign] | `campaign` |
| Adobe Target | [!UICONTROL Target] | `target` |
| 顧客属性 | [!UICONTROL Customer Attributes (CRS)] | `CRS` |
| Adobe Journey Optimizer | [!UICONTROL Adobe Journey Optimizer] | `cjm` |
| Marketo Engage | [!UICONTROL Marketo Engage / AJO B2B] | `marketo` |
| ID サービス | [!UICONTROL Identity] | `identity` |
| Marketo Measure | [!UICONTROL Marketo Measure] | `marketomeasure` |
| Adobe Commerce | [!UICONTROL Commerce (Personalization)] | `commerceMarketingData` |

{style="table-layout:auto"}
