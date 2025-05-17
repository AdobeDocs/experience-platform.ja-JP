---
title: MySQL Source コネクタの概要
description: API またはユーザーインターフェイスを使用して MySQL をAdobe Experience Platformに接続する方法について説明します。
last-substantial-update: 2025-05-17T00:00:00Z
exl-id: a18e8e69-880f-4bee-b339-726091d6f858
source-git-commit: 7a5dae76c5b58b302b4f3295efc17f40dbb9b18b
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 3%

---

# [!DNL MySQL]

[!DNL MySQL] は、構造化データの保存と管理に使用されるオープンソースのリレーショナルデータベース管理システムです。 データをテーブルに整理し、SQL （Structured Query Language）を使用して情報のクエリと更新を行います。 [!DNL MySQL] は、web アプリケーションで広く使用され、複数のプラットフォームをサポートし、速度、信頼性、使いやすさで知られています。 小規模な Web サイトから大規模な企業システムまで、あらゆる環境に最適です。

[!DNL MySQL] ソースを使用すると、アカウントを接続して [!DNL MySQL] データベースからAdobe Experience Platformにデータを取り込むことができます。

## 前提条件 {#prerequisites}

[!DNL MySQl] アカウントをExperience Platformに接続する前に、以下の節を参照して前提条件の設定を完了してください。

### IP アドレスの許可リスト

ソースを Azure またはAmazon Web Services（AWS）上のExperience Platformに接続する前に、許可リストに地域固有の IP アドレスを追加する必要があります。 詳しくは、[Azure およびAWS上のExperience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

### Azure 上のExperience Platformに対する認証 {#azure}

アカウントキー認証または基本認証のいずれかを使用して、[!DNL MySQL] データベースを Azure 上のExperience Platformに接続できます。

>[!BEGINTABS]

>[!TAB  アカウントキー認証 ]

アカウントキー認証を使用して [!DNL MySQL] データベースをExperience Platformに接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `connectionString` | アカウントに関連付けられた [!DNL MySQL] の接続文字列。 [!DNL MySQL] の接続文字列パターンは `Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}` です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL MySQL] の接続仕様 ID は `26d738e0-8963-47ea-aadf-c60de735468a` です。 |

詳しくは、[[!DNL MySQL]  接続文字列に関するドキュメント ](https://dev.mysql.com/doc/connector-net/en/connector-net-connections-string.html) を参照してください。

>[!TAB  基本認証 ]

基本認証を使用して [!DNL MySQL] データベースをExperience Platformに接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `server` | [!DNL MySQL] データベースの名前または IP アドレス。 |
| `database` | 接続先の [!DNL MySQL] データベースの名前。 |
| `username` | [!DNL MySQL] データベース認証に関連付けられたユーザー名。 |
| `password` | [!DNL MySQL] データベース認証に関連付けられたパスワード。 |
| `sslMode` | 接続に適用する [!DNL Secure Sockets Layer] （SSL）方式。 使用可能な値は次のとおりです。 <ul><li>`DISABLED`:SSL を無効にするには、このオプションを使用します。 サーバーに SSL 設定が必要な場合、接続は失敗します</li><li>`PREFERRED`：サーバーが SSL 接続をサポートしている場合に、SSL 接続を優先させるには、このオプションを使用します。 このオプションでは、非 SSL 接続も可能です。</li><li>`REQUIRED`：このオプションを使用して、SSL 接続を必須にします。 サーバーが SSL をサポートしていない場合、接続は失敗します。</li><li>`Verify-Ca`: サーバーが SSL をサポートしていない場合に、接続の失敗時にサーバー証明書を検証するには、このオプションを使用します。</li><li>`Verify Identity`: サーバーが SSL をサポートしていない場合に、接続の失敗時にホストの名前を使用してサーバー証明書を検証するには、このオプションを使用します。</li></ul> |

>[!ENDTABS]

## API を使用した [!DNL MySQL] のExperience Platformへの接続

- [Flow Service API [!DNL MySQL]  使用したデータベースへの接続](../../tutorials/api/create/databases/mysql.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用した MySQL のExperience Platformへの接続

- [UI を使用したデータベース  [!DNL MySQL] Experience Platformへの接続](../../tutorials/ui/create/databases/mysql.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
