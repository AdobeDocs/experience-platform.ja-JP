---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy ServiceAPI ガイドの付録
description: このドキュメントには、Privacy ServiceAPI の操作に関する追加情報が含まれています。
role: Developer
exl-id: 7099e002-b802-486e-8863-0630d66e330f
source-git-commit: 644e85fe5c9b1a37f69c75755713e929736c2e89
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 57%

---

# Privacy ServiceAPI ガイドの付録

次の節では、Adobe Experience Platform Privacy Service API の操作に関する追加情報を示します。

## 標準 ID 名前空間 {#standard-namespaces}

[!DNL Privacy Service] に送信される ID はすべて、特定の ID 名前空間の下で指定する必要があります。 ID 名前空間は [Adobe Experience Platform ID サービス](../../identity-service/home.md)のコンポーネントで、ID が関連付けられているコンテキストを示します。

次の表に、[!DNL Experience Platform] で使用可能になる、一般的に使用される事前定義済みの ID タイプと、関連する `namespace` 値の概要を示します。

| ID タイプ | `namespace` | `namespaceId` |
| --- | --- | --- |
| 電子メール | `Email` | `6` |
| Phone | `Phone` | `7` |
| Adobe Advertising Cloud ID | `AdCloud` | `411` |
| Adobe Audience Manager UUID | `CORE` | `0` |
| Adobe Experience Cloud ID | `ECID` | `4` |
| Adobe Target ID | `TNTID` | `9` |
| 広告主の [!DNL Apple] ID | `IDFA` | `20915` |
| [!DNL Google] 広告 ID | `GAID` | `20914` |
| [!DNL Windows] AID | `WAID` | `8` |

{style="table-layout:auto"}

>[!NOTE]
>
>各 ID タイプには `namespaceId` の整数値もあり、ID の `type` プロパティを「namespaceId」に設定すると、`namespace` 文字列の代わりに使用できます。 詳しくは、[名前空間修飾子](#namespace-qualifiers)の節を参照してください。

[!DNL Identity Service] API の `idnamespace/identities` エンドポイントにGETリクエストをおこなうことで、組織で使用されている ID 名前空間のリストを取得できます。 詳しくは、[ID サービス開発者ガイド](../../identity-service/api/getting-started.md)を参照してください。

## 名前空間修飾子

[!DNL Privacy Service] API で `namespace` 値を指定する場合は、対応する `type` パラメーターに **名前空間修飾子** を含める必要があります。 使用できる様々な名前空間修飾子の概要を次の表に示します。

| 修飾子 | 定義 |
| --------- | ---------- |
| `standard` | 個々の組織データセット（メールアドレス、電話番号など）に関連付けられていない、グローバルに定義された標準名前空間の 1 つです。名前空間 ID が指定されています。 |
| `custom` | 組織のコンテキストで作成された一意の名前空間で、[!DNL Experience Cloud] 全体では共有されません。 この値は、検索の対象となるわかりやすい名前（「name」フィールド）を表します。名前空間 ID が指定されています。 |
| `integrationCode` | 「custom」に似ていますが、特に、検索対象となるデータソースの統合コードとして定義されています。名前空間 ID が指定されています。 |
| `namespaceId` | 値が、名前空間サービスを通じて作成またはマッピングされた名前空間の実際の ID であることを示します。 |
| `unregistered` | 名前空間サービスで定義されておらず、「現状のまま」解釈されるフリーフォーム文字列です。この種の名前空間を処理するアプリケーションは、名前空間をチェックし、会社のコンテキストおよびデータセットに適している場合は処理します。名前空間 IDが指定されていません。 |
| `analytics` | 名前空間サービスではなく、[!DNL Analytics] 内部でマッピングされるカスタム名前空間。 これは、名前空間 ID なしで、元のリクエストで指定されたとおりに直接渡されます。 |
| `target` | 名前空間サービスではなく、[!DNL Target] によって内部的に認識されるカスタム名前空間。 これは、名前空間 ID なしで、元のリクエストで指定されたとおりに直接渡されます。 |

{style="table-layout:auto"}

## 使用可能な製品値

ジョブ作成リクエストの `include` 属性でアドビ製品を指定するために使用できる値の概要を次の表に示します。

>[!NOTE]
>
>商品のリストの値は、大文字と小文字を区別しません。 キャメルケースはお勧めしますが、適用されません。

| 製品 | `include` 属性で使用する値 |
| --- | --- |
| Adobe Advertising Cloud | `adCloud` |
| Adobe Analytics | `analytics` |
| Adobe Audience Manager | `audienceManager` |
| Adobe Campaign | `campaign` |
| Adobe Experience Platform（data lake） | `aepDataLake` |
| Adobe Experience Platform（リアルタイム顧客プロファイル） | `profileService` |
| Adobe Pass 認証 | `primetimeAuthentication` |
| Adobe Target | `target` |
| 顧客属性（CRS） | `CRS` |
| カスタマージャーニーの管理 | `cjm` |
| ID サービス | `identity` |
| Marketo Engage | `marketo` |
| Marketo Measure | `marketomeasure` |

{style="table-layout:auto"}
