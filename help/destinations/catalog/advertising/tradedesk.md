---
keywords: 広告；トレードデスク広告販売店
title: トレードデスクの連携
description: トレードデスクは、広告バイヤーがディスプレイ、ビデオ、モバイルの在庫ソースをまたいで再ターゲティングを実行し、オーディエンスにターゲットを絞ったデジタルキャンペーンを実施するためのセルフサービスプラットフォームです。
exl-id: b8f638e8-dc45-4aeb-8b4b-b3fa2906816d
source-git-commit: b1945d42b82b549985d848071762fa6ee2451368
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 5%

---

# [!DNL The Trade Desk] 接続

## 概要 {#overview}

[!DNL The Trade Desk] の宛先は、次にプロファイルデータを送信する際に役立ちます： [!DNL The Trade Desk].

[!DNL The Trade Desk] は、広告購入者がディスプレイ、ビデオ、モバイルの在庫ソースをまたいで、リターゲティングとオーディエンスをターゲットにしたデジタルキャンペーンを実行するためのセルフサービスプラットフォームです。

プロファイルデータをに送信するには [!DNL Trade Desk]の場合、最初に宛先に接続する必要があります。

## ユースケース {#use-cases}

マーケターは、 [!DNL Trade Desk IDs] またはデバイス ID を使用して、リターゲティングやオーディエンスターゲットのデジタルキャンペーンを作成します。

## サポートされる ID {#supported-identities}

[!DNL The Trade Desk] では、以下の表で説明する id のアクティブ化をサポートしています。 詳細情報： [id](/help/identity-service/namespaces.md).

| ターゲット ID | 説明 |
|---|---|
| GAID | [!DNL Google Advertising ID] |
| IDFA | [!DNL Apple ID for Advertisers] |
| トレードデスク ID | トレードデスクプラットフォームの広告主 ID |

{style=&quot;table-layout:auto&quot;}

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | セグメント（オーディエンス）のすべてのメンバーを宛先に書き出しています。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>を使用して最初の宛先を作成する場合は、 [!DNL The Trade Desk] 有効にしていない [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) 以前のExperience CloudID サービス (Adobe Audience Managerや他のアプリケーションを使用 ) では、Adobeコンサルティングまたはカスタマーケアに問い合わせて、ID 同期を有効にしてください。 以前に [!DNL The Trade Desk] Audience Manager内の統合、設定した ID 同期は Platform に引き継がれます。

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**:お使いの [!DNL Trade Desk] [!UICONTROL アカウント ID].
* **[!UICONTROL サーバーの場所]**:質問する [!DNL Trade Desk] 使用する必要のある地域サーバーを表す担当者。 次の中から選択できる地域サーバーを選択します。
   * **[!UICONTROL ヨーロッパ]**
   * **[!UICONTROL シンガポール]**
   * **[!UICONTROL 東京]**
   * **[!UICONTROL 北米東部]**
   * **[!UICONTROL 北米西部]**
   * **[!UICONTROL ラテンアメリカ]**

## この宛先へのセグメントのアクティブ化 {#activate}

詳しくは、 [ストリーミングセグメントの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md) を参照してください。

内 [セグメントスケジュール](../../ui/activate-segment-streaming-destinations.md#scheduling) 手順に従って、セグメントを、宛先プラットフォームの対応する ID またはわかりやすい名前に手動でマッピングする必要があります。

セグメントをマッピングする場合は、使いやすくするために、Platform セグメント名または短い形式を使用することをお勧めします。 ただし、宛先のセグメント ID または名前が Platform アカウントのセグメント ID と一致している必要はありません。 マッピングフィールドに挿入した値は、宛先によって反映されます。

複数のデバイスマッピング (Cookie ID、 [!DNL IDFA], [!DNL GAID]) の場合は、必ず 3 つのマッピングすべてに同じマッピング値を使用します。 [!DNL The Trade Desk] は、すべてを 1 つのセグメントに集計し、デバイスレベルの分類を使用します。

![Segment Mapping ID](../../assets/common/segment-mapping-id.png)

## 書き出されたデータ {#exported-data}

データがに正常に書き出されたかどうかを確認するには、以下を実行します。 [!DNL The Trade Desk] 宛先、 [!DNL Trade Desk] アカウント アクティブ化に成功した場合、オーディエンスがアカウントに入力されます。
