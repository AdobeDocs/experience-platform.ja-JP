---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでPhoenixソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: ec2d0a33e0ae92a3153b7bdcad29734e487a0439
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 9%

---


# Create a [!DNL Phoenix] source connector in the UI

>[!NOTE]
> コネクタ [!DNL Phoenix] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ユー [!DNL Phoenix] ザインタフェースを使用して [!DNL Platform] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model] (XDM)システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL リアルタイム顧客プロファイル]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な [!DNL Phoenix] 接続がある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)

### 必要な資格情報の収集

で [!DNL Phoenix] アカウントにアクセスするに [!DNL Platform]は、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | サー [!DNL Phoenix] バーのIPアドレスまたはホスト名。 |
| `port` | サー [!DNL Phoenix] バーがクライアント接続をリッスンするために使用するTCPポートです。 に接続する場合 [!DNL Azure HDInsights]は、ポートを443に指定します。 |
| `httpPath` | サー [!DNL Phoenix] バーに対応する部分的なURL。 ク [!DNL Azure HDInsights] ラスターを使用する場合は、/hbasephoenix0を指定します。 |
| `username` | サーバーへのアクセスに使用するユー [!DNL Phoenix] ザー名。 |
| `password` | ユーザーに対応するパスワードです。 |
| `enableSsl` | サーバーへの接続がSSLを使用して暗号化されるかどうかを指定するトグル。 |

使い始める方法の詳細については、 [ [!DNL Phoenix] このドキュメントを参照してください](https://python-phoenixdb.readthedocs.io/en/latest/api.html)。

## アカウントに接続 [!DNL Phoenix] する

必要な資格情報を収集したら、次の手順に従って、接続する [!DNL Phoenix] アカウントをリンクでき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 **[!UICONTROL ソース]** 」ワークスペースにアクセスします。 [ **[!UICONTROL カタログ]** ]画面には、アカウントを作成する際に使用できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

[ **[!UICONTROL Databases]** ]カテゴリで、[ **[!UICONTROL Phoenix]**]を選択します。 このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**」を選択します。 それ以外の場合は、 **[!UICONTROL 追加]** データを選択して新しい [!DNL Phoenix] アカウントを作成します。

![カタログ](../../../../images/tutorials/create/phoenix/catalog.png)

「Phoenix **[!UICONTROL に接続]** 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明および [!DNL Phoenix] 資格情報を入力します。 終了したら、 **[!UICONTROL [接続]** ]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/phoenix/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Phoenix] アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/phoenix/existing.png)

## 次の手順

このチュートリアルに従って、ア [!DNL Phoenix] カウントへの接続を確立しました。 次のチュートリアルに進み、データを取り込むデータフローを [設定できます [!DNL Platform]](../../dataflow/databases.md)。