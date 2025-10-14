---
title: Azure Synapse Analytics Source コネクタの概要
description: API またはユーザーインターフェイスを使用してAzure Synapse Analytics をAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
last-substantial-update: 2025-06-17T00:00:00Z
exl-id: 5b94ae74-e5a7-40e9-a952-41eddf06dcde
source-git-commit: f8eb8640360205e8ae9579d4b664d4880bf8a368
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 8%

---

# [!DNL Azure Synapse Analytics]

>[!IMPORTANT]
>
>Real-Time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL Azure Synapse Analytics] ソースを利用できます。

[!DNL Azure Synapse Analytics] は、ビッグデータとデータウェアハウスを統合するクラウドベースの分析サービスです。 SQL、[!DNL Spark]、リアルタイムのツールを使用して、データを移動することなく、データの取り込み、調査、準備、分析を行うことができます。

[!DNL Azure Synapse Analytics] ソースを使用してアカウントを接続し、データをAdobe Experience Platformに取り込むことができます。

## 前提条件 {#prerequisites}

[!DNL Azure Synapse Analytics] アカウントをExperience Platformに接続する前に、以下の節を参照して前提条件の設定を完了してください。

### IP アドレスの許可リスト

ソースをExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[Experience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

### 権限の設定

ソースアカウントをExperience Platformに接続するには、アカウントで次の両方の権限が有効になっている必要があります。

* **ソースの表示**
* **ソースの管理**

これらの権限がない場合は、製品管理者に連絡してアクセス権をリクエストしてください。 詳しくは、[&#x200B; アクセス制御 UI ガイド &#x200B;](../../../access-control/ui/overview.md) を参照してください。

### Experience Platformに対する認証

アカウントキー認証またはサービスプリンシパルキー認証のいずれかを使用して、[!DNL Azure Synapse Analytics] をExperience Platformに接続できます。

>[!BEGINTABS]

>[!TAB  アカウントキー認証 ]

アカウントキー認証を使用して [!DNL Azure Synapse Analytics] データベースをExperience Platformに接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| 接続文字列 | [!DNL Azure Synapse Analytics] での認証に使用する **接続文字列** です。 標準形式は `Server=tcp:{SERVER_NAME}.database.windows.net,1433;Database={DATABASE};User ID={USERNAME}@{SERVER_NAME};Password={PASSWORD};Trusted_Connection=False;Encrypt=True;Connection Timeout=30` です。 プレースホルダーを実際の接続の詳細に置き換える必要があります。 |
| 接続仕様 ID | **接続仕様** は、データソースのコネクタプロパティを提供します。 認証仕様や、**ベース** 接続と **ソース** 接続の両方を作成するための要件などの詳細が含まれます。 [!DNL Azure Synapse Analytics] の場合、接続仕様 ID は `a49bcc7d-8038-43af-b1e4-5a7a089a7d79` です。 **メモ：** この資格情報は、API 経由で接続する場合にのみ必要です。 |

>[!TAB  サービスプリンシパルキー認証 ]

サービスプリンシパルキー認証用の資格情報を取得するには、[[!DNL Microsfot Entra admin center]](https://entra.microsoft.com/#home) に移動して、次の値を取得します。

* アプリ ID
* 表示名
* 秘密鍵
* テナント ID

次に、[[!DNL Azure Synapse Analytics]  インスタンス &#x200B;](https://azure.microsoft.com/en-ca/products/synapse-analytics) に移動し、外部プロバイダーからユーザーを作成するオプションを選択します。 ここから、スキーマのサービスプリンシパルに適した権限を指定します。 **メモ：**：スキーマのプレビューには、「コピー」と同様に、「SELECT」を含める必要があります。 例えば、次のようなコマンドがあります。

```SQL
GRANT SELECT ON SCHEMA::dbo TO {APP_ID};
```

サービスプリンシパルキー認証を使用して [!DNL Azure Synapse Analytics] データベースをExperience Platformに接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| サーバー | [!DNL Azure Synapse Analytics] SQL エンドポイントの完全修飾ドメイン名。 |
| データベース | [!DNL Azure Synapse Analytics] ワークスペース内の特定のデータベースの名前。 |
| テナント | [!DNL Azure] サブスクリプションに関連付けられた [!DNL Azure Active Directory] テナント ID。 |
| サービスプリンシパル ID | [!DNL Azure Active Directory] アプリケーションのクライアント ID。 |
| サービスプリンシパルキー | サービスプリンシパルに関連付けられたクライアントの秘密鍵またはパスワード。 |
| 接続仕様 ID | **接続仕様** は、データソースのコネクタプロパティを提供します。 認証仕様や、**ベース** 接続と **ソース** 接続の両方を作成するための要件などの詳細が含まれます。 [!DNL Azure Synapse Analytics] の場合、接続仕様 ID は `a49bcc7d-8038-43af-b1e4-5a7a089a7d79` です。 **メモ：** この資格情報は、API 経由で接続する場合にのみ必要です。 |

詳しくは、[[!DNL Azure]  の ID の管理に関するドキュメント  [!DNL Azure Synapse Analytics]](https://learn.microsoft.com/en-us/azure/synapse-analytics/synapse-service-identity) を参照してください。

>[!ENDTABS]

## API を使用した [!DNL Azure Synapse Analytics] のExperience Platformへの接続

* [Flow Service API [!DNL Azure Synapse Analytics]  使用したExperience Platformへの接続](../../tutorials/api/create/databases/synapse-analytics.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用した [!DNL Azure Synapse Analytics] のExperience Platformへの接続

* [UI [!DNL Azure Synapse Analytics] Experience Platformへの接続](../../tutorials/ui/create/databases/synapse-analytics.md)
* [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)

