---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Power BIとの接続
topic: connect
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507

---


# Power BI (PC)との接続

PCユーザーは、https://powerbi.microsoft.com/en-us/desktop/からPower BIをインストールで [きます](https://powerbi.microsoft.com/en-us/desktop/)。

## Power Biの設定

Power BIをインストールした後、PostgreSQLコネクタをサポートするために必要なコンポーネントを設定する必要があります。 次の手順に従います。

- PowerBIが接続する `npgsql`公式の方法であるPostgreSQL用の.NETドライバパッケージを探してインストールします。

- v4.0.10を選択します（新しいバージョンでは、現在エラーが発生します）。

- カスタムセットアップ画面の「Npgsql GAC Installation」で、「 **Will be installed on local hard drive**. GACをインストールしないと、Power BIが後で失敗します。

- Windowsを再起動します。

- PowerBI Desktop評価版を見つけます。

## Power BIをクエリサービス

次の手順を実行した後、Power BIをクエリサービスに接続できます。

- Power BIを開きます。

- 上部のメ **ニューリボンで** [データの取得]をクリックします。

- PostgreSQLデー **タベースを選択し**、「接続」をクリ **ックします**。

- サーバーとデータベースの値を入力します。 **Serverは** 、接続の詳細で見つかったホストです。 実稼動環境の場合は、ホスト文 `:80` 字列の末尾にポートを追加します。 **データベース** には、「all」またはデータセットテーブル名を指定できます。 （CTASから派生したデータセットの1つを試してみてください）。

- [詳細オ **プション**]をクリックし、[関係列を含め **る]のチェックを外しま**&#x200B;す。 [完全な階層を使用し **たナビゲーション]は選択しな**&#x200B;い。

- *（オプションですが、データベースに対して「all」が宣言されている場合に推奨されます）* SQL文を入力します。

>[!NOTE] SQLステートメントを指定しない場合、Power BIはデータベース内のすべてのテーブルをプレビューします。 階層データの場合は、カスタムSQLステートメントを使用する必要があります。 テーブルスキーマがフラットな場合は、カスタムSQLステートメントを使用する場合と使用しない場合です。 複合型は、Power BIではまだサポートされていません。複合型からプリミティブ型を取得するには、SQLステートメントを記述して派生させる必要があります。

```sql
SELECT web.webPageDetails.name AS Page_Name, 
SUM(web.webPageDetails.pageviews.value) AS Page_Views 
FROM _TABLE_ 
WHERE _ACP_YEAR=2018 AND _ACP_MONTH=11 AND _ACP_DAY=20 
GROUP BY web.webPageDetails.name 
ORDER BY SUM(web.webPageDetails.pageviews.value) DESC 
LIMIT 10
```

- [ **DirectQuery** ]または[インポ **ート** ]モードを選択します。 インポ **ート** ・モードでは、データはPower BIにインポートされます。 DirectQueryモ **ードで** 、すべてのクエリが実行のためにDirectQueryサービスに送信されます。

- **OK** をクリックします。これで、Power BIはクエリサービスに接続し、エラーがない場合はプレビューを生成します。 数値列のレンダリングに関する既知のプレビューの問題があります。 次の手順に進みます。

- 「読み込 **み** 」をクリックして、データセットをPower BIに取り込みます。
