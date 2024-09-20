---
title: Google BigQuery Source コネクタの概要
description: API またはユーザーインターフェイスを使用してGoogle BigQuery をAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 35c61382-a909-47f4-a937-15cb725ecbe3
source-git-commit: 1fa79b31b5a257ebb3cbd60246b757d8a4a63d7c
workflow-type: tm+mt
source-wordcount: '529'
ht-degree: 25%

---

# [!DNL Google BigQuery] ソース

>[!IMPORTANT]
>
>Real-time Customer Data Platform Ultimate を購入したユーザーは、ソースカタログで [!DNL Google BigQuery] ソースを利用できます。

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Google BigQuery] アカウントをExperience Platformに正常に接続するために必要な前提条件の手順については、このドキュメントを参照してください。

## 前提条件 {#prerequisites}

次の節では、[!DNL Google BigQuery] ソース接続を作成する前に必要な前提条件の設定について詳しく説明します。

### IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

### [!DNL Google BigQuery] 資格情報の生成 {#credentials}

[!DNL Google BigQuery] をExperience Platformに接続するには、次の資格情報の値を生成する必要があります。

>[!BEGINTABS]

>[!TAB  基本認証 ]

OAuth 2.0 認証と基本認証の組み合わせを使用して認証するには、次の資格情報に適切な値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `project` | プロジェクトは、[!DNL Google BigQuery] を含む [!DNL Google Cloud] リソースのベースレベルの整理エンティティです。 |
| `clientID` | クライアント ID は、[!DNL Google BigQuery] OAuth 2.0 資格情報の半分です。 |
| `clientSecret` | クライアントシークレットは、[!DNL Google BigQuery] OAuth 2.0 資格情報の残り半分です。 |
| `refreshToken` | 更新トークンを使用すると、API の新しいアクセストークンを取得できます。 アクセストークンの有効期間は制限されており、プロジェクトの期間中に期限切れになる場合があります。 更新トークンを使用すると、必要に応じて、プロジェクトの後続のアクセストークンを認証およびリクエストできます。 |
| `largeResultsDataSetId` | （オプション）大きな結果セットのサポートを有効にするために必要な、事前に作成された [!DNL Google BigQuery] データセット ID。 |

[!DNL Google] API 用に OAuth 2.0 資格情報を生成する方法について詳しくは、次の [[!DNL Google] OAuth 2.0 認証ガイド ](https://developers.google.com/identity/protocols/oauth2) を参照してください。

>[!TAB  サービス認証 ]

サービス認証を使用して認証するには、次の資格情報に適切な値を指定します。

**注意**: サービス認証で正常に認証するには、サービス アカウントに **[!DNL BigQuery Job User]**、**[!DNL BigQuery Data Viewer]**、**[!DNL BigQuery Read Session User]**、**[!DNL BigQuery Data Owner]** などの十分な権限が必要です。

| 資格情報 | 説明 |
| --- | --- |
| `projectId` | クエリ対象となる [!DNL Google BigQuery] の ID。 |
| `keyFileContent` | サービスアカウントの認証に使用されるキーファイル。 この値は [[!DNL Google Cloud service accounts] dashboard](https://console.cloud.google.com) から取得できます。 キーファイルのコンテンツは JSON 形式です。 Experience Platformへの認証時に、[!DNL Base64] でこれをエンコードする必要があります。 |
| `largeResultsDataSetId` | （オプション）大きな結果セットのサポートを有効にするために必要な、事前に作成された [!DNL Google BigQuery] データセット ID。 |

[!DNL Google BigQuery] でのサービスアカウントの使用について詳しくは、[ でのサービスアカウントの使用  [!DNL Google BigQuery]](https://cloud.google.com/bigquery/docs/use-service-accounts) に関するガイドを参照してください。

>[!ENDTABS]

## [!DNL Google BigQuery] を Platform に接続

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Google BigQuery] と Platform を接続する方法について説明します。

### API の使用

- [Flow Service API を使用したGoogle BigQuery ベース接続の作成](../../tutorials/api/create/databases/bigquery.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

### UI の使用

- [UI でのGoogle BigQuery ソースコネクタの作成](../../tutorials/ui/create/databases/bigquery.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
