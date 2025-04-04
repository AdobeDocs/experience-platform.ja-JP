---
keywords: Experience Platform；ホーム；人気のトピック；Microsoft Dynamics;microsoft dynamics;dynamics;Dynamics
solution: Experience Platform
title: Microsoft Dynamics Source コネクタの概要
description: API またはユーザーインターフェイスを使用してMicrosoft DynamicsをAdobe Experience Platformに接続する方法について説明します。
exl-id: 6ca162ce-2016-4270-b741-301cf4230233
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 50%

---

# Microsoft Dynamics コネクタ

Adobe Experience Platform では、外部ソースからデータを取り込むと同時に、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、サードパーティの CRM システムからのデータ取り込みをサポートしています。 CRM プロバイダーのサポートは [!DNL Microsoft Dynamics] を含みます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

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
