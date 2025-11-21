---
keywords: 広告；トレードデスク；advertising トレードデスク
title: Trade Desk 接続
description: Trade Desk は、広告購入者がディスプレイ、ビデオ、モバイルの在庫ソース全体でリターゲティングやオーディエンスをターゲットにしたデジタルキャンペーンを実行するためのセルフサービスプラットフォームです。
exl-id: b8f638e8-dc45-4aeb-8b4b-b3fa2906816d
source-git-commit: 4472548fc5b5181cdf8ef8b1666d6e1fafbce588
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 24%

---

# [!DNL The Trade Desk] 接続

## 概要 {#overview}


>[!IMPORTANT]
>
> 2025 年 7 月からの宛先サービスへの [&#x200B; 内部アップグレード &#x200B;](../../../release-notes/2025/july-2025.md#destinations) に続いて、**へのデータフローに** アクティブ化されたプロファイルの数が減少 [!DNL The Trade Desk] することがあります。
> この低下は、監視の可視性の向上によって発生します。 ECID を持たないプロファイルは、アクティブ化指標でドロップされたと正しくカウントされるようになりました。 詳しくは、このページの [&#x200B; 必須マッピング &#x200B;](#mandatory-mappings) の節を参照してください。
>
>**変更点：**
>
>* Destinations サービスは、ECID のないプロファイルがアクティベーションからドロップされた場合に、正しくレポートするようになりました。
>* **重要：** ECID を持たないプロファイルは、このアップグレードの前でさえ [!DNL The Trade Desk] に対して ECID を持つことはありませんでした。 統合には常に ECID が必要でした。 このアップグレードにより、以前はこれらのドロップが指標に表示されなかったバグが修正されました。
>
>**必要な手順：**
>
>* オーディエンスデータを確認し、プロファイルに有効な ECID 値があることを確認します。
>* アクティベーション指標を監視して、予想されるプロファイル数を確認します。 カウントが少ない場合は、宛先の動作の変更ではなく、正確なレポートを反映します。

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
| [!DNL GAID] | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| [!DNL IDFA] | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| [!DNL ECID] | Experience Cloud ID | この ID は、統合が正しく機能するために必須ですが、オーディエンスのアクティベーションには使用されません。 |
| [!DNL Tradedesk] | [!DNL TDID] プラットフォームでの [!DNL The Trade Desk] | Trade Desk 独自の ID に基づいてオーディエンスをアクティブ化する場合は、この ID を使用します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | オーディエンスのすべてのメンバーを宛先に書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>[!DNL The Trade Desk] での最初の宛先を作成しようとしており、これまで（Adobe Audience Managerなどのアプリケーションを使用して）Experience Cloud ID サービスで [ID 同期機能 &#x200B;](https://experienceleague.adobe.com/en/docs/id-service/using/id-service-api/methods/idsync) を有効にしたことがない場合は、Adobe Consultingまたはカスタマーケアに連絡して ID 同期を有効にしてもらってください。 以前にAudience Managerで [!DNL The Trade Desk] 統合を設定していた場合、設定した ID 同期はExperience Platformに引き継がれます。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Account ID]**：あなたの [!DNL The Trade Desk] [!UICONTROL Account ID]。
* **[!UICONTROL Server Location]**: [!DNL The Trade Desk] 担当者に問い合わせて、使用する地域サーバーを確認してください。 使用可能な地域サーバーを次の中から選択できます。

   * **[!UICONTROL APAC]**
   * **[!UICONTROL China]**
   * **[!UICONTROL Tokyo]**
   * **[!UICONTROL UK/EU]**
   * **[!UICONTROL US East Coast]**
   * **[!UICONTROL US West Coast]**

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

[&#x200B; オーディエンススケジュール &#x200B;](../../ui/activate-segment-streaming-destinations.md#scheduling) 手順では、オーディエンスを、宛先プラットフォームの対応する ID またはわかりやすい名前に手動でマッピングする必要があります。

オーディエンスをマッピングする場合、Adobeでは、使いやすくするために、Experience Platform オーディエンス名またはより短い形式を使用することをお勧めします。 ただし、宛先のオーディエンス ID または名前がExperience Platform アカウントのオーディエンス ID または名前と一致する必要はありません。 マッピングフィールドに挿入する値は、すべて宛先に反映されます。

### 必須のマッピング {#mandatory-mappings}

[&#x200B; サポートされている ID](#supported-identities) の節で説明されているすべてのターゲット ID は、オーディエンスアクティベーションワークフローのマッピング手順でマッピングする必要があります。 これには以下が含まれます。

* [!DNL GAID] （Google Advertising ID）
* [!DNL IDFA] （広告主のApple ID）
* [!DNL ECID] （Experience Cloud ID）
* [!DNL The Trade Desk ID]

![&#x200B; 必須マッピングを示すスクリーンショット &#x200B;](../../assets/catalog/advertising/tradedesk/mandatory-mappings.png)

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
>[2025 年 7 月のアップグレード &#x200B;](/help/release-notes/2025/july-2025.md#destinations) に続き、宛先サービスで [!DNL ECID] が欠落しているプロファイルが、アクティベーション指標でドロップとして正しくレポートされるようになりました。 これは常に統合の動作です（プロファイルに到達していない場合 [!DNL ECID] プロファイル）。ただし、ドロップはデータフロー監視で正しく表示されるよ [!DNL The Trade Desk] になりました。 アクティブ化数が少ないのは、宛先機能の変更ではなく、正確なレポートを反映しています。

## 書き出したデータ {#exported-data}

データがに正常に [!DNL The Trade Desk] の宛先に書き出されたかどうかを確認するには、[!DNL The Trade Desk] アカウントを確認します。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。
