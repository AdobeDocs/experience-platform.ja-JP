---
title: UI での SFTP Source接続の作成
description: Adobe Experience Platform UI を使用して SFTP ソース接続を作成する方法を説明します。
exl-id: 1a00ed27-3c95-4e57-9f94-45ff256bf75c
source-git-commit: 4816a6b627dc6551e351bfe3cdc4bc8c8ea8b17e
workflow-type: tm+mt
source-wordcount: '662'
ht-degree: 25%

---

# UI での [!DNL SFTP] ソース接続の作成

このチュートリアルでは、Adobe Experience Platform UI を使用して [!DNL SFTP] ソース接続を作成する手順を説明します。

## 基本を学ぶ

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

>[!IMPORTANT]
>
>[!DNL SFTP] ソース接続を持つ JSON オブジェクトを取り込む場合は、改行や改行を避けることをお勧めします。 この制限を回避するには、1 行に 1 つの JSON オブジェクトを使用し、複数の行を使用してファイルを送信します。

既に有効な [!DNL SFTP] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

認証資格情報の取得方法の手順について詳しくは、[[!DNL SFTP]  認証ガイド ](../../../../connectors/cloud-storage/sftp.md#gather-required-credentials) を参照してください。

## [!DNL SFTP] サーバーへの接続

Experience Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、「[!UICONTROL  ソース ] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

[!UICONTROL  クラウドストレージ ] カテゴリで、「**[!UICONTROL SFTP]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![Experience Platform ソースカタログと SFTP ソースが選択されています。](../../../../images/tutorials/create/sftp/catalog.png)

**[!UICONTROL SFTP に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続する FTP アカウントまたは SFTP アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![Experience Platform UI の既存の SFTP アカウントのリスト。](../../../../images/tutorials/create/sftp/existing.png)

### 新規アカウント

>[!TIP]
>
>* 作成した後は、[!DNL SFTP] ベース接続の認証タイプを変更できません。 認証タイプを変更するには、新しいベース接続を作成する必要があります。
>
>* SFTP では、`ed25519`、`RSA` または `DSA` タイプの OpenSSH キーをサポートしています。 主要なファイルの内容が `"-----BEGIN [RSA/DSA] PRIVATE KEY-----"` で始まり、`"-----END [RSA/DSA] PRIVATE KEY-----"` で終わることを確認してください。 秘密鍵ファイルが PPK 形式のファイルの場合は、PuTTY ツールを使用して、PPK 形式から OpenSSH 形式に変換します。

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、新しい [!DNL SFTP] アカウントの名前と説明（オプション）を入力します。

![SFTP の新規アカウント画面 ](../../../../images/tutorials/create/sftp/new.png)

[!DNL SFTP] ソースは、基本認証と SSH 公開鍵を使用した認証の両方をサポートしています。

>[!BEGINTABS]

>[!TAB  基本認証 ]

基本認証を使用するには、「**[!UICONTROL パスワード]**」を選択し、次の資格情報に適切な値を入力します。

* host
* ポート
* ユーザー名
* password

この手順では、最大同時接続数を設定したり、フォルダーパスを定義したり、[!DNL SFTP] サーバーのチャンクを有効または無効にしたりすることもできます。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、接続が確立されるまでしばらく待ちます。

認証について詳しくは、[ の必要な資格情報の収集  [!DNL SFTP]](../../../../connectors/cloud-storage/sftp.md#gather-required-credentials) に関するガイドを参照してください。

![ 基本認証を使用した SFTP ソースの新しいアカウント画面 ](../../../../images/tutorials/create/sftp/password.png)

>[!TAB SSH 公開鍵認証 ]

SSH 公開鍵ベースの資格情報を使用するには、「**[!UICONTROL SSH 公開鍵]**」を選択して、次の資格情報に適切な値を入力します。

* host
* ポート
* ユーザー名
* 秘密鍵のコンテンツ
* パスフレーズ

この手順では、最大同時接続数を設定したり、フォルダーパスを定義したり、[!DNL SFTP] サーバーのチャンクを有効または無効にしたりすることもできます。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、接続が確立されるまでしばらく待ちます。

認証について詳しくは、[ の必要な資格情報の収集  [!DNL SFTP]](../../../../connectors/cloud-storage/sftp.md#gather-required-credentials) に関するガイドを参照してください。

![SSH 公開鍵を使用した SFTP ソースの新しいアカウント画面 ](../../../../images/tutorials/create/sftp/ssh.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、SFTP アカウントへの接続を確立しました。 次のチュートリアルに進み、[ データフローを設定して、クラウドストレージからExperience Platformにデータを取り込む ](../../dataflow/batch/cloud-storage.md) ことができます。
