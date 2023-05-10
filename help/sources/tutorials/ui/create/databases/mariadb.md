---
keywords: Experience Platform；ホーム；人気のトピック；Maria DB;maria db
solution: Experience Platform
title: UI での MariaDB ソース接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して Maria DB ソース接続を作成する方法を説明します。
exl-id: 259ca112-01f1-414a-bf9f-d94caf4c69df
source-git-commit: ed92bdcd965dc13ab83649aad87eddf53f7afd60
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 48%

---

# UI での [!DNL MariaDB] ソース接続の作成

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL MariaDB] 接続する場合は、このドキュメントの残りの部分をスキップして、 [データフローの設定](../../dataflow/databases.md).

### 必要な資格情報の収集

次の項目にアクセスするには、 [!DNL MariaDB] アカウント [!DNL Platform]に値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | MariaDB 認証に関連付けられた接続文字列。 この [!DNL MariaDB] 接続文字列のパターン： `Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |

導入の詳細については、 [[!DNL MariaDB] 文書](https://mariadb.com/kb/en/about-mariadb-connector-odbc/).

## [!DNL Maria DB] アカウントの接続

必要な認証情報が揃ったら、次の手順に従って、[!DNL Maria DB] アカウントを [!DNL Platform] にリンクします。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

以下 **[!UICONTROL データベース]** カテゴリ、選択 **[!UICONTROL Maria DB]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい [!DNL Maria DB] コネクタ。

![](../../../../images/tutorials/create/maria-db/catalog.png)

この **[!UICONTROL Maria DB に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）および [!DNL MariaDB] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/maria-db/new.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL MariaDB] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![](../../../../images/tutorials/create/maria-db/existing.png)

## 次の手順

このチュートリアルでは、[!DNL MariaDB] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを に取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/databases.md)を行いましょう。
