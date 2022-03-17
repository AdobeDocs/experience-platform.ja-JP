---
keywords: '広告；ビング '
title: Microsoft Bing 接続
description: Microsoft Bing の接続先を使用すると、Microsoft Display Advertising をまたいで、再ターゲティングとオーディエンスにターゲットを絞ったデジタルキャンペーンを実行できます。
exl-id: e1c0273b-7e3c-4d77-ae14-d1e528ca0294
source-git-commit: b1945d42b82b549985d848071762fa6ee2451368
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 5%

---

# [!DNL Microsoft Bing] 接続 {#bing-destination}

## 概要 {#overview}

この [!DNL Microsoft Bing] の宛先は、次にプロファイルデータを送信する際に役立ちます： [!DNL Microsoft Display Advertising].

プロファイルデータをに送信するには [!DNL Microsoft Bing]の場合、最初に宛先に接続する必要があります。

## ユースケース {#use-cases}

マーケターは、 [!DNL Microsoft Advertising IDs] をまたいで、ディスプレイ広告を介してユーザーをターゲットにします。 [!DNL Microsoft Advertising] チャネル。

## サポートされる ID {#supported-identities}

[!DNL Microsoft Bing] では、以下の表で説明する id のアクティブ化をサポートしています。 詳細情報： [id](/help/identity-service/namespaces.md).

| ターゲット ID | 説明 |
|---|---|
| MAID | Microsoft Advertising ID |

{style=&quot;table-layout:auto&quot;}

## エクスポートのタイプと頻度 {#export-type-frequency}

**[!DNL Segment Export]**  — セグメント（オーディエンス）のすべてのメンバーを [!DNL Microsoft Bing] 宛先。

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | セグメント（オーディエンス）のすべてのメンバーをに書き出しています [!DNL Microsoft Bing] 宛先。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>を使用して最初の宛先を作成する場合は、 [!DNL Microsoft Bing] 有効にしていない [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) 以前のExperience CloudID サービス (Adobe Audience Managerや他のアプリケーションを使用 ) では、Adobeコンサルティングまたはカスタマーケアに問い合わせて、ID 同期を有効にしてください。 以前に [!DNL Microsoft Bing] Audience Manager内の統合、設定した ID 同期は Platform に引き継がれます。

宛先を設定する際は、次の情報を指定する必要があります。

* [!UICONTROL アカウント ID]:こちらは [!DNL Bing Ads CID]（整数形式）

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**:お使いの [!DNL Bing Ads CID].

## この宛先へのセグメントのアクティブ化 {#activate}

詳しくは、 [ストリーミングセグメントの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md) を参照してください。

内 [セグメントスケジュール](../../ui/activate-segment-streaming-destinations.md#scheduling) 手順に従って、セグメントを、宛先の対応する ID またはわかりやすい名前に手動でマッピングする必要があります。

セグメントをマッピングする場合は、 [!DNL Platform] セグメント名または短い形式（使いやすく）。 ただし、宛先のセグメント ID または名前が [!DNL Platform] アカウント マッピングフィールドに挿入した値は、宛先によって反映されます。

## 書き出されたデータ {#exported-data}

データがに正常に書き出されたかどうかを確認するには、以下を実行します。 [!DNL Microsoft Bing] 宛先、 [!DNL Microsoft Bing Ads] アカウント アクティブ化に成功した場合、オーディエンスがアカウントに入力されます。
