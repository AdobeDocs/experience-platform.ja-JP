---
keywords: Experience Platform；ホーム；人気のあるトピック；Maria DB;maria db
solution: Experience Platform
title: UIでのMariaDBソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してMaria DBソース接続を作成する方法を説明します。
translation-type: tm+mt
source-git-commit: b2384bfe26fa3d111c342062b2d9bb37c4226857
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 14%

---


# UIに[!DNL MariaDB]ソース接続を作成する

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用してMaria DBソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

既に[!DNL MariaDB]ドキュメントをお持ちの場合は、この接続の残りの部分をスキップして、[データフロー](../../dataflow/databases.md)の設定に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Platform]の[!DNL MariaDB]アカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | MariaDB認証に関連付けられている接続文字列。 [!DNL MariaDB]接続文字列パターンは次のとおりです。`Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |

開始方法の詳細については、[[!DNL MariaDB] ドキュメント](https://mariadb.com/kb/en/about-mariadb-connector-odbc/)を参照してください。

## [!DNL Maria DB]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL Maria DB]アカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

**[!UICONTROL Databases]**&#x200B;カテゴリの下で、**[!UICONTROL Maria DB]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しい[!DNL Maria DB]コネクタを作成します。

![](../../../../images/tutorials/create/maria-db/catalog.png)

**[!UICONTROL Maria DB]**&#x200B;に接続ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、[!DNL MariaDB]資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/maria-db/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL MariaDB]アカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![](../../../../images/tutorials/create/maria-db/existing.png)

## 次の手順

このチュートリアルに従うと、[!DNL MariaDB]アカウントへの接続が確立されます。 次のチュートリアルに進み、[データを [!DNL Platform]](../../dataflow/databases.md)に取り込むようにデータフローを設定できます。