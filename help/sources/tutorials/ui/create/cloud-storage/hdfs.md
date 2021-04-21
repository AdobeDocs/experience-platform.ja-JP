---
keywords: Experience Platform；ホーム；人気の高いトピック；Apache HDFS;HDFS;hdfs
solution: Experience Platform
title: UIでのApache HDFSソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してApacheHadoop分散ファイルシステムのソース接続を作成する方法を学びます。
exl-id: 3b8bf210-13b6-44e6-9090-152998f67452
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 6%

---

# UIに[!DNL Apache] HDFSソース接続を作成する

>[!NOTE]
>
>[!DNL Apache] HDFSコネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

[!DNL Adobe Experience Platform]のソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Apache Hadoop Distributed File System] （以下「HDFS」と呼びます）ソースコネクタを[!DNL Platform]ユーザーインターフェイスを使用して認証する手順を説明します。

## はじめに

このチュートリアルでは、[!DNL Platform]の次のコンポーネントについて十分に理解している必要があります。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なHDFS接続を持っている場合は、このドキュメントの残りの部分をスキップして、[データフロー](../../dataflow/batch/cloud-storage.md)の設定に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

HDFSソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `url` | URLは、HDFSへの接続に必要な認証パラメーターを匿名で定義します。 この値の取得方法について詳しくは、[HDFSのHTTPS認証](https://hadoop.apache.org/docs/r1.2.1/HttpAuthentication.html)に関する次のドキュメントを参照してください。 |

## HDFSアカウントの接続

必要な資格情報を収集したら、次の手順に従ってHDFSアカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL クラウドストレージ]**&#x200B;カテゴリの下で、**[!UICONTROL Apache HDFS]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しいHDFSコネクタを作成します。

![カタログ](../../../../images/tutorials/create/hdfs/catalog.png)

**[!UICONTROL HDFSに接続]**&#x200B;ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、およびHDFS証明書を入力します。 終了したら、[**[!UICONTROL ソースに接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/hdfs/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するHDFSアカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![既存の](../../../../images/tutorials/create/hdfs/existing.png)

## 次の手順

このチュートリアルに従って、HDFSアカウントへの接続を確立しました。 次のチュートリアルに進み、[データフローを設定して、クラウドストレージのデータを [!DNL Platform]](../../dataflow/batch/cloud-storage.md)に取り込むことができます。
