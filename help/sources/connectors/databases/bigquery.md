---
keywords: Experience Platform；ホーム；人気のあるトピック；BigQuery;bigquery;Google BigQuery;google bigquery
solution: Experience Platform
title: Google BigQueryソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してGoogle BigQueryをAdobe Experience Platformに接続する方法を説明します。
exl-id: 35c61382-a909-47f4-a937-15cb725ecbe3
source-git-commit: 9d68e54baa894ebeff4603c7df01a1fe42aa217f
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 16%

---

# （ベータ版） [!DNL Google BigQuery]コネクタ

>[!NOTE]
>
>[!DNL Google BigQuery]はベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ソースの概要](../../home.md#terms-and-conditions)」を参照してください。

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。Platformは、リレーショナル、NoSQL、データウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダのサポートは[!DNL Google BigQuery]です。

## IPアドレス許可リスト

ソースコネクタを操作する前に、IPアドレスのリストを許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加しないと、ソースを使用する際にエラーやパフォーマンスが低下する可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)のページを参照してください。

## 前提条件

以下の節では、[!DNL Google BigQuery]ソース接続を作成する前に必要な前提条件の設定に関する詳細を説明します。

### [!DNL Google BigQuery]資格情報を生成する

[!DNL Google BigQuery]をPlatformに接続するには、次の資格情報の値を生成する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `project` | プロジェクトは、[!DNL Google BigQuery]を含む、[!DNL Google Cloud]リソースの基本レベルの組織エンティティです。 |
| `clientID` | クライアントIDは、[!DNL Google BigQuery] OAuth 2.0資格情報の半分です。 |
| `clientSecret` | クライアント秘密鍵は、[!DNL Google BigQuery] OAuth 2.0資格情報の残りの半分です。 |
| `refreshToken` | 更新トークンを使用すると、APIの新しいアクセストークンを取得できます。 アクセストークンの有効期間は限られており、プロジェクトの期間中に期限切れになる場合があります。 更新トークンを使用して、必要に応じて、プロジェクトの後続のアクセストークンを認証し、リクエストできます。 |

[!DNL Google] APIのOAuth 2.0資格情報を生成する方法について詳しくは、次の[[!DNL Google] OAuth 2.0認証ガイド](https://developers.google.com/identity/protocols/oauth2)を参照してください。

## [!DNL Google BigQuery]をPlatformに接続

以下のドキュメントでは、APIまたはユーザーインターフェイスを使用して[!DNL Google BigQuery]をPlatformに接続する方法について説明します。

### API の使用

- [フローサービスAPIを使用したGoogle BigQueryソース接続の作成](../../tutorials/api/create/databases/bigquery.md)
- [フローサービスAPIを使用したデータベースシステムの調査](../../tutorials/api/explore/database-nosql.md)
- [フローサービスAPIを使用したデータベースからのデータの収集](../../tutorials/api/collect/database-nosql.md)

### UI の使用

- [UIでのGoogle BigQueryソース接続の作成](../../tutorials/ui/create/databases/bigquery.md)
- [UIでのデータベース接続のデータフローの設定](../../tutorials/ui/dataflow/databases.md)
