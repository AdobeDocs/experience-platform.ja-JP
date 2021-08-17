---
keywords: 広告；交易所広告業務机
title: トレードデスクの接続
description: トレードデスクは、広告購入者がディスプレイ、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングを実行し、対象を絞ったデジタルキャンペーンをオーディエンスするためのセルフサービスプラットフォームです。
exl-id: b8f638e8-dc45-4aeb-8b4b-b3fa2906816d
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 2%

---

# [!DNL The Trade Desk] 接続

## 概要 {#overview}

[!DNL The Trade Desk] の宛先は、プロファイルデータをに送信する際に役立ち [!DNL The Trade Desk]ます。

[!DNL The Trade Desk] は、広告購入者がディスプレイ、ビデオ、モバイルの在庫ソースをまたいで、リターゲティングとオーディエンスをターゲット化したデジタルキャンペーンを実行するためのセルフサービスプラットフォームです。

プロファイルデータを[!DNL Trade Desk]に送信するには、まず宛先に接続する必要があります。

## ユースケース {#use-cases}

マーケターは、[!DNL Trade Desk IDs]やデバイスIDで構築されたセグメントを使用して、再ターゲティングやオーディエンスターゲット化されたデジタルキャンペーンを作成できます。

## サポートされるID {#supported-identities}

[!DNL The Trade Desk] では、以下の表で説明するIDのアクティブ化をサポートしています。[ID](/help/identity-service/namespaces.md)の詳細を説明します。

| ターゲットID | 説明 |
|---|---|
| GAID | [!DNL Google Advertising ID] |
| IDFA | [!DNL Apple ID for Advertisers] |
| トレードデスクID | トレードデスクプラットフォームの広告主ID |

## 書き出しタイプ {#export-type}

**[!DNL Segment export]** ：セグメント（オーディエンス）のすべてのメンバーを宛先に書き出します。

## 前提条件 {#prerequisites}

[!DNL The Trade Desk]で最初の宛先を作成したい場合で、(Adobe Audience Managerや他のアプリケーションで)以前にExperience CloudIDサービスで[ID同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html)を有効にしていない場合は、Adobeコンサルティングまたはカスタマーケアに連絡してID同期を有効にしてください。 以前にAudience Managerで[!DNL The Trade Desk]統合を設定していた場合、設定したID同期はPlatformに引き継がれます。

## 宛先に接続 {#connect}

この宛先に接続するには、[宛先の設定に関するチュートリアル](../../ui/connect-destination.md)で説明されている手順に従います。

### 接続パラメーター {#parameters}

[この宛先を設定](../../ui/connect-destination.md)する際に、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウントID]**:アカウ [!DNL Trade Desk] [!UICONTROL ントID]。
* **[!UICONTROL サーバーの場所]**:使用する地域 [!DNL Trade Desk] サーバーを担当者に問い合わせてください。次の中から選択できる地域サーバーを選択します。
   * **[!UICONTROL ヨーロッパ]**
   * **[!UICONTROL シンガポール]**
   * **[!UICONTROL 東京]**
   * **[!UICONTROL 北米東部]**
   * **[!UICONTROL 北米西部]**
   * **[!UICONTROL ラテンアメリカ]**

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

[セグメントスケジュール](../../ui/activate-segment-streaming-destinations.md#scheduling)の手順で、宛先プラットフォームの対応するIDまたはわかりやすい名前にセグメントを手動でマッピングする必要があります。

セグメントをマッピングする場合は、使いやすくするために、Platformセグメント名または短い形式を使用することをお勧めします。 ただし、宛先のセグメントIDまたは名前が、PlatformアカウントのセグメントIDまたは名前と一致する必要はありません。 マッピングフィールドに挿入した値は、宛先に反映されます。

複数のデバイスマッピング(cookie ID、[!DNL IDFA]、[!DNL GAID])を使用する場合は、3つのマッピングすべてで同じマッピング値を使用するようにしてください。 [!DNL The Trade Desk] は、すべてを1つのセグメントに集計し、デバイスレベルの分類をおこないます。

![Segment Mapping ID](../../assets/common/segment-mapping-id.png)

## エクスポートされたデータ {#exported-data}

データが[!DNL The Trade Desk]の宛先に正常に書き出されたかどうかを確認するには、[!DNL Trade Desk]アカウントを確認します。 アクティブ化に成功した場合、オーディエンスがアカウントに入力されます。
