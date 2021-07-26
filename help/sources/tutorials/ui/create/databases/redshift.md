---
keywords: Experience Platform；ホーム；人気のあるトピック；Amazon Redshift;amazon redshift;Redshift;redshift
solution: Experience Platform
title: UIでのAmazon Redshiftソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UIを使用してAmazon Redshiftソース接続を作成する方法を説明します。
exl-id: 4faf3200-673b-4a20-8f94-d049e800444b
source-git-commit: 600b216932a7d19440534c4b190fb2f3766c8785
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 12%

---

# UIでの[!DNL Amazon Redshift]ソース接続の作成

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL Amazon Redshift]（以下「[!DNL Redshift]」と呼びます）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用す [!DNL Experience Platform] る標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な[!DNL Redshift]接続がある場合は、このドキュメントの残りの部分をスキップし、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進んでください。

### 必要な資格情報の収集

[!DNL Platform]の[!DNL Redshift]アカウントにアクセスするには、次の値を指定する必要があります。

| **資格情報** | **説明** |
| -------------- | --------------- |
| `server` | [!DNL Redshift]アカウントに関連付けられたサーバー。 |
| `username` | [!DNL Redshift]アカウントに関連付けられているユーザー名。 |
| `password` | [!DNL Redshift]アカウントに関連付けられているパスワード。 |
| `database` | アクセスする[!DNL Redshift]データベース。 |

使い始める方法について詳しくは、[this [!DNL Redshift] document](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html)を参照してください。

## [!DNL Redshift]アカウントに接続する

必要な資格情報を収集したら、以下の手順に従って[!DNL Redshift]アカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「**[!UICONTROL データベース]**」カテゴリで、「**[!UICONTROL Amazon Redshift]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL データを追加]**」を選択して、新しい[!DNL Redshift]コネクタを作成します。

![](../../../../images/tutorials/create/redshift/catalog.png)

**[!UICONTROL Amazon Redshiftに接続]**&#x200B;ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）、[!DNL Redshift]資格情報を入力します。 終了したら、「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/redshift/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL Redshift]アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して次に進みます。

![](../../../../images/tutorials/create/redshift/existing.png)

## 次の手順

このチュートリアルに従って、[!DNL Redshift]アカウントへの接続を確立しました。 次のチュートリアルに進み、[にデータを取り込むようにデータフローを設定します。 [!DNL Platform]](../../dataflow/databases.md)
