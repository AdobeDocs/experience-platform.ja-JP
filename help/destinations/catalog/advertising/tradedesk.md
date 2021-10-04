---
keywords: 広告；商務所広告業務机
title: トレードデスクの接続
description: トレードデスクは、広告購入者がディスプレイ、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングを実行し、対象となるデジタルキャンペーンをオーディエンスに提供するセルフサービスプラットフォームです。
exl-id: b8f638e8-dc45-4aeb-8b4b-b3fa2906816d
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 4%

---

# [!DNL The Trade Desk] 接続

## 概要 {#overview}

[!DNL The Trade Desk] の宛先は、プロファイルデータをに送信する際に役立ち [!DNL The Trade Desk]ます。

[!DNL The Trade Desk] は、広告購入者がディスプレイ、ビデオ、モバイルの在庫ソースをまたいで、リターゲティングとオーディエンスのターゲットを絞ったデジタルキャンペーンを実行するためのセルフサービスプラットフォームです。

プロファイルデータを [!DNL Trade Desk] に送信するには、まず宛先に接続する必要があります。

## ユースケース {#use-cases}

マーケティング担当者は、[!DNL Trade Desk IDs] またはデバイス ID で構築されたセグメントを使用して、再ターゲティングや、オーディエンスのターゲットを絞ったデジタルキャンペーンを作成できます。

## サポートされる ID {#supported-identities}

[!DNL The Trade Desk] では、以下の表で説明する ID のアクティブ化をサポートしています。[ID](/help/identity-service/namespaces.md) の詳細をご覧ください。

| ターゲット ID | 説明 |
|---|---|
| GAID | [!DNL Google Advertising ID] |
| IDFA | [!DNL Apple ID for Advertisers] |
| トレードデスク ID | Trade Desk プラットフォームの広告主 ID |

## 書き出しタイプ {#export-type}

**[!DNL Segment export]** ：セグメント（オーディエンス）のすべてのメンバーを宛先に書き出します。

## 前提条件 {#prerequisites}

[!DNL The Trade Desk] で最初の宛先を作成したい場合で、以前 (Adobe Audience Managerや他のアプリケーションで )Experience CloudID サービスで [ID 同期機能 ](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) を有効にしていない場合は、Adobeコンサルティングまたはカスタマーケアに問い合わせて ID 同期を有効にしてください。 以前にAudience Managerで [!DNL The Trade Desk] 統合を設定していた場合、設定した ID 同期は Platform に引き継がれます。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:この宛先が将来認識される名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**:アカウ [!DNL Trade Desk] [!UICONTROL ント ID]。
* **[!UICONTROL サーバーの場所]**:使用する地域 [!DNL Trade Desk] サーバーを担当者に問い合わせてください。次の中から選択できる地域サーバーがあります。
   * **[!UICONTROL ヨーロッパ]**
   * **[!UICONTROL シンガポール]**
   * **[!UICONTROL 東京]**
   * **[!UICONTROL 北米東部]**
   * **[!UICONTROL 北米西部]**
   * **[!UICONTROL ラテンアメリカ]**

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-segment-streaming-destinations.md) を参照してください。

[ セグメントスケジュール ](../../ui/activate-segment-streaming-destinations.md#scheduling) の手順で、セグメントを、宛先プラットフォームの対応する ID またはわかりやすい名前に手動でマッピングする必要があります。

セグメントをマッピングする場合は、使いやすくするために、Platform セグメント名を使用するか、短い形式を使用することをお勧めします。 ただし、宛先のセグメント ID または名前は、Platform アカウントのセグメント ID または名前と一致する必要はありません。 マッピングフィールドに挿入した値は、宛先に反映されます。

複数のデバイスマッピング (cookie ID、[!DNL IDFA]、[!DNL GAID]) を使用する場合は、3 つのマッピングすべてで同じマッピング値を使用するようにしてください。 [!DNL The Trade Desk] は、すべてを 1 つのセグメントに集計し、デバイスレベルの分類を使用します。

![Segment Mapping ID](../../assets/common/segment-mapping-id.png)

## エクスポートされたデータ {#exported-data}

データが [!DNL The Trade Desk] の宛先に正常に書き出されたかどうかを確認するには、[!DNL Trade Desk] アカウントを確認します。 アクティブ化に成功した場合は、オーディエンスがアカウントに入力されます。
