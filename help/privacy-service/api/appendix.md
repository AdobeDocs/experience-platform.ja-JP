---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 承認されたID名前空間と修飾子
topic: developer guide
translation-type: tm+mt
source-git-commit: d0fcae6b1b75584a2c26d6eee5b47e0d60a142ba

---


# 付録

## 標準ID名前空間

プライバシーサービスに送信されるすべてのIDは、特定のID名前空間で提供されます。 ID名前空間は、IDが関連付けられるコ [ンテキストを示す](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/identity_services_architectural_overview/identity_services_architectural_overview.md) 、Adobe Experience Platform Identity Serviceのコンポーネントです。

次の表に、エクスペリエンスプラットフォームで使用可能な、一般的に使用される事前定義のIDタイプと、それに関連する値の概要を示 `namespace` します。

| IDタイプ | `namespace` | `namespaceId` |
| --- | --- | --- |
| 電子メール | 電子メール | 6 |
| 電話番号 | 電話番号 | 7 |
| Adobe Advertising Cloud ID | AdCloud | 411 |
| AdobeオーディエンスマネージャーUUID | コア | 0 |
| Adobe Experience Cloud ID | ECID | 4 |
| AdobeターゲットID | TNTID | 9 |
| 広告主のApple ID | IDFA | 20915 |
| Google広告ID | ガイド | 20914 |
| Windows AID | WAID | 8 |

>[!NOTE] 各IDタイプには整 `namespaceId` 数値も含まれ、IDのプロパティを「namespaceId」に設定する際に、 `namespace` 文字列の代わりに使 `type` 用できます。 詳しくは、 [名前空間修飾子の節を参照](#namespace-qualifiers) 。

IDサービスAPIのエンドポイントにGET要求を行うことで、組織で使用中のID名前空間のリストを取 `idnamespace/identities` 得することができます。 詳しくは、「IDサ [ービス開発者ガイド](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/identity_services_architectural_overview/identity_services_api.md) 」を参照してください。

## 名前空間修飾子

Privacy Service APIで値を指定す `namespace` る場合、対応するパラメーターに **名前空間** 修飾子を含める必要があり `type` ます。 次の表に、使用できる様々な名前空間修飾子の概要を示します。

| 限定子 | 定義 |
| --------- | ---------- |
| Standard | 個々の組織の名前空間セット（電子メール、電話番号など）に結び付けられず、グローバルに定義された標準データの1つ。 名前空間IDが指定されます。 |
| custom | 組織のコンテキストで作成された一意の名前空間で、Experience Cloud全体で共有されるものではありません。 この値は、検索するわかりやすい名前（「名前」フィールド）を表します。 名前空間IDが指定されます。 |
| integrationCode | 統合コード — 「カスタム」に似ていますが、特に、検索するデータソースの統合コードとして定義されています。 名前空間IDが指定されます。 |
| namespaceId | 値が、名前空間サービスを介して作成またはマッピングされた名前空間の実際のIDであることを示します。 |
| 未登録 | 名前空間サービスで定義されていない、「そのまま」と解釈されるフリーフォーム文字列。 この種のアプリケーションを処理する名前空間は、チェック対象をチェックし、会社のコンテキストとデータセットに適している場合は処理を行います。 名前空間IDが指定されていません。 |
| analytics | 名前空間サービスではなく、Analyticsで内部的にマッピングされるカスタム名前空間。 これは、元のリクエストで指定されたとおり、名前空間IDなしで直接渡されます |
| target | カスタム名前空間は、ターゲットサービスではなく、名前空間によって内部的に理解されます。 これは、元のリクエストで指定されたとおり、名前空間IDなしで直接渡されます |

## 受け入れられた製品値

次の表に、ジョブ作成リクエストの属性でアドビ製品を指定するた `include` めに使用できる値を示します。

| 製品 | 属性で使用する値 `include` 。 |
--- | ---
| Adobe Advertizing Cloud | &quot;AdCloud&quot; |
| Adobe Analytics | &quot;Analytics&quot; |
| Adobe Audience Manager | 「AudienceManager」 |
| Adobe Campaign | &quot;Campaign&quot; |
| Adobe Experience Platform | &quot;aepDataLake&quot; |
| Adobe Primetime Authentication | &quot;primetimeAuthentication&quot; |
| Adobe Target | &quot;Target&quot; |
| 顧客レコードサービス | &quot;CRS&quot; |
| リアルタイム顧客プロファイル | &quot;ProfileService&quot; |