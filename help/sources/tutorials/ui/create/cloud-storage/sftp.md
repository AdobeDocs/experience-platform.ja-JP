---
keywords: Experience Platform;home;popular topics;SFTP;sftp
solution: Experience Platform
title: UIでのSFTPソースコネクタの作成
topic: overview
type: Tutorial
description: このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してSFTPソースコネクタを作成する手順を説明します。
translation-type: tm+mt
source-git-commit: 7b638f0516804e6a2dbae3982d6284a958230f42
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 7%

---


# UIでのSFTPソースコネクタの作成

>[!NOTE]
>
>SFTPコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してSFTPソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なSFTP接続がある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/batch/cloud-storage.md)。

### 必要な資格情報の収集

SFTPに接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | SFTPサーバーに関連付けられている名前またはIPアドレス。 |
| `username` | SFTPサーバーへのアクセス権を持つユーザー名。 |
| `password` | SFTPサーバーのパスワードです。 |
| `privateKeyContent` | Base64エンコードされたSSH秘密鍵のコンテンツ。 SSH秘密鍵のOpenSSH(RSA/DSA)形式。 |
| `passPhrase` | 鍵ファイルや鍵の内容がパスフレーズで保護されている場合に、秘密鍵を復号化するためのパスフレーズまたはパスワード。 PrivateKeyContentがパスワードで保護されている場合は、PrivateKeyContentのパスフレーズを値として使用する必要があります。 |

必要な資格情報を収集したら、次の手順に従って新しいSFTPアカウントを作成し、プラットフォームに接続します。

## SFTPサーバーに接続する

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 [!UICONTROL ソース] 」ワークスペースにアクセスします。 [ [!UICONTROL カタログ] ]画面には様々なソースが表示され、このソースを使用して受信アカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 [!UICONTROL クラウドストレージ] 」カテゴリで、「 **[!UICONTROL SFTP]**」を選択します。 このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**」を選択します。 それ以外の場合は、 **** 追加データを選択して新しいSFTP接続を作成します。

![カタログ](../../../../images/tutorials/create/sftp/catalog.png)

「SFTP **[!UICONTROL に接続]** 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明および資格情報を入力します。 終了したら、 **[!UICONTROL [接続]** ]を選択し、新しい接続が確立されるまでの時間を許可します。

SFTPコネクタは、アクセスに使用できる様々な認証タイプを提供します。 「 **[!UICONTROL アカウント認証]** 」で、「 **[!UICONTROL パスワード]** 」を選択して、パスワードベースの秘密鍵証明書を使用します。

![connect-password](../../../../images/tutorials/create/sftp/password.png)

または、 **[SSH公開鍵を選択し]** 、秘密鍵の内容 [!UICONTROL と] パスフレーズの組み合わせを使用してSFTPアカウントに接続できます 。

>[!IMPORTANT]
>
>SFTPコネクタは、RSA/DSA OpenSSHキーをサポートします。 主なファイルコンテンツの開始をに確認し `"-----BEGIN [RSA/DSA] PRIVATE KEY-----"`ます。 秘密鍵ファイルがPPK形式のファイルの場合は、PuTTYツールを使用してPPK形式からOpenSSH形式に変換します。

![connect-ssh](../../../../images/tutorials/create/sftp/ssh.png)

| Credential | 説明 |
| ---------- | ----------- |
| 秘密鍵の内容 | Base64エンコードされたSSH秘密鍵のコンテンツ。 SSH秘密鍵はOpenSSH形式にする必要があります。 |
| Passphrase | キーファイルまたはキーの内容がパスフレーズで保護されている場合に、秘密鍵を復号化するためのパスフレーズまたはパスワードを指定します。 PrivateKeyContentがパスワードで保護されている場合は、PrivateKeyContentのパスフレーズを値として使用する必要があります。 |

### 既存のアカウント

既存のアカウントに接続するには、接続するFTPまたはSFTPアカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/sftp/existing.png)

## 次の手順

このチュートリアルに従って、FTPまたはSFTPアカウントへの接続を確立しました。 次のチュートリアルに進み、クラウドストレージのデータをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/batch/cloud-storage.md)。