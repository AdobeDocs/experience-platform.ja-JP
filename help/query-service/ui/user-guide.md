---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformクエリサービスクエリエディタガイド
topic: query editor
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 1%

---


# [!DNL Query Editor] ユーザーガイド

[!DNL Query Editor] は、Adobe Experience Platformが提供するインタラクティブなツール [!DNL Query Service][!DNL Experience Platform] です。ユーザーインターフェイス内で顧客体験データのクエリを作成、検証および実行できます。 [!DNL Query Editor] は、分析およびデータ探索用の開発クエリをサポートし、開発目的でインタラクティブクエリを実行できるほか、でデータセットを設定する非インタラクティブクエリも実行でき [!DNL Experience Platform]ます。

の概念と機能について詳し [!DNL Query Service]くは、 [クエリサービスの概要][query-service-overview]を参照してください。 のクエリサービスユーザーインターフェイスを操作する方法について詳し [!DNL Platform]くは、「 [クエリサービスのUIの概要][query-service-ui]」を参照してください。

## はじめに

[!DNL Query Editor] に接続することでクエリを柔軟に実行でき [!DNL Query Service]ます。クエリはこの接続がアクティブな間にのみ実行されます。

### Connecting to [!DNL Query Service]

[!DNL Query Editor] が初期化され、開かれたときに接続され [!DNL Query Service] るまでに数秒かかります。 次に示すように、コンソールは接続された時点を示します。 エディタが接続する前にクエリを実行しようとすると、接続が完了するまで実行が遅れます。

![画像](../images/queries/query-editor-overview/initializing-connection.png)

### クエリの実行元 [!DNL Query Editor]

対話的に [!DNL Query Editor] 実行されたクエリ。 これは、ブラウザーを閉じた場合や別の場所に移動した場合に、クエリがキャンセルされることを意味します。 これは、クエリ出力からデータセットを生成するクエリにも当てはまります。

## クエリオーサリング [!DNL Query Editor]

を使用 [!DNL Query Editor]すると、顧客体験データのクエリを書き込み、実行および保存できます。 で実行 [!DNL Query Editor]または保存されたすべてのクエリは、組織内でにアクセス権を持つすべてのユーザーが使用でき [!DNL Query Service]ます。

### Accessing [!DNL Query Editor]

UIで、左側のナビゲーションメニューの [!DNL Experience Platform] クエリ **[!UICONTROL をクリックして、]**[!DNL Query Service] ワークスペースを開きます。 次に、画面の右上にある「 **[!UICONTROL クエリを作成]** 」をクリックして、クエリを記述します。 このリンクは、ワークス [!DNL Query Service] ペースの任意のページから利用できます。

![画像](../images/queries/query-editor-overview/create-query.png)

### クエリの作成

[!UICONTROL クエリエディタ] (Editor Editor)は、書き込みクエリをできるだけ簡単にするように構成されています。 下のスクリーンショットは、 **** 再生ボタンとSQLエントリフィールドがハイライト表示された状態で、UIにエディターがどのように表示されるかを示しています。

![画像](../images/queries/query-editor-overview/editor.png)

開発に要する時間を最小限に抑えるには、返される行に対する制限を持つクエリを開発することをお勧めします。 例：`SELECT fields FROM table WHERE conditions LIMIT number_of_rows`。クエリが期待される出力を生成することを確認したら、制限を解除し、クエリを実行して出力のデータセット `CREATE TABLE tablename AS SELECT` を生成します。

### ツールの作成 [!DNL Query Editor]

- **自動構文のハイライト表示：** SQLの読み取りと整理を容易にします。

![画像](../images/queries/query-editor-overview/syntax-highlight.png)

- **SQLキーワードオートコンプリート：** 開始がクエリを入力し、矢印キーを使用して目的のキーワードに移動し、 **Enterキーを押します**。

![画像](../images/queries/query-editor-overview/syntax-auto.png)

- **テーブルとフィールドのオートコンプリート：** 目的のテーブル名を入力した開始が `SELECT` 、矢印キーを使用して目的のテーブルに移動し、 **Enterキーを押します**。 テーブルを選択すると、オートコンプリートでそのテーブル内のフィールドが認識されます。

![画像](../images/queries/query-editor-overview/tables-auto.png)

### エラー検出

[!DNL Query Editor] クエリの書き込み時に自動的に検証され、汎用のSQL検証と特定の実行検証が提供されます。 クエリの下に赤い下線が引かれている場合（下の画像を参照）は、クエリ内のエラーを表します。

![画像](../images/queries/query-editor-overview/syntax-error-highlight.png)

エラーが検出された場合は、SQLコードの上にカーソルを置くと、特定のエラーメッセージを表示できます。

![画像](../images/queries/query-editor-overview/linting-error.png)

### クエリの詳細

でクエリを表示しているとき [!DNL Query Editor]、 *[!UICONTROL クエリの詳細]* パネルには、選択したクエリを管理するツールが表示されます。

![画像](../images/queries/query-editor-overview/query-details.png)

このパネルでは、UIから直接出力データセットを生成したり、表示されたクエリを削除または名前を付けたり、SQLコードを表示したり、 *[!UICONTROL SQLクエリ]* タブに簡単にコピーできる形式で表示したりできます。 このパネルには、クエリが最後に変更されたときや、該当する場合は誰が変更したかなど、有用なメタデータも表示されます。 データセットを生成するには、「 **[!UICONTROL 出力データセット]**」をクリックします。 [ *[!UICONTROL 出力データセット]* ]ダイアログが表示されます。 名前と説明を入力し、「クエリを **[!UICONTROL 実行]**」をクリックします。 新しいデータセットは、のユー *[!UICONTROL ザーインターフェイスの「]* Datasets [!DNL Query Service] 」タブに表示され [!DNL Platform]ます。

### クエリの保存

[!DNL Query Editor] には、クエリを保存して後で操作できる保存関数があります。 クエリを保存するには、の右上隅にある **[!UICONTROL 「保存]** 」をクリックし [!DNL Query Editor]ます。 クエリを保存する前に、 *[!UICONTROL クエリの詳細]* パネルを使用してクエリに名前を指定する必要があります。

### 以前のクエリの検索方法

から実行されるすべてのクエリ [!DNL Query Editor] は、ログテーブルに取り込まれます。 「 *[!UICONTROL ログ]* 」タブの検索機能を使用して、クエリ実行を検索できます。 保存したクエリは、「 *[!UICONTROL 参照]* 」タブに表示されます。

詳しくは、 [クエリサービスのUIの概要][query-service-ui] （英語）を参照してください。

>[!NOTE]
>
>実行されないクエリは、ログに保存されません。 クエリをで使用するには、で実行または保存する [!DNL Query Service]必要があり [!DNL Query Editor]ます。

## クエリエディターを使用したクエリの実行

でクエリを実行するに [!DNL Query Editor]は、エディターでSQLを入力するか、「 *ログ* 」または「 *[!UICONTROL 参照]* 」タブから前のクエリを読み込み、「 **再生」をクリックします**。 クエリ実行のステータスは、下の *[!UICONTROL 「コンソール]* 」タブに表示され、出力データは「 *[!UICONTROL 結果]* 」タブに表示されます。

### コンソール

コンソールは、のステータスと操作に関する情報を提供し [!DNL Query Service]ます。 コンソールには、への接続ステータス、実行中のクエリ操作、 [!DNL Query Service]およびこれらのクエリによって生じたエラーメッセージが表示されます。

![画像](../images/queries/query-editor-overview/console.png)

>[!NOTE]
>
>コンソールには、クエリの実行に起因するエラーのみが表示されます。 クエリの実行前にクエリの検証エラーは表示されません。

### クエリ結果

クエリが完了すると、結果が「 *[!UICONTROL 結果]* 」タブの「 *[!UICONTROL コンソール]* 」タブの横に表示されます。 この表示は、クエリの表形式の出力を表示します。最大100行まで表示できます。 この表示を使用すると、クエリが期待どおりの出力を生成しているかどうかを確認できます。 クエリーでデータセットを生成するには、返される行に対する制限を解除し、を使用してクエリを実行し、出力データセット `CREATE TABLE tablename AS SELECT` を生成します。 のクエリ結果からデータセットを生成する方法については、 [「データセットの][query-service-create-datasets] 生成」のチュートリアル [!DNL Query Editor]を参照してください。

![画像](../images/queries/query-editor-overview/query-results.png)

## チュートリアルビデオでクエリを実行 [!DNL Query Service] する

次のビデオでは、Adobe Experience PlatformインターフェイスとPSQLクライアントでクエリを実行する方法を示します。 また、XDMオブジェクト内で個々のプロパティを使用し、Adobe定義の関数を使用し、CREATE TABLE AS SELECT(CTAS)を使用する方法も示します。

>[!VIDEO](https://video.tv.adobe.com/v/29796?quality=12&learn=on)

## 次の手順

で使用できる機能とアプリのナビゲーション方法がわかったので、で直接独自のクエリをオーサリングする開始を作成でき [!DNL Query Editor][!DNL Platform]ます。 でのデータセットに対するSQLクエリの実行の詳細については、『クエリの [!DNL Data Lake]実行に関するガイド [][query-service-running-queries]』を参照してください。 AdobeAnalyticsとAdobe Targetデータを使用する場合のSQLクエリの例については、 [サンプルクエリリファレンスを参照してください][query-service-sample-queries]。

[query-service-overview]: ../home.md
[query-service-ui]: overview.md
[query-service-running-queries]: ../creating-queries/creating-queries.md
[query-service-sample-queries]: ../sample-queries/overview.md
[query-service-create-datasets]: ../creating-queries/create-datasets.md
