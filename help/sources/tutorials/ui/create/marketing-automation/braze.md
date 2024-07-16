---
title: UI での Braze データのデータフローの作成
description: Adobe Experience Platform UI を使用して Braze アカウントのデータフローを作成する方法を説明します。
last-substantial-update: 2024-01-30T00:00:00Z
badge: ベータ版
exl-id: 6e94414a-176c-4810-80ff-02cf9e797756
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 24%

---

# UI での [!DNL Braze Currents] ソース接続の作成

>[!NOTE]
>
>[!DNL Braze Currents] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../../../home.md#terms-and-conditions) を参照してください。

[!DNL Braze] は、消費者とブランドの間で、顧客中心のやり取りをリアルタイムで強化します。 [!DNL Braze Currents] は、Braze プラットフォームからのエンゲージメントイベントのリアルタイムデータストリームです。これは、[!DNL Braze] プラットフォームからの最も堅牢かつ詳細な書き出しです。

UI で [!DNL Braze] アカウントからAdobe Experience Platformにエンゲージメントイベントデータを取り込む方法については、次のチュートリアルを参照してください。

## 前提条件

このガイドの手順を完了するには、以下が必要です。

* [Adobe Experience Platformにログインし ](https://platform.adobe.com) 新しいストリーミングソース接続を作成する権限。
* [[!DNL Braze]  ダッシュボード ](https://dashboard.braze.com/sign_in) へのログイン、未使用の [Current Connector ライセンス ](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents)、コネクタを作成するための権限。 詳しくは、[ 設定の要件  [!DNL Currents]](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents/setting_up_currents/#requirements) を参照してください。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

このチュートリアルでは、[[!DNL Braze]  電流 ](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents) に関する十分な知識も必要です。

既に [!DNL Braze] 接続がある場合は、このドキュメントの残りの部分をスキップして、[ データフローの設定 ](../../dataflow/marketing-automation.md) に関するチュートリアルに進むことができます。

## [!DNL Braze] アカウントをExperience Platformに接続する

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

*マーケティングオートメーション* カテゴリで、**[!UICONTROL Braze Currents]** を選択し、次に **[!UICONTROL データを追加]** を選択します。

![Experience PlatformUI のソースカタログで、「Braze Current」ソースが選択されています。](../../../../images/tutorials/create/braze/catalog.png)

次に、提供された [Braze Currents サンプルファイル ](https://github.com/Appboy/currents-examples/blob/master/sample-data/Adobe/adobe_examples.json) をアップロードします。 このファイルには、Braze がイベントの一部として送信する可能性のあるすべてのフィールドが含まれています。

![ 「データを追加」画面 ](../../../../images/tutorials/create/braze/select-data.png)

ファイルがアップロードされたら、データセットやマッピング先のスキーマに関する情報など、データフローの詳細を指定する必要があります。
![ 「データセットの詳細」を強調表示した「データフローの詳細」画面 ](../../../../images/tutorials/create/braze/dataflow-detail.png)

次に、マッピングインターフェイスを使用して、データのマッピングを設定します。

![ 「マッピング」画面 ](../../../../images/tutorials/create/braze/mapping.png)

>[!IMPORTANT]
>
>Braze タイムスタンプはミリ秒単位ではなく、秒単位で表現されます。 Experience Platformのタイムスタンプを正確に反映するには、計算フィールドをミリ秒単位で作成する必要があります。 「time * 1000」の計算は、Experience Platform内のタイムスタンプフィールドへのマッピングに適した、ミリ秒に適切に変換されます。
>
>![ タイムスタンプ ](../../../../images/tutorials/create/braze/create-calculated-field.png) の計算フィールドの作成

### 必要な資格情報の収集

接続が作成されたら、次の資格情報の値を収集する必要があります。これらは、Braze ダッシュボードで指定して、データをExperience Platformに送信します。 詳しくは、[!DNL Braze][ 電流への移動に関するガイド ](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents/setting_up_currents/#step-2-navigate-to-currents) を参照してください。

| フィールド | 説明 |
| --- | --- |
| クライアント ID | Experience Platformソースに関連付けられたクライアント ID。 |
| クライアント秘密鍵 | Experience Platformソースに関連付けられたクライアント秘密鍵。 |
| テナント ID | Experience Platformソースに関連付けられたテナント ID。 |
| サンドボックス名 | Experience Platformソースに関連付けられたサンドボックス。 |
| データフロー ID | Experience Platformソースに関連付けられたデータフロー ID。 |
| ストリーミングエンドポイント | Experience Platformソースに関連付けられたストリーミングエンドポイント。 **メモ**:[!DNL Braze] はこれをバッチストリーミングエンドポイントに自動的に変換します。 |

### データソースにデータをストリーミングするための [!DNL Braze Currents] の設定

[!DNL Braze Dashboard] 内で、「Partner Integrations **-> 「** Data Export」に移動し、「**[!DNL Create New Current]**」を選択します。 コネクタの名前、コネクタに関する通知の連絡先情報、および上記の資格情報を入力するように求められます。 受け取るイベントを選択し、必要に応じてフィールドの除外/変換を設定して、「**[!DNL Launch Current]**」を選択します。

## 次の手順

このチュートリアルでは、[!DNL Braze] アカウントとの接続を確立しました。次のチュートリアルに進み、[ マーケティング自動化システムデータをに取り込むためのデータフローの設定  [!DNL Platform]](../../dataflow/marketing-automation.md) を行いましょう。
