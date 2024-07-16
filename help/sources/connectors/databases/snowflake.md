---
title: SnowflakeSource コネクタの概要
description: API またはユーザーインターフェイスを使用してSnowflakeをAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: df066463-1ae6-4ecd-ae0e-fb291cec4bd5
source-git-commit: 8b0f6eca87deedd8090830e3375d5099bfb0dfc0
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 48%

---

# [!DNL Snowflake] ソース

>[!IMPORTANT]
>
>* Real-time Customer Data Platform Ultimate を購入したユーザーは、ソースカタログで [!DNL Snowflake] ソースを利用できます。
>* デフォルトでは、[!DNL Snowflake] ソースは `null` を空の文字列として解釈します。 Adobe担当者に問い合わせて、Adobe Experience Platformで `null` 定されているように `null` 値が正しく記述されていることを確認します。
>* Experience Platformでデータを取り込むには、すべてのテーブルベースのバッチソースのタイムゾーンを UTC に設定する必要があります。 [!DNL Snowflake] ソースに対してサポートされているタイムスタンプは、UTC 時間を使用した TIMESTAMP_NTZ のみです。

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platform は、サードパーティのデータベースからデータを取得する機能を備えています。Platform は、リレーショナル、NoSQL、データウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダーのサポートには、[!DNL Snowflake] が含まれます。

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
