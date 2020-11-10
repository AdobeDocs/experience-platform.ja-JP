---
keywords: Experience Platform;home;popular topics;Microsoft Dynamics;microsoft dynamics;Dynamics;dynamics
solution: Experience Platform
title: UIでMicrosoft Dynamicsソースコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してMicrosoft Dynamics （以下「Dynamics」と呼ばれる）ソースコネクタを作成する手順を説明します。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 10%

---


# Create a [!DNL Microsoft Dynamics] source connector in the UI

Adobe Experience Platformのソースコネクタは、外部ソースのCRMデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 [!DNL Microsoft Dynamics] ユーザインターフェイスを使用して、[!DNL Dynamics](以下「 [!DNL Platform] 」と呼ばれる)ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な [!DNL Dynamics] アカウントをお持ちの場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/crm.md)。

### 必要な資格情報の収集

| Credential | 説明 |
| ---------- | ----------- |
| `serviceUri` | インスタンスのサービスURL [!DNL Dynamics] 。 |
| `username` | ユーザーアカウントのユー [!DNL Dynamics] ザー名です。 |
| `password` | アカウントのパス [!DNL Dynamics] ワード。 |

使い始める方法の詳細については、 [ [!DNL Dynamics] このドキュメントを参照してください](https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/authenticate-oauth)。

## アカウントに接続 [!DNL Dynamics] する

必要な資格情報を収集したら、次の手順に従って [!DNL Dynamics] アカウントをにリンクでき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 **[!UICONTROL ソース]** 」ワークスペースにアクセスします。 [ **[!UICONTROL カタログ]** ]画面には、アカウントを作成する際に使用できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

[ **[!UICONTROL データベース]** ]カテゴリで、[ **[!UICONTROL ダイナミクス]**]を選択します。 このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**」を選択します。 それ以外の場合は、 **[!UICONTROL 追加]** データ [!DNL Dynamics] を選択して新しいコネクタを作成します。

![カタログ](../../../../images/tutorials/create/ms-dynamics/catalog.png)

[ **[!UICONTROL Dynamicsに]** 接続]ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明および [!DNL Dynamics] 資格情報を入力します。 終了したら、 **[!UICONTROL [接続]** ]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/ms-dynamics/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Dynamics] アカウントを選択し、右上隅の「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/ms-dynamics/existing.png)

## 次の手順

このチュートリアルに従って、ア [!DNL Dynamics] カウントへの接続を確立しました。 次のチュートリアルに進み、データを取り込むデータフローを [設定できます [!DNL Platform]](../../dataflow/crm.md)。