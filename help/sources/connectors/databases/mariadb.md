---
title: MariaDB Source コネクタの概要
description: API またはユーザーインターフェイスを使用して MariaDB をAdobe Experience Platformに接続する方法について説明します。
last-substantial-update: 2025-04-29T00:00:00Z
exl-id: 37b8f991-dca9-4f85-9bdd-4927a015e4c0
source-git-commit: bca4f40d452f0a5e70a388872a65640d1fd58533
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 26%

---

# [!DNL MariaDB]

Adobe Experience Platform では、外部ソースからデータを取り込むと同時に、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platform は、サードパーティのデータベースからデータを取得する機能を備えています。リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのデータベースに接続で [!DNL Experience Platform] ます。 データベースプロバイダーのサポートには、[!DNL MariaDB] が含まれます。

## 前提条件 {#prerequisites}

[!DNL MariaDB] アカウントをExperience Platformに接続する前に、以下の節を参照して前提条件の設定を完了してください。

### IP アドレスの許可リスト

ソースをExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[Experience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

### Experience Platformに対する認証

Experience Platformに接続するには、次の資格情報の値を指定する必要 [!DNL MariaDB] あります。

>[!BEGINTABS]

>[!TAB  アカウントキー認証 ]

アカウントキー認証を使用するには、次の資格情報に適切な値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `connectionString` | [!DNL MariaDB] 認証に関連付けられた接続文字列。 [!DNL MariaDB] の接続文字列パターンは `Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}` です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL MariaDB] の接続仕様 ID は `3000eb99-cd47-43f3-827c-43caf170f015` です。 **注意**：この資格情報は、[!DNL Flow Service] API を使用して接続する場合にのみ必要です。 |

接続文字列の取得について詳しくは、この [[!DNL MariaDB]  ドキュメント &#x200B;](https://mariadb.com/kb/en/about-mariadb-connector-odbc/) を参照してください。

>[!TAB  基本認証 ]

基本認証を使用するには、次の資格情報に適切な値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `server` | [!DNL MariaDB] データベースの名前または IP。 |
| `username` | データベースの名前。 |
| `port` | 接続先の通信エンドポイントのポート番号。 |
| `password` | データベースに対応するユーザー名。 |
| `database` | データベースに対応するパスワード。 |
| `sslMode` | データ転送中にデータを暗号化する方法。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL MariaDB] の接続仕様 ID は `3000eb99-cd47-43f3-827c-43caf170f015` です。 **注意**：この資格情報は、[!DNL Flow Service] API を使用して接続する場合にのみ必要です。 |

接続文字列の取得について詳しくは、この [[!DNL MariaDB]  ドキュメント &#x200B;](https://mariadb.com/kb/en/about-mariadb-connector-odbc/) を参照してください。

>[!ENDTABS]

## API を使用した [!DNL MariaDB] のExperience Platformへの接続

- [Flow Service API を使用した MariaDB ベース接続の作成](../../tutorials/api/create/databases/mariadb.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用した [!DNL MariaDB] のExperience Platformへの接続

- [UI での MariaDB ソース接続の作成](../../tutorials/ui/create/databases/mariadb.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
