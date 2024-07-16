---
title: Google BigQuery Source コネクタの概要
description: API またはユーザーインターフェイスを使用してGoogle BigQuery をAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 35c61382-a909-47f4-a937-15cb725ecbe3
source-git-commit: 9a8139c26b5bb5ff937a51986967b57db58aab6c
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 34%

---

# [!DNL Google BigQuery] ソース

>[!IMPORTANT]
>
>Real-time Customer Data Platform Ultimate を購入したユーザーは、ソースカタログで [!DNL Google BigQuery] ソースを利用できます。

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] では、サードパーティのデータベースからデータを取り込むことができます。 Platform は、リレーショナル、NoSQL、データウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダーのサポートには、[!DNL Google BigQuery] が含まれます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## 前提条件

次の節では、[!DNL Google BigQuery] ソース接続を作成する前に必要な前提条件の設定について詳しく説明します。

### [!DNL Google BigQuery] 資格情報の生成

[!DNL Google BigQuery] を Platform に接続するには、次の資格情報の値を生成する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `project` | プロジェクトは、[!DNL Google BigQuery] を含む [!DNL Google Cloud] リソースのベースレベルの整理エンティティです。 |
| `clientID` | クライアント ID は、[!DNL Google BigQuery] OAuth 2.0 資格情報の半分です。 |
| `clientSecret` | クライアントシークレットは、[!DNL Google BigQuery] OAuth 2.0 資格情報の残り半分です。 |
| `refreshToken` | 更新トークンを使用すると、API の新しいアクセストークンを取得できます。 アクセストークンの有効期間は制限されており、プロジェクトの期間中に期限切れになる場合があります。 更新トークンを使用すると、必要に応じて、プロジェクトの後続のアクセストークンを認証およびリクエストできます。 |
| `largeResultsDataSetId` | 大きな結果セットのサポートを有効にするために必要な、事前に作成された [!DNL Google BigQuery] データセット ID。 |

[!DNL Google] API 用に OAuth 2.0 資格情報を生成する方法について詳しくは、次の [[!DNL Google] OAuth 2.0 認証ガイド ](https://developers.google.com/identity/protocols/oauth2) を参照してください。

## [!DNL Google BigQuery] を Platform に接続

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Google BigQuery] と Platform を接続する方法について説明します。

### API の使用

- [Flow Service API を使用したGoogle BigQuery ベース接続の作成](../../tutorials/api/create/databases/bigquery.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

### UI の使用

- [UI でのGoogle BigQuery ソースコネクタの作成](../../tutorials/ui/create/databases/bigquery.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
