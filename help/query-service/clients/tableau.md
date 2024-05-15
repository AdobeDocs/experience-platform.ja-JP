---
keywords: Experience Platform；ホーム；人気のトピック；tableau;Tableau；クエリサービス；クエリサービスへの接続；
solution: Experience Platform
title: Tableau のクエリサービスへの接続
description: このドキュメントでは、Tableau とAdobe Experience Platform クエリサービスを接続する手順について説明します。
exl-id: f380aacd-5091-41bc-97ca-593e0b1670fd
source-git-commit: 26f0725f0f239707bd719ed46929648f8d557155
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 10%

---

# クエリサービスへの [!DNL Tableau] の接続

このドキュメントでは、の接続について説明します [!DNL Tableau] （Adobe Experience Platformを使用） [!DNL Query Service].

>[!NOTE]
>
> このガイドでは、以下へのアクセス権が既にあることを前提としています。 [!DNL Tableau] のインターフェイスの操作方法に精通している。 に関する詳細情報 [!DNL Tableau] 次の場所にあります。 [公用 [!DNL Tableau] 詳細を見る](https://help.tableau.com/current/pro/desktop/en-us/default.htm).

方法の説明 [tableau を使用した PostgreSQL サーバーへの接続](https://help.tableau.com/current/pro/desktop/en-us/examples_postgresql.htm) 公式 Tableau Web サイトから入手できます。 接続設定のダイアログが表示されたら、Adobe Experience Platformに接続するためのパラメーターフィールドに Platform の資格情報を入力します。 必要な接続パラメーターのリストを以下に示します。

| 接続パラメーター | 説明 |
|---|---|
| **[!DNL Server]** | SFTP ストレージの場所のアドレス。 Experience Platformの値を使用 **[!UICONTROL ホスト]** 認証情報。 |
| **[!DNL Port]:** | のポート [!DNL Query Service]. ポートを使用してください **80** または **5432** を接続する [!DNL Query Service]. |
| **[!DNL Database]** | アクセスするデータベース。 Experience Platformの値を使用 **[!UICONTROL データベース]** 資格情報： `prod:all`. |
| **[!DNL Authentication]:** | ユーザー ID を証明する方法を選択します。 を選択することをお勧めします。 [!DNL Username and Password] ドロップダウンメニューの使用可能なオプションから。 |
| **[!DNL Username]** | これは Platform 組織 ID です。 Experience Platformの値を使用 **[!UICONTROL ユーザー名]** 認証情報。 ID は、次の形式になります `ORG_ID@AdobeOrg`. |
| **[!DNL Password]** | この英数字の文字列はExperience Platformです **[!UICONTROL パスワード]** 認証情報。 有効期限のない認証情報を使用する場合、この値はからの連結引数です。 `technicalAccountID` および `credential` 設定の JSON ファイルにダウンロードされます。 パスワードの値は次の形式で指定します。 {technicalAccountId}:{credential}. 有効期限のない資格情報用の設定 JSON ファイルは、Adobeがコピーを保持しない、初期化中の 1 回限りのダウンロードです。 |

ユーザー名、パスワード、ログイン資格情報の検索について詳しくは、を参照してください。 [資格情報ガイド](../ui/credentials.md). 資格情報を見つけるには、にログインします。 [!DNL Platform]を選択してから、 **[!UICONTROL クエリ]**、続いて **[!UICONTROL 資格情報]**.

>[!IMPORTANT]
>
>Tableau またはPower BIユーザーは、「クエリサービス資格情報」タブからCustomer Journey Analyticsを BI ツールに接続できます。 の方法については、資格情報ドキュメントを参照してください。 [bi ツールをCustomer Journey Analyticsに接続](../ui/credentials.md#connect-to-customer-journey-analytics).

「」がオンになっていることを確認します **[!UICONTROL SSL を要求]** 接続を試みる前にボックスにチェックを入れます。 を参照してください。 [SSL モードのドキュメント](./ssl-modes.md) Adobe Experience Platform クエリサービスへのサードパーティ接続の SSL サポートについて説明します。

>[!IMPORTANT]
>
>ネストされたデータ構造をサードパーティの BI ツール用にフラット化して、より使いやすいデータ構造にし、データの取得、分析、変換およびレポートに必要なワークロードを軽減できます。データベースへの接続時にこの設定をアクティブにする方法については、[`FLATTEN`機能](../key-concepts/flatten-nested-data.md)に関するドキュメントを参照してください。

すべての資格情報を入力したら、設定を確認して続行します。 これで、Adobe Experience Platformと接続しました。

## 次の手順

これで、と接続できました [!DNL Query Service]、以下を使用できます [!DNL Tableau] クエリを書き込みます。 クエリの作成および実行方法について詳しくは、のガイドを参照してください。 [クエリの実行](../best-practices/writing-queries.md).
