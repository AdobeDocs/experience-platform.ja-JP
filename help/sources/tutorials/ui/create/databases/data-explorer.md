---
keywords: Experience Platform;home;popular topics;Azure Data Explorer;azure data explorer;data explorer;Data Explorer
solution: Experience Platform
title: UIでAzureData Explorerソースコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、Platformユーザーインターフェイスを使用してAzureData Explorer(以下「Data Explorer」と呼ばれる)ソースコネクタを作成する手順を説明します。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 9%

---


# Create an [!DNL Azure Data Explorer] source connector in the UI

>[!NOTE]
>
> コネクタ [!DNL Azure Data Explorer] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 [!DNL Azure Data Explorer] ユーザインターフェイスを使用して、[!DNL Data Explorer](以下「 [!DNL Platform] 」と呼ばれる)ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な [!DNL Data Explorer] 接続がある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

で [!DNL Data Explorer] アカウントにアクセスするに [!DNL Platform]は、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `endpoint` | The endpoint of the [!DNL Data Explorer] server. |
| `database` | The name of the [!DNL Data Explorer] database. |
| `tenant` | データベースへの接続に使用する一意のテナントID [!DNL Data Explorer] です。 |
| `servicePrincipalId` | データベースへの接続に使用する一意のサービスプリンシパルID [!DNL Data Explorer] です。 |
| `servicePrincipalKey` | データベースへの接続に使用する一意のサービスプリンシパルキー [!DNL Data Explorer] です。 |

使い始める方法の詳細については、 [ [!DNL Data Explorer] このドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/access-control/how-to-authenticate-with-aad)。

## アカウントに接続 [!DNL Azure Data Explorer] する

必要な資格情報を収集したら、次の手順に従って [!DNL Data Explorer] アカウントをにリンクでき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 **[!UICONTROL ソース]** 」ワークスペースにアクセスします。 [ **[!UICONTROL カタログ]** ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

[ **[!UICONTROL Databases]** ]カテゴリの下で、[ **[!UICONTROL AzureData Explorer]**]を選択します。 このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**」を選択します。 それ以外の場合は、 **[!UICONTROL 追加「]** data」を選択して新しいData Explorerコネクタを作成します。

![カタログ](../../../../images/tutorials/create/data-explorer/catalog.png)

[ **[!UICONTROL AzureData Explorerに]** 接続]ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明および [!DNL Data Explorer] 資格情報を入力します。 終了したら、 **[!UICONTROL [接続]** ]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/data-explorer/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Data Explorer] アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/data-explorer/existing.png)

## 次の手順

このチュートリアルに従って、ア [!DNL Data Explorer] カウントへの接続を確立しました。 次のチュートリアルに進み、データを取り込むデータフローを [設定できます [!DNL Platform]](../../dataflow/databases.md)。