---
keywords: Experience Platform；ホーム；人気のトピック；Oracle DB;oracle DB
solution: Experience Platform
title: UI でのOracle DB Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して、Oracle DB ソース接続を作成する方法を説明します。
exl-id: 4ca6ecc6-0382-4cee-acc5-1dec7eeb9443
source-git-commit: ed92bdcd965dc13ab83649aad87eddf53f7afd60
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 42%

---

# UI での [!DNL Oracle DB] ソース接続の作成

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Oracle DB] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Oracle DB] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Platform] で [!DNL Oracle DB] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL Oracle DB] への接続に使用する接続文字列。 [!DNL Oracle DB] の接続文字列パターンは `Host={HOST};Port={PORT};Sid={SID};User Id={USERNAME};Password={PASSWORD}` です。 |
| `connectionSpec.id` | 接続の作成に必要な一意の ID。 [!DNL Oracle DB] の接続仕様 ID は `d6b52d86-f0f8-475f-89d4-ce54c8527328` です。 |

基本について詳しくは、[ このOracleドキュメント ](https://docs.oracle.com/database/121/ODPNT/featConnecting.htm#ODPNT199) を参照してください。

## [!DNL Oracle DB] アカウントを接続

必要な資格情報を収集したら、次の手順に従って [!DNL Oracle DB] アカウントを [!DNL Platform] に接続するようにリンクできます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**[!UICONTROL Databases]** カテゴリで、**[!UICONTROL Oracle DB]** を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい [!DNL Oracle DB] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/oracle/catalog.png)

**[!UICONTROL OracleDB に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL Oracle DB] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ 接続 ](../../../../images/tutorials/create/oracle/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Oracle DB] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/oracle/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Oracle DB] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データをに取り込むためのデータフローの設定  [!DNL Platform]](../../dataflow/databases.md) を行いましょう。
