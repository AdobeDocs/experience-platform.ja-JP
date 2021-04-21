---
keywords: Experience Platform；ホーム；人気の高いトピック；SFTP;SFTP
solution: Experience Platform
title: UIでのSFTPソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してSFTPソース接続を作成する方法を説明します。
exl-id: 1a00ed27-3c95-4e57-9f94-45ff256bf75c
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 6%

---

# UIでのSFTPソース接続の作成

このチュートリアルでは、Adobe Experience PlatformUIを使用したSFTPソース接続の作成手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

>[!IMPORTANT]
>
>SFTPソース接続でJSONオブジェクトを取り込む場合は、改行やキャリッジリターンを避けることをお勧めします。 この制限を回避するには、1行に1つのJSONオブジェクトを使用し、続くファイルには複数行を使用します。

既に有効なSFTP接続をお持ちの場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)のチュートリアルに進むことができます。

### 必要な資格情報の収集

SFTPに接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | SFTPサーバーに関連付けられている名前またはIPアドレス。 |
| `username` | SFTPサーバーへのアクセス権を持つユーザー名。 |
| `password` | SFTPサーバーのパスワードです。 |
| `privateKeyContent` | Base64エンコードされたSSH秘密鍵のコンテンツ。 OpenSSHキーのタイプは、RSAまたはDSAに分類する必要があります。 |
| `passPhrase` | 鍵ファイルや鍵の内容がパスフレーズで保護されている場合に、秘密鍵を復号化するためのパスフレーズまたはパスワード。 PrivateKeyContentがパスワードで保護されている場合は、PrivateKeyContentのパスフレーズを値として使用する必要があります。 |

必要な資格情報を収集したら、次の手順に従って新しいSFTPアカウントを作成し、プラットフォームに接続します。

## SFTPサーバーに接続する

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して[!UICONTROL ソース]ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、様々なソースが表示され、これらのソースを使用して受信アカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

[!UICONTROL クラウドストレージ]カテゴリの下で、**[!UICONTROL SFTP]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して、新しいSFTP接続を作成します。

![カタログ](../../../../images/tutorials/create/sftp/catalog.png)

「**[!UICONTROL SFTPに接続]**」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明および資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

SFTPコネクタは、アクセスに使用できる様々な認証タイプを提供します。 「**[!UICONTROL アカウント認証]**」で、「**[!UICONTROL パスワード]**」を選択して、パスワードベースの秘密鍵証明書を使用します。

![connect-password](../../../../images/tutorials/create/sftp/password.png)

または、**[SSH公開鍵]**&#x200B;を選択し、[!UICONTROL 秘密鍵の内容]と[!UICONTROL パスフレーズ]の組み合わせを使用してSFTPアカウントに接続します。

>[!IMPORTANT]
>
>SFTPコネクタは、RSAまたはDSAタイプのOpenSSHキーをサポートします。 `"-----BEGIN [RSA/DSA] PRIVATE KEY-----"`で終わり`"-----END [RSA/DSA] PRIVATE KEY-----"`で終わる主要なファイルコンテンツ開始を確認してください。 秘密鍵ファイルがPPK形式のファイルの場合は、PuTTYツールを使用してPPK形式からOpenSSH形式に変換します。

![connect-ssh](../../../../images/tutorials/create/sftp/ssh.png)

| Credential | 説明 |
| ---------- | ----------- |
| 秘密鍵の内容 | Base64エンコードされたSSH秘密鍵のコンテンツ。 OpenSSHキーのタイプは、RSAまたはDSAに分類する必要があります。 |
| Passphrase | キーファイルまたはキーの内容がパスフレーズで保護されている場合に、秘密鍵を復号化するためのパスフレーズまたはパスワードを指定します。 PrivateKeyContentがパスワードで保護されている場合は、PrivateKeyContentのパスフレーズを値として使用する必要があります。 |

### 既存のアカウント

既存のアカウントに接続するには、接続するFTPまたはSFTPアカウントを選択し、「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/sftp/existing.png)

## 次の手順

このチュートリアルに従って、FTPまたはSFTPアカウントへの接続を確立しました。 次のチュートリアルに進み、[データフローを設定して、クラウドストレージのデータをプラットフォーム](../../dataflow/batch/cloud-storage.md)に取り込むことができます。
