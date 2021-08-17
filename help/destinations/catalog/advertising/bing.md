---
keywords: '広告；bing; '
title: Microsoft Bing接続
description: Microsoft Bingの接続先を使用すると、Microsoftディスプレイ広告全体で再ターゲティングとオーディエンスターゲティングされたデジタルキャンペーンを実行できます。
exl-id: e1c0273b-7e3c-4d77-ae14-d1e528ca0294
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 2%

---

# [!DNL Microsoft Bing] 接続 {#bing-destination}

## 概要 {#overview}

[!DNL Microsoft Bing]の宛先は、プロファイルデータを[!DNL Microsoft Display Advertising]に送信する際に役立ちます。

プロファイルデータを[!DNL Microsoft Bing]に送信するには、まず宛先に接続する必要があります。

## ユースケース {#use-cases}

マーケティング担当者の場合、[!DNL Microsoft Advertising IDs]で構築されたセグメントを使用して、[!DNL Microsoft Advertising]チャネルをまたいだディスプレイ広告を通じてユーザーをターゲットにできるようにしたいと考えています。

## サポートされるID {#supported-identities}

[!DNL Microsoft Bing] では、以下の表で説明するIDのアクティブ化をサポートしています。[ID](/help/identity-service/namespaces.md)の詳細を説明します。

| ターゲットID | 説明 |
|---|---|
| MAID | Microsoft Advertising ID |

## 書き出しタイプ {#export-type}

**[!DNL Segment Export]** ：セグメント（オーディエンス）のすべてのメンバーを宛先に書き出し [!DNL Microsoft Bing] ます。

## 前提条件 {#prerequisites}

[!DNL Microsoft Bing]で最初の宛先を作成したい場合で、(Adobe Audience Managerや他のアプリケーションで)以前にExperience CloudIDサービスで[ID同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html)を有効にしていない場合は、Adobeコンサルティングまたはカスタマーケアに連絡してID同期を有効にしてください。 以前にAudience Managerで[!DNL Microsoft Bing]統合を設定していた場合、設定したID同期はPlatformに引き継がれます。

宛先を設定する際は、次の情報を指定する必要があります。

* [!UICONTROL アカウントID]:これは、整 [!DNL Bing Ads CID]数形式のです。

## 宛先に接続 {#connect}

この宛先に接続するには、[宛先の設定に関するチュートリアル](../../ui/connect-destination.md)で説明されている手順に従います。

### 接続パラメーター {#parameters}

[この宛先を設定](../../ui/connect-destination.md)する際に、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウントID]**:お使いの [!DNL Bing Ads CID].

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

[セグメントスケジュール](../../ui/activate-segment-streaming-destinations.md#scheduling)の手順で、宛先の対応するIDまたはわかりやすい名前にセグメントを手動でマッピングする必要があります。

セグメントをマッピングする場合は、使いやすくするために、[!DNL Platform]セグメント名または短い形式を使用することをお勧めします。 ただし、宛先のセグメントIDまたは名前が[!DNL Platform]アカウントのIDと一致する必要はありません。 マッピングフィールドに挿入した値は、宛先に反映されます。

## エクスポートされたデータ {#exported-data}

データが[!DNL Microsoft Bing]の宛先に正常に書き出されたかどうかを確認するには、[!DNL Microsoft Bing Ads]アカウントを確認します。 アクティブ化に成功した場合、オーディエンスがアカウントに入力されます。
