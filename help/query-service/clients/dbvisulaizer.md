---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；Db Visualizer;Db Visualizer;db visualizer；クエリサービスへの接続；
solution: Experience Platform
title: DbVisualizer をクエリサービスに接続
topic-legacy: connect
description: このドキュメントでは、DbVisualizer とAdobe Experience Platform Query Service を接続する手順について説明します。
source-git-commit: 69e105b2c52a668ba708847795d4c92813aad0db
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---

# 接続 [!DNL DbVisualizer] から [!DNL Query Service] {#connect-dbvisualizer}

このドキュメントでは、 [!DNL DbVisualizer] Adobe Experience Platformのデータベースツール [!DNL Query Service].

## はじめに

このガイドでは、 [!DNL DbVisualizer] デスクトップアプリケーションのインターフェイスの操作方法を理解しているユーザーです。 次の手順で [!DNL DbVisualizer] デスクトップアプリケーションまたは詳しくは、 [公式 [!DNL DbVisualizer] ドキュメント](https://www.dbvis.com/download/).

>[!NOTE]
>
>次のものがあります。 [!DNL Windows], [!DNL macOS]、および [!DNL Linux] のバージョン [!DNL DbVisualizer]. このガイドのスクリーンショットは、 [!DNL macOS] デスクトップアプリケーション。 UI のバージョン間で若干の相違が生じる場合があります。

接続に必要な資格情報を取得するには [!DNL  DbVisualizer] をExperience Platformするには、Platform UI の「クエリ」ワークスペースにアクセスできる必要があります。 現在クエリワークスペースへのアクセス権がない場合は、IMS 組織管理者に問い合わせてください。

## データベース接続の作成 {#connect-database}

ローカルマシンにデスクトップアプリケーションをインストールしたら、デスクトップアプリケーションを起動し、「 」を選択します。 **[!DNL Create a Database Connection]** 最初の [!DNL DbVisualizer] メニュー 次に、 **[!DNL Create a Connection]** を右側のパネルに追加します。

![この [!DNL DbVisualizer] 「データベース接続の作成」がハイライトされたメインメニュー](../images/clients/dbvisualizer/create-db-connection.png)

検索バーを使用するか、「 」を選択します。 [!DNL PostgreSQL] を選択します。 「データベース接続」ワークスペースが表示されます。

![ドライバ名のドロップダウンメニュー [!DNL PostgreSQL] ハイライト表示されました。](../images/clients/dbvisualizer/driver-name.png)

「データベース接続」ワークスペースで、 **[!DNL Properties]** タブに続いて、 **[!DNL Driver Properties]** ナビゲーションサイドバーから。

![「プロパティ」タブがハイライト表示された「データベース接続」ワークスペース。](../images/clients/dbvisualizer/driver-properties.png)

次の表に、3 つの必要なドライバのプロパティを示します。

| プロパティ | 説明 |
| ------ | ------ |
| `PGHOST` | のホスト名 [!DNL PostgreSQL] サーバー。 この値はExperience Platform [!UICONTROL ホスト] 資格情報。 |
| `SSL` | SSL 要件の使用を制御します。 あなた **必須** 値「1」を使用して、この要件を有効にします。 |
| `user` | データベースに接続されているユーザー名は組織 ID です。 これは、で終わる英数字の文字列です。 `@adobe.org` |

### [!DNL Query Service] 資格情報

この `PGHOST` および `user` の値はAdobe Experience Platformの資格情報から取得されます。 資格情報を見つけるには、Platform UI にログインし、「 」を選択します。 **[!UICONTROL クエリ]** 左のナビゲーションから、の後に **[!UICONTROL 資格情報]**. データベース名、ホスト、ポート、ログイン資格情報の検索の詳細については、 [資格情報ガイド](../ui/credentials.md).

![Experience Platformクエリ資格情報ダッシュボード（認証情報付き）がハイライトされています。](../images/clients/dbvisualizer/query-service-credentials-page.png)

[!DNL Query Service] また、は、期限切れでない資格情報を提供して、サードパーティクライアントとの 1 回限りのセットアップを可能にします。 詳しくは、 [有効期限のない資格情報の生成および使用方法に関する完全な手順](../ui/credentials.md#non-expiring-credentials).

検索バーを使用して各プロパティを検索し、パラメーターの値に対応するセルを選択します。 セルが青でハイライト表示されます。 「値」フィールドに Platform の資格情報を入力し、「 」を選択します。 **[!DNL Apply]** をクリックして、driver プロパティを追加します。

>[!NOTE]
>
>秒を追加するには `user` プロファイル、選択 `user` 「パラメーター」列で、青い+（プラス）アイコンを選択して、各ユーザーの資格情報を追加します。 選択 **[!DNL Apply]** をクリックして、driver プロパティを追加します。

この [!DNL Edited] 列には、パラメータ値が更新されたことを示すチェックマークが表示されます。

## 認証

接続が確立されるたびにユーザー ID とパスワードベースの認証を要求するには、 **[!DNL Authentication]** 下のナビゲーションサイドバーから、 [!DNL PostgreSQL].

接続認証パネルで、 **[!DNL Require Userid]** および **[!DNL Require Password]** チェックボックスをオンにしてから、 **[!DNL Apply]**.

![「 Userid 」チェックボックスと「 Password 」チェックボックスがハイライトされた「 Connection Authentication 」パネル。](../images/clients/dbvisualizer/connection-authentication.png)

## Platform に接続

接続するには、 **[!DNL Connection]** 」タブをクリックし、次の設定のExperience Platform資格情報を入力します。

- **名前**:接続を認識するわかりやすい名前を付けることをお勧めします。
- **データベースサーバ**:これがExperience Platform [!UICONTROL ホスト] 資格情報。
- **データベースポート**:のポート [!DNL Query Service]. 接続には、ポート 80 を使用する必要があります [!DNL Query Service].
- **データベース**:秘密鍵証明書の使用 `dbname` 値 `prod:all`.
- **データベースユーザ ID**:これは、Platform 組織 ID です。 ユーザー ID の形式は次のとおりです。 `ORG_ID@AdobeOrg`.
- **データベースのパスワード**:これは、 [!DNL Query Service] 認証情報ダッシュボード。

関連するすべての資格情報を入力したら、「 」を選択します。 **[!DNL Connect]**.

![[ 接続 ] タブと [ 接続 ] ボタンがハイライト表示された [ データベース接続 ] ワークスペース](../images/clients/dbvisualizer/connect.png)

この [!DNL Connect] このセッションの最初の日にダイアログが表示されます。

![データベースの userid およびデータベースのパスワードテキストフィールドを含む [ 接続 ] ダイアログがハイライト表示されます。](../images/clients/dbvisualizer/connect-dialog.png)

ユーザー ID とパスワードを入力し、「 」を選択します。 **[!DNL Connect]**. 接続が成功したことを確認するメッセージがログに表示されます。

## 次の手順

これで、 [!DNL DbVisualizer] と [!DNL Query Service]を使用する場合、 [!DNL DbVisualizer] クエリを書き込みます。 クエリの書き込みと実行の方法について詳しくは、 [クエリ実行のガイド](../best-practices/writing-queries.md).
