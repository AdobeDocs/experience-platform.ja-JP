---
title: Salesforce Service Cloud Source コネクタの概要
description: API またはユーザーインターフェイスを使用してSalesforce Service Cloud をAdobe Experience Platformに接続する方法について説明します。
exl-id: 9bebbc00-55b3-4aec-9357-4127c05844e2
source-git-commit: 06b2108715ce368ff4ecf5c6c7dd3a327d9f61b1
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 35%

---

# [!DNL Salesforce Service Cloud]

>[!WARNING]
>
>[!DNL Salesforce Service Cloud] ソースの基本認証は、2026 年 1 月に廃止されます。 ソースの使用と [!DNL Salesforce Service Cloud] アカウントからExperience Platformへのデータの取り込みを続行するには、OAuth 2 クライアント資格情報認証に移行する必要があります。

Adobe Experience Platform では、外部ソースからデータを取り込むと同時に、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、サードパーティのカスタマーサクセスシステムからデータを取り込む機能を提供しています。 カスタマーサクセスプロバイダーのサポートには、[!DNL Salesforce Service Cloud] が含まれます。

## IP アドレスの許可リスト

ソースをExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[Experience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Salesforce Service Cloud] を [!DNL Experience Platform] に接続する方法について説明しています。

## API を使用した [!DNL Salesforce Service Cloud] と [!DNL Experience Platform] の接続

- [Flow Service API を使用したSalesforce Service Cloud ベース接続の作成](../../tutorials/api/create/customer-success/salesforce-service-cloud.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したカスタマーサクセスソースのデータフローの作成](../../tutorials/api/collect/customer-success.md)

## UIを使用して [!DNL Salesforce Service Cloud] と [!DNL Experience Platform] を接続する

- [UI でのSalesforce Service Cloud ソース接続の作成](../../tutorials/ui/create/customer-success/salesforce-service-cloud.md)
- [UI でのカスタマーサクセスソース接続のデータフローの作成](../../tutorials/ui/dataflow/customer-success.md)
