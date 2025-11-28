---
title: Snowflake Source コネクタの概要
description: API またはユーザーインターフェイスを使用してSnowflakeをAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: df066463-1ae6-4ecd-ae0e-fb291cec4bd5
source-git-commit: 687363ab664e43cc854b535760dfbfc55acefd2c
workflow-type: tm+mt
source-wordcount: '1570'
ht-degree: 2%

---

# [!DNL Snowflake] ソース

>[!IMPORTANT]
>
>* Real-Time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL Snowflake] ソースを利用できます。
>* デフォルトでは、[!DNL Snowflake] ソースは `null` を空の文字列として解釈します。 `null` 値がAdobe Experience Platformで `null` 定されたとおりに正しく記述されていることを確認するには、Adobe担当者にお問い合わせください。
>* Experience Platformでデータを取り込むには、すべてのテーブルベースのバッチソースのタイムゾーンを UTC に設定する必要があります。 [!DNL Snowflake] ソースに対してサポートされているタイムスタンプは、UTC 時間を使用した TIMESTAMP_NTZ のみです。

[!DNL Snowflake] は、組織が大量のデータを効率的に保存、処理、分析できるように設計されたクラウドベースのデータウェアハウスプラットフォームです。 クラウドのスケーラビリティと柔軟性を活用するように構築されたの [!DNL Snowflake]、データ統合、高度な分析、チーム間のシームレスな共有をサポートします。 完全マネージドサービスとして、[!DNL Snowflake] は従来のデータベースに共通するメンテナンスの複雑さを排除し、データからインサイトと価値を引き出すことに集中できます。

[!DNL Snowflake] ソースを使用して接続し、[!DNL Snowflake] からAdobe Experience Platformにデータを取り込むことができます。 [!DNL Snowflake] ソースを設定してExperience Platformに接続する方法については、以下のドキュメントをお読みください。

## 前提条件 {#prerequisites}

この節では、[!DNL Snowflake] ソースをExperience Platformに接続する前に行う必要がある設定作業の概要を説明します。

### IP アドレスの許可リスト

ソースをExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[Experience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

### 必要な資格情報の収集

[!DNL Snowflake] ソースを認証するには、次の資格情報プロパティの値を指定する必要があります。

>[!BEGINTABS]

>[!TAB  アカウントキー認証（Azure） ]

アカウントキー認証を使用して Azure 上のExperience Platformに接続するには、次の資格情報の値を [!DNL Snowflake] します。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `account` | アカウント名は、組織内のアカウントを一意に識別します。 この場合、アカウントを異なる [!DNL Snowflake] 組織で一意に識別する必要があります。 これを行うには、アカウント名の前に組織名を追加する必要があります。 例：`myorg-myaccount.snowflakecomputing.com`。 詳しくは、[&#x200B; アカウント識別子の取得  [!DNL Snowflake]  の節を参照し &#x200B;](#retrieve-your-account-identifier) ください。 詳しくは、[[!DNL Snowflake] ドキュメント](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization)を参照してください。 |
| `warehouse` | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データをExperience Platformに取り込む際は個別にアクセスする必要があります。 |
| `database` | [!DNL Snowflake] データベースには、Experience Platformに取り込むデータが含まれています。 |
| `username` | [!DNL Snowflake] アカウントのユーザー名。 |
| `password` | [!DNL Snowflake] ユーザーアカウントのパスワード。 |
| `role` | [!DNL Snowflake] セッションで使用する既定のアクセス制御ロールです。 役割は、指定したユーザーに既に割り当てられている既存の役割である必要があります。 デフォルトの役割は `PUBLIC` です。 |
| `connectionString` | [!DNL Snowflake] インスタンスへの接続に使用する接続文字列。 [!DNL Snowflake] の接続文字列のパターンは `jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}` です。 |

>[!TAB  キーペア認証（Azure） ]

キーペア認証を使用するには、まず 2048 ビット RSA キーペアを生成します。 次に、キーペア認証を使用して Azure 上のExperience Platformに接続するために、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `account` | アカウント名は、組織内のアカウントを一意に識別します。 この場合、アカウントを異なる [!DNL Snowflake] 組織で一意に識別する必要があります。 これを行うには、アカウント名の前に組織名を追加する必要があります。 例：`myorg-myaccount.snowflakecomputing.com`。 詳しくは、[&#x200B; アカウント識別子の取得  [!DNL Snowflake]  の節を参照し &#x200B;](#retrieve-your-account-identifier) ください。 詳しくは、[[!DNL Snowflake] ドキュメント](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization)を参照してください。 |
| `username` | [!DNL Snowflake] アカウントのユーザー名。 |
| `privateKey` | [!DNL Base64-] アカウントの [!DNL Snowflake] エンコードされた秘密鍵。 暗号化された秘密鍵または暗号化されていない秘密鍵のいずれかを生成できます。 暗号化された秘密鍵を使用している場合は、Experience Platformに対して認証を行う際に、秘密鍵のパスフレーズも指定する必要があります。 詳しくは、[&#x200B; 秘密鍵の取得 &#x200B;](#retrieve-your-private-key) の節を参照してください。 |
| `privateKeyPassphrase` | 秘密鍵のパスフレーズは、暗号化された秘密鍵を使用して認証を行う場合に使用する必要がある、追加のセキュリティレイヤーです。 暗号化されていない秘密鍵を使用している場合は、パスフレーズを指定する必要はありません。 |
| `port` | インターネット経由でサーバーに接続するときに [!DNL Snowflake] が使用するポート番号です。 |
| `database` | Experience Platformに取り込むデータを含んだ [!DNL Snowflake] データベース。 |
| `warehouse` | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データをExperience Platformに取り込む際は個別にアクセスする必要があります。 |

これらの値について詳しくは、[[!DNL Snowflake]  キーペア認証ガイド &#x200B;](https://docs.snowflake.com/en/user-guide/key-pair-auth.html) を参照してください。

>[!TAB  基本認証（AWS） ]

基本認証を使用してAWS上のExperience Platformに接続するには、次の資格情報の値を指定 [!DNL Snowflake] ます。

>[!WARNING]
>
>[!DNL Snowflake] ソースの基本認証（またはアカウントキー認証）は、2025 年 11 月に非推奨（廃止予定）になります。 ソースの使用とデータベースからExperience Platformへのデータの取り込みを続行するには、キーペアベースの認証に移行する必要があります。 非推奨（廃止予定）について詳しくは、[[!DNL Snowflake]  資格情報の漏洩リスクの軽減に関するベストプラクティスガイド &#x200B;](https://www.snowflake.com/en/resources/white-paper/best-practices-to-mitigate-the-risk-of-credential-compromise/) を参照してください。

| 資格情報 | 説明 |
| --- | --- |
| `host` | [!DNL Snowflake] アカウントが接続するホスト URL。 |
| `port` | インターネット経由でサーバーに接続するときに [!DNL Snowflake] が使用するポート番号です。 |
| `username` | [!DNL Snowflake] アカウントに関連付けられたユーザー名。 |
| `password` | [!DNL Snowflake] アカウントに関連付けられたパスワード。 |
| `database` | データの取得元となる [!DNL Snowflake] データベース。 |
| `schema` | [!DNL Snowflake] データベースに関連付けられたスキーマの名前。 データベースアクセス権を付与するユーザーが、このスキーマにもアクセスできることを確認する必要があります。 |
| `warehouse` | 使用している [!DNL Snowflake] ウェアハウス。 |

>[!TAB  キーペア認証（AWS） ]

キーペア認証を使用するには、まず 2048 ビット RSA キーペアを生成します。 次に、キーペア認証を使用してAWS上のExperience Platformに接続するために、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `account` | アカウント名は、組織内のアカウントを一意に識別します。 この場合、アカウントを異なる [!DNL Snowflake] 組織で一意に識別する必要があります。 これを行うには、アカウント名の前に組織名を追加する必要があります。 例：`http://myorg-myaccount.snowflakecomputing.com/`。 詳しくは、[&#x200B; アカウント識別子の取得  [!DNL Snowflake]  に関するガイドを参 &#x200B;](#etrieve-your-account-identifier) してください。 詳しくは、[[!DNL Snowflake] ドキュメント](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization)を参照してください。 |
| `username` | [!DNL Snowflake] アカウントのユーザー名。 |
| `privateKey` | [!DNL Snowflake] ユーザーの秘密鍵。base64 でエンコードされ、ヘッダーや改行のない 1 行として使用されます。 準備するには、PEM ファイルの内容をコピーし、`BEGIN`/`END` 行とすべての改行を削除してから、結果を base64 エンコードします。 詳しくは、[&#x200B; 秘密鍵の取得 &#x200B;](#retrieve-your-private-key) の節を参照してください。 **メモ：** 暗号化された秘密鍵は、現在、AWS接続ではサポートされていません。 |
| `port` | インターネット経由でサーバーに接続するときに [!DNL Snowflake] が使用するポート番号です。 |
| `database` | Experience Platformに取り込むデータを含んだ [!DNL Snowflake] データベース。 |
| `warehouse` | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データをExperience Platformに取り込む際は個別にアクセスする必要があります。 |

これらの値について詳しくは、[[!DNL Snowflake]  キーペア認証ガイド &#x200B;](https://docs.snowflake.com/en/user-guide/key-pair-auth.html) を参照してください。

>[!ENDTABS]

### アカウント識別子の取得 {#retrieve-your-account-identifier}

Experience Platformで [!DNL Snowflake] インスタンスの認証に使用するので、[!DNL Snowflake] UI ダッシュボードからアカウント ID を取得する必要があります。

アカウント識別子を取得するには：

* [[!DNL Snowflake]  アプリケーション UI ダッシュボード &#x200B;](https://app.snowflake.com/) を使用して、アカウントにアクセスします。
* 左側のナビゲーションで「**[!DNL Accounts]**」を選択し、ヘッダーから「**[!DNL Active Accounts]**」を選択します。
* 次に、情報アイコンを選択し、現在の URL のドメイン名を選択してコピーします。

![&#x200B; ドメイン名が選択されたSnowflake UI ダッシュボード。](../../images/tutorials/create/snowflake/snowflake-dashboard.png)

### RSA キーペアの生成

コマンドラインインターフェイスで OpenSSL を使用して、2048 ビットの RSA キーペアを PKCS#8 形式で生成します。 セキュリティを確保するために暗号化された秘密鍵を作成することをお勧めします。その場合、パスフレーズが必要になります。

>[!BEGINTABS]

>[!TAB  暗号化された秘密鍵の生成 ]

暗号化された [!DNL Snowflake] 秘密鍵を生成するには、ターミナルで次のコマンドを実行します。

```bash
openssl genrsa 2048 | openssl pkcs8 -topk8 -v2 des3 -inform PEM -out rsa_key.p8# You will be prompted to enter a passphrase. Store this securely!
```

>[!TAB  暗号化されていない秘密鍵の生成 ]

暗号化されていない [!DNL Snowflake] 秘密鍵を生成するには、ターミナルで次のコマンドを実行します。

```bash
openssl genrsa 2048 | openssl pkcs8 -topk8 -inform PEM -out rsa_key.p8 -nocrypt
```

>[!ENDTABS]

### 秘密鍵からの公開鍵の生成

次に、コマンドラインインターフェイスで次のコマンドを実行して、秘密鍵に基づいて公開鍵を作成します。

```bash
openssl rsa -in rsa_key.p8 -pubout -out rsa_key.pub# You will be prompted to enter the passphrase if the private key is encrypted.
```

### 公開鍵を [!DNL Snowflake] ユーザーに割り当てます

生成された公開鍵をExperience Platformが使用する [!DNL Snowflake] サービスユーザーに関連付けるには、**管理者の役割（** SECURITYADMIN[!DNL Snowflake] など）を使用する必要があります。 公開鍵の内容を取得するには、`rsa_key.pub` ファイルを開き、`-----BEGIN PUBLIC KEY----- and -----END PUBLIC KEY-----` 行を除く内容全体をコピーします。 次に、[!DNL Snowflake] で次の SQL を実行します。

```sql
ALTER USER {YOUR_SNOWFLAKE_USERNAME}>SET RSA_PUBLIC_KEY='{PUBLIC_KEY_CONTENT}';
```

### 秘密鍵を [!DNL Base64] でエンコードします

Experience Platformでは、秘密鍵を [!DNL Base64] エンコードし、接続設定時に文字列として指定する必要があります。 適切なツールまたはスクリプトを使用して、`rsa_key.p8` ファイルのコンテンツを 1 つの [!DNL Base64] 文字列にエンコードします。

>[!TIP]
>
>エンコーディングプロセスの前後に、ヘッダー/フッター行 `(-----BEGIN ENCRYPTED PRIVATE KEY----- and -----END ENCRYPTED PRIVATE KEY-----)` を含め、余分なスペースや改行がないことを確認してください。これにより、認証エラーが発生する可能性があります。

### 設定の検証

Experience Platformで [!DNL Snowflake] ソース接続を作成する前に、ユーザーの **[!DNL Default Role]** と **[!DNL Default Warehouse]** がExperience Platformで指定した値と一致していることを確認する必要があります。 これらの設定は、[!DNL Snowflake] UI で `DESCRIBE USER {USERNAME}` SQL コマンドを使用して確認できます。

または、以下の手順に従って設定を確認することもできます。

* 左側のナビゲーションで「**[!DNL Admin]**」を選択し、「**[!DNL Users & Roles]**」を選択します。
* 適切なユーザーを選択し、右上隅にある省略記号（`...`）を選択します。
* 表示される [!DNL Edit user] ウィンドウで、[!DNL Default Role] に移動し、特定のユーザーに関連付けられている役割を表示します。
* 同じウィンドウで、[!DNL Default Warehouse] に移動し、特定のユーザーに関連付けられているウェアハウスを表示します。

![&#x200B; ロールとウェアハウスを確認できるSnowflake UI。](../../images/tutorials/create/snowflake/snowflake-configs.png)

## 次の手順

設定が完了したら、[!DNL Snowflake] アカウントをExperience Platformに接続する手順に進むことができます。 詳しくは、次のドキュメントを参照してください。

### API を使用した [!DNL Snowflake] のExperience Platformへの接続

* [API [!DNL Snowflake]  使用したExperience Platformへの接続](../../tutorials/api/create/databases/snowflake.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

### UI を使用した [!DNL Snowflake] のExperience Platformへの接続

* [UI [!DNL Snowflake]  使用したExperience Platformへの接続](../../tutorials/ui/create/databases/snowflake.md)
* [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
