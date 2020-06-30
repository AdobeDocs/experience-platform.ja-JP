---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのAmazon Kinesisソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: d3c725c4760acb3857a67d0d30b24732c963a030
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 1%

---


# UIで [!DNL Amazon Kinesis] ソースコネクタを作成する

>[!NOTE]
>コネクタ [!DNL Amazon Kinesis] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 [!DNL Amazon Kinesis] ユーザーインターフェイスを使用して [!DNL "Kinesis"](以下「 [!DNL Platform] 」と呼びます)ソースコネクタを認証する手順を説明します。

## はじめに

このチュートリアルでは、次のAdobe Experience Platformのコンポーネントについて十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../../../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に [!DNL Kinesis] アカウントをお持ちの場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/streaming/cloud-storage.md)。

### 必要な資格情報の収集

ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があり [!DNL Kinesis] ます。

| Credential | 説明 |
| ---------- | ----------- |
| `accessKeyId` | アカウントのアクセスキーID [!DNL Kinesis] 。 |
| `Secret access key` | アカウントの秘密アクセスキー [!DNL Kinesis] 。 |
| `region` | AWSサーバーの地域です。 |

これらの値の詳細については、 [このKinesisドキュメントを参照してください](https://docs.aws.amazon.com/streams/latest/dev/getting-started.html)。

## アカウントに接続 [!DNL Kinesis] する

必要な資格情報を収集したら、次の手順に従って [!DNL Kinesis] アカウントをにリンクでき [!DNL Platform]ます。

「 [Adobe Experience Platform](https://platform.adobe.com) 」にログインし、左のナビゲーションバーで「 **[!UICONTROL ソース]** 」を選択して「 *ソース* 」ワークスペースにアクセスします。 「 *カタログ* 」タブには、接続可能な様々なソースが表示され [!DNL Platform]ます。 各ソースには、関連付けられた既存のアカウントの数が表示されます。

「 *[!UICONTROL クラウドストレージ]* 」カテゴリで、「 **[!UICONTROL Amazon Kinesis]** 」を選択し、「+」アイコン(+) **をクリックして新しい**[!DNL Kinesis] コネクタを作成します。

![](../../../../images/tutorials/create/kinesis/catalog.png)

[ *[!UICONTROL Amazon Kinesisに]* 接続]ダイアログが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明および [!DNL Kinesis] 資格情報を入力します。 終了したら、 **[!UICONTROL [接続]** ]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/kinesis/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Kinesis] アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![](../../../../images/tutorials/create/kinesis/existing.png)

## 次の手順

このチュートリアルに従って、に [!DNL Kinesis] アカウントに接続し [!DNL Platform]ました。 次のチュートリアルに進み、クラウドストレージのデータをPlatformに取り込むようにデータフローを [設定できます](../../dataflow/streaming/cloud-storage.md)。