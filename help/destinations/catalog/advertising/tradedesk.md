---
keywords: 広告；トレードデスク；advertising トレードデスク
title: Trade Desk 接続
description: Trade Desk は、広告購入者がディスプレイ、ビデオ、モバイルの在庫ソースをまたいでリターゲティングやオーディエンスをターゲットにしたデジタルキャンペーンを実行するためのセルフサービスプラットフォームです。
exl-id: b8f638e8-dc45-4aeb-8b4b-b3fa2906816d
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '781'
ht-degree: 38%

---

# [!DNL The Trade Desk] 接続

## 概要 {#overview}

この宛先コネクタを使用して、プロファイルデータを [!DNL The Trade Desk] に送信します。 このコネクタは、[!DNL The Trade Desk] のファーストパーティエンドポイントにデータを送信します。 Adobe Experience Platformと [!DNL The Trade Desk] の統合では、[!DNL The Trade Desk] サードパーティのエンドポイントへのデータの書き出しはサポートされていません。

[!DNL The Trade Desk] は、広告購入者がディスプレイ、ビデオ、モバイルなどの在庫ソースをまたいで、リターゲティングやオーディエンスをターゲットにしたデジタルキャンペーンを実行するためのセルフサービスプラットフォームです。

プロファイルデータを [!DNL Trade Desk] に送信するには、最初に宛先に接続する必要があります。このページの次の節で説明しています。

## ユースケース {#use-cases}

マーケターは、[!DNL Trade Desk IDs] またはデバイス ID から作成されたオーディエンスを使用して、リターゲティングやオーディエンスターゲット設定のデジタルキャンペーンを作成できるようにしたいと考えています。

## サポートされている ID {#supported-identities}

[!DNL The Trade Desk] では、以下の表に示す ID に基づいたオーディエンスのアクティベーションをサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ID | 説明 |
|---|---|
| GAID | [!DNL Google Advertising ID] |
| IDFA | [!DNL Apple ID for Advertisers] |
| Trade Desk ID | Trade Desk プラットフォームの広告主 ID |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

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
>[!DNL The Trade Desk] での最初の宛先を作成しようとしており、これまで（Adobe Audience Managerなどのアプリケーションを使用して）Experience Cloud ID サービスで [ID 同期機能 ](https://experienceleague.adobe.com/en/docs/id-service/using/id-service-api/methods/idsync) を有効にしたことがない場合は、Adobe Consultingまたはカスタマーケアに連絡して ID 同期を有効にしてもらってください。 以前にAudience Managerで [!DNL The Trade Desk] 統合を設定していた場合、設定した ID 同期はExperience Platformに引き継がれます。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**：お使いの [!DNL Trade Desk] [!UICONTROL &#x200B; アカウント ID]。
* **[!UICONTROL サーバーの場所]**:[!DNL Trade Desk] の担当者に、使用する地域サーバーを問い合わせてください。 使用可能な地域サーバーを次の中から選択できます。
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
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

[ オーディエンススケジュール ](../../ui/activate-segment-streaming-destinations.md#scheduling) 手順では、オーディエンスを、宛先プラットフォームの対応する ID またはわかりやすい名前に手動でマッピングする必要があります。

オーディエンスをマッピングする場合、Adobeでは、使いやすくするために、Experience Platform オーディエンス名またはより短い形式を使用することをお勧めします。 ただし、宛先のオーディエンス ID または名前がExperience Platform アカウントのオーディエンス ID または名前と一致する必要はありません。 マッピングフィールドに挿入する値は、すべて宛先に反映されます。

複数のデバイスマッピング（cookie ID、[!DNL IDFA]、[!DNL GAID]）を使用する場合は、3 つのマッピングすべてに同じマッピング値を使用してください。 [!DNL The Trade Desk] れらすべてを 1 つのセグメントに集計し、デバイスレベルの分類を行います。

![ セグメントマッピング ID](../../assets/common/segment-mapping-id.png)

## 書き出したデータ {#exported-data}

データがに正常に [!DNL The Trade Desk] の宛先に書き出されたかどうかを確認するには、[!DNL Trade Desk] アカウントを確認します。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。
