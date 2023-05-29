---
keywords: Experience Platform；ホーム；人気の高いトピック；FTP;ftp
solution: Experience Platform
title: UI での FTP ソース接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して FTP ソース接続を作成する方法を説明します。
exl-id: 8e505ead-4bae-43fe-830b-75620e8fba28
source-git-commit: e37c00863249e677f1645266859bf40fe6451827
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 34%

---

# UI での FTP ソース接続の作成

>[!NOTE]
>
>FTP コネクタはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

このチュートリアルでは、Adobe Experience Platform UI を使用して FTP ソース接続を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な FTP 接続がある場合は、このドキュメントの残りの部分をスキップし、次のドキュメントのチュートリアルに進んでください。 [データフローの設定](../../dataflow/batch/cloud-storage.md).

### 必要な資格情報の収集

FTP に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | FTP サーバーに関連付けられた名前または IP アドレス。 |
| `username` | FTP サーバーへのアクセス権を持つユーザー名。 |
| `password` | FTP サーバーのパスワード。 |

## FTP サーバーへの接続

必要な資格情報を収集したら、次の手順に従って、Platform に接続するための新しい FTP アカウントを作成できます。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから [!UICONTROL ソース] ワークスペース。 この [!UICONTROL カタログ] 画面には、インバウンドアカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 [!UICONTROL クラウドストレージ] カテゴリ、選択 **[!UICONTROL FTP]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** をクリックして、新しい FTP 接続を作成します。

![カタログ](../../../../images/tutorials/create/ftp/catalog.png)

この **[!UICONTROL FTP に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、アカウント名、説明（オプション）、 の資格情報を入力します。終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![新規](../../../../images/tutorials/create/ftp/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する FTP アカウントを選択し、「 」を選択します **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/ftp/existing.png)

## 次の手順

このチュートリアルに従って、FTP アカウントへの接続を確立しました。 次のチュートリアルに進み、 [データフローを設定して、クラウドストレージから Platform にデータを取り込みます。](../../dataflow/batch/cloud-storage.md).
