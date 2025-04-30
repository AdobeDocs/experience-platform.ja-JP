---
title: PostgreSQL Source コネクタの概要
description: Adobe Experience Platformの PostgreSQL ソースについて説明します。
last-substantial-update: 2025-04-29T00:00:00Z
exl-id: 27b891c5-5fc5-4539-8f98-e3a53e2eefe3
source-git-commit: 04634c6edc13d8b8da01a01077161866235c61b1
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 7%

---

# [!DNL PostgreSQL]

このドキュメントでは、[!DNL PostgreSQL] データベースをAdobe Experience Platformに接続する前に実行する必要がある前提条件の手順について説明します。

## 前提条件 {#prerequisites}

[!DNL PostgreSQL] データベースをExperience Platformに接続する前に、以下の節を参照して前提条件の設定を完了してください。

### IP アドレスの許可リスト

ソースを Azure またはAmazon Web Services（AWS）上のExperience Platformに接続する前に、許可リストに地域固有の IP アドレスを追加する必要があります。 詳しくは、[Azure およびAWS上のExperience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

### Azure 上のExperience Platformに対する認証 {#azure}

Azure 上のExperience Platformに接続するには、次の認証資格情報の値を指定 [!DNL PostgreSQL] る必要があります。

>[!BEGINTABS]

>[!TAB  アカウントキー認証 ]

アカウントキー認証を使用して [!DNL PostgreSQL] データベースを Azure 上のExperience Platformに接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `connectionString` | [!DNL PostgreSQL] アカウントに関連付けられた接続文字列。 [!DNL PostgreSQL] の接続文字列パターンは `Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}` です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL PostgreSQL] の接続仕様 ID は `74a1c565-4e59-48d7-9d67-7c03b8a13137` です。 |

詳しくは、[[!DNL PostgreSQL]  ドキュメント ](https://www.postgresql.org/docs/current/) を参照してください。

>[!TAB  基本認証 ]

基本認証を使用して [!DNL PostgreSQL] データベースを Azure 上のExperience Platformに接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `server` | [!DNL PostgreSQL] データベースの名前または IP アドレス。 |
| `port` | [!DNL PostgreSQL] サーバーの TCP ポート。 |
| `username` | [!DNL PostgreSQL] データベース認証に関連付けられたユーザー名。 |
| `password` | [!DNL PostgreSQL] データベース認証に関連付けられたパスワード。 |
| `database` | 接続先の [!DNL PostgreSQL] データベースの名前。 |
| `sslMode` | 接続に適用する [!DNL Secure Sockets Layer] （SSL）方式。 使用可能な値は次のとおりです。 <ul><li>`Disable`:SSL を無効にするには、このオプションを使用します。 サーバーに SSL 設定が必要な場合、接続は失敗します。</li><li>`Allow`：このオプションを使用して、SSL 接続を許可します。 非 SSL 接続は、サーバーがサポートしている場合でも使用できます。</li><li>`Prefer`：サーバーが SSL 接続をサポートしている場合に、SSL 接続を優先させるには、このオプションを使用します。 このオプションでは、非 SSL 接続も可能です。</li><li>`Require`：このオプションを使用して、SSL 接続を必須にします。 サーバーが SSL をサポートしていない場合、接続は失敗します。</li><li>`Verify-Ca`: サーバーが SSL をサポートしていない場合に、接続の失敗時にサーバー証明書を検証するには、このオプションを使用します。</li><li>`Verify-Full`: サーバーが SSL をサポートしていない場合に、接続の失敗時にホストの名前を使用してサーバー証明書を検証するには、このオプションを使用します。</li></ul> |

詳しくは、[[!DNL PostgreSQL]  ドキュメント ](https://www.postgresql.org/docs/current/) を参照してください。

>[!ENDTABS]

### Amazon Web Services上のExperience Platformに対する認証（AWS） {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../landing/multi-cloud.md) を参照してください。

基本認証を使用して [!DNL PostgreSQL] データベースをAWS上のExperience Platformに接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `server` | [!DNL PostgreSQL] データベースの名前または IP アドレス。 |
| `port` | [!DNL PostgreSQL] サーバーの TCP ポート。 |
| `username` | [!DNL PostgreSQL] データベース認証に関連付けられたユーザー名。 |
| `password` | [!DNL PostgreSQL] データベース認証に関連付けられたパスワード。 |
| `database` | 接続先の [!DNL PostgreSQL] データベースの名前。 |
| `sslMode` | 接続に適用する [!DNL Secure Sockets Layer] （SSL）方式。 使用可能な値は次のとおりです。 <ul><li>`Disable`:SSL を無効にするには、このオプションを使用します。 サーバーに SSL 設定が必要な場合、接続は失敗します。</li><li>`Allow`：このオプションを使用して、SSL 接続を許可します。 非 SSL 接続は、サーバーがサポートしている場合でも使用できます。</li><li>`Prefer`：サーバーが SSL 接続をサポートしている場合に、SSL 接続を優先させるには、このオプションを使用します。 このオプションでは、非 SSL 接続も可能です。</li><li>`Require`：このオプションを使用して、SSL 接続を必須にします。 サーバーが SSL をサポートしていない場合、接続は失敗します。</li><li>`Verify-Ca`: サーバーが SSL をサポートしていない場合に、接続の失敗時にサーバー証明書を検証するには、このオプションを使用します。</li><li>`Verify-Full`: サーバーが SSL をサポートしていない場合に、接続の失敗時にホストの名前を使用してサーバー証明書を検証するには、このオプションを使用します。</li></ul> |

詳しくは、[[!DNL PostgreSQL]  ドキュメント ](https://www.postgresql.org/docs/current/) を参照してください。

## API を使用した [!DNL PostgreSQL] のExperience Platformへの接続

* [Flow Service API を使用した [!DNL PostgreSQL] ベース接続の作成](../../tutorials/api/create/databases/postgres.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用した [!DNL PostgreSQL] のExperience Platformへの接続

* [UI での  [!DNL PostgreSQL]  ソース接続の作成](../../tutorials/ui/create/databases/postgres.md)
* [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
