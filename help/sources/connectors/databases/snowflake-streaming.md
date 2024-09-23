---
title: SnowflakeストリーミングSourceコネクタの概要
description: ソース接続とデータフローを作成して、SnowflakeインスタンスからAdobe Experience Platformにストリーミングデータを取り込む方法を説明します
badgeUltimate: label="Ultimate" type="Positive"
last-substantial-update: 2023-09-24T00:00:00Z
exl-id: ed937689-e844-487e-85fb-e3536c851fe5
source-git-commit: 34b1676ebb5405d73cf37cd786d1e6c26cb8fdaa
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 18%

---

# [!DNL Snowflake] ストリーミングソース

>[!IMPORTANT]
>
> Real-time Customer Data Platform Ultimate を購入したユーザーは、API で [!DNL Snowflake] ストリーミングソースを利用できます。

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、[!DNL Snowflake] データベースからのストリーミングデータのサポートを提供します。

## [!DNL Snowflake] ストリーミングソースについて

[!DNL Snowflake] ストリーミングソースは、定期的に SQL クエリを実行し、結果セットの各レコードに出力レコードを作成することで、データを読み込むことで機能します。

[!DNL Kafka Connect] を使用すると、[!DNL Snowflake] ストリーミングソースは各テーブルから受け取った最新のレコードを追跡するので、次のイテレーション用の正しい場所で開始できます。 ソースはこの機能を使用してデータをフィルタリングし、各反復でテーブルから更新された行のみを取得します。

## 前提条件

次の節では、[!DNL Snowflake] データベースからExperience Platformにデータをストリーミングする前に実行する必要がある前提条件の手順について説明します。

### IP アドレス許可リストの更新

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md#ip-address-allow-list-for-streaming-sources)ページを参照してください。

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Amazon Redshift] と Platform を接続する方法について説明します。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Snowflake] に接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `account` | サフィックス `snowflakecomputing.com` が付いた [!DNL Snowflake] アカウントの完全なアカウント識別子（アカウント名またはアカウントロケーター）。 アカウント識別子は、次のように異なる形式にすることができます。 <ul><li>{ORG_NAME}-{ACCOUNT_NAME}.snowflakecomputing.com （例：`acme-abc12345.snowflakecomputing.com`）</li><li>{ACCOUNT_LOCATOR}。{CLOUD_REGION_ID}.snowflakecomputing.com （例：`acme12345.ap-southeast-1.snowflakecomputing.com`）</li><li>{ACCOUNT_LOCATOR}。{CLOUD_REGION_ID}。{CLOUD}.snowflakecomputing.com （例：`acme12345.east-us-2.azure.snowflakecomputing.com`）</li></ul> 詳しくは、[[!DNL Snowflake document on account identifiers]](<https://docs.snowflake.com/en/user-guide/admin-account-identifier.html>) を参照してください。 |
| `warehouse` | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データを Platform に取り込む際は個別にアクセスする必要があります。 |
| `database` | [!DNL Snowflake] データベースには、Platform に取り込むデータが含まれています。 |
| `username` | [!DNL Snowflake] アカウントのユーザー名。 |
| `password` | [!DNL Snowflake] ユーザーアカウントのパスワード。 |
| `role` | （オプション）特定の接続に対してユーザーに提供できる、カスタム定義の役割。 指定しない場合、この値はデフォルトで `public` になります。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Snowflake] の接続仕様 ID は `51ae16c2-bdad-42fd-9fce-8d5dfddaf140` です。 |

{style="table-layout:auto"}

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
   * ソース接続を作成する際に、[!DNL Snowflake] ソースに対して `backfill` ブール値フラグを有効にできます。
      * バックフィルが true に設定されている場合、timestamp.initial の値は 0 に設定されます。 つまり、タイムスタンプ列が 0 エポック時間より大きいデータが取得されます。
      * バックフィルが false に設定されている場合、timestamp.initial の値は–1 に設定されます。 これは、現在の時刻（ソースの取り込みを開始する時刻）より大きいタイムスタンプ列を持つデータが取得されることを意味します。
   * タイムスタンプ列は、`TIMESTAMP_LTZ` または `TIMESTAMP_NTZ` 型の形式にする必要があります。 タイムスタンプ列が `TIMESTAMP_NTZ` に設定されている場合、値が保存される対応するタイムゾーンは `timezoneValue` パラメーターを介して渡す必要があります。 指定しない場合、値はデフォルトで UTC になります。
      * タイムスタンプ列またはマッピングでは `TIMESTAMP_TZ` を使用できません。

## 次の手順

次のチュートリアルでは、API を使用して [!DNL Snowflake] ストリーミングソースをExperience Platformに接続する方法の手順を説明します。

* [Flow Service API を使用した  [!DNL Snowflake]  データベースからExperience Platformへのデータのストリーミング](../../tutorials/api/create/databases/snowflake-streaming.md)
* [Experience Platformユーザーインターフェイス  [!DNL Snowflake]  ソースワークスペースを使用して、データベースからExperience Platformにデータをストリーミングします](../../tutorials/ui/create/databases/snowflake-streaming.md)
