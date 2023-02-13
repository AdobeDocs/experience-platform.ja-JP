---
keywords: Experience Platform;ホーム;人気のトピック;クエリサービス;Power BI;power bi;クエリサービスへの接続;
solution: Experience Platform
title: Power BI をクエリサービスに接続します。
description: このドキュメントでは、Power BI と Adobe Experience Platform クエリサービスを接続する手順について説明します。
exl-id: 8fcd3056-aac7-4226-a354-ed7fb8fe9ad7
source-git-commit: 668b2624b7a23b570a3869f87245009379e8257c
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 100%

---

# クエリサービスへの [!DNL Power BI] の接続

このドキュメントでは、[!DNL Power BI] Desktop と Adobe Experience Platform クエリサービスを接続する手順について説明します。

## はじめに

このガイドでは、[!DNL Power BI] Desktop アプリに既にアクセスしており、そのインターフェイスの操作方法に精通している必要があります。[!DNL Power BI] Desktop をダウンロードするには、または詳細情報については、[公式 [!DNL Power BI] ドキュメント](https://docs.microsoft.com/ja-JP/power-bi/)を参照してください。

>[!IMPORTANT]
>
> [!DNL Power BI] Desktop アプリケーションは、Windows デバイスで&#x200B;**のみ**&#x200B;使用できます。

[!DNL Power BI] を Experience Platform に接続するために必要な資格情報を取得するには、Platform UI のクエリワークスペースにアクセスできる必要があります。現在、クエリワークスペースにアクセスできない場合は、組織の管理者にお問い合わせください。

[!DNL Power BI] をインストールした後、PostgreSQL の .NET ドライバーパッケージである `Npgsql` をインストールする必要があります。Npgsql について詳しくは、[Npgsql のドキュメント](https://www.npgsql.org/doc/index.html)を参照してください。

>[!IMPORTANT]
>
>新しいバージョンではエラーが発生するので、v4.0.10 以前をダウンロードする必要があります。

カスタム設定画面の「[!DNL Npgsql GAC Installation]」で「**[!DNL Will be installed on local hard drive]**」を選択します。

Npgsql が正しくインストールされていることを確認するには、次の手順に進む前にコンピューターを再起動してください。

## クエリサービスへの [!DNL Power BI] の接続 {#connect-power-bi}

[!DNL Power BI] をクエリサービスに接続するには、[!DNL Power BI] を開き、上部のメニューリボンで「**[!DNL Get Data]**」を選択します。次に、検索バーに「[!DNL PostgreSQL]」と入力して、データソースのリストを絞り込みます。表示される結果から、「**[!DNL PostgreSQL database]**」を選択し、続いて「**[!DNL Connect]**」を選択します。

[!DNL PostgreSQL] データベースダイアログが表示され、サーバーとデータベースの値の入力が求められます。[Power Query Desktop から PostgreSQL データベースに接続](https://learn.microsoft.com/ja-jp/power-query/connectors/postgresql#connect-to-a-postgresql-database-from-power-query-desktop)する方法の追加手順については、[!DNL PowerBI] の公式ドキュメントを参照してください。

これらの必須値は、Adobe Experience Platform 資格情報から取得されます。資格情報を見つけるには、Platform UI にログインし、左側のナビゲーションから「**[!UICONTROL クエリ]**」を選択し、続いて「**[!UICONTROL 資格情報]**」を選択します。データベース名、ホスト、ポートおよびログイン資格情報の検索について詳しくは、[資格情報ガイド](../ui/credentials.md)を参照してください。

![「資格情報」タブと期限切れになる資格情報がハイライト表示されている Experience Platform クエリワークスペース](../images/clients/power-bi/query-service-credentials-page.png)

[!DNL PostgreSQL database] ダイアログの「**[!DNL Server]**」フィールドに、クエリサービスの「[!UICONTROL 資格情報]」セクションに表示されているホストの値を入力します。実稼動環境の場合は、ホスト文字列の末尾にポート `:80` を追加します。例：`made-up.platform-query.adobe.io:80`。

「**[!DNL Database]**」フィールドには、「all」かデータセットテーブル名のいずれかを指定できます。例：`prod:all`。

>[!IMPORTANT]
>
>ネストされたデータ構造をサードパーティの BI ツール用にフラット化して、より使いやすいデータ構造にし、データの取得、分析、変換およびレポートに必要なワークロードを軽減できます。データベースへの接続時にこの設定をアクティブにする方法については、[`FLATTEN`機能](../essential-concepts/flatten-nested-data.md)に関するドキュメントを参照してください。

### データ接続モード {#data-connectivity-mode}

次に、「**[!DNL Data Connectivity mode]**」を選択できます。[!DNL PostgreSQL database] ダイアログで、「**[!DNL Import]**」に続いて「**[!DNL OK]**」を選択して、使用可能なすべてのテーブルを一覧表示するか、「**[!DNL DirectQuery]**」を選択して、データを [!DNL Power BI] に直接読み込んだりコピーしたりせずにデータソースを直接クエリします。

**[!DNL Import]** モードについて詳しくは、[テーブルの読み込み](#import)の節を参照してください。**[!DNL DirectQuery]** モードについて詳しくは、[データの読み込みを伴わないデータセットのクエリ](#direct-query)の節を参照してください。

データベースの詳細を確認した後、「**[!DNL OK]**」を選択します。

### 認証 {#authentication}

データ接続モードを確認すると、ユーザー名、パスワードおよびアプリケーション設定の入力を求めるプロンプトが表示されます。この場合のユーザー名は組織 ID であり、パスワードは認証トークンです。両方ともクエリサービスの資格情報ページで確認できます。

これらの詳細を入力し、「**[!DNL Connect]**」を選択して次の手順に進みます。

## テーブルの読み込み {#import}

「**[!DNL Import]** [!DNL Data Connectivity mode]」を選択すると、完全なデータセットが読み込まれ、選択したテーブルと列を [!DNL Power BI] Desktop アプリケーション内でそのまま使用できます。

>[!IMPORTANT]
>
>最初の読み込み以降に発生したデータの変更を確認するには、データセット全体を再度読み込んで [!DNL Power BI] 内でデータを更新する必要があります。

テーブルを読み込むには、サーバーとデータベースの詳細を[上記のように](#connect-power-bi)入力し、「**[!DNL Import]** [!DNL Data Connectivity mode]」に続いて「**[!DNL OK]**」を選択します。[!DNL Navigator] ダイアログが表示され、使用可能なすべてのテーブルが一覧表示されます。プレビューするテーブルに続いて「**[!DNL Load]**」を選択して、データセットを Power BI に取り込みます。これで、テーブルが [!DNL Power BI] に読み込まれます。

[Power BI Desktop アプリでのデータへの接続に関する一般情報](https://learn.microsoft.com/ja-jp/power-bi/connect-data/desktop-quickstart-connect-to-data#connect-to-data)については、公式ドキュメントを参照してください。

### カスタム SQL を使用したテーブルの読み込み

[!DNL Power BI] のほか、 [!DNL Tableau] などのサードパーティツールでは、現在、Platform の XDM オブジェクトなどのネストしたオブジェクトを読み込めません。 これに対処するために、 [!DNL Power BI] では、カスタム SQL を使用してこれらのネストされたフィールドにアクセスし、フラット化されたデータビューを作成できます。 [!DNL Power BI] では、次に、以前にネストされたデータのこうしたフラット化されたビューを通常のテーブルとして読み込みます。

[!DNL PostgreSQL database] ダイアログで「**[!DNL Advanced options]**」を選択して、「**[!DNL SQL statement]**」セクションにカスタム SQL クエリを入力します。 このカスタムクエリは、JSON の名前と値のペアをテーブル形式にフラット化するために使用します。 [詳細オプションの SQL 文を使用して Power BI に接続する](https://learn.microsoft.com/ja-jp/power-query/connectors/postgresql#connect-using-advanced-options)方法についても、公式ドキュメントを参照してください。

カスタマイズしたクエリを入力したら、「**[!DNL OK]**」を選択して、データベースへの接続を続行します。 ワークフローのこの部分からデータベースに接続する方法については、前述の[認証](#authentication)の節を参照してください。

認証が完了すると、フラット化されたデータのプレビューが [!DNL Power BI] Desktop ダッシュボードにテーブルとして表示されます。 サーバー名とデータベース名がダイアログの上部に一覧表示されます。 「**[!DNL Load]**」を選択すると、読み込みプロセスが完了します。

これで、ビジュアライゼーションを [!DNL Power BI] Desktop アプリから編集したり書き出したりできるようになりました。

## データの読み込みを伴わないデータセットのクエリ {#direct-query}

**[!DNL DirectQuery]** [!DNL Data Connectivity mode] では、データを [!DNL Power BI] Desktop に読み込んだりコピーしたりせずにデータソースに対して直接クエリを実行します。 この接続モードを使用すると、UI を通じてすべてのビジュアライゼーションを現在のデータで更新できます。 ただし、ビジュアライゼーションの生成または更新に要する時間は、基になるデータソースのパフォーマンスによって異なります。

[ [!DNL DirectQuery] の使用](https://learn.microsoft.com/ja-jp/power-bi/connect-data/desktop-use-directquery)とその[接続オプション、ユースケースおよび制限事項](https://learn.microsoft.com/ja-jp/power-bi/connect-data/desktop-directquery-about)に関する包括的な議論について詳しくは、[!DNL PowerBI] の公式ドキュメントを参照してください。

この [!DNL Data Connectivity mode] を使用するには、「**[!DNL DirectQuery]**」切り替えスイッチに続いて「**[!DNL Advanced options]**」を選択して、「**[!DNL SQL statement]**」セクションにカスタム SQL クエリを入力します。 「**[!DNL Include relationship columns]**」が選択されていることを確認します。 クエリが完成したら、「**[!DNL OK]**」を選択して続行します。

クエリのプレビューが表示されます。 「**[!DNL Load]**」を選択して、クエリの結果を確認します。

## 次の手順

このドキュメントでは、 [!DNL Power BI] Desktop アプリに接続する方法と使用可能な様々なデータ接続モードについて説明しました。 クエリの作成および実行方法について詳しくは、 [クエリ実行のガイダンス](../best-practices/writing-queries.md)を参照してください。
