---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；クエリサービス；Aqua Data Studio;Aqua Data Studio；クエリサービスへの接続；
solution: Experience Platform
title: Aqua Data Studio をクエリサービスに接続
description: このドキュメントでは、Aqua Data Studio と Adobe Experience Platform クエリサービスを接続する手順について説明します。
exl-id: 4770e221-48a7-45d8-80a4-60b5cbc0ec33
source-git-commit: fcd44aef026c1049ccdfe5896e6199d32b4d1114
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 11%

---

# クエリサービスへの [!DNL Aqua Data Studio] の接続

このドキュメントでは、 [!DNL Aqua Data Studio] Adobe Experience Platform [!DNL Query Service].

## はじめに

このガイドでは、に対するアクセス権を既に持っている必要があります [!DNL Aqua Data Studio] インターフェイスの操作方法を理解している必要があります。 詳細情報： [!DNL Aqua Data Studio] は [公式 [!DNL Aqua Data Studio] ドキュメント](https://www.aquaclusters.com/app/home/project/public/aquadatastudio/wikibook/Documentation21.1/page/0/Aqua-Data-Studio-21-1).

>[!NOTE]
>
>次のものがあります。 [!DNL Windows] および [!DNL macOS] のバージョン [!DNL Aqua Data Studio]. このガイドのスクリーンショットは、 [!DNL macOS] デスクトップアプリケーション。 UI のバージョン間で若干の相違が生じる場合があります。

[!DNL Aqua Data Studio] を Experience Platform に接続するために必要な資格情報を取得するには、Platform UI のクエリワークスペースにアクセスできる必要があります。現在、 [!UICONTROL クエリ] ワークスペース。

## サーバーを登録 {#register-server}

インストール後 [!DNL Aqua Data Studio]を使用する場合、まずサーバーを登録する必要があります。 方法については、公式の Aqua Data Studio ドキュメントを参照してください。 [を起動します。 [!DNL Register Server] ダイアログ](https://www.aquaclusters.com/app/home/project/public/aquadatastudio/wikibook/Documentation18/page/81/Registering-a-Database-Server#launching_the_register_server_dialog) および [サーバーの登録](https://www.aquaclusters.com/app/home/project/public/aquadatastudio/wikibook/Documentation18/page/81/Registering-a-Database-Server#steps_to_register_a_server_in_aqua_data_studio).

一度 **[!DNL Register Server]** PostgresSQL サーバ用のダイアログが表示され、サーバ設定の次の詳細を指定します。

- **[!DNL Name]**:接続の名前。 接続を認識するわかりやすい名前を付けることをお勧めします。
- **[!DNL Login Name]**:ログイン名は、Platform 組織 ID です。 次の形式を取ります。 `ORG_ID@AdobeOrg`.
- **[!DNL Password]**:これは、 [!DNL Query Service] 認証情報ダッシュボード。
- **[!DNL Host and Port]**:ホストエンドポイントと、そののポート [!DNL Query Service]. 接続には、ポート 80 を使用する必要があります [!DNL Query Service].
- **[!DNL Database]:** 使用するデータベース。 Platform UI の資格情報の値を使用します。 `dbname`: `prod:all`.

### [!DNL Query Service] 資格情報

資格情報を確認するには、にログインします。 [!DNL Platform] UI とを選択します。 **[!UICONTROL クエリ]** 左のナビゲーションから、の後に **[!UICONTROL 資格情報]**. ログイン資格情報、ホスト、ポート、データベース名の確認方法については、 [資格情報ガイド](../ui/credentials.md).

[!DNL Query Service] また、は、期限切れでない資格情報を提供して、サードパーティクライアントとの 1 回限りのセットアップを可能にします。 詳しくは、 [有効期限のない資格情報の生成および使用方法に関する完全な手順](../ui/credentials.md#non-expiring-credentials).

### SSL モードの設定

次に、SSL モードの値を `?sslmode=require`. これは、 [!DNL Driver] タブ [!DNL Edit Server Properties] ダイアログ。 方法については、公式の Aqua Data Studio ドキュメントを参照してください。 [ドライバのプロパティを編集](https://www.aquaclusters.com/app/home/project/public/aquadatastudio/wikibook/Documentation13/page/116/PostgreSQL#drivers) および [用の SSL の設定 [!DNL PostgreSQL]](https://www.aquaclusters.com/app/home/project/public/aquadatastudio/wikibook/Documentation20/page/SSL-Configuration/SSL-Configuration). 検索バーを使用して `sslmode` プロパティ。

>[!IMPORTANT]
>
>詳しくは、 [[!DNL Query Service] SSL ドキュメント](./ssl-modes.md) を参照して、Adobe Experience Platform Query Service へのサードパーティ接続の SSL サポートと、 `verify-full` SSL モード。

接続の詳細を入力したら、同じタブで、「 」を選択します。 **[!DNL Test Connection]** 認証情報が正しく機能するようにします。 接続テストが成功した場合は、 **[!DNL Save]** サーバーを登録します。 接続を確認する確認ダイアログが表示され、接続アイコンがダッシュボードに表示されます。 これで、サーバーに接続し、そのスキーマオブジェクトを表示できます。

## 次の手順

これで、 [!DNL Query Service]を使用する場合、 **[!DNL Query Analyzer]** 範囲 [!DNL Aqua Data Studio] SQL 文を実行および編集する場合。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。
