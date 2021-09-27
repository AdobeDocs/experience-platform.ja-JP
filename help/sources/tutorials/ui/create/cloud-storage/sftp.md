---
keywords: Experience Platform；ホーム；人気のあるトピック；SFTP;sftp
solution: Experience Platform
title: UIでのSFTPソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UIを使用してSFTPソース接続を作成する方法を説明します。
exl-id: 1a00ed27-3c95-4e57-9f94-45ff256bf75c
source-git-commit: ade0da445b18108a7f8720404cc7a65139ed42b1
workflow-type: tm+mt
source-wordcount: '680'
ht-degree: 8%

---

# UIでの[!DNL SFTP]ソース接続の作成

このチュートリアルでは、Adobe Experience Platform UIを使用して[!DNL SFTP]ソース接続を作成する手順を説明します。

## はじめに

このチュートリアルは、 Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

>[!IMPORTANT]
>
>ソース接続[!DNL SFTP]でJSONオブジェクトを取り込む際には、改行やキャリッジリターンを避けることをお勧めします。 この制限を回避するには、1行に1つのJSONオブジェクトを使用し、その後のファイルに複数行を使用します。

既に有効な[!DNL SFTP]接続がある場合は、このドキュメントの残りの部分をスキップし、[データフローの設定](../../dataflow/batch/cloud-storage.md)に関するチュートリアルに進んでください。

### 必要な資格情報の収集

[!DNL SFTP]に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL SFTP]サーバーに関連付けられている名前またはIPアドレス。 |
| `port` | 接続先の[!DNL SFTP]サーバポート。 指定しない場合、値はデフォルトで`22`になります。 |
| `username` | [!DNL SFTP]サーバーへのアクセス権を持つユーザー名。 |
| `password` | [!DNL SFTP]サーバーのパスワード。 |
| `privateKeyContent` | Base64でエンコードされたSSH秘密鍵のコンテンツ。 OpenSSHキーのタイプは、RSAまたはDSAに分類する必要があります。 |
| `passPhrase` | キーファイルまたはキーコンテンツがパスフレーズで保護されている場合に秘密鍵を復号化するパスフレーズまたはパスワード。 PrivateKeyContentがパスワードで保護されている場合、このパラメーターをPrivateKeyContentのパスフレーズと共に値として使用する必要があります。 |

必要な資格情報を収集したら、以下の手順に従って新しい[!DNL SFTP]アカウントを作成し、Platformに接続します。

## [!DNL SFTP]サーバーに接続する

プラットフォームUIで、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「[!UICONTROL ソース]」ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、受信アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「[!UICONTROL クラウドストレージ]」カテゴリで「**[!UICONTROL SFTP]**」を選択し、「**[!UICONTROL データを追加]**」を選択します。

![カタログ](../../../../images/tutorials/create/sftp/catalog.png)

「**[!UICONTROL SFTPに接続]**」ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続するFTPまたはSFTPアカウントを選択し、「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存](../../../../images/tutorials/create/sftp/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、新しい[!DNL SFTP]アカウントの名前と説明（オプション）を入力します。

#### パスワードを使用した認証

[!DNL SFTP] は、アクセス用に様々な認証タイプをサポートしています。「**[!UICONTROL アカウント認証]**」で「**[!UICONTROL パスワード]**」を選択し、接続するホストとポートの値を、ユーザー名とパスワードと共に指定します。

![connect-password](../../../../images/tutorials/create/sftp/password.png)

#### SSH公開鍵を使用した認証

SSH公開鍵ベースの資格情報を使用するには、**[!UICONTROL SSH公開鍵]**&#x200B;を選択し、ホストとポートの値、および秘密鍵のコンテンツとパスフレーズの組み合わせを指定します。

>[!IMPORTANT]
>
>SFTPは、RSAまたはDSAタイプのOpenSSHキーをサポートします。 キーファイルの内容が`"-----BEGIN [RSA/DSA] PRIVATE KEY-----"`で始まり、`"-----END [RSA/DSA] PRIVATE KEY-----"`で終わることを確認します。 秘密鍵ファイルがPPK形式のファイルである場合は、PuTTYツールを使用して、PPKからOpenSSH形式に変換します。

![connect-ssh](../../../../images/tutorials/create/sftp/ssh-public-key.png)

| 資格情報 | 説明 |
| ---------- | ----------- |
| 秘密鍵コンテンツ | Base64でエンコードされたSSH秘密鍵のコンテンツ。 OpenSSHキーのタイプは、RSAまたはDSAに分類する必要があります。 |
| パスフレーズ | キーファイルやキーコンテンツがパスフレーズで保護されている場合に秘密鍵を復号化するパスフレーズまたはパスワードを指定します。 PrivateKeyContentがパスワードで保護されている場合、このパラメーターをPrivateKeyContentのパスフレーズと共に値として使用する必要があります。 |


## 次の手順

このチュートリアルに従って、SFTPアカウントへの接続を確立しました。 次のチュートリアルに進み、クラウドストレージからPlatform](../../dataflow/batch/cloud-storage.md)にデータを取り込むように[データフローを設定します。
