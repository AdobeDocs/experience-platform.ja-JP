---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでAzureData Explorerソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 41fe3e5b2a830c3182b46b3e0873b1672a1f1b03
workflow-type: tm+mt
source-wordcount: '505'
ht-degree: 15%

---


# Create an [!DNL Azure Data Explorer] source connector in the UI

>[!NOTE]
> コネクタ [!DNL Azure Data Explorer] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 [!DNL Azure Data Explorer] ユーザインターフェイスを使用して、[!DNL Data Explorer](以下「 [!DNL Platform] 」と呼ばれる)ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)[!DNL Experience Platform]： が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

既に有効な [!DNL Data Explorer] 接続がある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

で [!DNL Data Explorer] アカウントにアクセスするに [!DNL Platform]は、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `endpoint` | サー [!DNL Data Explorer] バーのエンドポイント。 |
| `database` | The name of the [!DNL Data Explorer] database. |
| `tenant` | データベースへの接続に使用する一意のテナントID [!DNL Data Explorer] です。 |
| `servicePrincipalId` | Data Explorerデータベースへの接続に使用する一意のサービスプリンシパルID。 |
| `servicePrincipalKey` | Data Explorerデータベースへの接続に使用する一意のサービスプリンシパルキーです。 |

使い始める前に詳しくは、 [このData Explorerドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/access-control/how-to-authenticate-with-aad)。

## アカウントに接続 [!DNL Azure Data Explorer] する

必要な資格情報を収集したら、次の手順に従って、接続する新しい [!DNL Data Explorer] アカウントを作成でき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 *ソース* 」ワークスペースにアクセスします。 カ *[!UICONTROL タログ]* 画面には様々なソースが表示され、このソースから受信アカウントを作成できます。各ソースには既存のアカウントの数と関連するデータセットフローが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

[ *[!UICONTROL Databases]***カテゴリで、[]** AzureData Explorer **を選択し、次に[]** AzureData Explorer]を選択して、新しいコネクタを作成します。

![カタログ](../../../../images/tutorials/create/data-explorer/catalog.png)

[ *[!UICONTROL AzureData Explorerに]* 接続]ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明および [!DNL Data Explorer] 資格情報を入力します。 完了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/data-explorer/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Data Explorer] アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/data-explorer/existing.png)

## 次の手順

このチュートリアルに従って、ア [!DNL Data Explorer] カウントへの接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/databases.md)。