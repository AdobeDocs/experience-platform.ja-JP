---
title: Snowflake Source コネクタの概要
description: APIまたはユーザーインターフェイスを使用してSnowflakeをAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: df066463-1ae6-4ecd-ae0e-fb291cec4bd5
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1570'
ht-degree: 3%

---

# [!DNL Snowflake] ソース

>[!IMPORTANT]
>
>* [!DNL Snowflake] ソースは、Real-Time Customer Data Platform Ultimateを購入したユーザーがソースカタログで利用できます。
>* デフォルトでは、[!DNL Snowflake] ソースは`null`を空の文字列として解釈します。 Adobe担当者に連絡して、`null`値が`null`としてAdobe Experience Platformに正しく書き込まれていることを確認してください。
>* Experience Platformでデータを取り込むには、すべてのテーブルベースのバッチソースのタイムゾーンをUTCに設定する必要があります。 [!DNL Snowflake] ソースでサポートされているタイムスタンプは、TIMESTAMP_NTZとUTC時間だけです。

[!DNL Snowflake]は、組織が大量のデータを効率的に保存、処理、分析できるように設計された、クラウドベースのデータ ウェアハウス プラットフォームです。 クラウドの拡張性と柔軟性を活用するために構築された[!DNL Snowflake]は、データ統合、高度な分析、チーム間でのシームレスな共有をサポートしています。 フルマネージドサービスとして、[!DNL Snowflake]は、従来のデータベースに共通するメンテナンスの複雑さを排除し、データからインサイトと価値を引き出すことに集中できるようになります。

[!DNL Snowflake] ソースを使用して、[!DNL Snowflake]からAdobe Experience Platformにデータを接続して取り込むことができます。 [!DNL Snowflake] ソースを設定してExperience Platformに接続する方法については、以下のドキュメントを参照してください。

## 前提条件 {#prerequisites}

このセクションでは、[!DNL Snowflake] ソースをExperience Platformに接続する前に実行する必要がある設定タスクの概要を説明します。

### IP アドレスの許可リスト

ソースをExperience Platformに接続する前に、リージョン固有のIP アドレスをードに追加する必要があります。 詳しくは、[Experience PlatformへのIP アドレスの許可リストに加える](../../ip-address-allow-list.md)に関するガイドを参照してください。

### 必要な資格情報の収集

[!DNL Snowflake] ソースを認証するには、次の資格情報プロパティの値を指定する必要があります。

>[!BEGINTABS]

>[!TAB  アカウントキー認証（Azure） ]

アカウント キー認証を使用してAzure上の[!DNL Snowflake]をExperience Platformに接続するために、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `account` | アカウント名は、組織内のアカウントを一意に識別します。 この場合、異なる[!DNL Snowflake]組織のアカウントを一意に識別する必要があります。 これを行うには、アカウント名の前に組織名を付ける必要があります。 例えば、`myorg-myaccount.snowflakecomputing.com` のようになります。追加のガイダンスについては、[ アカウント IDの取得 [!DNL Snowflake] に関する節を参照してください。](#retrieve-your-account-identifier) 詳しくは、[[!DNL Snowflake] ドキュメント](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization)を参照してください。 |
| `warehouse` | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各[!DNL Snowflake] ウェアハウスは互いに独立しており、Experience Platformにデータを取り込む際に個別にアクセスする必要があります。 |
| `database` | [!DNL Snowflake] データベースには、Experience Platformに取り込むデータが含まれています。 |
| `username` | [!DNL Snowflake] アカウントのユーザー名。 |
| `password` | [!DNL Snowflake] ユーザーアカウントのパスワード。 |
| `role` | [!DNL Snowflake] セッションで使用する既定のアクセス制御の役割。 役割は、指定されたユーザーに既に割り当てられている既存の役割である必要があります。 既定の役割は`PUBLIC`です。 |
| `connectionString` | [!DNL Snowflake] インスタンスへの接続に使用される接続文字列。 [!DNL Snowflake]の接続文字列パターンは`jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}`です。 |

>[!TAB  キーペア認証（Azure） ]

キーペア認証を使用するには、まず2048 ビット RSA キーペアを生成します。 次に、キーペア認証を使用してAzure上のExperience Platformに接続する次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `account` | アカウント名は、組織内のアカウントを一意に識別します。 この場合、異なる[!DNL Snowflake]組織のアカウントを一意に識別する必要があります。 これを行うには、アカウント名の前に組織名を付ける必要があります。 例えば、`myorg-myaccount.snowflakecomputing.com` のようになります。追加のガイダンスについては、[ アカウント IDの取得 [!DNL Snowflake] に関する節を参照してください。](#retrieve-your-account-identifier) 詳しくは、[[!DNL Snowflake] ドキュメント](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization)を参照してください。 |
| `username` | [!DNL Snowflake] アカウントのユーザー名。 |
| `privateKey` | [!DNL Base64-] アカウントの[!DNL Snowflake] エンコードされた秘密鍵。 暗号化された秘密鍵または暗号化されていない秘密鍵を生成できます。 暗号化された秘密鍵を使用している場合は、Experience Platformに対する認証時に秘密鍵パスフレーズも指定する必要があります。 詳しくは、[秘密鍵の取得](#retrieve-your-private-key)に関する節を参照してください。 |
| `privateKeyPassphrase` | 秘密鍵パスフレーズは、暗号化された秘密鍵で認証する際に使用する必要がある追加のセキュリティレイヤーです。 暗号化されていない秘密鍵を使用している場合は、パスフレーズを指定する必要はありません。 |
| `port` | インターネット経由でサーバーに接続する際に[!DNL Snowflake]が使用するポート番号。 |
| `database` | Experience Platformに取り込むデータを含む[!DNL Snowflake] データベース。 |
| `warehouse` | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各[!DNL Snowflake] ウェアハウスは互いに独立しており、Experience Platformにデータを取り込む際に個別にアクセスする必要があります。 |

これらの値について詳しくは、[[!DNL Snowflake]  キーペア認証ガイド ](https://docs.snowflake.com/en/user-guide/key-pair-auth.html)を参照してください。

>[!TAB 基本認証（AWS） ]

基本認証を使用してAWS上のExperience Platformに[!DNL Snowflake]を接続するには、次の資格情報の値を指定します。

>[!WARNING]
>
>[!DNL Snowflake] ソースの基本認証（またはアカウントキー認証）は、2025年11月に廃止されます。 ソースを引き続き使用し、データベースからExperience Platformにデータを取り込むには、キーペアベースの認証に移行する必要があります。 非推奨（廃止予定）について詳しくは、資格情報の侵害リスクの軽減に関する[[!DNL Snowflake]  ベストプラクティスガイド ](https://www.snowflake.com/en/resources/white-paper/best-practices-to-mitigate-the-risk-of-credential-compromise/)を参照してください。

| 資格情報 | 説明 |
| --- | --- |
| `host` | [!DNL Snowflake] アカウントが接続するホスト URL。 |
| `port` | インターネット経由でサーバーに接続する際に[!DNL Snowflake]が使用するポート番号。 |
| `username` | [!DNL Snowflake] アカウントに関連付けられているユーザー名。 |
| `password` | [!DNL Snowflake] アカウントに関連付けられているパスワード。 |
| `database` | データの取得元となる[!DNL Snowflake] データベース。 |
| `schema` | [!DNL Snowflake] データベースに関連付けられているスキーマの名前。 データベースにアクセス権を付与するユーザーがこのスキーマにもアクセス権を持っていることを確認する必要があります。 |
| `warehouse` | 使用している[!DNL Snowflake] ウェアハウス。 |

>[!TAB  キーペア認証（AWS） ]

キーペア認証を使用するには、まず2048 ビット RSA キーペアを生成します。 次に、キーペア認証を使用してAWS上のExperience Platformに接続する次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `account` | アカウント名は、組織内のアカウントを一意に識別します。 この場合、異なる[!DNL Snowflake]組織のアカウントを一意に識別する必要があります。 これを行うには、アカウント名の前に組織名を付ける必要があります。 例えば、`http://myorg-myaccount.snowflakecomputing.com/` のようになります。追加のガイダンスについては、[ アカウント IDの取得 [!DNL Snowflake] に関するガイドを参照してください。](#etrieve-your-account-identifier) 詳しくは、[[!DNL Snowflake] ドキュメント](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization)を参照してください。 |
| `username` | [!DNL Snowflake] アカウントのユーザー名。 |
| `privateKey` | [!DNL Snowflake] ユーザーの秘密鍵です。base64はヘッダーや改行のない1行としてエンコードされています。 準備するには、PEM ファイルの内容をコピーし、`BEGIN`/`END`行とすべての改行を削除してから、結果をbase64 エンコードします。 詳しくは、[秘密鍵の取得](#retrieve-your-private-key)に関する節を参照してください。 **メモ：**&#x200B;暗号化された秘密鍵は、現在、AWS接続ではサポートされていません。 |
| `port` | インターネット経由でサーバーに接続する際に[!DNL Snowflake]が使用するポート番号。 |
| `database` | Experience Platformに取り込むデータを含む[!DNL Snowflake] データベース。 |
| `warehouse` | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各[!DNL Snowflake] ウェアハウスは互いに独立しており、Experience Platformにデータを取り込む際に個別にアクセスする必要があります。 |

これらの値について詳しくは、[[!DNL Snowflake]  キーペア認証ガイド ](https://docs.snowflake.com/en/user-guide/key-pair-auth.html)を参照してください。

>[!ENDTABS]

### アカウント IDの取得 {#retrieve-your-account-identifier}

Experience Platformで[!DNL Snowflake] インスタンスを認証するためにこれを使用するので、[!DNL Snowflake] UI ダッシュボードからアカウント IDを取得する必要があります。

アカウント IDを取得するには：

* [[!DNL Snowflake]  アプリケーション UI ダッシュボード ](https://app.snowflake.com/)を使用して、アカウントにアクセスします。
* 左側のナビゲーションで「**[!DNL Accounts]**」を選択し、ヘッダーから「**[!DNL Active Accounts]**」を選択します。
* 次に、情報アイコンを選択し、現在のURLのドメイン名を選択してコピーします。

![ ドメイン名が選択されたSnowflake UI ダッシュボード。](../../images/tutorials/create/snowflake/snowflake-dashboard.png)

### RSA キーペアの生成

コマンドラインインターフェイスでOpenSSLを使用して、2048 ビットのRSA キーペアをPKCS#8形式で生成します。 セキュリティ用に暗号化された秘密鍵を作成することをお勧めします。これにはパスフレーズが必要です。

>[!BEGINTABS]

>[!TAB 暗号化された秘密鍵を生成]

暗号化された[!DNL Snowflake]秘密鍵を生成するには、ターミナルで次のコマンドを実行します。

```bash
openssl genrsa 2048 | openssl pkcs8 -topk8 -v2 des3 -inform PEM -out rsa_key.p8# You will be prompted to enter a passphrase. Store this securely!
```

>[!TAB 暗号化されていない秘密鍵を生成]

暗号化されていない[!DNL Snowflake]秘密鍵を生成するには、ターミナルで次のコマンドを実行します。

```bash
openssl genrsa 2048 | openssl pkcs8 -topk8 -inform PEM -out rsa_key.p8 -nocrypt
```

>[!ENDTABS]

### 秘密鍵から公開鍵を生成

次に、コマンドラインインターフェイスで次のコマンドを実行して、秘密鍵に基づく公開鍵を作成します。

```bash
openssl rsa -in rsa_key.p8 -pubout -out rsa_key.pub# You will be prompted to enter the passphrase if the private key is encrypted.
```

### 公開鍵を[!DNL Snowflake] ユーザーに割り当てます

生成された公開鍵をExperience Platformが使用する[!DNL Snowflake] サービスユーザーに関連付けるには、**管理者ロール（** SECURITYADMIN[!DNL Snowflake]など）を使用する必要があります。 公開鍵コンテンツを取得するには、`rsa_key.pub` ファイルを開き、`-----BEGIN PUBLIC KEY----- and -----END PUBLIC KEY-----`行を除くコンテンツ全体をコピーします。 次に、[!DNL Snowflake]で次のSQLを実行します。

```sql
ALTER USER {YOUR_SNOWFLAKE_USERNAME}>SET RSA_PUBLIC_KEY='{PUBLIC_KEY_CONTENT}';
```

### [!DNL Base64]で秘密鍵をエンコードします

Experience Platformでは、接続設定時に秘密鍵を[!DNL Base64]でエンコードし、文字列として提供する必要があります。 適切なツールまたはスクリプトを使用して、`rsa_key.p8` ファイルの内容を1つの[!DNL Base64]文字列にエンコードします。

>[!TIP]
>
>エンコーディングプロセスの前または後に、ヘッダー/フッター行`(-----BEGIN ENCRYPTED PRIVATE KEY----- and -----END ENCRYPTED PRIVATE KEY-----)`を含む余分なスペースや改行がないことを確認してください。これにより、認証エラーが発生する可能性があります。

### 設定の確認

Experience Platformで[!DNL Snowflake] ソース接続を作成する前に、ユーザーの&#x200B;**[!DNL Default Role]**&#x200B;と&#x200B;**[!DNL Default Warehouse]**&#x200B;がExperience Platformで提供する値と一致していることを確認する必要があります。 これらの設定は、[!DNL Snowflake] SQL コマンドを使用して`DESCRIBE USER {USERNAME}` UIで確認できます。

または、次の手順に従って設定を確認することもできます。

* 左側のナビゲーションで「**[!DNL Admin]**」を選択し、「**[!DNL Users & Roles]**」を選択します。
* 適切なユーザーを選択し、右上隅の省略記号（`...`）を選択します。
* 表示される[!DNL Edit user] ウィンドウで、[!DNL Default Role]に移動して、特定のユーザーに関連付けられている役割を表示します。
* 同じウィンドウで、[!DNL Default Warehouse]に移動して、特定のユーザーに関連付けられているウェアハウスを表示します。

![ ロールとウェアハウスを確認できるSnowflake UI。](../../images/tutorials/create/snowflake/snowflake-configs.png)

## 次の手順

設定が完了したら、次に[!DNL Snowflake] アカウントをExperience Platformに接続します。 詳しくは、次のドキュメントを参照してください。

### APIを使用して[!DNL Snowflake]をExperience Platformに接続する

* [APIを使用してExperience Platformに [!DNL Snowflake] 接続する](../../tutorials/api/create/databases/snowflake.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [Flow Service APIを使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

### UIを使用して[!DNL Snowflake]をExperience Platformに接続する

* [UIを使用してExperience Platformに [!DNL Snowflake] 接続](../../tutorials/ui/create/databases/snowflake.md)
* [UIでのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
