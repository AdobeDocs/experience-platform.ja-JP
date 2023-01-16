---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；postico;Postico；クエリサービスへの接続；
solution: Experience Platform
title: Postico をクエリサービスに接続
description: このドキュメントには、Adobe Experience Platform Query Service 用のバックアップクライアント Postico をインストールするためのリンクが含まれています。
exl-id: a19abfc8-b431-4e57-b44d-c6130041af4a
source-git-commit: 9fe7e618d251867c90c88f8bee6ef5863ae78f60
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 4%

---

# 接続 [!DNL Postico] クエリサービス (Mac) へ

このドキュメントでは、 [!DNL Postico] Adobe Experience Platform [!DNL Query Service].

>[!NOTE]
>
> このガイドは、ユーザーが既に [!DNL Postico] インターフェイスの操作方法を熟知している。 詳細情報： [!DNL Postico] は [公式 [!DNL Postico] ドキュメント](https://eggerapps.at/postico/docs).
> 
> また、 [!DNL Postico] が **のみ** はmacOSデバイスで使用できます。

接続するには [!DNL Postico] クエリサービスにアクセスするには、 [!DNL Postico] を選択し、 **[!DNL New Favorite]**. 接続設定のダイアログが表示されます。 ここから、Adobe Experience Platformに接続するためのパラメーター値を入力できます。 以下に示す接続設定を入力します。 方法に関する説明 [Postico を使用した PostgreSQL サーバーへの接続](https://eggerapps.at/postico/docs/v1.5.21/favorite-window.html) また、ポスティコの公式ウェブサイトからもご利用いただけます。

| 接続パラメーター | 説明 |
|---|---|
| **[!DNL Host]:** | PostgreSQL サーバーのホスト名。 |
| **[!DNL Port]:** | のポート [!DNL Query Service]. ポートを使用する必要があります **80** または **5432** ～とつながる [!DNL Query Service]. |
| **[!DNL User]** | 特定の接続の名前を作成します。 このフィールドを空白のままにして、Macのログイン名を使用します。 |
| **[!DNL Password]** | この英数字の文字列はExperience Platform **[!UICONTROL パスワード]** 資格情報。 有効期限のない資格情報を使用する場合、この値は `technicalAccountID` そして `credential` 設定 JSON ファイルにダウンロードされました。 パスワードの値は次の形式で指定します。{technicalAccountId}:{credential}。 有効期限のない資格情報の設定 JSON ファイルは、Adobeがのコピーを保持しない、初期化中に 1 回限りのダウンロードです。 |
| **[!DNL Database]** | Experience Platform **[!UICONTROL データベース]** 資格情報の値： `prod:all`. |

データベース名、ホスト、ポート、ログイン資格情報の検索の詳細については、 [資格情報ガイド](../ui/credentials.md). 資格情報を検索するには、にログインします。 [!DNL Platform]を選択し、「 **[!UICONTROL クエリ]**&#x200B;に続いて **[!UICONTROL 資格情報]**.

資格情報を挿入したら、「 」を選択します。 **[!DNL Connect]** をクリックして、クエリサービスに接続します。

Platform に接続すると、以前にクエリサービスでおこなわれたすべての関係のリストを表示できます。

## SQL 文の作成

新しい SQL クエリを作成するには、 **[!DNL SQL Query]** サイドバーから。 または、キーボードショートカット (⇧ ⌘ T) を使用してクエリビューに移動し、実行するクエリを入力します。 終了したら、「 」を選択します。 **[!DNL Execute Statement]** をクリックしてクエリを実行します。 完了したクエリの実行結果が表に表示されます。

詳しくは、ポスティコの公式ドキュメントを参照してください。 [クエリビューの使用](https://eggerapps.at/postico/docs/v1.3.1/sql-query-view.html).

## 次の手順

これで、 [!DNL Query Service]を使用する場合、 [!DNL Postico] クエリを書き込みます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。
