---
keywords: Experience Platform；ホーム；人気のあるトピック；Microsoft Dynamics;microsoft dynamics;dynamics;Dynamics
solution: Experience Platform
title: Microsoft Dynamicsソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してMicrosoft DynamicsをAdobe Experience Platformに接続する方法を説明します。
exl-id: 6ca162ce-2016-4270-b741-301cf4230233
source-git-commit: 1f9948d6e419ee5d6a021a589378f7aa990b7291
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 7%

---

# Microsoft Dynamics コネクタ

Adobe Experience Platformを使用すると、[!DNL Platform]サービスを使用して、受信データの構造化、ラベル付け、拡張を行いながら、外部ソースからデータを取り込むことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのCRMシステムからデータを取り込むためのサポートを提供します。CRMプロバイダーのサポートには[!DNL Microsoft Dynamics]が含まれます。

## IPアドレス許可リスト

ソースコネクタを操作する前に、IPアドレスのリストを許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加しないと、ソースを使用する際にエラーやパフォーマンスが低下する可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)のページを参照してください。

>[!IMPORTANT]
>
>[!DNL Microsoft Dynamics]ソースコネクタは、現在、Platformへの同じ領域接続をサポートしていません。 つまり、AzureインスタンスがPlatformと同じネットワーク領域を使用している場合、Platformソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、担当のAdobeアカウントマネージャーにお問い合わせください。

以下のドキュメントでは、APIまたはユーザーインターフェイスを使用して[!DNL Microsoft Dynamics]を[!DNL Platform]に接続する方法について説明します。

## APIを使用して[!DNL Microsoft Dynamics]を[!DNL Platform]に接続します

- [フローサービスAPIを使用したMicrosoft Dynamicsベース接続の作成](../../tutorials/api/create/crm/ms-dynamics.md)
- [フローサービスAPIを使用したCRMソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/crm.md)
- [フローサービスAPIを使用したCRMソースのデータフローの作成](../../tutorials/api/collect/crm.md)

## UIを使用して[!DNL Microsoft Dynamics]を[!DNL Platform]に接続します

- [UIでのMicrosoft Dynamicsソース接続の作成](../../tutorials/ui/create/crm/dynamics.md)
- [UIでのCRM接続のデータフローの作成](../../tutorials/ui/dataflow/crm.md)
