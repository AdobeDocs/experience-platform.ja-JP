---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；Power BI;power bi；クエリサービスへの接続；
solution: Experience Platform
title: Power BI をクエリサービスに接続します。
description: このドキュメントでは、Adobe Experience Platformクエリサービスを使用してPower BIを接続する手順について説明します。
exl-id: 8fcd3056-aac7-4226-a354-ed7fb8fe9ad7
source-git-commit: 1af89160cbf5b689396921869fec6c30a5bcfff0
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 1%

---

# 接続 [!DNL Power BI] クエリサービスへ

このドキュメントでは、 [!DNL Power BI] Adobe Experience Platformクエリサービスを使用したデスクトップ。

## はじめに

このガイドでは、 [!DNL Power BI] デスクトップアプリケーションのインターフェイスの操作方法を理解しているユーザーです。 ダウンロードするには [!DNL Power BI] デスクトップまたは詳しくは、 [公式 [!DNL Power BI] ドキュメント](https://docs.microsoft.com/ja-JP/power-bi/).

>[!IMPORTANT]
>
> この [!DNL Power BI] デスクトップアプリケーションは **のみ** は、Windows デバイスで使用できます。

接続に必要な資格情報を取得するには [!DNL Power BI] をExperience Platformするには、Platform UI の「クエリ」ワークスペースにアクセスできる必要があります。 現在クエリワークスペースへのアクセス権がない場合は、組織の管理者に問い合わせてください。

インストール後 [!DNL Power BI]をインストールするには、 `Npgsql`:PostgreSQL 用の.NET ドライバーパッケージです。 Npgsql の詳細については、 [Npgsql ドキュメント](https://www.npgsql.org/doc/index.html).

>[!IMPORTANT]
>
>新しいバージョンではエラーが発生するので、v4.0.10 以前をダウンロードする必要があります。

&quot;の下[!DNL Npgsql GAC Installation]」をクリックします。 **[!DNL Will be installed on local hard drive]**.

Npgsql が正しくインストールされていることを確認するには、次の手順に進む前にコンピューターを再起動してください。

## 接続 [!DNL Power BI] クエリサービスへ {#connect-power-bi}

接続するには [!DNL Power BI] クエリサービスにアクセスするには、 [!DNL Power BI] を選択し、 **[!DNL Get Data]** をクリックします。 次に、&quot;[!DNL PostgreSQL]」と入力して、データソースのリストを絞り込みます。 表示された結果から、「 」を選択します。 **[!DNL PostgreSQL database]**&#x200B;に続いて **[!DNL Connect]**.

この [!DNL PostgreSQL] データベースダイアログが表示され、サーバーとデータベースの値をリクエストします。 次の手順に関する追加の説明 [Power Query Desktop から PostgreSQL データベースに接続](https://learn.microsoft.com/en-us/power-query/connectors/postgresql#connect-to-a-postgresql-database-from-power-query-desktop) 公式の [!DNL PowerBI] ドキュメント。

これらの必須値は、Adobe Experience Platformの資格情報から取得されます。 資格情報を見つけるには、Platform UI にログインし、「 」を選択します。 **[!UICONTROL クエリ]** 左のナビゲーションから、の後に **[!UICONTROL 資格情報]**. データベース名、ホスト、ポート、ログイン資格情報の検索の詳細については、 [資格情報ガイド](../ui/credentials.md).

![「Experience Platform情報」タブと有効期限が切れている資格情報がハイライト表示された「クエリ」ワークスペース。](../images/clients/power-bi/query-service-credentials-page.png)

内 **[!DNL Server]** フィールド [!DNL PostgreSQL database] ダイアログで、クエリサービスで見つかったホストの値を入力します。 [!UICONTROL 資格情報] 」セクションに入力します。 本番用に、ポートを追加 `:80` をホスト文字列の末尾に追加します。 例：`made-up.platform-query.adobe.io:80`。

この **[!DNL Database]** フィールドには、「すべて」またはデータセットテーブル名を指定できます。 例：`prod:all`。

>[!IMPORTANT]
>
>サードパーティの BI ツールのネストされたデータ構造は、フラット化して、使いやすさを向上させ、データの取得、分析、変換、レポートに必要なワークロードを削減できます。 詳しくは、[`FLATTEN` 機能](../best-practices/flatten-nested-data.md) を参照してください。

### データ接続モード {#data-connectivity-mode}

次に、 **[!DNL Data Connectivity mode]**. 内 [!DNL PostgreSQL database] ダイアログ、選択 **[!DNL Import]** 続いて **[!DNL OK]** すべての使用可能なテーブルのリストを表示するには、または **[!DNL DirectQuery]** データを直接に読み込んだりコピーしたりせずに、データソースに対して直接クエリを実行するには [!DNL Power BI].

詳しくは、 **[!DNL Import]** モード、次の項を参照してください： [テーブルのインポート](#import). 詳しくは、以下を参照してください。 **[!DNL DirectQuery]** モード、次の項を参照してください： [データをインポートせずにデータセットをクエリする](#direct-query).

選択 **[!DNL OK]** データベースの詳細を確認した後。

### 認証 {#authentication}

データ接続モードを確認すると、ユーザー名、パスワード、アプリケーションの設定を求めるプロンプトが表示されます。 この場合のユーザー名は組織 ID で、パスワードは認証トークンです。 両方とも、クエリサービス資格情報ページにあります。

これらの詳細を入力し、「 **[!DNL Connect]** をクリックして、次の手順に進みます。

## テーブルのインポート {#import}

次を選択すると、 **[!DNL Import]** [!DNL Data Connectivity mode]」を選択すると、完全なデータセットがインポートされ、選択したテーブルと列を [!DNL Power BI] デスクトップアプリケーションのそのまま

>[!IMPORTANT]
>
>最初のインポート以降に発生したデータの変更を確認するには、 [!DNL Power BI] データセット全体を再度読み込む。

テーブルをインポートするには、サーバーとデータベースの詳細を入力します [上記のように](#connect-power-bi) をクリックし、 **[!DNL Import]** [!DNL Data Connectivity mode]に続いて **[!DNL OK]**. この [!DNL Navigator] ダイアログが開き、使用可能なすべてのテーブルのリストが表示されます。 プレビューするテーブルを選択し、その後に **[!DNL Load]** データセットをPower BIに取り込む これで、テーブルが [!DNL Power BI].

[PowerBi デスクトップでのデータへの接続に関する一般情報](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-quickstart-connect-to-data#connect-to-data) アプリは公式ドキュメントに記載されています。

### カスタム SQL を使用したテーブルのインポート

[!DNL Power BI] や、 [!DNL Tableau] 現在、ユーザーは、Platform の XDM オブジェクトなど、ネストされたオブジェクトの読み込みを許可しないでください。 これを考慮するには、 [!DNL Power BI] では、カスタム SQL を使用してこれらの入れ子フィールドにアクセスし、データの統合表示を作成できます。 [!DNL Power BI] 次に、以前にネストされたデータのこのフラット化されたビューを通常のテーブルとして読み込みます。

次の [!DNL PostgreSQL database] ダイアログ、選択 **[!DNL Advanced options]** カスタム SQL クエリを **[!DNL SQL statement]** 」セクションに入力します。 このカスタムクエリは、JSON の名前と値のペアをテーブル形式に統合するために使用する必要があります。 公式ドキュメントでは、 [詳細オプションの SQL 文を使用した PowerBI の接続](https://learn.microsoft.com/en-us/power-query/connectors/postgresql#connect-using-advanced-options).

カスタマイズしたクエリを入力したら、 **[!DNL OK]** をクリックして、データベースへの接続を続行します。 詳しくは、 [認証](#authentication) 上記の節を参照して、ワークフローのこの部分からデータベースに接続する方法を確認してください。

認証が完了すると、統合されたデータのプレビューが [!DNL Power BI] 表形式のデスクトップダッシュボード。 サーバー名とデータベース名がダイアログの上部に表示されます。 選択 **[!DNL Load]** をクリックして、インポートプロセスを完了します。

これで、ビジュアライゼーションを [!DNL Power BI] デスクトップアプリケーション。

## データをインポートせずにデータセットをクエリする {#direct-query}

この **[!DNL DirectQuery]** [!DNL Data Connectivity mode] は、 [!DNL Power BI] デスクトップ。 この接続モードを使用すると、UI を使用して、すべてのビジュアライゼーションを現在のデータで更新できます。 ただし、ビジュアライゼーションの作成または更新に要する時間は、基になるデータソースのパフォーマンスに応じて異なります。

詳細情報： [使用 [!DNL DirectQuery]](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-use-directquery) その包括的な議論と同様に [接続オプション、使用例、制限事項](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-directquery-about) 公式の [!DNL PowerBI] ドキュメント。

これを使用するには [!DNL Data Connectivity mode]を選択し、 **[!DNL DirectQuery]** 切り替え **[!DNL Advanced options]** カスタム SQL クエリを **[!DNL SQL statement]** 」セクションに入力します。 以下を確認します。 **[!DNL Include relationship columns]** が選択されている。 クエリを完了したら、「 **[!DNL OK]** をクリックして続行します。

クエリのプレビューが表示されます。 選択 **[!DNL Load]** をクリックして、クエリの結果を確認します。

## 次の手順

このドキュメントを読むと、 [!DNL Power BI] デスクトップアプリケーションと様々なデータ接続モードを使用できます。 クエリの書き込みと実行の方法について詳しくは、 [クエリ実行のガイダンス](../best-practices/writing-queries.md).
