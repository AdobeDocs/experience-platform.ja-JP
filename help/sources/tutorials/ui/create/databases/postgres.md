---
keywords: Experience Platform；ホーム；人気の高いトピック；[!DNL PostgreSQL];[!DNL PostgreSQL];PostgreSQL
solution: Experience Platform
title: UI での PostgreSQL ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して PostgreSQL ソース接続を作成する方法を説明します。
exl-id: e556d867-a1eb-4900-b8a9-189666a4f3f1
source-git-commit: eea815f72c1e807f4ad6ca6273ba18a9da09ac6e
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 54%

---

# UI での [!DNL PostgreSQL] ソース接続の作成

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Platform] のユーザーインターフェイスを使用して [!DNL PostgreSQL] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL PostgreSQL] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な認証情報の収集

次の項目にアクセスするには、 [!DNL PostgreSQL] アカウント [!DNL Platform]に値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | 次に示すように、 [!DNL PostgreSQL] アカウント この [!DNL PostgreSQL] 接続文字列のパターン： `Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}`. |

導入の詳細については、 [[!DNL PostgreSQL] 文書](https://www.postgresql.org/docs/9.2/app-psql.html).

#### 接続文字列の SSL 暗号化の有効化

の SSL 暗号化を有効にすることができます [!DNL PostgreSQL] 接続文字列を追加します。

| プロパティ | 説明 | 例 |
| --- | --- | --- |
| `EncryptionMethod` | で SSL 暗号化を有効にできます [!DNL PostgreSQL] データ。 | <uL><li>`EncryptionMethod=0`(無効)</li><li>`EncryptionMethod=1`(有効)</li><li>`EncryptionMethod=6`(RequestSSL)</li></ul> |
| `ValidateServerCertificate` | が送信した証明書を検証します [!DNL PostgreSQL] データベース `EncryptionMethod` が適用されます。 | <uL><li>`ValidationServerCertificate=0`(無効)</li><li>`ValidationServerCertificate=1`(有効)</li></ul> |

次に、 [!DNL PostgreSQL] SSL 暗号化が追加された接続文字列： `Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD};EncryptionMethod=1;ValidateServerCertificate=1`.

## [!DNL PostgreSQL] アカウントの接続

必要な認証情報が揃ったら、次の手順に従って、[!DNL PostgreSQL] アカウントを [!DNL Platform] にリンクします。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL データベース]** カテゴリ、選択 **[!UICONTROL PostgreSQL DB]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい [!DNL PostgreSQL] コネクタ。

![](../../../../images/tutorials/create/postgresql/catalog.png)

この **[!UICONTROL 接続先[!DNL PostgreSQL]]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、名前、説明（オプション）および [!DNL PostgreSQL] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/postgresql/new.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL PostgreSQL] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![](../../../../images/tutorials/create/postgresql/existing.png)

## 次の手順

このチュートリアルでは、[!DNL PostgreSQL] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを に取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/databases.md)を行いましょう。
