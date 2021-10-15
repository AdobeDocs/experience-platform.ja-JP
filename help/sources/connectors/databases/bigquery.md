---
keywords: Experience Platform；ホーム；人気のあるトピック；BigQuery;bigquery;Google BigQuery;google bigquery
solution: Experience Platform
title: Google BigQuery ソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用してGoogle BigQuery をAdobe Experience Platformに接続する方法を説明します。
exl-id: 35c61382-a909-47f4-a937-15cb725ecbe3
source-git-commit: 5821f9304a37c1a03d17f0113d09548799662a2e
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 15%

---

# （ベータ版） [!DNL Google BigQuery] コネクタ

>[!NOTE]
>
>[!DNL Google BigQuery] はベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../home.md#terms-and-conditions)」を参照してください。

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。Platform は、リレーショナル、NoSQL、データウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダのサポートは [!DNL Google BigQuery] です。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

## 前提条件

次の節では、[!DNL Google BigQuery] ソース接続を作成する前に必要な前提条件の設定に関する詳細を説明します。

### [!DNL Google BigQuery] 資格情報を生成する

[!DNL Google BigQuery] を Platform に接続するには、次の資格情報の値を生成する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `project` | プロジェクトは、[!DNL Google BigQuery] を含む [!DNL Google Cloud] リソースの基本レベルの組織エンティティです。 |
| `clientID` | クライアント ID は、[!DNL Google BigQuery] OAuth 2.0 資格情報の半分です。 |
| `clientSecret` | クライアントシークレットは、[!DNL Google BigQuery] OAuth 2.0 資格情報の残りの半分です。 |
| `refreshToken` | 更新トークンを使用すると、API 用の新しいアクセストークンを取得できます。 アクセストークンの有効期間は限られており、プロジェクトの期間中に期限切れになる場合があります。 更新トークンを使用して、必要に応じて、プロジェクトの認証と後続のアクセストークンのリクエストをおこなうことができます。 |

[!DNL Google] API の OAuth 2.0 資格情報を生成する方法について詳しくは、次の [[!DNL Google] OAuth 2.0 認証ガイド ](https://developers.google.com/identity/protocols/oauth2) を参照してください。

## [!DNL Google BigQuery] を Platform に接続

以下のドキュメントでは、API またはユーザーインターフェイスを使用して [!DNL Google BigQuery] を Platform に接続する方法について説明します。

### API の使用

- [フローサービス API を使用したGoogle BigQuery ベース接続の作成](../../tutorials/api/create/databases/bigquery.md)
- [フローサービス API を使用したデータベースソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/database-nosql.md)
- [フローサービス API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

### UI の使用

- [UI でのGoogle BigQuery ソース接続の作成](../../tutorials/ui/create/databases/bigquery.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
