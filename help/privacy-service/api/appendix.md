---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 承認されたID名前空間と修飾子
topic: developer guide
translation-type: tm+mt
source-git-commit: cc296670db91640e75fd7a47b874a46eaf57ecde

---


# 付録

## 標準ID名前空間 {#standard-namespaces}

プライバシーサービスに送信されるすべてのIDは、特定のID名前空間の下に提供される必要があります。 ID名前空間は、IDが関連付けられるコンテキストを示す [Adobe Experience Platform ID Service](../../identity-service/home.md) のコンポーネントです。

次の表に、エクスペリエンスプラットフォームで使用可能な、一般的に使用される事前定義IDタイプと、その関連 `namespace` 値の概要を示します。

| IDタイプ | `namespace` | `namespaceId` |
| --- | --- | --- |
| 電子メール | 電子メール | 6 |
| 電話番号 | 電話番号 | 7 |
| Adobe Advertising Cloud ID | AdCloud | 411 |
| AdobeオーディエンスマネージャーのUUID | コア | 0 |
| Adobe Experience Cloud ID | ECID | 4 |
| AdobeターゲットID | TNTID | 9 |
| 広告主用Apple ID | IDFA | 20915 |
| Google広告ID | ガイド | 20914 |
| Windows AID | WAID | 8 |

>[!NOTE] 各IDタイプにも `namespaceId` 整数値があります。この値は、IDのプ `namespace``type` ロパティを「namespaceId」に設定する場合に、文字列の代わりに使用できます。 詳しくは、 [名前空間修飾子に関する節を参照してください](#namespace-qualifiers) 。

IDサービスAPIのエンドポイントにGET要求を行うことで、組織で使用しているID名前空間のリストを取得でき `idnamespace/identities` ます。 詳しくは、『 [IDサービス開発者ガイド](../../identity-service/api/getting-started.md) 』を参照してください。

## 名前空間修飾子

Privacy Service APIで `namespace` 値を指定する場合、 **名前空間修飾子を対応する**`type` パラメーターに含める必要があります。 次の表に、使用できる様々な名前空間修飾子の概要を示します。

| 限定子 | 定義 |
| --------- | ---------- |
| Standard | 個々の組織のデータセットに結び付けられず、グローバルに定義された標準名前空間の1つ（電子メール、電話番号など）。 名前空間IDが入力されます。 |
| custom | 組織のコンテキストで作成された、Experience Cloud全体で共有されない一意の名前空間です。 この値は、検索するフレンドリ名（「名前」フィールド）を表します。 名前空間IDが入力されます。 |
| integrationCode | 統合コード — 「カスタム」に似ていますが、特に、検索対象のデータソースの統合コードとして定義されます。 名前空間IDが入力されます。 |
| namespaceId | 値が、名前空間サービスを使用して作成またはマップされた名前空間の実際のIDであることを示します。 |
| 未登録 | 名前空間サービスで定義されていない、「現状のまま」解釈されるフリーフォーム文字列。 この種の名前空間を処理するアプリケーションは、チェック対象の会社をチェックし、アプリケーションのコンテキストとデータセットに適している場合は処理します。 名前空間IDが指定されていません。 |
| analytics | 名前空間サービスではなく、Analyticsで内部的にマッピングされるカスタム名前空間。 これは、元のリクエストで指定されたとおりに、名前空間IDなしで直接渡されます |
| target | 名前空間サービスではなく、ターゲットが内部的に理解するカスタム名前空間。 これは、元のリクエストで指定されたとおりに、名前空間IDなしで直接渡されます |

## 指定可能な製品値

次の表に、ジョブ作成リクエストの `include` 属性でAdobe製品を指定する際に使用できる値の概要を示します。

| 製品 | 属性で使用する値 `include` です |
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