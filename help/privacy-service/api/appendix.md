---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: Privacy ServiceAPI開発者ガイド付録
topic: developer guide
description: このドキュメントには、Privacy ServiceAPIを使用するための追加情報が含まれています。
translation-type: tm+mt
source-git-commit: 5dad1fcc82707f6ee1bf75af6c10d34ff78ac311
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 73%

---


# 付録

以下の節では、Adobe Experience Platform Privacy ServiceAPIを使用するための追加情報を示します。

## 標準 ID 名前空間 {#standard-namespaces}

[!DNL Privacy Service]に送信されるすべてのIDは、特定のID名前空間で提供する必要があります。 ID 名前空間は [Adobe Experience Platform ID サービス](../../identity-service/home.md)のコンポーネントで、ID が関連付けられているコンテキストを示します。

次の表に、[!DNL Experience Platform]が使用できる、一般的に使用される事前定義IDの種類と、関連する`namespace`値の概要を示します。

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

[!DNL Identity Service] APIの`idnamespace/identities`エンドポイントにGETリクエストを行うことで、組織で使用されているID名前空間のリストを取得できます。 詳しくは、[ID サービス開発者ガイド](../../identity-service/api/getting-started.md)を参照してください。

## 名前空間修飾子

 API で `namespace`[!DNL Privacy Service] 値を指定する場合、対応する **パラメーターに**&#x200B;名前空間修飾子`type`を含める必要があります。使用できる様々な名前空間修飾子の概要を次の表に示します。

| 修飾子 | 定義 |
| --------- | ---------- |
| standard | 個々の組織データセット（E メールアドレス、電話番号など）に関連付けられていない、グローバルに定義された標準名前空間の 1 つです。名前空間 ID が指定されています。 |
| custom | 組織のコンテキストで作成された一意の名前空間で、[!DNL Experience Cloud]間で共有されるものではありません。 この値は、検索の対象となるわかりやすい名前（「name」フィールド）を表します。名前空間 ID が指定されています。 |
| integrationCode | 「custom」に似ていますが、特に、検索対象となるデータソースの統合コードとして定義されています。名前空間 ID が指定されています。 |
| namespaceId | 値が、名前空間サービスを通じて作成またはマッピングされた名前空間の実際の ID であることを示します。 |
| unregistered | 名前空間サービスで定義されておらず、「現状のまま」解釈されるフリーフォーム文字列です。この種の名前空間を処理するアプリケーションは、名前空間をチェックし、会社のコンテキストおよびデータセットに適している場合は処理します。名前空間 IDが指定されていません。 |
| analytics | 名前空間サービスではなく、[!DNL Analytics]で内部的にマップされるカスタム名前空間。 これは、名前空間 ID なしで、元のリクエストで指定されたとおりに直接渡されます。 |
| target | [!DNL Target]が内部的に理解したカスタム名前空間で、名前空間サービスには含まれません。 これは、名前空間 ID なしで、元のリクエストで指定されたとおりに直接渡されます。 |

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