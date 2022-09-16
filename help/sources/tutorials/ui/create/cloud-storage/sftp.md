---
keywords: Experience Platform;ホーム;人気のトピック;SFTP;sftp
solution: Experience Platform
title: UI での SFTP ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して SFTP ソース接続を作成する方法を説明します。
exl-id: 1a00ed27-3c95-4e57-9f94-45ff256bf75c
source-git-commit: ade0da445b18108a7f8720404cc7a65139ed42b1
workflow-type: tm+mt
source-wordcount: '680'
ht-degree: 24%

---

# の作成 [!DNL SFTP] UI のソース接続

このチュートリアルでは、 [!DNL SFTP] Adobe Experience Platform UI を使用したソース接続

## はじめに

このチュートリアルは、 Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを整理する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

>[!IMPORTANT]
>
>JSON オブジェクトを [!DNL SFTP] ソース接続。 この制限を回避するには、1 行に 1 つの JSON オブジェクトを使用し、その後のファイルに複数行を使用します。

既に有効な [!DNL SFTP] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)に関するチュートリアルに進むことができます。

### 必要な認証情報の収集

に接続するには [!DNL SFTP]を使用する場合、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `host` | に関連付けられている名前または IP アドレス [!DNL SFTP] サーバー。 |
| `port` | この [!DNL SFTP] 接続先のサーバーポート。 指定しない場合、値はデフォルトでになります。 `22`. |
| `username` | ユーザー名 ( [!DNL SFTP] サーバー。 |
| `password` | ユーザーのパスワード [!DNL SFTP] サーバー。 |
| `privateKeyContent` | Base64 でエンコードされた SSH 秘密鍵コンテンツ。 OpenSSH キーのタイプは、RSA または DSA に分類する必要があります。 |
| `passPhrase` | キーファイルまたはキーコンテンツがパスフレーズで保護されている場合に秘密鍵を復号化するためのパスフレーズまたはパスワード。 PrivateKeyContent がパスワードで保護されている場合、このパラメーターを PrivateKeyContent のパスフレーズと共に値として使用する必要があります。 |

必要な資格情報を収集したら、次の手順に従って新しい [!DNL SFTP] Platform に接続するアカウント。

## に接続 [!DNL SFTP] server

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。この [!UICONTROL カタログ] 画面には、インバウンドアカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 [!UICONTROL クラウドストレージ] カテゴリ、選択 **[!UICONTROL SFTP]** 次に、 **[!UICONTROL データを追加]**.

![カタログ](../../../../images/tutorials/create/sftp/catalog.png)

この **[!UICONTROL SFTP に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続する FTP または SFTP アカウントを選択し、「 」を選択します **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/sftp/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「 **[!UICONTROL 新しいアカウント]**&#x200B;新しい [!DNL SFTP] アカウント

#### パスワードを使用した認証

[!DNL SFTP] は、アクセス用に様々な認証タイプをサポートしています。 の下 **[!UICONTROL アカウント認証]** 選択 **[!UICONTROL パスワード]** 次に、接続先のホストとポートの値を、ユーザー名とパスワードと共に指定します。

![connect-password](../../../../images/tutorials/create/sftp/password.png)

#### SSH 公開鍵を使用した認証

SSH 公開鍵ベースの資格情報を使用するには、「 」を選択します。 **[!UICONTROL SSH 公開鍵]**  次に、ホストとポートの値、秘密鍵のコンテンツとパスフレーズの組み合わせを指定します。

>[!IMPORTANT]
>
>SFTP では、RSA または DSA タイプの OpenSSH キーをサポートしています。 鍵となるファイルコンテンツが `"-----BEGIN [RSA/DSA] PRIVATE KEY-----"` で終わる `"-----END [RSA/DSA] PRIVATE KEY-----"`. 秘密鍵ファイルが PPK 形式のファイルの場合は、PuTTY ツールを使用して PPK から OpenSSH 形式に変換します。

![connect-ssh](../../../../images/tutorials/create/sftp/ssh-public-key.png)

| 認証情報 | 説明 |
| ---------- | ----------- |
| 秘密鍵コンテンツ | Base64 でエンコードされた SSH 秘密鍵コンテンツ。 OpenSSH キーのタイプは、RSA または DSA に分類する必要があります。 |
| パスフレーズ | キーファイルまたはキーコンテンツがパスフレーズで保護されている場合に秘密鍵を復号化するパスフレーズまたはパスワードを指定します。 PrivateKeyContent がパスワードで保護されている場合、このパラメーターを PrivateKeyContent のパスフレーズと共に値として使用する必要があります。 |


## 次の手順

このチュートリアルに従って、SFTP アカウントへの接続を確立しました。 次のチュートリアルに進み、 [データフローを設定して、クラウドストレージから Platform にデータを取り込みます。](../../dataflow/batch/cloud-storage.md).
