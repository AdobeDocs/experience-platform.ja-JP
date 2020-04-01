---
keywords: Experience Platform;attribution ai;access scores;popular topics
solution: Experience Platform
title: アトリビューションAIのスコアへのアクセス
topic: Accessing scores
translation-type: tm+mt
source-git-commit: 689b4c7a96856e1167ee435a6740fed006136256

---


# アトリビューションAIのスコアへのアクセス

>[!IMPORTANT] データの一括エクスポートの生のスコアのダウンロードの詳細については、attributionai-support@adobe.comにお問い合わせください。

アトリビューションAIのスコアへのアクセスは、Snowflakeを使用して行います。 現在、Snowflakeの読者アカウントを設定し、その資格情報を受け取るには、attributionai-support@adobe.comでアドビサポートに電子メールでお問い合わせいただく必要があります。

アドビサポートがリクエストを処理すると、Snowflakeの読者アカウントのURLと、次の対応する資格情報が提供されます。

- 雪片URL
- ユーザー名
- パスワード

>[!NOTE] Readerアカウントは、JDBCコネクタをサポートするSQLクライアント、Worksheet、BIソリューションを使用してデータを照会するためのものです。

資格情報とURLを取得したら、モデル表を生の形式でクエリし、タッチポイントの日付またはコンバージョンの日付で集計できます。

## 雪片でスキーマを探す

提供された資格情報を使用して、Snowflakeにログインします。 左上のメイ **ンナビゲーションで** 「ワークシート」タブをクリックし、左側のパネルでデータベースディレクトリに移動します。

![ワークシートと移動](./images/download-scores/edited_snowflake_1.png)

次に、画面の **右上隅** 「スキーマを選択」をクリックします。 表示されるプローバーで、適切なデータベースが選択されていることを確認します。 次に、[ *スキーマ* ]ドロップダウンをクリックし、一覧からスキーマを選択します。 選択したクエリの下に表示されるスコアテーブルから直接スキーマできます。

![スキーマ](./images/download-scores/edited_snowflake_2.png)

## 生のスコアのダウンロード

生のスコアのダウンロードについて詳しくは、attributionai-support@adobe.comにお問い合わせください。

## PowerBIと雪片の接続（オプション）

PowerBI DesktopとSnowflakeデータベース間の接続の設定には、雪片資格情報を使用できます。

まず、「 *Server* 」ボックスに雪片URLを入力します。 次に、「 *Warehouse*」で「XSMALL」と入力します。 次に、ユーザ名とパスワードを入力します。

![POWERBIの例](./images/download-scores/powerbi-snowflake.png)

接続が確立されたら、雪片データベースを選択し、適切なデータベースをスキーマします。 これで、すべてのテーブルを読み込むことができます。

アトリビューションを選択すると、スキーマスコアを含むテーブルが表示されます。

| テーブル | 説明 |
| ----- | ----------- |
| APP_{APP_ID} | 生のアトリビューションスコア。 |
| APP_{APP_ID}_BY_CONV_DATE | コンバージョン日レベルで集計された生のアトリビューションスコア。 |
| APP_{APP_ID}_BY_TP_DATE | タッチポイントの日付レベルで集計された生のアトリビューションスコア。 |

## 次の手順

このドキュメントでは、データのクエリとアトリビューションAIのスコアへのアクセスに必要な手順を説明しています。 提供される他のインテリジェントサービス [とガイドを](../home.md) 、引き続き参照できます。