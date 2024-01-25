---
keywords: 広告、トレードデスク、広告業務用デスク
title: Trade Desk 接続
description: トレードデスクは、広告バイヤーがディスプレイ、ビデオ、モバイルの在庫ソースをまたいで再ターゲティングを実行し、オーディエンスにターゲットを絞ったデジタルキャンペーンを実施するためのセルフサービスプラットフォームです。
exl-id: b8f638e8-dc45-4aeb-8b4b-b3fa2906816d
source-git-commit: ba39f62cd77acedb7bfc0081dbb5f59906c9b287
workflow-type: tm+mt
source-wordcount: '742'
ht-degree: 41%

---

# [!DNL The Trade Desk] 接続

## 概要 {#overview}

[!DNL The Trade Desk] の宛先は、次にプロファイルデータを送信する際に役立ちます： [!DNL The Trade Desk].

[!DNL The Trade Desk] は、広告購入者がディスプレイ、ビデオ、モバイルの在庫ソースをまたいで、リターゲティングとオーディエンスをターゲットにしたデジタルキャンペーンを実行するためのセルフサービスプラットフォームです。

プロファイルデータをに送信するには [!DNL Trade Desk]の場合、最初に宛先に接続する必要があります。

## ユースケース {#use-cases}

マーケターは、 [!DNL Trade Desk IDs] またはデバイス ID を使用して、リターゲティングやオーディエンスターゲットのデジタルキャンペーンを作成します。

## サポートされている ID {#supported-identities}

[!DNL The Trade Desk] は、以下の表に示す id に基づいてオーディエンスをアクティブ化できます。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ID | 説明 |
|---|---|
| GAID | [!DNL Google Advertising ID] |
| IDFA | [!DNL Apple ID for Advertisers] |
| トレードデスク ID | トレードデスクプラットフォームの広告主 ID |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの起源 | サポートあり | 説明 |
---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | オーディエンスのすべてのメンバーを宛先に書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>を使用して最初の宛先を作成する場合は、 [!DNL The Trade Desk] 有効にしていない [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) 以前のExperience CloudID サービス (Adobe Audience Managerや他のアプリケーションを使用 ) では、Adobeコンサルティングまたはカスタマーケアに問い合わせて、ID 同期を有効にしてください。 以前に Audience Manager で [!DNL The Trade Desk] 統合を設定していた場合、設定した ID 同期は Platform に引き継がれます。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**: [!DNL Trade Desk] [!UICONTROL アカウント ID].
* **[!UICONTROL サーバーの場所]**: [!DNL Trade Desk] 使用する必要のある地域サーバーを表す担当者。 次の中から選択できる地域サーバーがあります。
   * **[!UICONTROL ヨーロッパ]**
   * **[!UICONTROL シンガポール]**
   * **[!UICONTROL 東京]**
   * **[!UICONTROL 北米東部]**
   * **[!UICONTROL 北米西部]**
   * **[!UICONTROL ラテンアメリカ]**

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

Adobe Analytics の [オーディエンススケジュール](../../ui/activate-segment-streaming-destinations.md#scheduling) 手順に従って、オーディエンスを、宛先プラットフォームの対応する ID またはわかりやすい名前に手動でマッピングする必要があります。

セグメントをマッピングする場合は、使いやすくするために、Platform オーディエンス名または短い形式を使用することをお勧めします。 ただし、宛先のオーディエンス ID または名前が Platform アカウントのオーディエンス ID と一致している必要はありません。 マッピングフィールドに挿入した値は、宛先によって反映されます。

複数のデバイスマッピング (Cookie ID、 [!DNL IDFA], [!DNL GAID]) の場合は、必ず 3 つのマッピングすべてに同じマッピング値を使用します。 [!DNL The Trade Desk] は、すべてを 1 つのセグメントに集計し、デバイスレベルの分類を使用します。

![Segment Mapping ID](../../assets/common/segment-mapping-id.png)

## 書き出したデータ {#exported-data}

データがに正常に書き出されたかどうかを確認するには、以下を実行します。 [!DNL The Trade Desk] 宛先、 [!DNL Trade Desk] アカウント。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。
