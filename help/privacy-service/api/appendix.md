---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 使用可能な ID 名前空間と修飾子
topic: developer guide
translation-type: tm+mt
source-git-commit: 5b32c1955fac4f137ba44e8189376c81cdbbfc40
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 79%

---


# 付録

## 標準 ID 名前空間 {#standard-namespaces}

All identities that are sent to [!DNL Privacy Service] must be provided under a specific identity namespace. ID 名前空間は [Adobe Experience Platform ID サービス](../../identity-service/home.md)のコンポーネントで、ID が関連付けられているコンテキストを示します。

The following table outlines several commonly used, pre-defined identity types made available by [!DNL Experience Platform], along with their associated `namespace` values:

| ID タイプ | `namespace` | `namespaceId` |
| --- | --- | --- |
| 電子メール | 電子メール | 6 |
| 電話番号 | 電話番号 | 7 |
| Adobe Advertising Cloud ID | AdCloud | 411 |
| Adobe Audience Manager UUID | CORE | 0 |
| Adobe Experience Cloud ID | ECID | 4 |
| Adobe Target ID | TNTID | 9 |
| [!DNL Apple] 広告主の ID | IDFA | 20915 |
| [!DNL Google] 広告 ID | GAID | 20914 |
| [!DNL Windows] AID | WAID | 8 |

>[!NOTE]
>
> 各 ID タイプには整数値の `namespaceId` があります。これは、ID の `namespace` プロパティを「namespaceId」に設定する際に `type` 文字列の代わりに使用できます。詳しくは、[名前空間修飾子](#namespace-qualifiers)の節を参照してください。

You can retrieve a list of identity namespaces in use by your organization by making a GET request to the `idnamespace/identities` endpoint in the [!DNL Identity Service] API. 詳しくは、[ID サービス開発者ガイド](../../identity-service/api/getting-started.md)を参照してください。

## 名前空間修飾子

 API で `namespace`[!DNL Privacy Service] 値を指定する場合、対応する **パラメーターに**&#x200B;名前空間修飾子`type`を含める必要があります。使用できる様々な名前空間修飾子の概要を次の表に示します。

| 修飾子 | 定義 |
| --------- | ---------- |
| standard | 個々の組織データセット（E メールアドレス、電話番号など）に関連付けられていない、グローバルに定義された標準名前空間の 1 つです。名前空間 ID が指定されています。 |
| custom | A unique namespace created in the context of an organization, not shared across the [!DNL Experience Cloud]. この値は、検索の対象となるわかりやすい名前（「name」フィールド）を表します。名前空間 ID が指定されています。 |
| integrationCode | 「custom」に似ていますが、特に、検索対象となるデータソースの統合コードとして定義されています。名前空間 ID が指定されています。 |
| namespaceId | 値が、名前空間サービスを通じて作成またはマッピングされた名前空間の実際の ID であることを示します。 |
| unregistered | 名前空間サービスで定義されておらず、「現状のまま」解釈されるフリーフォーム文字列です。この種の名前空間を処理するアプリケーションは、名前空間をチェックし、会社のコンテキストおよびデータセットに適している場合は処理します。名前空間 IDが指定されていません。 |
| analytics | A custom namespace that is mapped internally in [!DNL Analytics], not in the namespace service. これは、名前空間 ID なしで、元のリクエストで指定されたとおりに直接渡されます。 |
| target | A custom namespace understood internally by [!DNL Target], not in the namespace service. これは、名前空間 ID なしで、元のリクエストで指定されたとおりに直接渡されます。 |

## 使用可能な製品値

ジョブ作成リクエストの `include` 属性でアドビ製品を指定するために使用できる値の概要を次の表に示します。

| 製品 | `include` 属性で使用する値 |
--- | ---
| Adobe Advertising Cloud | &quot;AdCloud&quot; |
| Adobe Analytics | &quot;Analytics&quot; |
| Adobe Audience Manager | &quot;AudienceManager&quot; |
| Adobe Campaign | &quot;Campaign&quot; |
| Adobe Experience Platform | &quot;aepDataLake&quot; |
| Adobe Primetime Authentication | &quot;primetimeAuthentication&quot; |
| Adobe Target | &quot;Target&quot; |
| 顧客レコードサービス | &quot;CRS&quot; |
| リアルタイム顧客プロファイル | &quot;ProfileService&quot; |