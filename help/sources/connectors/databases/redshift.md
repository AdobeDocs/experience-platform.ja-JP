---
title: Amazon Redshift Source コネクタの概要
description: API またはユーザーインターフェイスを使用してAmazon Redshift をAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 75e577dd-a0b0-4f82-a371-5ec9255544f8
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 22%

---

# [!DNL Amazon Redshift] ソース

>[!IMPORTANT]
>
>- Real-Time CDP Ultimateを購入したユーザーは、ソースカタログで [!DNL Amazon Redshift] ソースを利用できます。
>
>- Amazon Web Services（AWS）でAdobe Experience Platformを実行するときに、[!DNL Amazon Redshift] ソースを使用できるようになりました。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../landing/multi-cloud.md) を参照してください。


Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platform は、サードパーティのデータベースからデータを取得する機能を備えています。Experience Platformは、リレーショナル、NoSQL、データウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダーのサポートには、[!DNL Amazon Redshift] が含まれます。

## Azure 上のExperience Platformの [!DNL Amazon Redshift] ソースを設定する {#azure}

Azure でExperience Platform用に [!DNL Amazon Redshift] アカウントを設定する方法については、次の手順に従います。

### IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## Amazon Web Services上のExperience Platform用の [!DNL Amazon Redshift] ソースの設定 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../landing/multi-cloud.md) を参照してください。

### 許可リストに加える AWSでの接続用 IP アドレス。

ソースをAWSのExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[AWSでExperience Platformに接続するための IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

## API を使用した [!DNL Amazon Redshift] のExperience Platformへの接続

- [Flow Service API を使用したAmazon Redshift のExperience Platformへの接続](../../tutorials/api/create/databases/redshift.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用した [!DNL Amazon Redshift] のExperience Platformへの接続

- [UI でのAmazon Redshift ソースコネクタの作成](../../tutorials/ui/create/databases/redshift.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
