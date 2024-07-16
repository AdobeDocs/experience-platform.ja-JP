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

このドキュメントでは、[!DNL Tableau] とAdobe Experience Platform [!DNL Query Service] を接続する方法について説明します。

>[!NOTE]
>
> このガイドは、[!DNL Tableau] へのアクセス権を既に持ち、そのインターフェイスの操作方法に精通していることを前提としています。 [!DNL Tableau] について詳しくは、[official [!DNL Tableau] documentation](https://help.tableau.com/current/pro/desktop/en-us/default.htm) を参照してください。

[Tableau を使用して PostgreSQL サーバーに接続する ](https://help.tableau.com/current/pro/desktop/en-us/examples_postgresql.htm) 方法については、Tableau の公式 Web サイトで説明しています。 接続設定のダイアログが表示されたら、Adobe Experience Platformに接続するためのパラメーターフィールドに Platform の資格情報を入力します。 必要な接続パラメーターのリストを以下に示します。

| 接続パラメーター | 説明 |
|---|---|
| **[!DNL Server]** | SFTP ストレージの場所のアドレス。 Experience Platformの **[!UICONTROL ホスト]** 資格情報の値を使用します。 |
| **[!DNL Port]:** | [!DNL Query Service] のポート。 [!DNL Query Service] に接続するには、ポート **80** または **5432** を使用する必要があります。 |
| **[!DNL Database]** | アクセスするデータベース。 Experience Platform **[!UICONTROL データベース]** 資格情報の値を使用します：`prod:all`。 |
| **[!DNL Authentication]:** | ユーザー ID を証明する方法を選択します。 ドロップダウンメニューの使用可能 [!DNL Username and Password] オプションから選択することをお勧めします。 |
| **[!DNL Username]** | これは Platform 組織 ID です。 Experience Platformの **[!UICONTROL ユーザー名]** 資格情報の値を使用します。 ID は、`ORG_ID@AdobeOrg` の形式になります。 |
| **[!DNL Password]** | この英数字の文字列は、Experience Platformの **[!UICONTROL パスワード]** 資格情報です。 有効期限のない認証情報を使用する場合、この値は `technicalAccountID` からの連結引数と、設定 JSON ファイルでダウンロードされた `credential` です。 パスワードの値は {technicalAccountId}:{credential} 形式で指定します。 有効期限のない資格情報用の設定 JSON ファイルは、Adobeがコピーを保持しない、初期化中の 1 回限りのダウンロードです。 |

ユーザー名、パスワード、ログイン資格情報の検索について詳しくは、[ 資格情報ガイド ](../ui/credentials.md) を参照してください。 資格情報を見つけるには、[!DNL Platform] にログインし、「**[!UICONTROL クエリ]**」を選択してから、「**[!UICONTROL 資格情報]** を選択します。

>[!IMPORTANT]
>
>Tableau またはPower BIユーザーは、「クエリサービス資格情報」タブからCustomer Journey Analyticsを BI ツールに接続できます。 [BI ツールをCustomer Journey Analyticsに接続 ](../ui/credentials.md#connect-to-customer-journey-analytics) する手順については、資格情報ドキュメントを参照してください。

接続する前に、「**[!UICONTROL SSL が必要]** ボックスがオンになっていることを確認します。 Adobe Experience Platform クエリサービスへのサードパーティ接続での SSL サポートについては、[SSL モードのドキュメント ](./ssl-modes.md) を参照してください。

>[!IMPORTANT]
>
>ネストされたデータ構造をサードパーティの BI ツール用にフラット化して、より使いやすいデータ構造にし、データの取得、分析、変換およびレポートに必要なワークロードを軽減できます。データベースへの接続時にこの設定をアクティブにする方法については、[`FLATTEN`機能](../key-concepts/flatten-nested-data.md)に関するドキュメントを参照してください。

すべての資格情報を入力したら、設定を確認して続行します。 これで、Adobe Experience Platformと接続しました。

## 次の手順

[!DNL Query Service] に接続したので、[!DNL Tableau] を使用してクエリを記述できます。 クエリの作成および実行方法について詳しくは、[ クエリの実行 ](../best-practices/writing-queries.md) に関するガイドを参照してください。
