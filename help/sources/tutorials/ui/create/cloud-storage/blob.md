---
keywords: Experience Platform;home;popular topics;Azure Blob;azure blob;Azure blob connector
solution: Experience Platform
title: UIにAzure BLOBソースコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してAzure BLOB （以下、「BLOB」と呼ばれる）を作成する手順を説明します。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 9%

---


# Create an [!DNL Azure Blob] source connector in the UI

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 [!DNL Azure Blob] ユーザインターフェイスを使用してを作成する手順[!DNL Blob](以下「 [!DNL Platform] 」と呼びます)について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な [!DNL Blob] 接続がある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/batch/cloud-storage.md)。

### サポートされているファイル形式

[!DNL Experience Platform] は、外部ストレージから取り込む次のファイル形式をサポートしています。

- 区切り文字区切り値(DSV):DSV形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 DSV形式のファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 一般的なDSVファイルは、今後サポートされる予定です。
- JavaScript Object Notation (JSON):JSON形式のデータファイルは、XDMに準拠している必要があります。
- Apacheパーケット：パーケット形式のデータファイルは、XDMに準拠している必要があります。

### 必要な資格情報の収集

で [!DNL Blob] ストレージにアクセスするに [!DNL Platform]は、次の秘密鍵証明書の有効な値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | BLOBストレージのデータにアクセスするために必要な接続文字列です。 接続文字 [!DNL Blob] 列パターンは次のとおりです。 `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. |

使い始める方法の詳細については、 [ [!DNL Azure Blob] このドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string)。

## BLOBアカウントの接続

必要な資格情報を収集したら、次の手順に従って [!DNL Blob] アカウントをにリンクでき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 **[!UICONTROL ソース]** 」ワークスペースにアクセスします。 [ **[!UICONTROL カタログ]** ]画面には、アカウントを作成する際に使用できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

[ **[!UICONTROL Databases]** ]カテゴリの下で、[ **[!UICONTROL Azure Blobストレージ]**]を選択します。 このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**」を選択します。 それ以外の場合は、 **[!UICONTROL 追加]** データ [!DNL Blob]を選択して新しいコネクタを作成します。

![カタログ](../../../../images/tutorials/create/blob/catalog.png)

[ **[!UICONTROL Azure BLOBストレージに]** 接続]ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明および [!DNL Blob] 資格情報を入力します。 終了したら、 **[!UICONTROL [接続]** ]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/blob/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Blob] アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/blob/existing.png)

## 次の手順とその他のリソース

このチュートリアルに従って、ア [!DNL Blob] カウントへの接続を確立しました。 次のチュートリアルに進み、クラウドストレージのデータをに取り込むようにデータフローを [設定できます [!DNL Platform]](../../dataflow/batch/cloud-storage.md)。