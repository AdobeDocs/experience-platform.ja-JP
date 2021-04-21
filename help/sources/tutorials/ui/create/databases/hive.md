---
keywords: Experience Platform；ホーム；人気のあるトピック；Apache Hive;Azure HDInsights;azure hdinsights
solution: Experience Platform
title: UIのAzure HDInsights Source ConnectionでApacheハイブを作成します
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してAzure HDInsightsソース接続でApache Hiveを作成する方法を説明します。
exl-id: 3eb3cb02-9867-451a-b847-ab895310eedf
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 9%

---

# UIの[!DNL Azure HDInsights]ソース接続に[!DNL Apache Hive]を作成

>[!NOTE]
>
> [!DNL Azure HDInsights]コネクタの[!DNL Apache Hive]はベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL Azure HDInsights]ソースコネクタ上に[!DNL Apache Hive]を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な[!DNL Hive]接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)のチュートリアルに進むことができます

### 必要な資格情報の収集

[!DNL Platform]の[!DNL Hive]アカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | [!DNL Hive]サーバーのIPアドレスまたはホスト名。 |
| `username` | [!DNL Hive]サーバーへのアクセスに使用するユーザー名。 |
| `password` | ユーザーに対応するパスワードです。 |

開始方法の詳細については、[この [!DNL Hive] ドキュメント](https://cwiki.apache.org/confluence/display/Hive/Tutorial#Tutorial-GettingStarted)を参照してください。

## [!DNL Hive]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL Hive]アカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL データベース]**&#x200B;カテゴリの下で、**[!UICONTROL ハイブ]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しい[!DNL Hive]コネクタを作成します。

![カタログ](../../../../images/tutorials/create/hive/catalog.png)

**[!UICONTROL Hive]**&#x200B;に接続ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、[!DNL Hive]資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/hive/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL Hive]アカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![既存の](../../../../images/tutorials/create/hive/existing.png)

## 次の手順

このチュートリアルに従うと、[!DNL Hive]アカウントへの接続が確立されます。 次のチュートリアルに進み、[データを [!DNL Platform]](../../dataflow/databases.md)に取り込むようにデータフローを設定できます。
