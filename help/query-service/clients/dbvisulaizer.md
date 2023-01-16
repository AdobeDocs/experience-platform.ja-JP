---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；Db Visualizer;Db Visualizer;db visualizer；クエリサービスへの接続；
solution: Experience Platform
title: DbVisualizer をクエリサービスに接続
description: このドキュメントでは、DbVisualizer とAdobe Experience Platform Query Service を接続する手順について説明します。
exl-id: badb0d89-1713-438c-8a9c-d1404051ff5f
source-git-commit: 106a2e4606e94f71d6359cf947e05f193c19c660
workflow-type: tm+mt
source-wordcount: '922'
ht-degree: 1%

---

# 接続 [!DNL DbVisualizer] から [!DNL Query Service] {#connect-dbvisualizer}

このドキュメントでは、 [!DNL DbVisualizer] Adobe Experience Platformのデータベースツール [!DNL Query Service].

## はじめに

このガイドでは、 [!DNL DbVisualizer] デスクトップアプリケーションのインターフェイスの操作方法を理解しているユーザーです。 次の手順で [!DNL DbVisualizer] デスクトップアプリケーションまたは詳しくは、 [公式 [!DNL DbVisualizer] ドキュメント](https://www.dbvis.com/download/).

接続に必要な資格情報を取得するには [!DNL  DbVisualizer] をExperience Platformするには、Platform UI の「クエリ」ワークスペースにアクセスできる必要があります。 現在クエリワークスペースへのアクセス権がない場合は、IMS 組織管理者に問い合わせてください。

## データベース接続の作成 {#connect-database}

デスクトップアプリケーションをローカルマシンにインストールしたら、BDVisualizer の公式手順に従って、 [新しいデータベース接続を作成する](https://confluence.dbvis.com/display/UG130/Create+a+New+Database+Connection).

次を選択したら、 **[!DNL PostgreSQL]** から [!DNL Connections] リスト、 [!DNL Object View] 新しい [!DNL PostgreSQL] 接続が表示されます。

### 接続のドライバのプロパティを設定する {#properties}

次の [!DNL PostgreSQL] 「オブジェクトビュー」タブで、 **[!DNL Properties]** タブに続いて、 **[!DNL Driver Properties]** ナビゲーションサイドバーから。 詳細情報： [ドライバーのプロパティ](https://confluence.dbvis.com/display/UG130/Configuring+Connection+Properties#ConfiguringConnectionProperties-DriverProperties) は公式ドキュメントに記載されています。

次に、以下の表に示すドライバのプロパティを入力します。

>[!IMPORTANT]
>
>DBVisualizer をAdobe Experience Platformに接続するには、SSL の使用を有効にする必要があります。 詳しくは、 [SSL モードのドキュメント](./ssl-modes.md) を参照して、Adobe Experience Platform Query Service へのサードパーティ接続の SSL サポートと、 `verify-full` SSL モード。

| プロパティ | 説明 |
| ------ | ------ |
| `PGHOST` | のホスト名 [!DNL PostgreSQL] サーバー。 この値はExperience Platform **[!UICONTROL ホスト] 資格情報**. |
| `ssl` | SSL 値の定義 `1` をクリックして、SSL を使用できるようにします。 |
| `sslmode` | SSL 保護のレベルを制御します。 次を使用することをお勧めします。 `require` サードパーティクライアントをAdobe Experience Platformに接続する際の SSL モード。 この `require` モードでは、すべての通信で暗号化が必要で、正しいサーバに接続するためにネットワークが信頼されていることを確認します。 サーバー SSL 証明書の検証は不要です。 |
| `user` | データベースに接続されるユーザー名は組織 ID です。 これは、で終わる英数字の文字列です。 `@Adobe.Org`. この値はExperience Platform **[!UICONTROL ユーザー名] 資格情報**. |

検索バーを使用して各プロパティを検索し、パラメーターの値に対応するセルを選択します。 セルが青でハイライト表示されます。 「値」フィールドに Platform の資格情報を入力し、「 」を選択します。 **[!DNL Apply]** をクリックして、driver プロパティを追加します。

>[!NOTE]
>
>秒を追加するには `user` プロファイル、選択 `user` 「パラメーター」列で、青い+（プラス）アイコンを選択して、各ユーザーの資格情報を追加します。 選択 **[!DNL Apply]** をクリックして、driver プロパティを追加します。

この [!DNL Edited] 列には、パラメータ値が更新されたことを示すチェックマークが表示されます。

### クエリサービスの資格情報を入力 {#query-service-credentials}

BBVisualizer をクエリサービスに接続するために必要な資格情報を見つけるには、Platform UI にログインし、「 」を選択します。 **[!UICONTROL クエリ]** 左のナビゲーションから、の後に **[!UICONTROL 資格情報]**. の検索方法の詳細 **ホスト**, **ポート**, **データベース**, **ユーザー名**、および **パスワード** 認証情報、お読みください [資格情報ガイド](../ui/credentials.md).

![「認証情報クエリ」ワークスペースの「Experience Platform」ページ（「認証情報」と「有効期限」がハイライト表示されています）。](../images/clients/dbvisualizer/query-service-credentials-page.png)

>[!IMPORTANT]
>
>[!DNL Query Service] また、は、期限切れでない資格情報を提供して、サードパーティクライアントとの 1 回限りのセットアップを可能にします。 詳しくは、 [有効期限のない資格情報の生成および使用方法に関する完全な手順](../ui/credentials.md#non-expiring-credentials). BDVisualizer を 1 回限りのセットアップとして接続する場合は、このプロセスを完了する必要があります。 この `credential` および `technicalAccountId` 取得した値が DBVisualizer の値を構成します `password` パラメーター。

## 認証 {#authentication}

接続が確立されるたびにユーザー ID とパスワードベースの認証を要求するには、 [!DNL Properties] 「 」タブで「 」を選択します。 **[!DNL Authentication]** 下のナビゲーションサイドバーから、 [!DNL PostgreSQL].

接続認証パネルで、 **[!DNL Require Userid]** および **[!DNL Require Password]** チェックボックスをオンにしてから、 **[!DNL Apply]**. 詳細情報： [認証オプションの設定](https://confluence.dbvis.com/display/UG140/Setting+Common+Authentication+Options) は、公式ドキュメントを参照してください。

##  を Platform に接続

接続は、有効期限切れまたは期限切れでない資格情報を使用しておこなえます。 接続するには、 **[!DNL Connection]** タブ [!DNL PostgreSQL] 「オブジェクト表示」タブに移動し、次の設定のExperience Platform資格情報を入力します。 補足的な指示 [手動接続の設定](https://confluence.dbvis.com/display/UG100/Setting+Up+a+Connection+Manually) 公式の DBVisualizer の Web サイトで入手できます。

>[!NOTE]
>
>以下の表に示す BDVisualizer に必要な資格情報は、パラメーターの説明に記載されていない限り、資格情報の有効期限と有効期限が切れない場合に同じです。

| 接続パラメーター | 説明 |
|---|---|
| **[!UICONTROL 名前]** | 接続の名前を作成します。 接続を認識する人間にわかりやすい名前を付けることをお勧めします。 |
| **[!UICONTROL データベースサーバ]** | これがExperience Platform **[!UICONTROL ホスト]** 資格情報。 |
| **[!UICONTROL データベースポート]** | のポート [!DNL Query Service]. ポートを使用する必要があります **80** または **5432** ～とつながる [!DNL Query Service]. |
| **[!UICONTROL データベース]** | Experience Platform **[!UICONTROL データベース]** 資格情報の値： `prod:all`. |
| **[!UICONTROL データベースユーザ ID]** | これは Platform 組織 ID です。 Experience Platform **[!UICONTROL ユーザー名]** 資格情報の値。 ID は、 `ORG_ID@AdobeOrg`. |
| **[!UICONTROL データベースのパスワード]** | この英数字の文字列はExperience Platform **[!UICONTROL パスワード]** 資格情報。 有効期限のない資格情報を使用する場合、この値は `technicalAccountID` そして `credential` 設定 JSON ファイルにダウンロードされました。 パスワードの値は次の形式で指定します。{technicalAccountId}:{credential}。 有効期限のない資格情報の設定 JSON ファイルは、Adobeがのコピーを保持しない、初期化中に 1 回限りのダウンロードです。 |

関連するすべての資格情報を入力したら、「 」を選択します。 **[!DNL Connect]**.

この [!DNL Connect] このセッションの最初の日にダイアログが表示されます。 ユーザー ID とパスワードを入力し、「 」を選択します。 **[!DNL Connect]**. 接続が成功したことを確認するメッセージがログに表示されます。

## 次の手順

これで、 [!DNL DbVisualizer] と [!DNL Query Service]を使用する場合、 [!DNL DbVisualizer] クエリを書き込みます。 クエリの書き込みと実行の方法について詳しくは、 [クエリ実行のガイド](../best-practices/writing-queries.md).
