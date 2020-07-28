---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Salesforceコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: 25f4589ff1f4fa11f3cd5b96c11731577949b5b0
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 10%

---


# [!DNL Salesforce] コネクタ

Adobe Experience Platform allows data to be ingested from external sources while providing you with the ability to structure, label, and enhance incoming data using [!DNL Platform] services. アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのCRMシステムからデータを取り込むためのサポートを提供します。 CRMプロバイダのサポートには以下が含まれ [!DNL Salesforce]ます。

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

次のドキュメントは、APIまたはユーザーインターフェイス [!DNL Salesforce] を [!DNL Platform] 使用した接続方法に関する情報を提供しています。

## API [!DNL Salesforce] を [!DNL Platform] 使用した接続

- [フローサービスAPIを使用したSalesforceコネクタの作成](../../tutorials/api/create/crm/salesforce.md)
- [Flow Service APIを使用したCRMシステムの調査](../../tutorials/api/explore/crm.md)
- [Flow Service APIを使用してCRMデータを収集する](../../tutorials/api/collect/crm.md)

## UI [!DNL Salesforce] を [!DNL Platform] 使用して接続

- [UIでのSalesforceソースコネクタの作成](../../tutorials/ui/create/crm/salesforce.md)
- [UIでのCRMコネクタのデータフローの設定](../../tutorials/ui/dataflow/crm.md)