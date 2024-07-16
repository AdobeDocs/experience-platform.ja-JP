---
title: UI での SFTP Source接続の作成
description: Adobe Experience Platform UI を使用して SFTP ソース接続を作成する方法を説明します。
exl-id: 1a00ed27-3c95-4e57-9f94-45ff256bf75c
source-git-commit: f6d1cc811378f2f37968bf0a42b428249e52efd8
workflow-type: tm+mt
source-wordcount: '820'
ht-degree: 21%

---

# UI での [!DNL SFTP] ソース接続の作成

このチュートリアルでは、Adobe Experience Platform UI を使用して [!DNL SFTP] ソース接続を作成する手順を説明します。

## はじめに

このチュートリアルでは、Platform の次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

>[!IMPORTANT]
>
>[!DNL SFTP] ソース接続を持つ JSON オブジェクトを取り込む場合は、改行や改行を避けることをお勧めします。 この制限を回避するには、1 行に 1 つの JSON オブジェクトを使用し、複数の行を使用してファイルを送信します。

既に有効な [!DNL SFTP] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

を [!DNL SFTP] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL SFTP] サーバーに関連付けられた名前または IP アドレス。 |
| `port` | 接続先の [!DNL SFTP] サーバーポート。 指定しない場合、値はデフォルトで `22` になります。 |
| `username` | [!DNL SFTP] サーバーにアクセスできるユーザー名。 |
| `password` | [!DNL SFTP] サーバーのパスワード。 |
| `privateKeyContent` | Base64 でエンコードされた SSH 秘密鍵のコンテンツ。 OpenSSH キーのタイプは、RSA または DSA のいずれかに分類する必要があります。 |
| `passPhrase` | キーファイルまたはキーの内容がパスフレーズによって保護されている場合に秘密鍵を復号化するためのパスフレーズまたはパスワード。 PrivateKeyContent がパスワードで保護されている場合、このパラメーターは、PrivateKeyContent のパスフレーズを値として使用する必要があります。 |
| `maxConcurrentConnections` | このパラメーターを使用すると、SFTP サーバーへの接続時に Platform が作成する同時接続数の上限を指定できます。 この値は、SFTP で設定された制限以下に設定する必要があります。 **注意**：既存の SFTP アカウントに対してこの設定が有効になっている場合、既存のデータフローではなく、今後のデータフローにのみ影響します。 |
| フォルダーパス | アクセス権を付与するフォルダーへのパス。 ソース [!DNL SFTP]、フォルダーパスを指定して、選択したサブフォルダーへのユーザーアクセスを指定できます。 |

必要な資格情報を収集したら、次の手順に従って Platform に接続する新しい [!DNL SFTP] アカウントを作成できます。

## [!DNL SFTP] サーバーへの接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

[!UICONTROL  クラウドストレージ ] カテゴリで、「**[!UICONTROL SFTP]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![SFTP ソースが選択されたExperience Platformソースカタログ ](../../../../images/tutorials/create/sftp/catalog.png)

**[!UICONTROL SFTP に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続する FTP アカウントまたは SFTP アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![Experience PlatformUI の既存の SFTP アカウントのリスト。](../../../../images/tutorials/create/sftp/existing.png)

### 新規アカウント

>[!TIP]
>
>* 作成した後は、[!DNL SFTP] ベース接続の認証タイプを変更できません。 認証タイプを変更するには、新しいベース接続を作成する必要があります。
>
>* SFTP では、RSA または DSA タイプの OpenSSH キーをサポートしています。 主要なファイルの内容が `"-----BEGIN [RSA/DSA] PRIVATE KEY-----"` で始まり、`"-----END [RSA/DSA] PRIVATE KEY-----"` で終わることを確認してください。 秘密鍵ファイルが PPK 形式のファイルの場合は、PuTTY ツールを使用して、PPK 形式から OpenSSH 形式に変換します。

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、新しい [!DNL SFTP] アカウントの名前と説明（オプション）を入力します。

![SFTP の新規アカウント画面 ](../../../../images/tutorials/create/sftp/new.png)

[!DNL SFTP] ソースは、基本認証と SSH 公開鍵を使用した認証の両方をサポートしています。

>[!BEGINTABS]

>[!TAB  基本認証 ]

基本認証を使用するには、「**[!UICONTROL パスワード]**」を選択し、接続するホストとポートの値を、ユーザー名とパスワードと共に指定します。 この手順では、アクセス権を付与するサブフォルダーへのパスを指定することもできます。 終了したら「**[!UICONTROL ソースに接続]**」を選択します。

![ 基本認証を使用した SFTP ソースの新しいアカウント画面 ](../../../../images/tutorials/create/sftp/password.png)

>[!TAB SSH 公開鍵認証 ]

SSH 公開鍵ベースの資格情報を使用するには、「**[!UICONTROL SSH 公開鍵]**」を選択し、ホストとポートの値のほか、秘密鍵の内容とパスフレーズの組み合わせを指定します。 この手順では、アクセス権を付与するサブフォルダーへのパスを指定することもできます。 終了したら「**[!UICONTROL ソースに接続]**」を選択します。

![SSH 公開鍵を使用した SFTP ソースの新しいアカウント画面 ](../../../../images/tutorials/create/sftp/ssh.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、SFTP アカウントへの接続を確立しました。 次のチュートリアルに進み、[ データフローを設定して、クラウドストレージから Platform にデータを取り込む ](../../dataflow/batch/cloud-storage.md) ことができます。
