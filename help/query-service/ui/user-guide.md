---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリエディタ；クエリエディタ；クエリサービス；クエリサービス；
solution: Experience Platform
title: クエリエディターUIガイド
topic-legacy: query editor
description: クエリエディターは、Adobe Experience Platformクエリサービスが提供するインタラクティブなツールです。このツールを使用すると、Experience Platformユーザーインターフェイス内で顧客体験データのクエリを作成、検証、実行できます。 クエリエディターでは、分析およびデータ調査のためのクエリを開発できます。また、開発目的でインタラクティブクエリを実行できるほか、非インタラクティブクエリを実行して Experience Platform のデータセットに入力することもできます。
exl-id: d7732244-0372-467d-84e2-5308f42c5d51
translation-type: tm+mt
source-git-commit: d2f19cc97082f75e66cf38e54b5bdb89482930ed
workflow-type: tm+mt
source-wordcount: '1082'
ht-degree: 50%

---

# [!DNL Query Editor] UIガイド

[!DNL Query Editor] は、Adobe Experience Platformが提供するインタラクティブなツール [!DNL Query Service] [!DNL Experience Platform] です。ユーザーインターフェイス内で顧客体験データのクエリを作成、検証および実行できます。[!DNL Query Editor] は、分析およびデータ探索用の開発クエリをサポートし、開発目的でインタラクティブクエリを実行できるほか、でデータセットを設定する非インタラクティブクエリも実行でき [!DNL Experience Platform]ます。

[!DNL Query Service]の概念と機能について詳しくは、[クエリサービスの概要][query-service-overview]を参照してください。 [!DNL Platform]のクエリサービスユーザーインターフェイスを操作する方法について詳しくは、[クエリサービスのUIの概要][query-service-ui]を参照してください。

## はじめに

[!DNL Query Editor] に接続することでクエリを柔軟に実行でき [!DNL Query Service]ます。クエリはこの接続がアクティブな間にのみ実行されます。

### [!DNL Query Service]に接続中

[!DNL Query Editor] が初期化され、開かれた [!DNL Query Service] ときに接続されるまでに数秒かかります。クエリサービスに接続されると、コンソールに示されます（以下を参照）。エディターがクエリサービスに接続される前にクエリを実行しようとすると、接続が完了するまで実行が待機されます。

![画像](../images/ui/query-editor/connect.png)

### [!DNL Query Editor]からのクエリの実行方法

[!DNL Query Editor]から実行されたクエリは、対話的に実行されます。 つまり、ブラウザーを閉じたり、別の場所に移動したりすると、クエリはキャンセルされます。また、クエリ出力からデータセットを生成するために実行するクエリも同様です。

## [!DNL Query Editor]を使用したクエリオーサリング

[!DNL Query Editor]を使用して、顧客体験データのクエリを書き込み、実行、および保存できます。 [!DNL Query Editor]で実行、または保存されたすべてのクエリは、[!DNL Query Service]にアクセスできる組織内のすべてのユーザーが利用できます。

### [!DNL Query Editor] へのアクセス

[!DNL Experience Platform] UIで、左側のナビゲーションメニューの&#x200B;**[!UICONTROL クエリ]**&#x200B;を選択して[!DNL Query Service]ワークスペースを開きます。 次に、画面の右上にある「クエリ&#x200B;**[!UICONTROL 作成]**」を選択して、クエリを書き込む開始を選択します。 このリンクは[!DNL Query Service]ワークスペースの任意のページから利用できます。

![画像](../images/ui/query-editor/create-query.png)

### クエリの記述

[!UICONTROL クエリエディターは、クエリをできるだけ簡単に記述できるように構成されています。]次のスクリーンショットは、UI でエディターがどのように表示されるかを示しています。ここでは、**プレイする**&#x200B;ボタンと SQL 入力フィールドがハイライトされています。

![画像](../images/ui/query-editor/editor.png)

開発時間を最小限に抑えるために、返される行を制限してクエリを開発することをお勧めします。たとえば、`SELECT fields FROM table WHERE conditions LIMIT number_of_rows` のように設定します。クエリが目的どおりの出力を生成することを確認したら、制限を解除して、`CREATE TABLE tablename AS SELECT` と設定してクエリを実行し、データセットを生成します。

### [!DNL Query Editor]でツールを書く

- **構文の自動ハイライト：** SQL の読み取りと構成が容易になります。

![画像](../images/ui/query-editor/syntax-highlight.png)

- **SQL キーワードのオートコンプリート：**&#x200B;クエリの入力を開始し、矢印キーを使用して目的の用語に移動して、**Enter** キーを押します。

![画像](../images/ui/query-editor/syntax-auto.png)

- **テーブルとフィールドのオートコンプリート**：`SELECT` 元のテーブル名の入力を開始し、矢印キーを使用して目的の表に移動して、**Enter** キーを押します。テーブルを選択すると、オートコンプリートによってそのテーブルのフィールドが認識されます。

![画像](../images/ui/query-editor/tables-auto.png)

### エラーの検出

[!DNL Query Editor] クエリの書き込み時に自動的に検証され、汎用のSQL検証と特定の実行検証が提供されます。以下の画像のように、クエリの下に赤い下線が表示される場合は、クエリ内にエラーがあります。

![画像](../images/ui/query-editor/syntax-error-highlight.png)

エラーが検出された場合、SQL コードの上にカーソルを置くと、特定のエラーメッセージが表示されます。

![画像](../images/ui/query-editor/linting-error.png)

### クエリの詳細

[!DNL Query Editor]でクエリを表示している間、**[!UICONTROL クエリの詳細]**&#x200B;パネルには、選択したクエリを管理するためのツールが表示されます。

![画像](../images/ui/query-editor/query-details.png)

このパネルでは、UI から出力データセットを直接生成したり、表示されたクエリを削除したり、クエリに名前を付けたりすることができます。また、「**[!UICONTROL SQL クエリ]**」タブには、SQL コードが簡単にコピーできる形式で表示されます。このパネルには、クエリの最終変更日時や最終変更者（該当する場合）などの有効なメタデータも表示されます。データセットを生成するには、**[!UICONTROL 出力データセット]**&#x200B;を選択します。 **[!UICONTROL データセットを出力]**&#x200B;ダイアログが表示されます。名前と説明を入力し、[**[!UICONTROL クエリを実行]**]を選択します。 新しいデータセットは、[!DNL Platform]の[!DNL Query Service]ユーザーインターフェイスの&#x200B;**[!UICONTROL Datasets]**&#x200B;タブに表示されます。

### クエリの保存

[!DNL Query Editor] には、クエリを保存して後で操作できる保存関数があります。クエリを保存するには、[!DNL Query Editor]の右上隅にある「**[!UICONTROL 保存]**」を選択します。 クエリを保存する前に、**[!UICONTROL クエリの詳細]**&#x200B;パネルを使用してクエリに名前を付ける必要があります。

### 以前のクエリを検索する方法

[!DNL Query Editor]から実行されるすべてのクエリは、ログテーブルに取り込まれます。 「**[!UICONTROL ログ]**」タブの検索機能を使用して、クエリの実行を検索できます。保存したクエリは「**[!UICONTROL 参照]**」タブに表 示されます。

詳しくは、[クエリサービス UI の概要][query-service-ui]を参照してください。

>[!NOTE]
>
>実行されなかったクエリは「ログ」に保存されません。クエリを[!DNL Query Service]で使用するには、[!DNL Query Editor]で実行または保存する必要があります。

## クエリエディターを使用してクエリを実行する

[!DNL Query Editor]でクエリを実行するには、エディターでSQLを入力するか、「**[!UICONTROL ログ]**」または「**[!UICONTROL 参照]**」タブから前のクエリを読み込み、**再生**&#x200B;を選択します。 クエリ実行のステータスは下の「**[!UICONTROL コンソール]**」タブに表示され、出力データは「**[!UICONTROL 結果]**」タブに表示されます。

### コンソール

コンソールは[!DNL Query Service]の状態と操作に関する情報を提供します。 コンソールには、[!DNL Query Service]への接続ステータス、実行中のクエリ操作、およびこれらのクエリから発生するエラーメッセージが表示されます。

![画像](../images/ui/query-editor/console.png)

>[!NOTE]
>
>コンソールには、クエリの実行時に発生したエラーのみが表示されます。クエリを実行する前に、クエリ検証エラーは表示されません。

### クエリの結果

クエリが完了すると、結果が「**[!UICONTROL コンソール]**」タブの横の「**[!UICONTROL 結果]**」タブに表示されます。このビューには、クエリの出力が表形式で出力され、最大 100 行まで表示されます。このビューを使用すると、クエリが目的どおりの出力を生成することを確認できます。クエリでデータセットを生成するには、返される行の制限を解除し、`CREATE TABLE tablename AS SELECT` と設定してクエリを実行します。[!DNL Query Editor]のクエリ結果からデータセットを生成する方法については、[データセットの生成のチュートリアル][query-service-create-datasets]を参照してください。

![画像](../images/ui/query-editor/query-results.png)

## [!DNL Query Service]チュートリアルビデオでクエリを実行

次のビデオでは、Adobe Experience PlatformインターフェイスとPSQLクライアントでクエリを実行する方法を示します。 また、XDMオブジェクトで個々のプロパティを使用し、Adobe定義関数を使用し、CREATE TABLE AS SELECT (CTAS)を使用する方法も示します。

>[!VIDEO](https://video.tv.adobe.com/v/29796?quality=12&learn=on)

## 次の手順

[!DNL Query Editor]で利用できる機能とアプリの操作方法がわかったので、[!DNL Platform]で直接独自のクエリを作成する開始を作成できます。 [!DNL Data Lake]内のデータセットに対するSQLクエリの実行について詳しくは、[実行中のクエリ][query-service-running-queries]のガイドを参照してください。

[query-service-overview]: ../home.md
[query-service-ui]: overview.md
[query-service-running-queries]: ../best-practices/writing-queries.md
[query-service-create-datasets]: ./create-datasets.md
