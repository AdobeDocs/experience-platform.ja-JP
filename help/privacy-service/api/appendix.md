---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy ServiceAPI ガイドの付録
topic-legacy: developer guide
description: このドキュメントには、Privacy ServiceAPI の操作に関する追加情報が含まれています。
exl-id: 7099e002-b802-486e-8863-0630d66e330f
source-git-commit: 532c9c9d07c112944ff26b1e48545d2b4777b06c
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 69%

---

# Privacy ServiceAPI ガイドの付録

以下の節では、Adobe Experience Platform Privacy Service API の操作に関する追加情報を示します。

## 標準 ID 名前空間 {#standard-namespaces}

に送信されるすべての ID [!DNL Privacy Service] は、特定の id 名前空間で指定する必要があります。 ID 名前空間は [Adobe Experience Platform ID サービス](../../identity-service/home.md)のコンポーネントで、ID が関連付けられているコンテキストを示します。

次の表に、で使用可能な、一般的に使用される事前定義済みの ID タイプの概要を示します [!DNL Experience Platform]と関連付けられた `namespace` 値：

| ID タイプ | `namespace` | `namespaceId` |
| --- | --- | --- |
| 電子メール | `Email` | `6` |
| Phone | `Phone` | `7` |
| Adobe Advertising Cloud ID | `AdCloud` | `411` |
| Adobe Audience Manager UUID | `CORE` | `0` |
| Adobe Experience Cloud ID | `ECID` | `4` |
| Adobe Target ID | `TNTID` | `9` |
| [!DNL Apple] 広告主の ID | `IDFA` | `20915` |
| [!DNL Google] 広告 ID | `GAID` | `20914` |
| [!DNL Windows] AID | `WAID` | `8` |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
> 各 ID タイプには整数値の `namespaceId` があります。これは、ID の `namespace` プロパティを「namespaceId」に設定する際に `type` 文字列の代わりに使用できます。詳しくは、[名前空間修飾子](#namespace-qualifiers)の節を参照してください。

組織で使用中の ID 名前空間のリストを取得するには、にGETリクエストを送信します `idnamespace/identities` エンドポイント [!DNL Identity Service] API 詳しくは、[ID サービス開発者ガイド](../../identity-service/api/getting-started.md)を参照してください。

## 名前空間修飾子

 API で `namespace`[!DNL Privacy Service] 値を指定する場合、対応する **パラメーターに**&#x200B;名前空間修飾子`type`を含める必要があります。使用できる様々な名前空間修飾子の概要を次の表に示します。

| 修飾子 | 定義 |
| --------- | ---------- |
| `standard` | 個々の組織データセット（E メールアドレス、電話番号など）に関連付けられていない、グローバルに定義された標準名前空間の 1 つです。名前空間 ID が指定されています。 |
| `custom` | 組織のコンテキストで作成された一意の名前空間で、すべての [!DNL Experience Cloud]. この値は、検索の対象となるわかりやすい名前（「name」フィールド）を表します。名前空間 ID が指定されています。 |
| `integrationCode` | 「custom」に似ていますが、特に、検索対象となるデータソースの統合コードとして定義されています。名前空間 ID が指定されています。 |
| `namespaceId` | 値が、名前空間サービスを通じて作成またはマッピングされた名前空間の実際の ID であることを示します。 |
| `unregistered` | 名前空間サービスで定義されておらず、「現状のまま」解釈されるフリーフォーム文字列です。この種の名前空間を処理するアプリケーションは、名前空間をチェックし、会社のコンテキストおよびデータセットに適している場合は処理します。名前空間 IDが指定されていません。 |
| `analytics` | 内部的にマッピングされるカスタム名前空間 [!DNL Analytics]ではなく、名前空間サービスで使用されます。 これは、名前空間 ID なしで、元のリクエストで指定されたとおりに直接渡されます。 |
| `target` | 内部的に認識されるカスタム名前空間 [!DNL Target]ではなく、名前空間サービスで使用されます。 これは、名前空間 ID なしで、元のリクエストで指定されたとおりに直接渡されます。 |

{style=&quot;table-layout:auto&quot;}

## 使用可能な製品値

ジョブ作成リクエストの `include` 属性でアドビ製品を指定するために使用できる値の概要を次の表に示します。

| 製品 | `include` 属性で使用する値 |
| --- | --- |
| Adobe Advertising Cloud | `adCloud` |
| Adobe Analytics | `analytics` |
| Adobe Audience Manager | `AudienceManager` |
| Adobe Campaign | `campaign` |
| Adobe Experience Platform（データレイク） | `aepDataLake` |
| Adobe Experience Platform（リアルタイム顧客プロファイル） | `profileService` |
| Adobe Primetime Authentication | `primetimeAuthentication` |
| Adobe Target | `target` |
| 顧客属性 (CRS) | `CRS` |
| ID サービス | `Identity` |
| Marketo Engage | `marketo` |

{style=&quot;table-layout:auto&quot;}
