---
keywords: Experience Platform；ホーム；人気の高いトピック；mysql;MySQL
solution: Experience Platform
title: UIでのMySQLソース接続の作成
topic: 概要
type: チュートリアル
description: Adobe Experience PlatformUIを使用してMySQLソース接続を作成する方法を説明します。
translation-type: tm+mt
source-git-commit: 8851e11e956b393e56714d4d48870b7f68947c18
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 11%

---


# UIに[!DNL MySQL]ソース接続を作成する

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、Adobe Experience PlatformUIを使用して[!DNL MySQL]ソース接続を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に[!DNL MySQL]ドキュメントをお持ちの場合は、この接続の残りの部分をスキップして、[データフロー](../../dataflow/databases.md)の設定に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Platform]の[!DNL MySQL]アカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | アカウントに関連付けられている[!DNL MySQL]接続文字列。 [!DNL MySQL]接続文字列パターンは次のとおりです。`Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. 接続文字列の詳細や、接続文字列を取得する方法については、[[!DNL MySQL] ドキュメント](https://dev.mysql.com/doc/connector-net/en/connector-net-connections-string.html)を読んでみてください。 |

## [!DNL MySQL]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL MySQL]アカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

**[!UICONTROL Databases]**&#x200B;カテゴリの下で、**[!UICONTROL MySQL]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しい[!DNL MySQL]コネクタを作成します。

![](../../../../images/tutorials/create/my-sql/catalog.png)

**[!UICONTROL MySQLに接続]**&#x200B;ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、[!DNL MySQL]資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/my-sql/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL MySQL]アカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![](../../../../images/tutorials/create/my-sql/existing.png)

## 次の手順

このチュートリアルに従って、MySQLアカウントへの接続を確立しました。 次のチュートリアルに進み、[データを [!DNL Platform]](../../dataflow/databases.md)に取り込むようにデータフローを設定できます。