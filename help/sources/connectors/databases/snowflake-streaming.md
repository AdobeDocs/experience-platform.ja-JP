---
title: Snowflake Streaming Source コネクタの概要
description: ソース接続とデータフローを作成して、Snowflake インスタンスからAdobe Experience Platformにストリーミングデータを取り込む方法を説明します
badgeUltimate: label="Ultimate" type="Positive"
exl-id: ed937689-e844-487e-85fb-e3536c851fe5
source-git-commit: 0d646136da2c508fe7ce99a15787ee15c5921a6c
workflow-type: tm+mt
source-wordcount: '1390'
ht-degree: 6%

---

# [!DNL Snowflake] ストリーミングソース

>[!IMPORTANT]
>
>* Real-Time CDP Ultimateを購入したユーザーは、API で [!DNL Snowflake] ストリーミングソースを利用できます。
>
>* Amazon Web Services（AWS）でAdobe Experience Platformを実行するときに、[!DNL Snowflake] ストリーミングソースを使用できるようになりました。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../landing/multi-cloud.md) を参照してください。

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、[!DNL Snowflake] データベースからのストリーミングデータをサポートしています。

## [!DNL Snowflake] ストリーミングソースについて

[!DNL Snowflake] ストリーミングソースは、定期的に SQL クエリを実行し、結果セットの各レコードに出力レコードを作成することで、データを読み込むことで機能します。

[!DNL Kafka Connect] を使用すると、[!DNL Snowflake] ストリーミングソースは各テーブルから受け取った最新のレコードを追跡するので、次のイテレーション用の正しい場所で開始できます。 ソースはこの機能を使用してデータをフィルタリングし、各反復でテーブルから更新された行のみを取得します。

## 前提条件

次の節では、[!DNL Snowflake] データベースからExperience Platformにデータをストリーミングする前に実行する必要がある前提条件の手順について説明します。

### IP アドレスの許可リスト

ソースをExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[Experience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Amazon Redshift] をExperience Platformに接続する方法について説明しています。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Snowflake] に接続するには、次の接続プロパティを指定する必要があります。

>[!BEGINTABS]

>[!TAB  基本認証 ]

| 資格情報 | 説明 |
| --- | --- |
| `account` | サフィックス [!DNL Snowflake] が付いた `snowflakecomputing.com` アカウントの完全なアカウント識別子（アカウント名またはアカウントロケーター）。 アカウント識別子は、次のように異なる形式にすることができます。 <ul><li>{ORG_NAME}-{ACCOUNT_NAME}.snowflakecomputing.com （例：`acme-abc12345.snowflakecomputing.com`）</li><li>{ACCOUNT_LOCATOR}。{CLOUD_REGION_ID}.snowflakecomputing.com （例：`acme12345.ap-southeast-1.snowflakecomputing.com`）</li><li>{ACCOUNT_LOCATOR}。{CLOUD_REGION_ID}。{CLOUD}.snowflakecomputing.com （例：`acme12345.east-us-2.azure.snowflakecomputing.com`）</li></ul> 詳しくは、[[!DNL Snowflake document on account identifiers]](<https://docs.snowflake.com/en/user-guide/admin-account-identifier.html>) を参照してください。 |
| `warehouse` | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データをExperience Platformに取り込む際は個別にアクセスする必要があります。 |
| `database` | [!DNL Snowflake] データベースには、Experience Platformに取り込むデータが含まれています。 |
| `username` | [!DNL Snowflake] アカウントのユーザー名。 |
| `password` | [!DNL Snowflake] ユーザーアカウントのパスワード。 |
| `role` | （オプション）特定の接続に対してユーザーに提供できる、カスタム定義の役割。 指定しない場合、この値はデフォルトで `public` になります。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Snowflake] の接続仕様 ID は `51ae16c2-bdad-42fd-9fce-8d5dfddaf140` です。 |

>[!TAB  キーペア認証 ]

キーペア認証を使用するには、[!DNL Snowflake] ソースのアカウントを作成する際に、2048 ビットの RSA キーペアを生成してから、次の値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `account` | アカウント名は、組織内のアカウントを一意に識別します。 この場合、アカウントを異なる [!DNL Snowflake] 組織で一意に識別する必要があります。 これを行うには、アカウント名の前に組織名を追加する必要があります。 例：`orgname-account_name`。 詳しくは、[ アカウント識別子の取得  [!DNL Snowflake]  に関するガイドを参 ](./snowflake.md#retrieve-your-account-identifier) してください。 詳しくは、[[!DNL Snowflake] ドキュメント](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization)を参照してください。 |
| `username` | [!DNL Snowflake] アカウントのユーザー名。 |
| `privateKey` | [!DNL Base64-] アカウントの [!DNL Snowflake] エンコードされた秘密鍵。 暗号化された秘密鍵または暗号化されていない秘密鍵のいずれかを生成できます。 暗号化された秘密鍵を使用している場合は、Experience Platformに対して認証を行う際に、秘密鍵のパスフレーズも指定する必要があります。 詳しくは、[ 秘密鍵の取得  [!DNL Snowflake]  に関す ](./snowflake.md) ガイドを参照してください。 |
| `passphrase` | パスフレーズは、暗号化された秘密鍵を使用して認証を行う場合に使用する必要がある、追加のセキュリティレイヤーです。 暗号化されていない秘密鍵を使用している場合は、パスフレーズを指定する必要はありません。 |
| `database` | Experience Platformに取り込むデータを含んだ [!DNL Snowflake] データベース。 |
| `warehouse` | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データをExperience Platformに取り込む際は個別にアクセスする必要があります。 |

これらの値について詳しくは、[[!DNL Snowflake]  キーペア認証ガイド ](https://docs.snowflake.com/en/user-guide/key-pair-auth.html) を参照してください。

>[!ENDTABS]

### アカウント識別子の取得 {#retrieve-your-account-identifier}

Experience Platformで [!DNL Snowflake] インスタンスを認証するには、[!DNL Snowflake] UI ダッシュボードからアカウント ID を取得する必要があります。

アカウント識別子を見つけるには、次の手順に従います。

* [[!DNL Snowflake]  アプリケーション UI ダッシュボード ](https://app.snowflake.com/) でアカウントに移動します。
* 左側のナビゲーションで「**[!DNL Accounts]**」を選択し、続いてヘッダーから「**[!DNL Active Accounts]**」を選択します。
* 次に、情報アイコンを選択し、現在の URL のドメイン名を選択してコピーします。

### 秘密鍵の取得 {#retrieve-your-private-key}

[!DNL Snowflake] 接続でキーペア認証を使用する場合は、Experience Platformに接続する前に秘密鍵を生成する必要があります。

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

秘密鍵を生成したら、形式やコンテンツを変更せずに、[!DNL Base64] で直接エンコードします。 エンコードする前に、秘密鍵の末尾に余分なスペースや空白行（末尾の改行を含む）がないことを確認します。

### 設定の検証

[!DNL Snowflake] データのソース接続を作成する前に、次の設定も満たしていることを確認する必要があります。

* 特定のユーザーに割り当てられるデフォルトのウェアハウスは、Experience Platformへの認証時に入力するウェアハウスと同じである必要があります。
* 特定のユーザーに割り当てられたデフォルトのロールは、Experience Platformへの認証時に入力したのと同じデータベースにアクセスできる必要があります。

ロールとウェアハウスを検証するには：

* 左側のナビゲーションで「**[!DNL Admin]**」を選択し、「**[!DNL Users & Roles]**」を選択します。
* 適切なユーザーを選択し、右上隅にある省略記号（`...`）を選択します。
* 表示される [!DNL Edit user] ウィンドウで、[!DNL Default Role] に移動し、特定のユーザーに関連付けられている役割を表示します。
* 同じウィンドウで、[!DNL Default Warehouse] に移動し、特定のユーザーに関連付けられているウェアハウスを表示します。

正常にエンコードされると、[!DNL Base64] でエンコードされた秘密鍵をExperience Platformで使用して [!DNL Snowflake] アカウントを認証できます。

### 役割の設定 {#configure-role-settings}

デフォルトのパブリックロールが割り当てられている場合でも、ソース接続が関連する [!DNL Snowflake] データベース、スキーマおよびテーブルにアクセスできるようにするには、ロールへの権限を設定する必要があります。 様々な [!DNL Snowflake] エンティティに対する様々な権限は次のとおりです。

| [!DNL Snowflake] エンティティ | 役割権限を必要とする |
| --- | --- |
| ウェアハウス | 操作、使用 |
| データベース | 使用方法 |
| スキーマ | 使用方法 |
| テーブル | を選択 |

>[!NOTE]
>
>ウェアハウスの詳細設定コンフィギュレーションで、自動レジュームと自動休止を有効にする必要があります。

役割および権限の管理について詳しくは、[[!DNL Snowflake] API リファレンス ](<https://docs.snowflake.com/en/sql-reference/sql/grant-privilege>) を参照してください。

## 制限事項とよくある質問 {#limitations-and-frequently-asked-questions}

* [!DNL Snowflake] ソースのデータスループットは、1 秒あたり 2000 レコードです。
* 価格は、倉庫がアクティブな時間と倉庫のサイズによって異なります。 [!DNL Snowflake] ソース統合の場合、最小サイズの x-small ウェアハウスで十分です。 自動休止を有効にして、使用されていないときにウェアハウスが独自に休止できるようにすることをお勧めします。
* [!DNL Snowflake] ソースは、10 秒ごとに新しいデータをデータベースにポーリングします。
* 設定オプション：
   * ソース接続を作成する際に、`backfill` ソースに対して [!DNL Snowflake] ブール値フラグを有効にできます。
      * バックフィルが true に設定されている場合、timestamp.initial の値は 0 に設定されます。 つまり、タイムスタンプ列が 0 エポック時間より大きいデータが取得されます。
      * バックフィルが false に設定されている場合、timestamp.initial の値は–1 に設定されます。 これは、現在の時刻（ソースの取り込みを開始する時刻）より大きいタイムスタンプ列を持つデータが取得されることを意味します。
   * タイムスタンプ列は、`TIMESTAMP_LTZ` または `TIMESTAMP_NTZ` 型の形式にする必要があります。 タイムスタンプ列が `TIMESTAMP_NTZ` に設定されている場合、値が保存される対応するタイムゾーンは `timezoneValue` パラメーターを介して渡す必要があります。 指定しない場合、値はデフォルトで UTC になります。
      * タイムスタンプ列またはマッピングでは `TIMESTAMP_TZ` を使用できません。

## 次の手順

>[!NOTE]
>
>ストリーミングデータフローを作成または更新した後、データ損失やデータ削除が発生する可能性を防ぐために、データ取り込みを 5 分間ほど一時停止する必要があります。

次のチュートリアルでは、API を使用して [!DNL Snowflake] ストリーミングソースをExperience Platformに接続する方法の手順を説明します。

* [Flow Service API を使用した  [!DNL Snowflake]  データベースからExperience Platformへのデータのストリーミング](../../tutorials/api/create/databases/snowflake-streaming.md)
* [Experience Platform ユーザーインターフェイスのソ  [!DNL Snowflake]  スワークスペースを使用して、データベースからExperience Platformにデータをストリーミングします](../../tutorials/ui/create/databases/snowflake-streaming.md)
