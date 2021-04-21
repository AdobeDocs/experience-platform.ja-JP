---
keywords: Experience Platform；ホーム；人気のあるトピック；Microsoft Dynamics;microsoft dynamics;dynamics;Dynamics
solution: Experience Platform
title: Microsoft Dynamics Source Connectorの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してMicrosoft DynamicsをAdobe Experience Platformに接続する方法を説明します。
exl-id: 6ca162ce-2016-4270-b741-301cf4230233
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 8%

---

# Microsoft Dynamics コネクタ

Adobe Experience Platformは、[!DNL Platform]サービスを使用して、外部ソースからデータを取り込むと同時に、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのCRMシステムからデータを取り込むためのサポートを提供します。CRMプロバイダーのサポートには[!DNL Microsoft Dynamics]が含まれます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

>[!IMPORTANT]
>
>[!DNL Microsoft Dynamics]ソースコネクタは、現在、プラットフォームとの同じ領域接続をサポートしていません。 つまり、Azureインスタンスがプラットフォームと同じネットワーク領域を使用している場合、プラットフォームソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、Adobeのアカウントマネージャーにお問い合わせください。

次のドキュメントは、APIまたはユーザーインターフェイスを使用して[!DNL Microsoft Dynamics]を[!DNL Platform]に接続する方法に関する情報を提供しています。

## APIを使用して[!DNL Microsoft Dynamics]を[!DNL Platform]に接続

- [Flow Service APIを使用してMicrosoft Dynamicsソース接続を作成する](../../tutorials/api/create/crm/ms-dynamics.md)
- [Flow Service APIを使用したCRMシステムの調査](../../tutorials/api/explore/crm.md)
- [Flow Service APIを使用してCRMデータを収集する](../../tutorials/api/collect/crm.md)

## UIを使用して[!DNL Microsoft Dynamics]を[!DNL Platform]に接続

- [UIでMicrosoft Dynamicsソース接続を作成する](../../tutorials/ui/create/crm/dynamics.md)
- [UIでのCRM接続のデータフローの設定](../../tutorials/ui/dataflow/crm.md)
