---
keywords: Experience Platform;home;popular topics;query service;Query service;query
solution: Experience Platform
title: Adobe Experience Platform クエリサービス UI ガイド
topic: guide
description: Adobe Experience Platform クエリサービスは、クエリの書き込みと実行、以前に実行したクエリの表示、IMS 組織内のユーザーが保存したクエリへのアクセスに使用できるユーザーインターフェイスを提供します。
translation-type: tm+mt
source-git-commit: c6c5ada52321b11543254f80399c38365f0fb9d7
workflow-type: tm+mt
source-wordcount: '617'
ht-degree: 61%

---


# [!DNL Query Service] ガイド

The Adobe Experience Platform [!DNL Query Service] provides a user interface that can be used to write and execute queries, view previously executed queries, and access queries saved by users within your IMS Organization. [Adobe Experience Platform][platform-ui] 内の UI にアクセスするには、左側のナビゲーションで「**[!UICONTROL クエリ]**」を選択します。

## [!DNL Query Editor]

The [!DNL Query Editor] enables you to write and execute queries without using an external client. Click **[!UICONTROL Create Query]** to open the [!DNL Query Editor] and create a new query. You can also access the [!DNL Query Editor] by selecting a query from the **[!UICONTROL Log]** or **[!UICONTROL Browse]** tabs. Selecting a previously executed or saved query will open the [!DNL Query Editor] and display the SQL for the selected query.

![画像](../images/queries/ui-overview/overview.png)

[!DNL Query Editor] クエリの入力を開始できる編集領域を提供します。 ユーザーが入力すると、エディターによって、テーブル内の SQL 予約語、テーブル、およびフィールド名が自動入力されます。When finished writing your query, click the **Play** button to run the query. The **[!UICONTROL Console]** tab below the editor shows what [!DNL Query Service] is currently doing, indicating when a query has been returned. コンソールの横にある「**[!UICONTROL 結果]**」タブには、クエリ結果が表示されます。See the [Query Editor guide][query-editor] for more information on using the [!DNL Query Editor].

![画像](../images/queries/ui-overview/query-editor.png)

## 参照

「**[!UICONTROL 参照]**」タブには、組織のユーザーによって保存されたクエリが表示されます。これらをクエリプロジェクトと考えると便利です。ここで保存したクエリは、まだ作成中の可能性があります。Queries displayed on the **[!UICONTROL Browse]** tab also display as run queries in the **[!UICONTROL Log]** tab if they have been previously executed by [!DNL Query Service].

![画像](../images/queries/ui-overview/browse.png)

| 列 | 説明 |
| --- | --- |
| 名前 | ユーザーが作成したクエリ名。You can click on the name to open the query in the [!DNL Query Editor]. 検索バーを使用して、クエリの名前で検索することもできます。検索では大文字と小文字が区別されます。 |
| SQL | SQL クエリの最初の数文字。コードの上にカーソルを置くと、完全なクエリが表示されます。 |
| 変更者 | 最後にクエリを変更したユーザー。Any user in your organization with access to [!DNL Query Service] can modify queries. |
| 最終変更日 | ブラウザーのタイムゾーンでの、クエリが最後に編集された日付と時間。 |

## ログ

「**[!UICONTROL ログ]**」タブには、以前に実行されたクエリのリストが表示されます。デフォルトでは、ログはクエリを逆年代順にリストします。

![画像](../images/queries/ui-overview/log.png)

| 列 | 説明 |
| --- | --- |
| **[!UICONTROL 名前]** | クエリ名。SQL クエリの最初の数文字で構成されます。Clicking on the name opens the [!DNL Query Editor], allowing you to edit the query. 検索バーを使用して、クエリの名前で検索できます。検索では大文字と小文字が区別されます。 |
| **[!UICONTROL 作成者]** | クエリを作成した人物の名前。 |
| **[!UICONTROL クライアント]** | クエリに使用されるクライアント。 |
| **[!UICONTROL データセット]** | クエリが使用する入力データセット。データセットをクリックして、入力データセットの詳細画面に移動します。 |
| **[!UICONTROL ステータス]** | クエリの現在の状態。 |
| **[!UICONTROL 前回の実行]** | 最後にクエリが実行された日時。この列の上にある矢印をクリックして、リストを昇順または降順で並べ替えることができます。 |
| **[!UICONTROL 実行時]** | クエリの実行に要した時間。 |

## 資格情報

「**[!UICONTROL 資格情報]**」タブには、 の資格情報が表示されます。[!DNL Postgres]任意のフィールド 横にある「**[!UICONTROL コピー]**」アイコンをクリックして、その内容をキーボードバッファに保存します。これらの資格情報を使用して外部クライアントと接続する方法の詳細については、[クライアントとの接続ガイド][connect-clients]を参照してください。

![画像](../images/queries/ui-overview/credentials.png)

## 次の手順

Now that you are familiar with [!DNL Query Service] user interface on [!DNL Platform], you can access [!DNL Query Editor] to start creating your own query projects to share with other users in your organization. For more information on authoring and running queries in [!DNL Query Editor], see the [Query Editor user guide][query-editor].

[platform-ui]: https://platform.adobe.com
[query-editor]: user-guide.md
[connect-clients]: ../clients/overview.md
