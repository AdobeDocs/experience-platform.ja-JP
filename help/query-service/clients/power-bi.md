---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Power BIに接続
topic: connect
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---


# Power BI (PC)との接続

PCユーザーは、https://powerbi.microsoft.com/en-us/desktop/からPower BIをインストールでき [ます](https://powerbi.microsoft.com/en-us/desktop/)。

## Power Biの設定

Power BIをインストールした後、PostgreSQLコネクタをサポートするために必要なコンポーネントを設定する必要があります。 次の手順に従います。

- PowerBIが接続する正式な方法 `npgsql`である、PostgreSQL用の.NETドライバパッケージを探し出してインストールします。

- v4.0.10を選択します（現在、新しいバージョンではエラーが発生します）。

- カスタム設定画面の「Npgsql GAC Installation」で、「 **Will be installed on local hard drive**」を選択します。 GACをインストールしないと、Power BIが後で失敗します。

- Windowsを再起動します。

- PowerBI Desktop評価版を見つけます。

## Power BIをクエリサービスに接続

次の準備手順を実行した後、Power BIをクエリサービスに接続できます。

- Power BIを開きます。

- 上部のメニューリボンで **「データの取得** 」をクリックします。

- 「 **PostgreSQLデータベース**」を選択し、「 **接続**」をクリックします。

- サーバーとデータベースの値を入力します。 **サーバ** ：接続の詳細で見つかったホストです。 実稼動環境の場合、ホスト文字列 `:80` の末尾にポートを追加します。 **Database** には、「all」またはデータセットテーブル名を指定できます。 （CTASから派生したデータセットの1つを試してみてください）。

- [ **詳細オプション**]をクリックし、[関係列を **含める**]のチェックを外します。 完全な階層を使用した **ナビゲーションをチェックしない**。

- *（オプションですが、データベースに対して「all」が宣言されている場合に推奨されます）* SQL文を入力します。

>[!NOTE] SQL文を指定しない場合、Power BIはデータベース内のすべてのテーブルをプレビューします。 階層データの場合は、カスタムSQLステートメントを使用する必要があります。 テーブルスキーマがフラットな場合は、カスタムSQLステートメントを使用するか、使用しないかを指定します。 複合型はPower BIではまだサポートされていません。複合型からプリミティブ型を取得するには、SQL文を記述して派生させる必要があります。

```sql
SELECT web.webPageDetails.name AS Page_Name, 
SUM(web.webPageDetails.pageviews.value) AS Page_Views 
FROM _TABLE_ 
WHERE _ACP_YEAR=2018 AND _ACP_MONTH=11 AND _ACP_DAY=20 
GROUP BY web.webPageDetails.name 
ORDER BY SUM(web.webPageDetails.pageviews.value) DESC 
LIMIT 10
```

- 「DirectQuery **」または「** インポート **** 」モードを選択します。 イン **ポート** ・モードでは、データはPower BIでインポートされます。 DirectQuery **モードでは** 、すべてのクエリがクエリサービスに送信され、実行されます。

- **OK** をクリックします。これで、Power BIはクエリサービスに接続し、エラーがない場合にプレビューを生成します。 プレビューレンダリングの数値列に関する既知の問題があります。 次の手順に進みます。

- 「 **読み込み** 」をクリックして、データセットをPower BIに取り込みます。
