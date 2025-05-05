---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；Query service;Aqua Data Studio;Aqua Data Studio；クエリサービスへの接続；
solution: Experience Platform
title: Aqua Data Studio のクエリサービスへの接続
description: このドキュメントでは、Aqua Data Studio と Adobe Experience Platform クエリサービスを接続する手順について説明します。
exl-id: 4770e221-48a7-45d8-80a4-60b5cbc0ec33
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 8%

---

# クエリサービスへの [!DNL Aqua Data Studio] の接続

このドキュメントでは、[!DNL Aqua Data Studio] とAdobe Experience Platform [!DNL Query Service] を接続する手順について説明します。

## はじめに

このガイドでは、[!DNL Aqua Data Studio] へのアクセス権を既に持ち、そのインターフェイスの操作方法に精通している必要があります。 [!DNL Aqua Data Studio] について詳しくは、[official [!DNL Aqua Data Studio] documentation](https://www.aquaclusters.com/app/home/project/public/aquadatastudio/wikibook/Documentation21.1/page/0/Aqua-Data-Studio-21-1) を参照してください。

>[!NOTE]
>
>[!DNL Aqua Data Studio] には [!DNL Windows] バージョンと [!DNL macOS] バージョンがあります。 このガイドのスクリーンショットは、[!DNL macOS] デスクトップアプリを使用して撮影したものです。 UI とバージョンの間に小さな不一致が生じる場合があります。

[!DNL Aqua Data Studio] をExperience Platformに接続するために必要な資格情報を取得するには、Experience Platform UI の [!UICONTROL &#x200B; クエリ &#x200B;] ワークスペースにアクセスできる必要があります。 現在、[!UICONTROL &#x200B; クエリ &#x200B;] ワークスペースにアクセスできない場合は、組織の管理者にお問い合わせください。

## サーバーを登録 {#register-server}

[!DNL Aqua Data Studio] をインストールしたら、まずサーバーを登録する必要があります。 [ ダイアログを起動 ](https://www.aquaclusters.com/app/home/project/public/aquadatastudio/wikibook/Documentation18/page/81/Registering-a-Database-Server#launching_the_register_server_dialog)、[ サーバーを登録 ](https://www.aquaclusters.com/app/home/project/public/aquadatastudio/wikibook/Documentation18/page/81/Registering-a-Database-Server#steps_to_register_a_server_in_aqua_data_studio) する方法については、Aqua Data Studio の公式ドキュメ  [!DNL Register Server]  トを参照してください。

PostgresSQL サーバの **[!DNL Register Server]** ダイアログが表示されたら、サーバ設定の次の詳細を入力します。

- **[!DNL Name]**：接続名。 接続を認識するために、わかりやすい名前を付けることをお勧めします。
- **[!DNL Login Name]**: ログイン名はExperience Platformの組織 ID です。 `ORG_ID@AdobeOrg` の形式です。
- **[!DNL Password]**：これは、[!DNL Query Service] 資格情報ダッシュボードで見つかった英数字の文字列です。
- **[!DNL Host and Port]**:[!DNL Query Service] のホストエンドポイントとそのポート。 [!DNL Query Service] に接続するには、ポート 80 を使用する必要があります。
- **[!DNL Database]:** 使用するデータベース。 Experience Platform UI 資格情報の `dbname` には `prod:all` の値を使用します。

### [!DNL Query Service] 資格情報

資格情報を見つけるには、[!DNL Experience Platform] UI にログインし、左側のナビゲーションから **[!UICONTROL クエリ]** を選択し、続いて **[!UICONTROL 資格情報]** を選択します。 ログイン資格情報、ホスト、ポートおよびデータベース名の検索に関する詳細な手順については、[ 資格情報ガイド ](../ui/credentials.md) を参照してください。

[!DNL Query Service] では、有効期限のない資格情報も提供され、サードパーティのクライアントとの 1 回限りのセットアップが可能になります。 詳しくは、ドキュメント [ 有効期限のない資格情報の生成と使用の方法に関する完全な手順 ](../ui/credentials.md#non-expiring-credentials) を参照してください。

### SSL モードの設定

次に、SSL モードの値を `?sslmode=require` に設定する必要があります。 これは、[!DNL Edit Server Properties] ダイアログの「[!DNL Driver]」タブから行います。 [ ドライバープロパティの編集 ](https://www.aquaclusters.com/app/home/project/public/aquadatastudio/wikibook/Documentation13/page/116/PostgreSQL#drivers) および [SSL の構成  [!DNL PostgreSQL]](https://www.aquaclusters.com/app/home/project/public/aquadatastudio/wikibook/Documentation20/page/SSL-Configuration/SSL-Configuration) の手順については、Aqua Data Studio の公式ドキュメントを参照してください。 検索バーを使用して、`sslmode` プロパティを検索します。

>[!IMPORTANT]
>
>Adobe Experience Platform クエリサービスへのサードパーティ接続での SSL サポートと、SSL モードを使用した接続方法については、[[!DNL Query Service] SSL ドキュメント ](./ssl-modes.md) を参照し `verify-full` ください。

接続の詳細を入力した後、同じタブから「**[!DNL Test Connection]**」を選択して、資格情報が正しく機能することを確認します。 接続テストが成功した場合は、「**[!DNL Save]**」を選択してサーバーを登録します。 接続を確認する確認ダイアログが表示され、ダッシュボードに接続アイコンが表示されます。 これで、サーバーに接続し、そのスキーマオブジェクトを表示できます。

## 次の手順

[!DNL Query Service] に接続したので、[!DNL Aqua Data Studio] 内の **[!DNL Query Analyzer]** を使用して SQL 文を実行および編集できます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。
