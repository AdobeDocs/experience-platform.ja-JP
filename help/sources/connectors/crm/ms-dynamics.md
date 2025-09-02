---
keywords: Experience Platform；ホーム；人気のトピック；Microsoft Dynamics;microsoft dynamics;dynamics;Dynamics
solution: Experience Platform
title: Microsoft Dynamics Source コネクタの概要
description: API またはユーザーインターフェイスを使用してMicrosoft DynamicsをAdobe Experience Platformに接続する方法について説明します。
exl-id: 6ca162ce-2016-4270-b741-301cf4230233
source-git-commit: 16fe5340582dcea0ff40000fb516c1b72d5f150e
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 14%

---

# [!DNL Microsoft Dynamics] ソース

[!DNL Microsoft Dynamics] は、業務をより効果的に管理するために使用できるビジネスアプリケーションスイートです。 顧客との関係、財務、サプライチェーンのいずれを監視してい [!DNL Microsoft Dynamics] 場合でも、ワークフローを合理化し、よりスマートな意思決定を行うためのツールを提供します。 プラットフォームは、エンタープライズリソースプランニングと顧客関係管理（CRM）の両方をサポートするように構築されており、ビジネスプロセスを 1 つの統合システムに統合できます。

[!DNL Microsoft Dynamics] ソースを使用すると、[!DNL Microsoft Dynamics] アカウントからAdobe Experience Platformにデータを取り込むことができます。

## IP アドレスの許可リスト

ソースをExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[Experience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

## [!DNL Microsoft Dynamics] から XDM へのフィールドマッピング

[!DNL Microsoft Dynamics] とExperience Platformの間にソース接続を確立するには、Experience Platformに取り込まれる前に、[!DNL Microsoft Dynamics] ソースデータフィールドを適切なターゲット XDM フィールドにマッピングする必要があります。

データセットとExperience Platform間のフィールドマッピングルールについて詳 [!DNL Microsoft Dynamics] くは、次を参照してください。

- [連絡先](../adobe-applications/mapping/dynamics.md#contacts)
- [リード数](../adobe-applications/mapping/dynamics.md#leads)
- [アカウント](../adobe-applications/mapping/dynamics.md#accounts)
- [商談](../adobe-applications/mapping/dynamics.md#opportunities)
- [商談連絡先の役割](../adobe-applications/mapping/dynamics.md#opportunity-contact-roles)
- [キャンペーン](../adobe-applications/mapping/dynamics.md#campaigns)
- [マーケティング担当者](../adobe-applications/mapping/dynamics.md#marketing-list)
- [マーケティングリストのメンバー](../adobe-applications/mapping/dynamics.md#marketing-list-members)

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Microsoft Dynamics] を [!DNL Experience Platform] に接続する方法について説明しています。

## API を使用して [!DNL Microsoft Dynamics] と [!DNL Experience Platform] を接続する

- [Flow Service API を使用したMicrosoft Dynamics ベース接続の作成](../../tutorials/api/create/crm/ms-dynamics.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用して、CRM ソースのデータフローを作成する](../../tutorials/api/collect/crm.md)

## UIを使用して [!DNL Microsoft Dynamics] と [!DNL Experience Platform] を接続する

- [UI でのMicrosoft Dynamics ソースコネクタの作成](../../tutorials/ui/create/crm/dynamics.md)
- [UI での CRM 接続のデータフローの作成](../../tutorials/ui/dataflow/crm.md)
