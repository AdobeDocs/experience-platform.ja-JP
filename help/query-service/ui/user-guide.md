---
keywords: Experience Platform;home;popular topics;Query editor;query editor;Query service;query service;
solution: Experience Platform
title: クエリエディターユーザガイド
topic: query editor
description: クエリエディターは、Adobe Experience Platform クエリサービスが提供するインタラクティブなツールで、Experience Platform ユーザーインターフェイス内で顧客体験データのクエリを記述、検証および実行できます。クエリエディターでは、分析およびデータ調査のためのクエリを開発できます。また、開発目的でインタラクティブクエリを実行できるほか、非インタラクティブクエリを実行して Experience Platform のデータセットに入力することもできます。
translation-type: tm+mt
source-git-commit: 9bd893820c7ab60bf234456fdd110fb2fbe6697c
workflow-type: tm+mt
source-wordcount: '1086'
ht-degree: 58%

---


# [!DNL Query Editor] ユーザーガイド

[!DNL Query Editor] はAdobe Experience Platformが提供するインタラクティブなツール [!DNL Query Service][!DNL Experience Platform] です。ユーザーインターフェイス内で顧客体験データのクエリを作成、検証および実行できます。 [!DNL Query Editor] は、分析およびデータ探索用の開発クエリをサポートし、開発目的でインタラクティブクエリを実行できるほか、でデータセットを設定する非インタラクティブクエリも実行でき [!DNL Experience Platform]ます。

For more information about the concepts and features of [!DNL Query Service], see the [Query Service overview][query-service-overview]. To learn more about how to navigate the Query Service user interface on [!DNL Platform], see the [Query Service UI overview][query-service-ui].

## はじめに

[!DNL Query Editor] に接続することでクエリを柔軟に実行でき [!DNL Query Service]ます。クエリはこの接続がアクティブな間にのみ実行されます。

### 接続先 [!DNL Query Service]

[!DNL Query Editor] が初期化され、開かれたときに接続され [!DNL Query Service] るまでに数秒かかります。 クエリサービスに接続されると、コンソールに示されます（以下を参照）。エディターがクエリサービスに接続される前にクエリを実行しようとすると、接続が完了するまで実行が待機されます。

![画像](../images/queries/query-editor-overview/initializing-connection.png)

### クエリの実行元 [!DNL Query Editor]

対話的に [!DNL Query Editor] 実行されたクエリ。 つまり、ブラウザーを閉じたり、別の場所に移動したりすると、クエリはキャンセルされます。また、クエリ出力からデータセットを生成するために実行するクエリも同様です。

## クエリオーサリング [!DNL Query Editor]

Using [!DNL Query Editor], you can write, execute, and save queries for customer experience data. All queries executed in [!DNL Query Editor], or saved, are available to all users in your organization with access to [!DNL Query Service].

### [!DNL Query Editor] へのアクセス

In the [!DNL Experience Platform] UI, click **[!UICONTROL Queries]** in the left navigation menu to open the [!DNL Query Service] workspace. 次に、画面の右上にある「**[!UICONTROL クエリを作成]**」をクリックして、クエリの記述を開始します。This link is available from any of the pages in the [!DNL Query Service] workspace.

![画像](../images/queries/query-editor-overview/create-query.png)

### クエリの記述

[!UICONTROL クエリエディターは、クエリをできるだけ簡単に記述できるように構成されています。]次のスクリーンショットは、UI でエディターがどのように表示されるかを示しています。ここでは、**プレイする**&#x200B;ボタンと SQL 入力フィールドがハイライトされています。

![画像](../images/queries/query-editor-overview/editor.png)

開発時間を最小限に抑えるために、返される行を制限してクエリを開発することをお勧めします。たとえば、`SELECT fields FROM table WHERE conditions LIMIT number_of_rows` のように設定します。クエリが目的どおりの出力を生成することを確認したら、制限を解除して、`CREATE TABLE tablename AS SELECT` と設定してクエリを実行し、データセットを生成します。

### ツールの作成 [!DNL Query Editor]

- **構文の自動ハイライト：** SQL の読み取りと構成が容易になります。

![画像](../images/queries/query-editor-overview/syntax-highlight.png)

- **SQL キーワードのオートコンプリート：**&#x200B;クエリの入力を開始し、矢印キーを使用して目的の用語に移動して、**Enter** キーを押します。

![画像](../images/queries/query-editor-overview/syntax-auto.png)

- **テーブルとフィールドのオートコンプリート**：`SELECT` 元のテーブル名の入力を開始し、矢印キーを使用して目的の表に移動して、**Enter** キーを押します。テーブルを選択すると、オートコンプリートによってそのテーブルのフィールドが認識されます。

![画像](../images/queries/query-editor-overview/tables-auto.png)

### エラーの検出

[!DNL Query Editor] クエリの書き込み時に自動的に検証され、汎用のSQL検証と特定の実行検証が提供されます。 以下の画像のように、クエリの下に赤い下線が表示される場合は、クエリ内にエラーがあります。

![画像](../images/queries/query-editor-overview/syntax-error-highlight.png)

エラーが検出された場合、SQL コードの上にカーソルを置くと、特定のエラーメッセージが表示されます。

![画像](../images/queries/query-editor-overview/linting-error.png)

### クエリの詳細

While you are viewing a query in [!DNL Query Editor], the **[!UICONTROL Query Details]** panel provides tools to manage the selected query.

![画像](../images/queries/query-editor-overview/query-details.png)

このパネルでは、UI から出力データセットを直接生成したり、表示されたクエリを削除したり、クエリに名前を付けたりすることができます。また、「**[!UICONTROL SQL クエリ]**」タブには、SQL コードが簡単にコピーできる形式で表示されます。このパネルには、クエリの最終変更日時や最終変更者（該当する場合）などの有効なメタデータも表示されます。データセットを生成するには、「**[!UICONTROL データセットを出力]**」をクリックします。**[!UICONTROL データセットを出力]**&#x200B;ダイアログが表示されます。名前と説明を入力し、「**[!UICONTROL クエリを実行]**」をクリックします。The new dataset is displayed in the **[!UICONTROL Datasets]** tab on the [!DNL Query Service] user interface on [!DNL Platform].

### クエリの保存

[!DNL Query Editor] には、クエリを保存して後で操作できる保存関数があります。 To save a query, click **[!UICONTROL Save]** in the top right corner of [!DNL Query Editor]. クエリを保存する前に、**[!UICONTROL クエリの詳細]**&#x200B;パネルを使用してクエリに名前を付ける必要があります。

### 以前のクエリを検索する方法

All queries executed from [!DNL Query Editor] are captured in the Log table. 「**[!UICONTROL ログ]**」タブの検索機能を使用して、クエリの実行を検索できます。保存したクエリは「**[!UICONTROL 参照]**」タブに表 示されます。

詳しくは、[クエリサービス UI の概要][query-service-ui]を参照してください。

>[!NOTE]
>
>実行されなかったクエリは「ログ」に保存されません。In order for the query to be available in [!DNL Query Service], it must be run or saved in [!DNL Query Editor].

## クエリエディターを使用してクエリを実行する

To run a query in [!DNL Query Editor], you can enter SQL in the editor or load a previous query from the **[!UICONTROL Log]** or **[!UICONTROL Browse]** tab, and click **Play**. クエリ実行のステータスは下の「**[!UICONTROL コンソール]**」タブに表示され、出力データは「**[!UICONTROL 結果]**」タブに表示されます。

### コンソール

The console provides information on the status and operation of [!DNL Query Service]. The console displays the connection status to [!DNL Query Service], query operations being executed, and any error messages that result from those queries.

![画像](../images/queries/query-editor-overview/console.png)

>[!NOTE]
>
>コンソールには、クエリの実行時に発生したエラーのみが表示されます。クエリを実行する前に、クエリ検証エラーは表示されません。

### クエリの結果

クエリが完了すると、結果が「**[!UICONTROL コンソール]**」タブの横の「**[!UICONTROL 結果]**」タブに表示されます。このビューには、クエリの出力が表形式で出力され、最大 100 行まで表示されます。このビューを使用すると、クエリが目的どおりの出力を生成することを確認できます。クエリでデータセットを生成するには、返される行の制限を解除し、`CREATE TABLE tablename AS SELECT` と設定してクエリを実行します。See the [generating datasets tutorial][query-service-create-datasets] for instructions on how to generate a dataset from query results in [!DNL Query Editor].

![画像](../images/queries/query-editor-overview/query-results.png)

## チュートリアルビデオでクエリを実行 [!DNL Query Service] する

次のビデオでは、Adobe Experience PlatformインターフェイスとPSQLクライアントでクエリを実行する方法を示します。 また、XDMオブジェクトで個々のプロパティを使用し、Adobe定義関数を使用し、CREATE TABLE AS SELECT (CTAS)を使用する方法も示します。

>[!VIDEO](https://video.tv.adobe.com/v/29796?quality=12&learn=on)

## 次の手順

Now that you know what features are available in [!DNL Query Editor] and how to navigate the application, you can start authoring your own queries directly in [!DNL Platform]. For more information about running SQL queries against datasets in [!DNL Data Lake], see the guide on [running queries][query-service-running-queries]. Adobe Analytics および Adobe Target データで使用する SQL クエリのサンプルについては、[サンプルクエリのリファレンス][query-service-sample-queries]を参照してください。

[query-service-overview]: ../home.md
[query-service-ui]: overview.md
[query-service-running-queries]: ../creating-queries/creating-queries.md
[query-service-sample-queries]: ../sample-queries/overview.md
[query-service-create-datasets]: ../creating-queries/create-datasets.md
