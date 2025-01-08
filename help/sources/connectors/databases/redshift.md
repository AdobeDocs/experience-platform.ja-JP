---
title: Amazon Redshift Source コネクタの概要
description: API またはユーザーインターフェイスを使用してAmazon Redshift をAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 75e577dd-a0b0-4f82-a371-5ec9255544f8
source-git-commit: dbeeab9182ae67e5c9c691707faeddf04f4e94b2
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 41%

---

# [!DNL Amazon Redshift] ソース

>[!IMPORTANT]
>
>Real-time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL Amazon Redshift] ソースを利用できます。

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platform は、サードパーティのデータベースからデータを取得する機能を備えています。Platform は、リレーショナル、NoSQL、データウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダーのサポートには、[!DNL Amazon Redshift] が含まれます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## Amazon Web Services上でのExperience Platform用の [!DNL Amazon Redshift] ソースの設定 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWSで実行されるExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platformインフラストラクチャについて詳しくは、[Experience Platformマルチクラウドの概要 ](../../../landing/multi-cloud.md) を参照してください。

[!DNL Amazon Redshift] アカウントをAmazon Web Services（AWS）のExperience Platformに接続するには、次の IP アドレスを許可リストに追加します。

- `34.193.63.59`
- `44.217.93.240`
- `44.194.79.229`

## API を使用して [!DNL Amazon Redshift] と Platform を接続する

- [Flow Service API を使用したAmazon Redshift ベース接続の作成](../../tutorials/api/create/databases/redshift.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用した [!DNL Amazon Redshift] の Platform への接続

- [UI でのAmazon Redshift ソースコネクタの作成](../../tutorials/ui/create/databases/redshift.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
