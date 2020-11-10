---
keywords: Experience Platform;home;popular topics;SFTP;FTP;ftp;sftp
solution: Experience Platform
title: UI での FTP または SFTP ソースコネクタの作成
topic: overview
type: Tutorial
description: このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してFTPまたはSFTPソースコネクタを作成する手順を説明します。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 9%

---


# UI での FTP または SFTP ソースコネクタの作成

>[!NOTE]
>
>FTPおよびSFTPコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ユー [!DNL Platform] ザインターフェイスを使用してFTPまたはSFTPソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なFTPまたはSFTP接続がある場合は、このドキュメントの残りの部分をスキップし、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/batch/cloud-storage.md)。

### サポートされているファイル形式

[!DNL Experience Platform] は、外部ソースから取り込む次のファイル形式をサポートしています。

* 区切り文字区切り値(DSV):DSV形式のデータ・ファイルのサポートは、現在、コンマ区切り値(CSV)に制限されています。 DSV形式のファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 将来、一般的なDSVのサポートが提供される予定です。
* JavaScript Object Notation (JSON):JSON形式のデータファイルは、XDMに準拠している必要があります。
* Apacheパーケット：パーケット形式のデータファイルは、XDMに準拠している必要があります。

### 必要な資格情報の収集

でFTPまたはSFTPサーバーにアクセスするに [!DNL Platform]は、サーバーのホスト名、ユーザー名、パスワードを指定する必要があります。

## FTPまたはSFTPサーバーに接続する

必要な資格情報を収集したら、次の手順に従って、接続先の新しいFTPまたはSFTPアカウントを作成でき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 **[!UICONTROL ソース]** 」ワークスペースにアクセスします。 [ **[!UICONTROL カタログ]** ]画面には様々なソースが表示され、このソースを使用して受信アカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 **[!UICONTROL Databases]** 」カテゴリで、「 **[!UICONTROL SFTP]**」を選択します。 このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**」を選択します。 それ以外の場合は、 **[!UICONTROL データ]** を選択して新しいFTPまたはSFTPコネクタを作成します。

![カタログ](../../../../images/tutorials/create/sftp/catalog.png)

「SFTP **[!UICONTROL に接続]** 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明および資格情報を入力します。 終了したら、 **[!UICONTROL [接続]** ]を選択し、新しい接続が確立されるまでの時間を許可します。

SFTPコネクタは、アクセスに使用できる様々な認証タイプを提供します。 「 **[!UICONTROL アカウント認証]** 」で、「 **[!UICONTROL パスワード]** 」を選択して、パスワードベースの秘密鍵証明書を使用します。

![connect-password](../../../../images/tutorials/create/sftp/password.png)

または、 **[SSH公開鍵を選択し]** 、秘密鍵の内容 **[!UICONTROL と]** パスフレーズの組み合わせを使用してSFTPアカウントに接続できます ****。

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

このチュートリアルに従って、FTPまたはSFTPアカウントへの接続を確立しました。 次のチュートリアルに進み、クラウドストレージのデータをに取り込むようにデータフローを [設定できます [!DNL Platform]](../../dataflow/batch/cloud-storage.md)。