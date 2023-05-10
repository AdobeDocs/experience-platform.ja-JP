---
keywords: Experience Platform；ホーム；人気の高いトピック；tableau;Tableau；クエリサービス；クエリサービス；クエリサービスへの接続；
solution: Experience Platform
title: Tableau のクエリーサービスへの接続
description: このドキュメントでは、Tableau とAdobe Experience Platform Query Service を接続する手順について説明します。
exl-id: f380aacd-5091-41bc-97ca-593e0b1670fd
source-git-commit: 668b2624b7a23b570a3869f87245009379e8257c
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 12%

---

# クエリサービスへの [!DNL Tableau] の接続

このドキュメントでは、 [!DNL Tableau] Adobe Experience Platform [!DNL Query Service].

>[!NOTE]
>
> このガイドは、ユーザーが既に [!DNL Tableau] インターフェイスの操作方法を熟知している。 詳細情報： [!DNL Tableau] は [公式 [!DNL Tableau] ドキュメント](https://help.tableau.com/current/pro/desktop/en-us/default.htm).

方法に関する説明 [Tableau を使用した PostgreSQL サーバへの接続](https://help.tableau.com/current/pro/desktop/en-us/examples_postgresql.htm) は、Tableau の公式 Web サイトから入手できます。 接続設定のダイアログが表示されたら、パラメーターフィールドに Platform の資格情報を入力して、Adobe Experience Platformに接続します。 必要な接続パラメータのリストを以下に示します。

| 接続パラメーター | 説明 |
|---|---|
| **[!DNL Server]** | SFTP ストレージの場所のアドレス。 Experience Platformの値を使用 **[!UICONTROL ホスト]** 資格情報。 |
| **[!DNL Port]:** | のポート [!DNL Query Service]. ポートを使用する必要があります **80** または **5432** ～とつながる [!DNL Query Service]. |
| **[!DNL Database]** | アクセスするデータベース。 Experience Platformの値を使用 **[!UICONTROL データベース]** 資格情報： `prod:all`. |
| **[!DNL Authentication]:** | ユーザー ID を証明するために選択した方法。 次を選択することをお勧めします。 [!DNL Username and Password] をクリックします。 |
| **[!DNL Username]** | これは Platform 組織 ID です。 Experience Platformの値を使用 **[!UICONTROL ユーザー名]** 資格情報。 ID は、 `ORG_ID@AdobeOrg`. |
| **[!DNL Password]** | この英数字の文字列はExperience Platform **[!UICONTROL パスワード]** 資格情報。 有効期限のない資格情報を使用する場合、この値は `technicalAccountID` そして `credential` 設定 JSON ファイルにダウンロードされました。 パスワードの値は次の形式で指定します。{technicalAccountId}:{credential}。 有効期限のない資格情報の設定 JSON ファイルは、Adobeがのコピーを保持しない、初期化中に 1 回限りのダウンロードです。 |

ユーザー名、パスワード、ログイン資格情報の検索について詳しくは、 [資格情報ガイド](../ui/credentials.md). 資格情報を検索するには、にログインします。 [!DNL Platform]を選択し、「 **[!UICONTROL クエリ]**&#x200B;に続いて **[!UICONTROL 資格情報]**.

必ず **[!UICONTROL SSL が必要]** 」ボックスをクリックして接続を試みます。 詳しくは、 [SSL モードのドキュメント](./ssl-modes.md) を参照して、Adobe Experience Platform Query Service へのサードパーティ接続の SSL サポートについて確認してください。

>[!IMPORTANT]
>
>ネストされたデータ構造をサードパーティの BI ツール用にフラット化して、より使いやすいデータ構造にし、データの取得、分析、変換およびレポートに必要なワークロードを軽減できます。データベースへの接続時にこの設定をアクティブにする方法については、[`FLATTEN`機能](../essential-concepts/flatten-nested-data.md)に関するドキュメントを参照してください。

すべての資格情報を入力した後、設定を確認して続行します。 Adobe Experience Platformとの連携が完了しました。

## 次の手順

これで、 [!DNL Query Service]を使用する場合、 [!DNL Tableau] クエリを書き込みます。 クエリの書き込みと実行の方法について詳しくは、 [クエリの実行](../best-practices/writing-queries.md).
