---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでMicrosoft Dynamicsソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: d3c725c4760acb3857a67d0d30b24732c963a030
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 17%

---


# Create a [!DNL Microsoft Dynamics] source connector in the UI

Adobe Experience Platformのソースコネクタは、外部ソースのCRMデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 [!DNL Microsoft Dynamics] ユーザインターフェイスを使用して、[!DNL Dynamics](以下「 [!DNL Platform] 」と呼ばれる)ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

既に有効な [!DNL Dynamics] アカウントをお持ちの場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/crm.md)。

### 必要な資格情報の収集

| Credential | 説明 |
| ---------- | ----------- |
| `serviceUri` | インスタンスのサービスURL [!DNL Dynamics] 。 |
| `username` | ユーザーアカウントのユー [!DNL Dynamics] ザー名です。 |
| `password` | Dynamicsアカウントのパスワードです。 |

開始方法の詳細については、 [このDynamicsドキュメントを参照してください](https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/authenticate-oauth)。

## アカウントに接続 [!DNL Dynamics] する

必要な資格情報を収集したら、次の手順に従って、接続する新しい [!DNL Dynamics] アカウントを作成でき [!DNL Platform]ます。

「 [Adobe Experience Platform](https://platform.adobe.com) 」にログインし、左のナビゲーションバーで「 **[!UICONTROL ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 カ *[!UICONTROL タログ]* 画面には様々なソースが表示され、このソースを使用してインバウンドアカウントを作成できます。各ソースには既存のアカウントの数と関連するデータセットフローが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

[ *[!UICONTROL Databases]* ] **[!UICONTROL カテゴリで、[]** Dynamics **]を選択し、[+]アイコン(+)** をクリックして新しい [!DNL Dynamics] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/ms-dynamics/catalog.png)

[ *[!UICONTROL Dynamicsに]* 接続]ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明および [!DNL Dynamics] 資格情報を入力します。 完了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/ms-dynamics/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Dynamics] アカウントを選択し、右上隅の「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/ms-dynamics/existing.png)

## 次の手順

このチュートリアルに従って、ア [!DNL Dynamics] カウントへの接続を確立しました。 次のチュートリアルに進み、データをPlatformに取り込むようにデータフローを [設定できます](../../dataflow/crm.md)。