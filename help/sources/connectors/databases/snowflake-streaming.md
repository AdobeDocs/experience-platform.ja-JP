---
title: Snowflakeストリーミングソースコネクタの概要
description: ソース接続とデータフローを作成して、SnowflakeインスタンスからAdobe Experience Platformにストリーミングデータを取り込む方法を説明します。
badgeBeta: label="ベータ版" type="Informative"
badgeUltimate: label="Ultimate" type="Positive"
last-substantial-update: 2023-05-25T00:00:00Z
exl-id: ed937689-e844-487e-85fb-e3536c851fe5
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '710'
ht-degree: 11%

---

# [!DNL Snowflake] ストリーミングソース

>[!IMPORTANT]
>
>* The [!DNL Snowflake] ストリーミングソースはベータ版です。 詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細
>* The [!DNL Snowflake] ストリーミングソースは、Real-time Customer Data Platform Ultimate を購入したユーザーが API で使用できます。

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、 [!DNL Snowflake] データベース。

## について [!DNL Snowflake] ストリーミングソース

The [!DNL Snowflake] ストリーミングソースは、SQL クエリを定期的に実行し、結果セットの各行の出力レコードを作成することで、データを読み込むことで機能します。

次を使用： [!DNL Kafka Connect]、 [!DNL Snowflake] ストリーミングソースは、各テーブルから受け取った最新のレコードをトラッキングし、次の反復の正しい場所で開始できるようにします。 ソースは、この機能を使用してデータをフィルタリングし、各反復でテーブルから更新された行のみを取得します。

## 前提条件

次の節では、データをからストリーミングする前に完了する必要がある前提条件の手順について説明します。 [!DNL Snowflake] データベースからExperience Platformへ：

### 必要な資格情報の収集

次の条件を満たすため [!DNL Flow Service] ～と繋がる [!DNL Snowflake]に値を入力する場合は、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `account` | 次に関連付けられた完全なアカウント名： [!DNL Snowflake] アカウント。 完全修飾 [!DNL Snowflake] アカウント名には、アカウント名、地域、クラウドプラットフォームが含まれます。 たとえば、`cj12345.east-us-2.azure` のように設定します。アカウント名について詳しくは、 [[!DNL Snowflake document on account identifiers]](<https://docs.snowflake.com/en/user-guide/admin-account-identifier.html>). |
| `warehouse` | The [!DNL Snowflake] warehouse は、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データを Platform に引き渡す際には、個別にアクセスする必要があります。 |
| `database` | The [!DNL Snowflake] データベースには、Platform を取り込むデータが含まれます。 |
| `username` | のユーザー名 [!DNL Snowflake] アカウント。 |
| `password` | のパスワード [!DNL Snowflake] ユーザーアカウント。 |
| `role` | （オプション）特定の接続に対してユーザーに提供できるカスタム定義の役割です。 指定しない場合、この値はデフォルトでになります。 `public`. |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。の接続仕様 ID [!DNL Snowflake] 次に該当 `51ae16c2-bdad-42fd-9fce-8d5dfddaf140`. |

認証について詳しくは、 [[!DNL Snowflake] 文書](<https://docs.snowflake.com/en/user-guide/key-pair-auth.html>).

### 役割の設定を構成する {#configure-role-settings}

デフォルトのパブリックロールが割り当てられている場合でも、ソース接続が関連する [!DNL Snowflake] データベース、スキーマ、テーブル。 様々な権限 [!DNL Snowflake] エンティティは次のようになります。

| [!DNL Snowflake] エンティティ | 役割権限が必要です |
| --- | --- |
| Warehouse | 運用、使用 |
| データベース | 使用方法 |
| スキーマ | 使用方法 |
| テーブル | 選択 |

>[!NOTE]
>
>ウェアハウスの詳細設定で、自動再開と自動休止を有効にする必要があります。

役割と権限の管理の詳細については、 [[!DNL Snowflake] API リファレンス](<https://docs.snowflake.com/en/sql-reference/sql/grant-privilege>).

## 制限事項とよくある質問 {#limitations-and-frequently-asked-questions}

* のデータスループット [!DNL Snowflake] ソースは、1 秒あたり 2,000 レコードです。
* 価格は、ウェアハウスがアクティブな時間とウェアハウスのサイズに応じて異なる場合があります。 の [!DNL Snowflake] 最小サイズの x-small ウェアハウスで十分なソース統合。 非使用時にウェアハウスを単独で休止できるように、自動休止を有効にすることをお勧めします。
* The [!DNL Snowflake] ソースは、10 秒ごとに新しいデータに対してデータベースをポーリングします。
* 設定オプション：
   * 以下を有効にすると、 `backfill` のブール型フラグ [!DNL Snowflake] ソース接続を作成する際のソース。
      * backfill が true に設定されている場合、timestamp.initial の値は 0 に設定されます。 つまり、タイムスタンプ列が 0 エポック時間を超えるデータが取得されます。
      * backfill が false に設定されている場合、timestamp.initial の値は —1 に設定されます。 つまり、タイムスタンプ列が現在の時刻（ソースの取り込みが開始される時刻）より大きいデータが取得されます。
   * timestamp 列は、次の形式にする必要があります。 `TIMESTAMP_LTZ` または `TIMESTAMP_NTZ`. timestamp 列が `TIMESTAMP_NTZ`を使用する場合、値が格納される対応するタイムゾーンを `timezoneValue` パラメーター。 指定しない場合、値はデフォルトで UTC に設定されます。
      * `TIMESTAMP_TZ` timestamp 列またはマッピングでは使用できません。

## 次の手順

次のチュートリアルでは、 [!DNL Snowflake] ストリーミングソースからExperience Platformへの変換（API を使用）:

* [からのデータのストリーミング [!DNL Snowflake] フローサービス API を使用してExperience Platformにデータベースを送信する](../../tutorials/api/create/databases/snowflake-streaming.md)
