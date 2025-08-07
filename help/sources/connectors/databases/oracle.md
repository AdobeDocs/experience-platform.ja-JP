---
title: Oracle DB Source コネクタの概要
description: API またはユーザーインターフェイスを使用してOracleをAdobe Experience Platformに接続する方法について説明します。
last-substantial-update: 2025-08-06T00:00:00Z
exl-id: be422cf8-fb24-48c7-8369-34f0f2ec95fc
source-git-commit: aa5496be968ee6f117649a6fff2c9e83a4ed7681
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 8%

---

# [!DNL Oracle DB]

[!DNL Oracle DB] は、[!DNL Oracle Corporation] が開発した強力なリレーショナルデータベース管理システムです。 [!DNL Oracle DB] を使用すると、大量の構造化データを効率的かつ安全に保存、管理、取得できます。

Adobe Experience Platform ソースカタログの [!DNL Oracle DB] ソースを使用すると、データベースに接続してデータをExperience Platformに取り込むことができます。

## 前提条件 {#prerequisites}

[!DNL Oracle DB] アカウントをExperience Platformに接続する前に、以下の節を参照して前提条件の設定を完了してください。

### IP アドレスの許可リスト

ソースを Azure またはAmazon Web Services（AWS）上のExperience Platformに接続する前に、許可リストに地域固有の IP アドレスを追加する必要があります。 詳しくは、[Azure およびAWS上のExperience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

### Azure 上のExperience Platformに対する認証 {#azure}

[!DNL Oracle DB] アカウントを認証し、Azure 上のExperience Platformに接続するための接続文字列を指定します。

| 資格情報 | 説明 |
| --- | --- |
| 接続文字列 | 接続文字列は、アプリケーションがデータベースへの接続に使用するテキストの文字列です。 [!DNL Oracle DB] の接続文字列パターンは `Host={HOST};Port={PORT};Sid={SID};User Id={USERNAME};Password={PASSWORD}` です。 |
| 接続仕様 ID | 接続仕様 ID は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。 [!DNL Oracle] の接続仕様 ID は `d6b52d86-f0f8-475f-89d4-ce54c8527328` です。 **注意**：この資格情報は、[!DNL Flow Service] API を使用して接続する場合にのみ必要です。 |

### Amazon Web ServicesでExperience Platformに対する認証 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../landing/multi-cloud.md) を参照してください。

次の資格情報の値を指定して [!DNL Oracle DB] アカウントを認証し、AWSでExperience Platformに接続します。

| 資格情報 | 説明 |
| --- | --- |
| サーバー | [!DNL Oracle DB] サーバーの IP アドレスまたはホストマネージャー。 |
| ポート | データベースサーバーのポート番号。 デフォルトのポート番号は `1521` です。 |
| ユーザー名 | データベースの認証およびアクセスに使用する [!DNL Oracle DB] ユーザーアカウント。 |
| パスワード | 認証に使用される、ユーザー名に関連付けられた秘密鍵。 |
| データベース | 接続先の特定の [!DNL Oracle] データベースインスタンス。 |
| スキーマ | テーブル、ビュー、プロシージャなどのデータベース・オブジェクトのコンテナ。 |
| SSL モード | SSL を適用するかどうかを制御するブール値。 この設定は、サーバーのサポートによって異なり、デフォルトは `false` です。 |
| 接続仕様 ID | 接続仕様 ID は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。 [!DNL Oracle] の接続仕様 ID は `d6b52d86-f0f8-475f-89d4-ce54c8527328` です。 **注意**：この資格情報は、[!DNL Flow Service] API を使用して接続する場合にのみ必要です。 |


## API を使用して [!DNL Oracle] と [!DNL Experience Platform] を接続する

- [Flow Service API を使用したOracle DB の接続](../../tutorials/api/create/databases/oracle.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UIを使用して [!DNL Oracle] と [!DNL Experience Platform] を接続する

- [Oracle UI ワークスペースを使用してExperience Platform DB に接続します。](../../tutorials/ui/create/databases/oracle.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
