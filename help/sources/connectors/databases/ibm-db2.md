---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: IBM DB2コネクタ
topic: overview
translation-type: tm+mt
source-git-commit: 690ddbd92f0a2e4e06b988e761dabff399cd2367
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 0%

---


# （ベータ版）IBM DB2 Connector

>[!NOTE]
>
>IBM DB2 Connectorはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformは、 [!DNL Microsoft]、MySQL、などのデータベースプロバイダに対してネイティブの接続を提供 [!DNL Azure]します。 これらのシステムのデータをに取り込むことができ [!DNL Platform]ます。

リレーショナル、NoSQL、データ・ウェアハウスなど、サード・パーティのデータベースは、サポートされるタイプが異なります。 データベースプロバイダーのサポートには、IBM DB2が含まれます。

## IPアドレス許可リスト

ソースコネクタを使用する前に、許可リストに次のIPアドレスを追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。

### 米国東部地域

- `20.41.2.0/23`
- `20.41.4.0/26`
- `20.44.17.80/28`
- `20.49.102.16/29`
- `40.70.148.160/28`
- `52.167.107.224/28`

### 西ヨーロッパ地域

- `13.69.67.192/28`
- `13.69.107.112/28`
- `13.69.112.128/28`
- `40.74.24.192/26`
- `40.74.26.0/23`
- `40.113.176.232/29`
- `52.236.187.112/28`

### オーストラリア東部

- `13.70.74.144/28`
- `20.37.193.0/25`
- `20.37.193.128/26`
- `20.37.198.224/29`
- `40.79.163.80/28`
- `40.79.171.160/28`

次のドキュメントは、APIまたはユーザーインターフェイスを [!DNL Platform] 使用してIBM DB2を接続する方法に関する情報を提供しています。

## APIを [!DNL Platform] 使用したIBM DB2との接続

- [Flow Service APIを使用してIBM DB2コネクターを作成する](../../tutorials/api/create/databases/ibm-db2.md)
- [Flow Service APIを使用したデータベースシステムの調査](../../tutorials/api/explore/database-nosql.md)
- [Flow Service APIを使用してデータベースからデータを収集する](../../tutorials/api/collect/database-nosql.md)

## UIを使用したIBM DB2 [!DNL Platform] への接続

- [UIでのIBM DB2ソースコネクターの作成](../../tutorials/ui/create/databases/ibm-db2.md)
- [UIでのデータベースコネクタのデータフローの設定](../../tutorials/ui/dataflow/databases.md)