---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Power BIに接続
topic: connect
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---


# 接続先 [!DNL Power BI] (PC)

PCユーザーは、https://powerbi.microsoft.com/en-us/desktop/ [!DNL Power BI] からインストールでき [ます](https://powerbi.microsoft.com/en-us/desktop/)。

## 設定 [!DNL Power BI]

をインストールした後、PostgreSQLコネクタをサポートするために必要なコンポーネントを設定する必要があり [!DNL Power BI] ます。 次の手順に従います。

- PowerBIが接続する正式な方法 `npgsql`である、PostgreSQL用の.NETドライバパッケージを探し出してインストールします。

- v4.0.10を選択します（現在、新しいバージョンではエラーが発生します）。

- カスタム設定画面の「Npgsql GAC Installation」で、「 **[!UICONTROL Will be installed on local hard drive]**」を選択します。 GACをインストールしないと、Power BIが後で失敗します。

- Windowsを再起動します。

- デスクトップ評価版 [!DNL PowerBI] を見つけます。

## 接続 [!DNL Power BI] 先 [!DNL Query Service]

これらの準備手順を実行した後、次 [!DNL Power BI] に接続できま [!DNL Query Service]す。

- Open [!DNL Power BI].

- 上部のメニューリボンで **[!UICONTROL 「データの取得]** 」をクリックします。

- 「 **[!UICONTROL PostgreSQLデータベース]**」を選択し、「 **[!UICONTROL 接続]**」をクリックします。

- サーバーとデータベースの値を入力します。 **[!UICONTROL サーバ]** ：接続の詳細で見つかったホストです。 実稼動環境の場合、ホスト文字列 `:80` の末尾にポートを追加します。 **[!UICONTROL Database]** には、「all」またはデータセットテーブル名を指定できます。 （CTASから派生したデータセットの1つを試してみてください）。

- [ **[!UICONTROL 詳細オプション]**]をクリックし、[関係列を **[!UICONTROL 含める]**]のチェックを外します。 完全な階層を使用した **[!UICONTROL ナビゲーションをチェックしない]**。

- *（オプションですが、データベースに対して「all」が宣言されている場合に推奨されます）* SQL文を入力します。

>[!NOTE]
>
>SQLステートメントを指定しない場合は、データベース内のすべてのテーブル [!DNL Power BI] をプレビューします。 階層データの場合は、カスタムSQLステートメントを使用する必要があります。 テーブルスキーマがフラットな場合は、カスタムSQLステートメントを使用するか、使用しないかを指定します。 複合型はまだでサポートされていません。複合型からプリミティブ型を取得するに [!DNL Power BI] は、SQL文を記述して派生させる必要があります。

```sql
SELECT web.webPageDetails.name AS Page_Name, 
SUM(web.webPageDetails.pageviews.value) AS Page_Views 
FROM _TABLE_ 
WHERE _ACP_YEAR=2018 AND _ACP_MONTH=11 AND _ACP_DAY=20 
GROUP BY web.webPageDetails.name 
ORDER BY SUM(web.webPageDetails.pageviews.value) DESC 
LIMIT 10
```

- 「DirectQuery **[!UICONTROL 」または「]** インポート **** 」モードを選択します。 **[!UICONTROL 読み込み]** モードでは、データはで読み込まれ [!DNL Power BI]ます。 DirectQuery **[!UICONTROL モードでは]** 、すべてのクエリがに送信され、実行 [!DNL Query Service] されます。

- **[!UICONTROL OK]** をクリックします。では、に [!DNL Power BI] 接続し、エラーがない場合 [!DNL Query Service] はプレビューを生成します。 プレビューレンダリングの数値列に関する既知の問題があります。 次の手順に進みます。

- 「 **[!UICONTROL Load]** 」をクリックして、データセットをに取り込み [!DNL Power BI]ます。
