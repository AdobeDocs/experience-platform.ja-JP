---
keywords: Experience Platform；ホーム；人気のトピック；[!DNL PostgreSQL];[!DNL PostgreSQL];PostgreSQL
solution: Experience Platform
title: UI での PostgreSQL Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して PostgreSQL ソース接続を作成する方法を説明します。
exl-id: e556d867-a1eb-4900-b8a9-189666a4f3f1
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 47%

---

# UI での [!DNL PostgreSQL] ソース接続の作成

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Experience Platform] のユーザーインターフェイスを使用して [!DNL PostgreSQL] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL PostgreSQL] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Experience Platform] で [!DNL PostgreSQL] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL PostgreSQL] アカウントに関連付けられた接続文字列。 [!DNL PostgreSQL] の接続文字列パターンは `Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}` です。 |

基本について詳しくは、この [[!DNL PostgreSQL]  ドキュメント ](https://www.postgresql.org/docs/9.2/app-psql.html) を参照してください。

#### 接続文字列の SSL 暗号化を有効にする

次のプロパティを使用して接続文字列を追加することで、[!DNL PostgreSQL] 接続文字列の SSL 暗号化を有効にできます。

| プロパティ | 説明 | 例 |
| --- | --- | --- |
| `EncryptionMethod` | [!DNL PostgreSQL] データに対して SSL 暗号化を有効にできます。 | <uL><li>`EncryptionMethod=0` （無効）</li><li>`EncryptionMethod=1` （有効）</li><li>`EncryptionMethod=6` （RequestSSL）</li></ul> |
| `ValidateServerCertificate` | `EncryptionMethod` ータの適用時に [!DNL PostgreSQL] データベースから送信された証明書を検証します。 | <uL><li>`ValidationServerCertificate=0` （無効）</li><li>`ValidationServerCertificate=1` （有効）</li></ul> |

次に、SSL 暗号化が追加された [!DNL PostgreSQL] 接続文字列の例を示します。`Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD};EncryptionMethod=1;ValidateServerCertificate=1`

## [!DNL PostgreSQL] アカウントの接続

必要な資格情報が揃ったら、次の手順に従って、[!DNL PostgreSQL] アカウントを [!DNL Experience Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL Databases]** カテゴリで、**[!UICONTROL PostgreSQL DB]** を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい [!DNL PostgreSQL] コネクタを作成します。

![](../../../../images/tutorials/create/postgresql/catalog.png)

**[!UICONTROL [!DNL PostgreSQL]]** に接続ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL PostgreSQL] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![](../../../../images/tutorials/create/postgresql/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL PostgreSQL] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![](../../../../images/tutorials/create/postgresql/existing.png)

## 次の手順

このチュートリアルでは、[!DNL PostgreSQL] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データをに取り込むためのデータフローの設定  [!DNL Experience Platform]](../../dataflow/databases.md) を行いましょう。
