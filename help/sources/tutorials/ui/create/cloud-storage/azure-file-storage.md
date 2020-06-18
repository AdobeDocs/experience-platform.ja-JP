---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIにAzure Fileストレージソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: ced839f64bea48703c530c83d8592f3842c17e53
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 0%

---


# UIにAzure Fileストレージソースコネクタを作成する

>[!NOTE]
>Azure Fileストレージコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、Platformユーザーインターフェイスを使用してAzureファイルストレージソースコネクタを認証する手順を説明します。

## はじめに

このチュートリアルでは、次のAdobe Experience Platformのコンポーネントについて十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../../../xdm/home.md): Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

ファイルストレージ接続が既に存在する場合は、このドキュメントの残りの部分をスキップし、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/batch/cloud-storage.md)。

### 必要な資格情報の収集

Azure Fileストレージソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | アクセスするAzure Fileストレージインスタンスのエンドポイントです。 |
| `userId` | Azure Fileストレージエンドポイントへの十分なアクセス権を持つユーザー。 |
| `password` | Azure Fileストレージアクセスキー。 |

開始方法の詳細については、 [このAzure Fileストレージドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-windows)。

## Azure Fileストレージアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しいAzureファイルストレージアカウントを作成し、Platformに接続します。

「 [Adobe Experience Platform](https://platform.adobe.com) 」にログインし、左のナビゲーションバーで「 **[!UICONTROL ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 「 *[!UICONTROL カタログ]* 」画面には様々なソースが表示され、このソースを使用してインバウンドアカウントを作成できます。各ソースには、関連付けられた既存のアカウントおよびデータフローの数が表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

[ *[!UICONTROL Databases]* ] **[!UICONTROL カテゴリの下で、[]** Azure File]ストレージを選択し、[+]アイコン **(+)** をクリックして新しいAzure Fileストレージコネクタを作成します。

![カタログ](../../../../images/tutorials/create/azure-file-storage/catalog.png)

[ *[!UICONTROL Azure Fileストレージに]* 接続]ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明、およびファイルストレージの資格情報を入力します。 完了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/azure-file-storage/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するAzure Fileストレージアカウントを選択し、[ **[!UICONTROL 次へ]** ]を選択して次に進みます。

![既存の](../../../../images/tutorials/create/azure-file-storage/existing.png)

## 次の手順

このチュートリアルに従うと、Azure Fileストレージアカウントへの接続が確立されます。 次のチュートリアルに進み、クラウドストレージのデータをPlatformに取り込むようにデータフローを [設定できます](../../dataflow/batch/cloud-storage.md)。