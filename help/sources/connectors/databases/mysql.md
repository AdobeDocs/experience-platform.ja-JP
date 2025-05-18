---
title: MySQL Source コネクタの概要
description: API またはユーザーインターフェイスを使用して MySQL をAdobe Experience Platformに接続する方法について説明します。
last-substantial-update: 2025-05-17T00:00:00Z
exl-id: a18e8e69-880f-4bee-b339-726091d6f858
source-git-commit: f758479c37b72752bbb8a371de88bf653b2e6030
workflow-type: tm+mt
source-wordcount: '714'
ht-degree: 5%

---

# [!DNL MySQL]

[!DNL MySQL] は、構造化データの保存と管理に使用されるオープンソースのリレーショナルデータベース管理システムです。 データをテーブルに整理し、SQL （Structured Query Language）を使用して情報のクエリと更新を行います。 [!DNL MySQL] は、web アプリケーションで広く使用され、複数のプラットフォームをサポートし、速度、信頼性、使いやすさで知られています。

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
| `username` | [!DNL MySQL] データベース認証に関連付けられたユーザー名。 |
| `password` | [!DNL MySQL] データベース認証に関連付けられたパスワード。 |
| `database` | 接続先の [!DNL MySQL] データベースの名前。 |
| `sslMode` | 接続に適用する [!DNL Secure Sockets Layer] （SSL）方式。 使用可能な値は次のとおりです。 <ul><li>`DISABLED`:SSL を無効にするには、このオプションを使用します。 サーバーに SSL 設定が必要な場合、接続は失敗します</li><li>`PREFERRED`：サーバーが SSL 接続をサポートしている場合に、SSL 接続を優先させるには、このオプションを使用します。 このオプションでは、非 SSL 接続も可能です。</li><li>`REQUIRED`：このオプションを使用して、SSL 接続を必須にします。 サーバーが SSL をサポートしていない場合、接続は失敗します。</li><li>`Verify-Ca`: サーバーが SSL をサポートしていない場合に、接続の失敗時にサーバー証明書を検証するには、このオプションを使用します。</li><li>`Verify Identity`: サーバーが SSL をサポートしていない場合に、接続の失敗時にホストの名前を使用してサーバー証明書を検証するには、このオプションを使用します。</li></ul> |

>[!ENDTABS]

### Amazon Web Services上のExperience Platformに対する認証（AWS） {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../landing/multi-cloud.md) を参照してください。

AWS上のExperience Platformに接続するには、次の資格情報の値 [!DNL MySQL] 指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `server` | [!DNL MySQL] データベースの名前または IP。 |
| `username` | データベースの名前。 |
| `password` | データベースに対応するユーザー名。 |
| `database` | データベースに対応するパスワード。 |
| `sslMode` | 接続に適用する [!DNL Secure Sockets Layer] （SSL）方式。 使用可能な値は次のとおりです。 <ul><li>`DISABLED`:SSL を無効にするには、このオプションを使用します。 サーバーに SSL 設定が必要な場合、接続は失敗します</li><li>`PREFERRED`：サーバーが SSL 接続をサポートしている場合に、SSL 接続を優先させるには、このオプションを使用します。 このオプションでは、非 SSL 接続も可能です。</li><li>`REQUIRED`：このオプションを使用して、SSL 接続を必須にします。 サーバーが SSL をサポートしていない場合、接続は失敗します。</li><li>`Verify-Ca`: サーバーが SSL をサポートしていない場合に、接続の失敗時にサーバー証明書を検証するには、このオプションを使用します。</li><li>`Verify Identity`: サーバーが SSL をサポートしていない場合に、接続の失敗時にホストの名前を使用してサーバー証明書を検証するには、このオプションを使用します。</li></ul> |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL MySQL] の接続仕様 ID は `26d738e0-8963-47ea-aadf-c60de735468a` です。 **注意**：この資格情報は、[!DNL Flow Service] API を使用して接続する場合にのみ必要です。 |

## API を使用した [!DNL MySQL] のExperience Platformへの接続

- [Flow Service API [!DNL MySQL]  使用したデータベースへの接続](../../tutorials/api/create/databases/mysql.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用した MySQL のExperience Platformへの接続

- [UI を使用したデータベース  [!DNL MySQL] Experience Platformへの接続](../../tutorials/ui/create/databases/mysql.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
