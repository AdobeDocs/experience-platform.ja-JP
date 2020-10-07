---
keywords: Experience Platform;home;popular topics;query service;Query service;Power BI;power bi;connect to query service;
solution: Experience Platform
title: Power BI との接続
topic: connect
description: このドキュメントでは、Power BIとAdobe Experience Platformクエリサービスを接続する手順について説明します。
translation-type: tm+mt
source-git-commit: 8c94d3631296c1c3cc97501ccf1a3ed995ec3cab
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 57%

---


# 接続先 [!DNL Power BI] (PC)

PC users can install [!DNL Power BI] from [https://powerbi.microsoft.com/en-us/desktop/](https://powerbi.microsoft.com/ja-jp/desktop/).

## 設定 [!DNL Power BI]

After you have [!DNL Power BI] installed, you need to set up the necessary components to support the PostgreSQL connector. 次の手順に従います。

- `npgsql`（PowerBI の公式な接続方法である PostgreSQL 用 .NET ドライバパッケージ）を見つけてインストールします。

- V4.0.10 を選択します（現在、新しいバージョンではエラーが発生します）。

- カスタムセットアップ画面の「Npgsql GAC インストール」で、「**[!UICONTROL ハードドライブ上にインストール]**」を選択します。GAC をインストールしないと、後で Power BI に不具合が発生します。

- Windows を再起動します。

- Find the [!DNL PowerBI] Desktop evaluation version.

## 接続 [!DNL Power BI] 先 [!DNL Query Service]

これらの準備手順を実行した後、次 [!DNL Power BI] に接続できま [!DNL Query Service]す。

- Open [!DNL Power BI].

- 上部のメニューリボンで「**[!UICONTROL データを取得]**」をクリックします。

- **[!UICONTROL PostgreSQL データベース]**&#x200B;を選択し、「**[!UICONTROL 接続]**」をクリックします。

- サーバーとデータベースの値を入力します。**[!UICONTROL サーバー]**&#x200B;は、接続の詳細で見つかったホストです。実稼動環境の場合は、ホスト文字列の末尾にポート `:80` を追加します。**[!UICONTROL データベース]**&#x200B;には、「すべて」またはデータセットテーブル名を指定できます。（CTAS から派生したデータセットの 1 つを試してみてください）

- 「**[!UICONTROL 詳細オプション]**」をクリックし、「**[!UICONTROL 関係列を含める]**」のチェックを外します。「**[!UICONTROL 完全な階層を使用したナビゲーション]**」は選択しないでください。

- *（オプションですが、データベースで「すべて」が宣言されている場合に推奨されます）* SQL 文を入力します。

>[!NOTE]
>
>If a SQL statement is not provided, then [!DNL Power BI] will preview all of the tables in database. 階層データの場合は、カスタム SQL ステートメントを使用する必要があります。テーブルスキーマがフラットな場合は、カスタム SQL ステートメントを使用するか否かに関わらず機能します。Compound types are yet not supported by [!DNL Power BI] - to get primitive types from compound types, you will need to write SQL statements to derive them.

```sql
SELECT web.webPageDetails.name AS Page_Name, 
SUM(web.webPageDetails.pageviews.value) AS Page_Views 
FROM _TABLE_ 
WHERE TIMESTAMP >= to_timestamp('2018-11-20')
GROUP BY web.webPageDetails.name 
ORDER BY SUM(web.webPageDetails.pageviews.value) DESC 
LIMIT 10
```

- Select either &quot;[!UICONTROL DirectQuery]&quot; or &quot;[!UICONTROL Import]&quot; mode. In [!UICONTROL DirectQuery] mode, all the queries will be sent to [!DNL Query Service] for execution. [!UICONTROL 読み込み] モードでは、データはで読み込まれ [!DNL Power BI]ます。

- 「**[!UICONTROL OK]**」をクリックします。Now, [!DNL Power BI] connects to the [!DNL Query Service] and produces a preview if there are no errors. 数値列の「プレビュー」レンダリングには、既知の問題があります。次の手順に進みます。

- Click **[!UICONTROL Load]** to bring the dataset into [!DNL Power BI].
