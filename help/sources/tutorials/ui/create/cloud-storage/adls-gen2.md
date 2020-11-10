---
keywords: Experience Platform;home;popular topics;Azure Data Lake Storage Gen2;ADLS Gen2;adls gen2;adls connector
solution: Experience Platform
title: UI での Azure データレイクストレージ Gen2 ソースコネクタの作成
topic: overview
type: Tutorial
description: このチュートリアルでは、Azure Data LakeストレージGen2 （以下「ADLS Gen2」と呼ばれる）ソースコネクタをプラットフォームユーザーインターフェイスを使用して認証する手順を説明します。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 12%

---


# Create an [!DNL Azure Data Lake Storage Gen2] source connector in the UI

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 [!DNL Azure Data Lake Storage Gen2] ユーザインターフェイスを使用して、[!DNL ADLS Gen2](以下「 [!DNL Platform] 」と呼ばれる)ソースコネクタを認証する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なADLS Gen2接続がある場合は、このドキュメントの残りの部分をスキップし、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/batch/cloud-storage.md)。

### 必要な資格情報の収集

ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があり [!DNL ADLS Gen2] ます。

| Credential | 説明 |
| ---------- | ----------- |
| `url` | のエンドポイントで [!DNL ADLS Gen2]す。 |
| `servicePrincipalId` | アプリケーションのクライアントID。 |
| `servicePrincipalKey` | アプリのキー。 |
| `tenant` | アプリケーションを含むテナント情報。 |

これらの値の詳細については、 [ [!DNL ADLS Gen2] このドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-storage)。

## アカウントに接続 [!DNL ADLS Gen2] する

必要な資格情報を収集したら、次の手順に従って、接続する [!DNL ADLS Gen2] アカウントをリンクでき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 **[!UICONTROL ソース]** 」ワークスペースにアクセスします。 [ **[!UICONTROL カタログ]** ]画面には、アカウントを作成する際に使用できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

[ **[!UICONTROL Databases]** ]カテゴリの下で、[ **[!UICONTROL Azure Data Lake Gen2]**]を選択します。 このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**」を選択します。 それ以外の場合は、 **[!UICONTROL データ]** を選択して新しいADLS Gen2コネクタを作成します。

![](../../../../images/tutorials/create/adls-gen2/catalog.png)

[ **[!UICONTROL Azure Data Lake Gen2に]** 接続]ダイアログが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明および [!DNL ADLS Gen2] 資格情報を入力します。 終了したら、 **[!UICONTROL [接続]** ]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/adls-gen2/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL ADLS Gen2] アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![](../../../../images/tutorials/create/adls-gen2/existing.png)

## 次の手順

このチュートリアルに従って、ア [!DNL ADLS Gen2] カウントへの接続を確立しました。 次のチュートリアルに進み、クラウドストレージのデータをに取り込むようにデータフローを [設定できます [!DNL Platform]](../../dataflow/batch/cloud-storage.md)。