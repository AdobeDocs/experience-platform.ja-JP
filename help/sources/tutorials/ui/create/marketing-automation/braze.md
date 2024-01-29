---
title: UI での Braze データのデータフローの作成
description: Adobe Experience Platform UI を使用して Braze アカウントのデータフローを作成する方法を説明します。
hide: true
hidefromtoc: true
badge: ベータ版
source-git-commit: 92d3a7143edc81cc5266ef5a33a8c53dcfdf1074
workflow-type: tm+mt
source-wordcount: '665'
ht-degree: 23%

---

# UI での [!DNL Braze] ソース接続の作成

>[!NOTE]
>
>[!DNL Braze] ソースはベータ版です。詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

[!DNL Braze] を使用すると、消費者とブランド間の顧客中心のインタラクションをリアルタイムで実現できます。 [!DNL Braze Currents] は、Braze プラットフォームのエンゲージメントイベントのリアルタイムデータストリームで、 [!DNL Braze] プラットフォーム。

次のチュートリアルを読んで、 [!DNL Braze] をAdobe Experience Platformにアカウントします。

## 前提条件

このガイドの手順を完了するには、次が必要です。

* へのログイン [Adobe Experience Platform](https://platform.adobe.com) 新しいストリーミングソース接続を作成する権限
* 次にログイン： [[!DNL Braze] dashboard](https://dashboard.braze.com/sign_in)、未使用 [Currents コネクタライセンス](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents)、およびコネクタを作成する権限。 詳しくは、 [設定要件 [!DNL Currents]](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents/setting_up_currents/#requirements).

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

このチュートリアルでは、 [[!DNL Braze] 電流](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents).

既に [!DNL Braze] 接続する場合は、このドキュメントの残りの部分をスキップして、 [データフローの設定](../../dataflow/marketing-automation.md).

## 接続する [!DNL Braze] アカウントからExperience Platform

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

の下 *マーケティング自動化* カテゴリ、選択 **[!UICONTROL ブレーズ]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![ソースカタログは、Experience PlatformUI 上で、「Braze Currements」ソースを選択した状態で表示されます。](../../../../images/tutorials/create/braze/catalog.png)

次に、指定した [Braze Currents のサンプルファイル](https://github.com/Appboy/currents-examples/blob/master/sample-data/Adobe/adobe_examples.json). このファイルには、Braze がイベントの一部として送信する可能性のあるすべてのフィールドが含まれています。

![「データを追加」画面。](../../../../images/tutorials/create/braze/select-data.png)

ファイルをアップロードしたら、データセットに関する情報や、マッピング先のスキーマに関する情報など、データフローの詳細を指定する必要があります。
![「データフローの詳細」画面で、「データセットの詳細」がハイライトされます。](../../../../images/tutorials/create/braze/dataflow-detail.png)

次に、マッピングインターフェイスを使用して、データのマッピングを設定します。

![「マッピング」画面。](../../../../images/tutorials/create/braze/mapping.png)

>[!IMPORTANT]
>
>Braze のタイムスタンプは、ミリ秒単位ではなく、秒単位で表されます。 Experience Platformのタイムスタンプを正確に反映するには、ミリ秒単位の計算フィールドを作成する必要があります。 「time * 1000」の計算は、ミリ秒に正しく変換され、Experience Platform内のタイムスタンプフィールドへのマッピングに適しています。
>
>![タイムスタンプの計算フィールドの作成 ](../../../../images/tutorials/create/braze/create-calculated-field.png)

### 必要な資格情報の収集

接続を作成したら、次の資格情報値を収集する必要があります。収集した資格情報値は、Braze ダッシュボードでに指定して、にデータを送信します。 [!DNL Platform]. 詳しくは、 [!DNL Braze] [Currents への移動に関するガイド](https://www.braze.com/docs/user_guide/data_and_analytics/braze_currents/setting_up_currents/#step-2-navigate-to-currents).

| フィールド | 説明 |
| ---------- | ----------- |
| `Client ID` | 次に関連付けられたクライアント ID: [!DNL Platform] ソース。 |
| `Client Secret` | に関連付けられたクライアント秘密鍵 [!DNL Platform] ソース。 |
| `Tenant ID` | に関連付けられたテナント ID [!DNL Platform] ソース。 |
| `Sandbox Name` | に関連付けられたサンドボックス [!DNL Platform] ソース。 |
| `Dataflow ID` | に関連付けられたデータフロー ID [!DNL Platform] ソース。 |
| `Streaming Endpoint` | 次に関連付けられたストリーミングエンドポイント： [!DNL Platform] ソース。 Braze はこれをバッチストリーミングエンドポイントに自動的に変換します。 |

### 設定 [!DNL Braze Currents] を使用して、データソースにデータをストリーミングする

内 [!DNL Braze Dashboard]、「パートナー統合」に移動します。 **->** データのエクスポートを選択し、 **[!DNL Create New Current]**. コネクタの名前、コネクタに関する通知の連絡先情報、上記の資格情報の入力を求めるプロンプトが表示されます。 受け取るイベントを選択し、必要に応じてフィールドの除外/変換を設定して、「 」を選択します。 **[!DNL Launch Current]**.

## 次の手順

このチュートリアルでは、[!DNL Braze] アカウントとの接続を確立しました。次のチュートリアルに進み、 [マーケティングオートメーションシステムデータをに取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/marketing-automation.md).