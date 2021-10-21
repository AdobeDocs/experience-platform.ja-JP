---
keywords: エクスペリエンス Platform、home、人気のある話題。Veeva CRM、veeva
solution: Experience Platform
title: UI で Veeva CRM ソース接続を作成します。
topic-legacy: overview
type: Tutorial
description: Adobe エクスペリエンスプラットフォーム UI を使用して、Veeva CRM ソース接続を作成する方法を説明しています。
exl-id: 4ef76c28-9bd2-4e54-a3d6-dceb89162337
source-git-commit: 8b4e3b9e95dd4c2ff8f3b5a1399eb7d114024bb6
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 13%

---

# [!DNL Veeva CRM]UI でのソース接続の作成

Adobe エクスペリエンスプラットフォームのソースコネクターには、指定された時間に外部の CRM データを取り込む機能が用意されています。 このチュートリアルでは [!DNL Veeva CRM] 、ユーザーインターフェイスを使用してソースコネクタを作成する方法について説明し [!DNL Platform] ます。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] System ](../../../../../xdm/home.md) : [!DNL Experience Platform] カスタマーエクスペリエンスデータを整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターチュートリアル ](../../../../../xdm/tutorials/create-schema-ui.md) : スキーマエディター UI を使用してカスタムスキーマを作成する方法について説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効なアカウントを持っている場合は、 [!DNL Veeva CRM] このドキュメントの残りの部分をスキップして、フローの設定に関するチュートリアルに進んで [ ](../../dataflow/crm.md) ください。

### 必要な資格情報の収集

| Chap | 説明 |
| ---------- | ----------- |
| `environmentUrl` | ソースインスタンスの URL を指定 [!DNL Veeva CRM] します。 |
| `username` | ユーザーアカウントのユーザー名 [!DNL Veeva CRM] です。 |
| `password` | ユーザーアカウントのパスワードを入力し [!DNL Veeva CRM] ます。 |
| `securityToken` | ユーザーアカウントのセキュリティトークン [!DNL Veeva CRM] 。 |

概要について詳しくは、このドキュメントを参照してください [[!DNL Veeva CRM]  ](https://developer.veevacrm.com/api/#order-management-rest-api) 。

## アカウントの接続 [!DNL Veeva CRM]

必要な情報を収集したら、次の手順に従って [!DNL Veeva CRM] アカウントをにリンク [!DNL Platform] します。

プラットフォーム UI で、左側のナビゲーションバーの「ソース」を選択して、 **** [!UICONTROL  ソースワークスペースにアクセスし ] ます。 [!UICONTROL カタログ画面にはさまざまなソースが ] 表示されます。これは、でアカウントを作成することができます。

画面の左側のカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的のソースを検索することもできます。

「 [!UICONTROL  CRM ] 」カテゴリーで「VEEVA CRM」を選択し、 **** 「データを追加」を選択し **** ます。

![差し込み](../../../../images/tutorials/create/veeva/catalog.png)

「 **[!UICONTROL 接続 Veeva CRM アカウント」]** ページが表示されます。 このページでは、新しい資格情報を使用するか、既存の資格情報を使用することができます。

### 既存のアカウント

既存のアカウントを使用するには、新しいフローを作成するアカウントを選択し、「 [!DNL Veeva CRM] 次へ」を選択して先 **** に進みます。

![従来](../../../../images/tutorials/create/veeva/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「 **[!UICONTROL 新規アカウント」を選択]** し、名前、オプションの説明、および資格情報を入力し [!DNL Veeva CRM] ます。 終了したら、「ソースに接続」を選択し、 **** 新しい接続が確立されるまでしばらく待ちます。

![新規](../../../../images/tutorials/create/veeva/new.png)

## 次の手順

このチュートリアルでは、お使いのアカウントへの接続が確立されてい [!DNL Veeva CRM] ます。 次のチュートリアルに進み、 [ データをプラットフォームに取り込むようにデータフローを設定することができ ](../../dataflow/crm.md) ます。
