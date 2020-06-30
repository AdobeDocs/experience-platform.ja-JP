---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのAmazon Redshiftソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: d3c725c4760acb3857a67d0d30b24732c963a030
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 1%

---


# UIで [!DNL Amazon Redshift] ソースコネクタを作成する

>「[!NOTE]
>コネクタ [!DNL Amazon Redshift] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 [!DNL Amazon Redshift] ユーザインターフェイスを使用して、[!DNL Redshift](以下「 [!DNL Platform] 」と呼ばれる)ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、次のAdobe Experience Platformのコンポーネントについて十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../../../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に [!DNL Redshift] ベース接続がある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

で [!DNL Redshift] アカウントにアクセスするに [!DNL Platform]は、次の値を指定する必要があります。

| **Credential** | **説明** |
| -------------- | --------------- |
| `server` | アカウントに関連付けられているサー [!DNL Redshift] バー。 |
| `username` | アカウントに関連付けられているユー [!DNL Redshift] ザ名。 |
| `password` | アカウントに関連付けられているパス [!DNL Redshift] ワードです。 |
| `database` | アクセスしている [!DNL Redshift] データベース。 |

開始方法の詳細については、 [このRedshiftドキュメントを参照してください](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html)。

## アカウントに接続 [!DNL Redshift] する

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、ア [!DNL Redshift] カウントをリンクし [!DNL Platform]ます。

「 <a href="https://platform.adobe.com" target="_blank">Adobe Experience Platform</a> 」にログインし、左のナビゲーションバーで「 **[!UICONTROL ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 [ *[!UICONTROL カタログ]* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *[!UICONTROL Databases]* 」カテゴリの下で、「 **[!UICONTROL Amazon Redshift]** 」を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、「 **[!UICONTROL 接続ソース]**」を選択します。

![](../../../../images/tutorials/create/redshift/catalog.png)

「Amazon Redshift *[!UICONTROL に接続]* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、基本接続に名前、オプションの説明および [!DNL Redshift] 資格情報を指定します。 終了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいベース接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/redshift/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Redshift] アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![](../../../../images/tutorials/create/redshift/existing.png)

## 次の手順

このチュートリアルに従って、ア [!DNL Redshift] カウントへの基本的な接続を確立しました。 次のチュートリアルに進み、データをPlatformに取り込むようにデータフローを [設定できます](../../dataflow/databases.md)。