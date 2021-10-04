---
keywords: '広告；bing; '
title: Microsoft Bing 接続
description: Microsoft Bing の接続先を使用して、Microsoft ディスプレイ広告をまたいで、再ターゲティングとオーディエンスにターゲットを絞ったデジタルキャンペーンを実行できます。
exl-id: e1c0273b-7e3c-4d77-ae14-d1e528ca0294
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 4%

---

# [!DNL Microsoft Bing] 接続 {#bing-destination}

## 概要 {#overview}

[!DNL Microsoft Bing] の宛先は、プロファイルデータを [!DNL Microsoft Display Advertising] に送信する際に役立ちます。

プロファイルデータを [!DNL Microsoft Bing] に送信するには、まず宛先に接続する必要があります。

## ユースケース {#use-cases}

マーケティング担当者の場合、[!DNL Microsoft Advertising IDs] で構築されたセグメントを使用し、[!DNL Microsoft Advertising] チャネルをまたいだディスプレイ広告を通じてユーザーをターゲットにすることができます。

## サポートされる ID {#supported-identities}

[!DNL Microsoft Bing] では、以下の表で説明する ID のアクティブ化をサポートしています。[ID](/help/identity-service/namespaces.md) の詳細をご覧ください。

| ターゲット ID | 説明 |
|---|---|
| MAID | Microsoft 広告 ID |

## 書き出しタイプ {#export-type}

**[!DNL Segment Export]** ：セグメント（オーディエンス）のすべてのメンバーを宛先に書き出し [!DNL Microsoft Bing] ます。

## 前提条件 {#prerequisites}

[!DNL Microsoft Bing] で最初の宛先を作成したい場合で、以前 (Adobe Audience Managerや他のアプリケーションで )Experience CloudID サービスで [ID 同期機能 ](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) を有効にしていない場合は、Adobeコンサルティングまたはカスタマーケアに問い合わせて ID 同期を有効にしてください。 以前にAudience Managerで [!DNL Microsoft Bing] 統合を設定していた場合、設定した ID 同期は Platform に引き継がれます。

宛先を設定する際は、次の情報を指定する必要があります。

* [!UICONTROL アカウント ID]:これは、整数 [!DNL Bing Ads CID]形式のです。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:この宛先が将来認識される名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**:お使いの [!DNL Bing Ads CID].

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-segment-streaming-destinations.md) を参照してください。

[ セグメントスケジュール ](../../ui/activate-segment-streaming-destinations.md#scheduling) の手順で、セグメントを、対応する宛先の ID またはわかりやすい名前に手動でマッピングする必要があります。

セグメントをマッピングする際は、使いやすくするために、[!DNL Platform] セグメント名を使用するか、短い形式を使用することをお勧めします。 ただし、宛先のセグメント ID または名前は、[!DNL Platform] アカウントの ID または名前と一致する必要はありません。 マッピングフィールドに挿入した値は、宛先に反映されます。

## エクスポートされたデータ {#exported-data}

データが [!DNL Microsoft Bing] の宛先に正常に書き出されたかどうかを確認するには、[!DNL Microsoft Bing Ads] アカウントを確認します。 アクティブ化に成功した場合は、オーディエンスがアカウントに入力されます。
