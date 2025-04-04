---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；Query Service;Db Visualizer;DbVisualizer;db Visulaizer；クエリサービスへの接続；
solution: Experience Platform
title: DbVisualizer のクエリサービスへの接続
description: ここでは、DbVisualizer をAdobe Experience Platform クエリサービスに接続する手順について説明します。
exl-id: badb0d89-1713-438c-8a9c-d1404051ff5f
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 3%

---

# [!DNL DbVisualizer] の [!DNL Query Service] への接続 {#connect-dbvisualizer}

このドキュメントでは、[!DNL DbVisualizer] データベースツールをAdobe Experience Platform [!DNL Query Service] に接続する手順を説明します。

## はじめに

このガイドでは、[!DNL DbVisualizer] Desktop アプリに既にアクセスしており、そのインターフェイスの操作方法に精通している必要があります。[!DNL DbVisualizer] デスクトップアプリケーションをダウンロードするには、または詳細情報については、[official [!DNL DbVisualizer] documentation](https://www.dbvis.com/download/) を参照してください。

[!DNL  DbVisualizer] をExperience Platformに接続するために必要な資格情報を取得するには、Experience Platform UI のクエリワークスペースにアクセスできる必要があります。 現在、クエリワークスペースにアクセスできない場合は、組織の管理者にお問い合わせください。

## データベース接続の作成 {#connect-database}

デスクトップアプリケーションをローカルマシンにインストールしたら、BDVisualizer の公式の手順に従って [ 新しいデータベース接続の作成 ](https://confluence.dbvis.com/display/UG130/Create+a+New+Database+Connection) を行います。

[!DNL Connections] リストから **[!DNL PostgreSQL]** を選択すると、新しい [!DNL PostgreSQL] 接続の [!DNL Object View] タブが表示されます。

### 接続のドライバ プロパティを設定する {#properties}

「[!DNL PostgreSQL] object view」タブで、「**[!DNL Properties]**」タブを選択し、次にナビゲーション・サイドバーから **[!DNL Driver Properties]** を選択します。 [ ドライバーのプロパティ ](https://confluence.dbvis.com/display/UG130/Configuring+Connection+Properties#ConfiguringConnectionProperties-DriverProperties) について詳しくは、公式ドキュメントを参照してください。

次に、以下の表で説明されているドライバのプロパティを入力します。

>[!IMPORTANT]
>
>DBVisualizer をAdobe Experience Platformに接続するには、SSL の使用を有効にする必要があります。 Adobe Experience Platform クエリサービスへのサードパーティ接続での SSL サポートと、SSL モードを使用した接続方法については、[SSL モードのドキュメント ](./ssl-modes.md) を参照し `verify-full` ください。

| プロパティ | 説明 |
| ------ | ------ |
| `PGHOST` | [!DNL PostgreSQL] サーバーのホスト名。 この値はExperience Platform **[!UICONTROL ホスト ] 資格情報** です。 |
| `ssl` | SSL 値 `1` を定義して、SSL の使用を有効にします。 |
| `sslmode` | SSL 保護のレベルを制御します。 サードパーティクライアントをAdobe Experience Platformに接続する場合は、`require` SSL モードを使用することをお勧めします。 `require` モードを使用すると、すべての通信で暗号化が必要になり、ネットワークが正しいサーバーに接続するために信頼されます。 サーバー SSL 証明書の検証は不要です。 |
| `user` | データベースに接続するユーザー名は組織 ID です。 `@Adobe.Org` で終わる英数字の文字列です。 この値はExperience Platformの **[!UICONTROL ユーザー名 ] 資格情報** です。 |

検索バーを使用して各プロパティを検索し、パラメーター値に対応するセルを選択します。 セルが青でハイライト表示されます。 Experience Platformの資格情報を「値」フィールドに入力し、「**[!DNL Apply]**」を選択してドライバープロパティを追加します。

>[!NOTE]
>
>2 つ目の `user` プロファイルを追加するには、パラメーター列で `user` を選択し、青い+（プラス）アイコンを選択して、各ユーザーの資格情報を追加します。 「**[!DNL Apply]**」を選択して、ドライバのプロパティを追加します。

[!DNL Edited] の列には、パラメーター値が更新されたことを示すチェックマークが表示されます。

### クエリサービス資格情報を入力 {#query-service-credentials}

BBVisualizer をクエリサービスに接続するために必要な資格情報を見つけるには、Experience Platform UI にログインし、左側のナビゲーションから **[!UICONTROL クエリ]** を選択し、続いて **[!UICONTROL 資格情報]** を選択します。 **host**、**port**、**database**、**username** および **password** 資格情報の検索について詳しくは、[ 資格情報ガイド ](../ui/credentials.md) を参照してください。

![ 資格情報と期限切れになる資格情報がハイライト表示されているExperience Platform クエリワークスペースの「資格情報」ページ ](../images/clients/dbvisualizer/query-service-credentials-page.png)

>[!IMPORTANT]
>
>[!DNL Query Service] では、有効期限のない資格情報も提供され、サードパーティのクライアントとの 1 回限りのセットアップが可能になります。 詳しくは、ドキュメント [ 有効期限のない資格情報の生成と使用の方法に関する完全な手順 ](../ui/credentials.md#non-expiring-credentials) を参照してください。 BDVisualizer を 1 回限りの設定として接続する場合は、このプロセスを完了する必要があります。 取得される `credential` と `technicalAccountId` の値は、DBVisualizer `password` パラメーターの値を構成します。

## 認証 {#authentication}

接続が確立されるたびにユーザー ID とパスワードベースの認証を要求するには、「[!DNL Properties]」タブに移動し、[!DNL PostgreSQL] の下のナビゲーションサイドバーから **[!DNL Authentication]** を選択します。

接続認証パネルで、「**[!DNL Require Userid]**」チェックボックスと「**[!DNL Require Password]**」チェックボックスの両方をオンにし、「**[!DNL Apply]**」を選択します。 [ 認証オプションの設定 ](https://confluence.dbvis.com/display/UG140/Setting+Common+Authentication+Options) について詳しくは、公式ドキュメントを参照してください。

## Experience Platformへの接続

有効期限のある資格情報や、有効期限のない資格情報を使用して接続を作成できます。 接続を確立するには、「オブジェクトの [!DNL PostgreSQL] 示」タブから「**[!DNL Connection]**」タブを選択し、次の設定のExperience Platform資格情報を入力します。 [ 手動接続の設定 ](https://confluence.dbvis.com/display/UG100/Setting+Up+a+Connection+Manually) を補完する手順は、DBVisualizer の公式 Web サイトで入手できます。

>[!NOTE]
>
>以下の表の BDVisualizer に必要なすべての資格情報は、パラメーターの説明に記載されていない限り、有効期限が切れる資格情報と有効期限が切れない資格情報で同じです。

| 接続パラメーター | 説明 |
|---|---|
| **[!UICONTROL 名前]** | 接続の名前を作成します。 接続を認識するために、人間にとってわかりやすい名前を指定することをお勧めします。 |
| **[!UICONTROL データベースサーバー]** | これはExperience Platform **[!UICONTROL ホスト]** 資格情報です。 |
| **[!UICONTROL データベースポート]** | [!DNL Query Service] のポート。 [!DNL Query Service] に接続するには、ポート **80** または **5432** を使用する必要があります。 |
| **[!UICONTROL データベース]** | Experience Platform **[!UICONTROL Database]** 資格情報の値 `prod:all` を使用します。 |
| **[!UICONTROL データベース ユーザー ID]** | これはExperience Platform組織 ID です。 Experience Platform **[!UICONTROL ユーザー名]** の資格情報の値を使用します。 ID は、`ORG_ID@AdobeOrg` の形式になります。 |
| **[!UICONTROL データベース パスワード]** | この英数字の文字列は、Experience Platform **[!UICONTROL パスワード]** 資格情報です。 有効期限のない認証情報を使用する場合、この値は `technicalAccountID` からの連結引数と、設定 JSON ファイルでダウンロードされた `credential` です。 パスワードの値は {technicalAccountId}:{credential} 形式で指定します。 有効期限のない資格情報用の設定 JSON ファイルは、Adobeがコピーを保持しない、初期化中の 1 回限りのダウンロードです。 |

関連するすべての資格情報を入力したら、「**[!DNL Connect]**」を選択します。

セッションの初回に [!DNL Connect] ダイアログが表示されます。 ユーザー ID とパスワードを入力し、「**[!DNL Connect]**」を選択します。 接続が成功したことを確認するメッセージがログに表示されます。

## 次の手順

[!DNL DbVisualizer] と [!DNL Query Service] を接続したので、[!DNL DbVisualizer] を使用してクエリを記述できます。 クエリの作成および実行方法について詳しくは、[ クエリの実行に関するガイド ](../best-practices/writing-queries.md) を参照してください。
