---
keywords: Experience Platform；ホーム；人気の高いトピック；BigQuery;bigquery;Google BigQuery;google bigquery
solution: Experience Platform
title: Google BigQuery Source Connector の概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用してGoogle BigQuery をAdobe Experience Platformに接続する方法を説明します。
exl-id: 35c61382-a909-47f4-a937-15cb725ecbe3
source-git-commit: fa861e9740e05b4fcc4e8039bb288301d42b8357
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 34%

---

# （ベータ版） [!DNL Google BigQuery] コネクタ

>[!NOTE]
>
>この [!DNL Google BigQuery] はベータ版です。 詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。Platform は、リレーショナル、NoSQL、データウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダーのサポートは次のとおりです [!DNL Google BigQuery].

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## 前提条件

次の節では、を作成する前に必要な前提条件の設定について詳しく説明します [!DNL Google BigQuery] ソース接続。

### を生成 [!DNL Google BigQuery] 資格情報

接続するには [!DNL Google BigQuery] Platform に対して、次の資格情報の値を生成する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `project` | プロジェクトは、 [!DNL Google Cloud] リソース [!DNL Google BigQuery]. |
| `clientID` | クライアント ID は、 [!DNL Google BigQuery] OAuth 2.0 資格情報。 |
| `clientSecret` | クライアントの秘密は、 [!DNL Google BigQuery] OAuth 2.0 資格情報。 |
| `refreshToken` | 更新トークンを使用すると、API の新しいアクセストークンを取得できます。 アクセストークンの有効期間は制限され、プロジェクトの期間中に期限切れになる場合があります。 更新トークンを使用して、必要に応じて、プロジェクトの後続のアクセストークンを認証し、リクエストできます。 |

の OAuth 2.0 資格情報の生成方法に関する詳細な手順 [!DNL Google] API については、以下を参照してください。 [[!DNL Google] OAuth 2.0 認証ガイド](https://developers.google.com/identity/protocols/oauth2).

## [!DNL Google BigQuery] を Platform に接続

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Google BigQuery] と Platform を接続する方法について説明します。

### API の使用

- [フローサービス API を使用してGoogle BigQuery ベース接続を作成する](../../tutorials/api/create/databases/bigquery.md)
- [フローサービス API を使用したデータテーブルの調査](../../tutorials/api/explore/tabular.md)
- [フローサービス API を使用して、データベースソースのデータフローを作成します](../../tutorials/api/collect/database-nosql.md)

### UI の使用

- [UI でのGoogle BigQuery ソース接続の作成](../../tutorials/ui/create/databases/bigquery.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
