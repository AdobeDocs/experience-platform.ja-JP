---
keywords: 広告；トレードデスク；advertising トレードデスク
title: Trade Desk 接続
description: Trade Desk は、広告購入者がディスプレイ、ビデオ、モバイルの在庫ソース全体でリターゲティングやオーディエンスをターゲットにしたデジタルキャンペーンを実行するためのセルフサービスプラットフォームです。
exl-id: b8f638e8-dc45-4aeb-8b4b-b3fa2906816d
source-git-commit: f078d7b20bc16bf1a6cca065e5e6fba85d9d0648
workflow-type: tm+mt
source-wordcount: '1066'
ht-degree: 28%

---

# [!DNL The Trade Desk] 接続

## 概要 {#overview}


>[!IMPORTANT]
>
> 2025 年 7 月からの宛先サービスへの [ 内部アップグレード ](../../../release-notes/2025/july-2025.md#destinations) に続いて、**へのデータフローで** アクティブ化されたプロファイルの数が減少 [!DNL The Trade Desk] する場合があります。
> &#x200B;> このドロップは、この宛先プラットフォームへのすべてのアクティベーションに対して **ECID マッピング要件** が導入されたことによって発生します。 詳しくは、このページの [ 必須マッピング ](#mandatory-mappings) の節を参照してください。
>
>**変更点：**
>
>* すべてのプロファイルアクティベーションで、ECID （Experience Cloud ID）マッピングが **必須** になりました。
>* ECID マッピングのないプロファイルは、既存のアクティベーションデータフローから **ドロップ** されます。
>
>**必要な手順：**
>
>* オーディエンスデータを確認し、プロファイルに有効な ECID 値があることを確認します。
>* アクティベーション指標を監視して、予想されるプロファイル数を確認します。


この宛先コネクタを使用して、プロファイルデータを [!DNL The Trade Desk] に送信します。 このコネクタは、[!DNL The Trade Desk] のファーストパーティエンドポイントにデータを送信します。 Adobe Experience Platformと [!DNL The Trade Desk] の統合では、[!DNL The Trade Desk] サードパーティのエンドポイントへのデータの書き出しはサポートされていません。

[!DNL The Trade Desk] は、広告購入者がディスプレイ、ビデオ、モバイルなどの在庫ソースをまたいで、リターゲティングやオーディエンスをターゲットにしたデジタルキャンペーンを実行するためのセルフサービスプラットフォームです。

プロファイルデータを [!DNL The Trade Desk] に送信するには、最初に宛先に接続する必要があります。このページの次の節で説明しています。

## ユースケース {#use-cases}

マーケターは、[!DNL Trade Desk IDs] またはデバイス ID から作成されたオーディエンスを使用して、リターゲティングやオーディエンスターゲット設定のデジタルキャンペーンを作成できるようにしたいと考えています。

## サポートされている ID {#supported-identities}

[!DNL The Trade Desk] では、以下の表に示す ID に基づいたオーディエンスのアクティベーションをサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

宛先でサポートされている ID[!DNL The Trade Desk] 以下に示します。 これらの ID を使用して、オーディエンスをアクティブ化して [!DNL The Trade Desk] ーザーに対してアクティブ化できます。

以下の表に示す ID はすべて必須マッピングです。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| ECID | Experience Cloud ID | この ID は、統合が正しく機能するために必須ですが、オーディエンスのアクティベーションには使用されません。 |
| Trade Desk ID | [!DNL The Trade Desk] プラットフォームの広告主 ID | Trade Desk 独自の ID に基づいてオーディエンスをアクティブ化する場合は、この ID を使用します。 |

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
* **[!UICONTROL アカウント ID]**：お使いの [!DNL The Trade Desk] [!UICONTROL &#x200B; アカウント ID]。
* **[!UICONTROL サーバーの場所]**:[!DNL The Trade Desk] の担当者に、使用する地域サーバーを問い合わせてください。 使用可能な地域サーバーを次の中から選択できます。

   * **[!UICONTROL APAC]**
   * **[!UICONTROL 中国]**
   * **[!UICONTROL 東京]**
   * **[!UICONTROL 英国/EU]**
   * **[!UICONTROL 米国東海岸]**
   * **[!UICONTROL 米国西海岸]**

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

### 必須のマッピング {#mandatory-mappings}

[ サポートされている ID](#supported-identities) の節で説明されているすべてのターゲット ID は、オーディエンスアクティベーションワークフローのマッピング手順でマッピングする必要があります。 これには以下が含まれます。

* [!DNL GAID] （Google Advertising ID）
* [!DNL IDFA] （広告主のApple ID）
* [!DNL ECID] （Experience Cloud ID）
* [!DNL The Trade Desk ID]

![ 必須マッピングを示すスクリーンショット ](../../assets/catalog/advertising/tradedesk/mandatory-mappings.png)

すべてのターゲット ID をマッピングすることで、アクティブ化で、存在する任意の ID を使用してプロファイルを正しく分割および配信できるようにします。 つまり、すべての ID が各プロファイルに存在する必要があるわけではありません。

Trade Desk への書き出しを正常に行うには、プロファイルに次の情報が含まれている必要があります。

* [!DNL ECID] および
* [!DNL GAID]、[!DNL IDFA]、[!DNL The Trade Desk ID] のうち少なくとも 1 つ

例：

* [!DNL ECID] のみ：書き出されていません
* [!DNL ECID] + [!DNL The Trade Desk ID]：書き出されました
* [!DNL ECID] + [!DNL IDFA]：書き出されました
* [!DNL ECID] + [!DNL GAID]：書き出されました
* [!DNL IDFA] + [!DNL The Trade Desk ID] （[!DNL ECID] なし）：書き出されていません

>[!NOTE]
> 
>[2025 年 7 月のアップグレード ](/help/release-notes/2025/july-2025.md#destinations) に続いて、宛先サービスへのマッピング [!DNL ECID] 適用されます。 [!DNL ECID] が見つからないプロファイルは、期待どおりにドロップされるようになりました。これにより、従来の動作と比較してアクティベーション数が少ない可能性があります。

## 書き出したデータ {#exported-data}

データがに正常に [!DNL The Trade Desk] の宛先に書き出されたかどうかを確認するには、[!DNL The Trade Desk] アカウントを確認します。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。
