---
keywords: Experience Platform；ホーム；人気のあるトピック；Veeva CRM;veeva
solution: Experience Platform
title: UI での Veeva CRM ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して Veeva CRM ソース接続を作成する方法を説明します。
exl-id: 4ef76c28-9bd2-4e54-a3d6-dceb89162337
source-git-commit: 89a0e2544a17fe10e6dfd7611b5223ca4fc55501
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 13%

---

# UI での [!DNL Veeva CRM] ソース接続の作成

Adobe Experience Platformのソースコネクタは、外部ソースの CRM データをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Veeva CRM] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用す [!DNL Experience Platform] る標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Veeva CRM] アカウントがある場合は、このドキュメントの残りの部分をスキップし、[ データフローの設定 ](../../dataflow/crm.md) に関するチュートリアルに進んでください。

### 必要な資格情報の収集

| 資格情報 | 説明 |
| ---------- | ----------- |
| `environmentUrl` | [!DNL Veeva CRM] ソースインスタンスの URL。 |
| `username` | [!DNL Veeva CRM] ユーザーアカウントのユーザー名。 |
| `password` | [!DNL Veeva CRM] ユーザーアカウントのパスワード。 |
| `securityToken` | [!DNL Veeva CRM] ユーザーアカウントのセキュリティトークン。 |

開始方法の詳細については、[ この Veeva CRM ドキュメント ] を参照してください。

## [!DNL Veeva CRM] アカウントに接続

必要な資格情報を収集したら、以下の手順に従って [!DNL Veeva CRM] アカウントを [!DNL Platform] にリンクします。

プラットフォーム UI で、左のナビゲーションバーから「 **[!UICONTROL ソース]** 」を選択して、「 [!UICONTROL  ソース ] 」ワークスペースにアクセスします。 [!UICONTROL  カタログ ] 画面には、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「[!UICONTROL CRM]」カテゴリで「**[!UICONTROL Veeva CRM]**」を選択し、「**[!UICONTROL データを追加]**」を選択します。

![カタログ](../../../../images/tutorials/create/veeva/catalog.png)

**[!UICONTROL Veeva CRM アカウントに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Veeva CRM] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存](../../../../images/tutorials/create/veeva/existing.png)

### 新規アカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、名前、説明（オプション）、[!DNL Veeva CRM] 資格情報を入力します。 終了したら、[**[!UICONTROL ソースに接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![新規](../../../../images/tutorials/create/veeva/new.png)

## 次の手順

このチュートリアルに従って、[!DNL Veeva CRM] アカウントへの接続を確立しました。 次のチュートリアルに進み、データを Platform に取り込むように [ データフローを設定します。](../../dataflow/crm.md)
