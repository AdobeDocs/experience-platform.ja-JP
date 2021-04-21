---
keywords: Experience Platform；ホーム；人気のあるトピック；FTP;ftp
solution: Experience Platform
title: UIでのFTPソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してFTPソース接続を作成する方法を説明します。
exl-id: 8e505ead-4bae-43fe-830b-75620e8fba28
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 10%

---

# UIでのFTPソース接続の作成

>[!NOTE]
>
>FTPコネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

このチュートリアルでは、Adobe Experience PlatformUIを使用したFTPソース接続の作成手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なFTP接続をお持ちの場合は、このドキュメントの残りの部分をスキップして、[データフロー](../../dataflow/batch/cloud-storage.md)の設定のチュートリアルに進むことができます。

### 必要な資格情報の収集

FTPに接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | FTPサーバーに関連付けられている名前またはIPアドレス。 |
| `username` | FTPサーバーへのアクセス権を持つユーザー名。 |
| `password` | FTPサーバーのパスワードです。 |

## FTPサーバーに接続する

必要な資格情報を収集したら、次の手順に従って新しいFTPアカウントを作成し、プラットフォームに接続します。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して[!UICONTROL ソース]ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、様々なソースが表示され、これらのソースを使用して受信アカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

[!UICONTROL クラウドストレージ]カテゴリの下で、**[!UICONTROL FTP]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しいFTP接続を作成します。

![カタログ](../../../../images/tutorials/create/ftp/catalog.png)

「**[!UICONTROL FTPに接続]**」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明および資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![新規](../../../../images/tutorials/create/ftp/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するFTPアカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![既存の](../../../../images/tutorials/create/ftp/existing.png)

## 次の手順

このチュートリアルに従って、FTPアカウントへの接続を確立しました。 次のチュートリアルに進み、[データフローを設定して、クラウドストレージのデータをプラットフォーム](../../dataflow/batch/cloud-storage.md)に取り込むことができます。
