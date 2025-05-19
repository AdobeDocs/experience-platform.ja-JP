---
title: Snowflake Source コネクタの概要
description: API またはユーザーインターフェイスを使用してSnowflakeをAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: df066463-1ae6-4ecd-ae0e-fb291cec4bd5
source-git-commit: 573691db9f71fcbe8b5edd4ea647d718ab3784e4
workflow-type: tm+mt
source-wordcount: '751'
ht-degree: 12%

---

# [!DNL Snowflake] ソース

>[!IMPORTANT]
>
>* Real-Time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL Snowflake] ソースを利用できます。
>* デフォルトでは、[!DNL Snowflake] ソースは `null` を空の文字列として解釈します。 `null` 値がAdobe Experience Platformで `null` 定されたとおりに正しく記述されていることを確認するには、Adobe担当者にお問い合わせください。
>* Experience Platformでデータを取り込むには、すべてのテーブルベースのバッチソースのタイムゾーンを UTC に設定する必要があります。 [!DNL Snowflake] ソースに対してサポートされているタイムスタンプは、UTC 時間を使用した TIMESTAMP_NTZ のみです。

>[!WARNING]
>
>[!DNL Snowflake] ソースの基本認証（またはアカウントキー認証）は、2025 年 11 月に非推奨（廃止予定）になります。 ソースの使用とデータベースからExperience Platformへのデータの取り込みを続行するには、キーペアベースの認証に移行する必要があります。 非推奨（廃止予定）について詳しくは、[[!DNL Snowflake]  資格情報の漏洩リスクの軽減に関するベストプラクティスガイド ](https://www.snowflake.com/en/resources/white-paper/best-practices-to-mitigate-the-risk-of-credential-compromise/) を参照してください。

Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platform は、サードパーティのデータベースからデータを取得する機能を備えています。Experience Platformは、リレーショナル、NoSQL、データウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダーのサポートには、[!DNL Snowflake] が含まれます。

## 前提条件 {#prerequisites}

この節では、[!DNL Snowflake] ソースをExperience Platformに接続する前に行う必要がある設定作業の概要を説明します。

### アカウント識別子の取得 {#retrieve-your-account-identifier}

アカウント ID を使用してExperience Platformで [!DNL Snowflake] インスタンスを認証するため、[!DNL Snowflake] UI ダッシュボードからアカウント ID を取得する必要があります。

アカウント識別子を取得するには：

* [[!DNL Snowflake]  アプリケーション UI ダッシュボード ](https://app.snowflake.com/) でアカウントに移動します。
* 左側のナビゲーションで「**[!DNL Accounts]**」を選択し、続いてヘッダーから「**[!DNL Active Accounts]**」を選択します。
* 次に、情報アイコンを選択し、現在の URL のドメイン名を選択してコピーします。

![ ドメイン名が選択されたSnowflake UI ダッシュボード。](../../images/tutorials/create/snowflake/snowflake-dashboard.png)

### 秘密鍵の取得 {#retrieve-your-private-key}

[!DNL Snowflake] 接続でキーペア認証を使用している場合は、Experience Platformに接続する前に秘密鍵も生成する必要があります。

>[!BEGINTABS]

>[!TAB  暗号化された秘密鍵の作成 ]

暗号化された [!DNL Snowflake] 秘密鍵を生成するには、ターミナルで次のコマンドを実行します。

```shell
openssl genrsa 2048 | openssl pkcs8 -topk8 -v2 des3 -inform PEM -out rsa_key.p8
```

成功した場合は、PEM 形式の秘密鍵が届きます。

```shell
-----BEGIN ENCRYPTED PRIVATE KEY-----
MIIE6T...
-----END ENCRYPTED PRIVATE KEY-----
```

>[!TAB  暗号化されていない秘密鍵の作成 ]

暗号化されていない [!DNL Snowflake] 秘密鍵を生成するには、ターミナルで次のコマンドを実行します。

```shell
openssl genrsa 2048 | openssl pkcs8 -topk8 -inform PEM -out rsa_key.p8 -nocrypt
```

成功した場合は、PEM 形式の秘密鍵が届きます。

```shell
-----BEGIN PRIVATE KEY-----
MIIE6T...
-----END PRIVATE KEY-----
```

>[!ENDTABS]

次に、秘密鍵を取得し、[!DNL Base64] でエンコードします。 [!DNL Snowflake] 秘密鍵に対して変換や形式変換を行わないようにしてください。 さらに、秘密鍵を [!DNL Base64] でエンコードする前に、秘密鍵の末尾に末尾の改行文字がないことを確認する必要があります。

### 設定の検証

[!DNL Snowflake] データのソース接続を作成する前に、次の設定も満たしていることを確認する必要があります。

* 特定のユーザーに割り当てられるデフォルトのウェアハウスは、Experience Platformへの認証時に入力するウェアハウスと同じである必要があります。
* 特定のユーザーに割り当てられたデフォルトのロールは、Experience Platformへの認証時に入力したのと同じデータベースにアクセスできる必要があります。

ロールとウェアハウスを検証するには：

* 左側のナビゲーションで「**[!DNL Admin]**」を選択し、「**[!DNL Users & Roles]**」を選択します。
* 適切なユーザーを選択し、右上隅にある省略記号（`...`）を選択します。
* 表示される [!DNL Edit user] ウィンドウで、[!DNL Default Role] に移動し、特定のユーザーに関連付けられている役割を表示します。
* 同じウィンドウで、[!DNL Default Warehouse] に移動し、特定のユーザーに関連付けられているウェアハウスを表示します。

![ ロールとウェアハウスを確認できるSnowflake UI。](../../images/tutorials/create/snowflake/snowflake-configs.png)

正常にエンコードされると、[!DNL Base64] でエンコードされた秘密鍵をExperience Platformで使用して [!DNL Snowflake] アカウントを認証できます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Snowflake] をExperience Platformに接続する方法について説明しています。

## API を使用した [!DNL Snowflake] のExperience Platformへの接続

* [Flow Service API を使用したSnowflake ベース接続の作成](../../tutorials/api/create/databases/snowflake.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用した [!DNL Snowflake] のExperience Platformへの接続

* [UI でのSnowflake ソースコネクタの作成](../../tutorials/ui/create/databases/snowflake.md)
* [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
