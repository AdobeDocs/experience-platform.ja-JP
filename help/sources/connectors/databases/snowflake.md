---
title: SnowflakeSource コネクタの概要
description: API またはユーザーインターフェイスを使用してSnowflakeをAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: df066463-1ae6-4ecd-ae0e-fb291cec4bd5
source-git-commit: 8d6baef1549498e137d336ac2c8a42428496dedf
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 21%

---

# [!DNL Snowflake] ソース

>[!IMPORTANT]
>
>* Real-time Customer Data Platform Ultimate を購入したユーザーは、ソースカタログで [!DNL Snowflake] ソースを利用できます。
>* デフォルトでは、[!DNL Snowflake] ソースは `null` を空の文字列として解釈します。 Adobe担当者に問い合わせて、Adobe Experience Platformで `null` 定されているように `null` 値が正しく記述されていることを確認します。
>* Experience Platformでデータを取り込むには、すべてのテーブルベースのバッチソースのタイムゾーンを UTC に設定する必要があります。 [!DNL Snowflake] ソースに対してサポートされているタイムスタンプは、UTC 時間を使用した TIMESTAMP_NTZ のみです。

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platform は、サードパーティのデータベースからデータを取得する機能を備えています。Platform は、リレーショナル、NoSQL、データウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダーのサポートには、[!DNL Snowflake] が含まれます。

## 前提条件 {#prerequisites}

この節では、[!DNL Snowflake] ソースをExperience Platformに接続する前に行う必要がある設定作業の概要を説明します。

### アカウント識別子の取得 {#retrieve-your-account-identifier}

アカウント ID を使用してExperience Platform時に [!DNL Snowflake] インスタンスを認証するため、[!DNL Snowflake] UI ダッシュボードからアカウント ID を取得する必要があります。

アカウント識別子を取得するには：

* [[!DNL Snowflake]  アプリケーション UI ダッシュボード ](https://app.snowflake.com/) でアカウントに移動します。
* 左側のナビゲーションで「**[!DNL Accounts]**」を選択し、続いてヘッダーから「**[!DNL Active Accounts]**」を選択します。
* 次に、情報アイコンを選択し、現在の URL のドメイン名を選択してコピーします。

![ ドメイン名が選択されたSnowflakeUI ダッシュボード。](../../images/tutorials/create/snowflake/snowflake-dashboard.png)

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
* 特定のユーザーに割り当てられたデフォルトのロールは、Experience Platformへの認証時に入力したデータベースと同じデータベースにアクセスできる必要があります。

ロールとウェアハウスを検証するには：

* 左側のナビゲーションで「**[!DNL Admin]**」を選択し、「**[!DNL Users & Roles]**」を選択します。
* 適切なユーザーを選択し、右上隅にある省略記号（`...`）を選択します。
* 表示される [!DNL Edit user] ウィンドウで、[!DNL Default Role] に移動し、特定のユーザーに関連付けられている役割を表示します。
* 同じウィンドウで、[!DNL Default Warehouse] に移動し、特定のユーザーに関連付けられているウェアハウスを表示します。

![ ロールとウェアハウスをSnowflakeできる検証 UI。](../../images/tutorials/create/snowflake/snowflake-configs.png)

正常にエンコードされると、[!DNL Base64] でエンコードされたExperience Platformの秘密鍵を使用して [!DNL Snowflake] アカウントを認証できます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Snowflake] と Platform を接続する方法について説明します。

## API を使用して [!DNL Snowflake] と Platform を接続する

* [Flow Service API を使用したSnowflakeベース接続の作成](../../tutorials/api/create/databases/snowflake.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用した [!DNL Snowflake] の Platform への接続

* [UI でのSnowflakeソースコネクタの作成](../../tutorials/ui/create/databases/snowflake.md)
* [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
