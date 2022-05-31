---
keywords: Experience Platform；ホーム；人気の高いトピック；Microsoft Dynamics;microsoft dynamics;dynamics;Dynamics
solution: Experience Platform
title: Microsoft Dynamics Source Connector の概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用してMicrosoft Dynamics をAdobe Experience Platformに接続する方法を説明します。
exl-id: 6ca162ce-2016-4270-b741-301cf4230233
source-git-commit: 4fbf1b9a55c755d0bac9e15efbf6bdb25fa24deb
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 56%

---

# Microsoft Dynamics コネクタ

Adobe Experience Platform では、外部ソースからデータを取り込むと同時に、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、サードパーティの CRM システムからのデータ取り込みをサポートしています。CRM プロバイダーのサポートは [!DNL Microsoft Dynamics] を含みます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## 次のフィールドマッピング [!DNL Microsoft Dynamics] XDM に

間にソース接続を確立するには [!DNL Microsoft Dynamics] また、Platform、 [!DNL Microsoft Dynamics] Platform に取り込む前に、ソースデータフィールドを適切なターゲット XDM フィールドにマッピングする必要があります。

次の間のフィールドマッピングルールの詳細については、 [!DNL Microsoft Dynamics] データセットとプラットフォーム：

- [連絡先](../adobe-applications/mapping/dynamics.md#contacts)
- [リード数](../adobe-applications/mapping/dynamics.md#leads)
- [アカウント](../adobe-applications/mapping/dynamics.md#accounts)
- [商談](../adobe-applications/mapping/dynamics.md#opportunities)
- [商談連絡先の役割](../adobe-applications/mapping/dynamics.md#opportunity-contact-roles)
- [キャンペーン](../adobe-applications/mapping/dynamics.md#campaigns)
- [マーケティングリスト](../adobe-applications/mapping/dynamics.md#marketing-list)
- [マーケティングリストメンバー](../adobe-applications/mapping/dynamics.md#marketing-list-members)

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Microsoft Dynamics] と を接続する方法について説明します。[!DNL Platform]

## API を使用して [!DNL Microsoft Dynamics] と [!DNL Platform] を接続する

- [フローサービス API を使用したMicrosoft Dynamics ベース接続の作成](../../tutorials/api/create/crm/ms-dynamics.md)
- [フローサービス API を使用したデータテーブルの調査](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用して、CRM ソースのデータフローを作成する](../../tutorials/api/collect/crm.md)

## UIを使用して [!DNL Microsoft Dynamics] と [!DNL Platform] を接続する

- [UI でのMicrosoft Dynamics ソース接続の作成](../../tutorials/ui/create/crm/dynamics.md)
- [UI での CRM 接続のデータフローの作成](../../tutorials/ui/dataflow/crm.md)
