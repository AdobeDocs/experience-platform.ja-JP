---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのOracle DBソース・コネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 5ad763d2167c68f3293a2813248efaee22230a52
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 1%

---


# UIでのOracleソースコネクタの作成

> [!NOTE]
> Oracleコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、Platform・ユーザー・インタフェースを使用してOracleソース・コネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、次のAdobe Experience Platformのコンポーネントについて十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

有効なOracle DB接続が既に存在する場合は、このドキュメントの残りの部分をスキップし、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

Platform上のOracleアカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | Oracleへの接続に使用する接続文字列。 Oracle接続文字列パターンは次のとおりです。 `Host={HOST};Port={PORT};Sid={SID};User Id={USERNAME};Password={PASSWORD}`. |
| `connectionSpec.id` | 接続を作成するために必要な一意の識別子。 Oracleの接続仕様IDはで `d6b52d86-f0f8-475f-89d4-ce54c8527328`す。 |

開始方法の詳細は、 [このOracleドキュメントを参照してください](https://docs.oracle.com/database/121/ODPNT/featConnecting.htm#ODPNT199)。

## Oracleアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しいOracleアカウントを作成し、Platformに接続します。

「 [Adobe Experience Platform](https://platform.adobe.com) 」にログインし、左のナビゲーションバーで「 **[!UICONTROL ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 カ *[!UICONTROL タログ]* 画面には様々なソースが表示され、このソースから受信アカウントを作成できます。各ソースには既存のアカウントの数と関連するデータセットフローが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 *Databases* 」 **[!UICONTROL カテゴリで「]** Oracle DB **」を選択し、「+」アイコン(+)** をクリックして、新しいOracleコネクタを作成します。

![カタログ](../../../../images/tutorials/create/oracle/catalog.png)

「 *[!UICONTROL Oracle DB]* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明およびOracle秘密鍵証明書を入力します。 完了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/oracle/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するOracleアカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/oracle/existing.png)

## 次の手順

このチュートリアルに従って、Oracleアカウントへの接続を確立しました。 次のチュートリアルに進み、データをPlatformに取り込むようにデータフローを [設定できます](../../dataflow/databases.md)。