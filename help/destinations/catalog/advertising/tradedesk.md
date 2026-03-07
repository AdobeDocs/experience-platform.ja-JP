---
keywords: 広告；トレードデスク；advertising トレードデスク
title: Trade Desk 接続
description: Trade Desk は、広告購入者がディスプレイ、ビデオ、モバイルの在庫ソース全体でリターゲティングやオーディエンスをターゲットにしたデジタルキャンペーンを実行するためのセルフサービスプラットフォームです。
exl-id: b8f638e8-dc45-4aeb-8b4b-b3fa2906816d
source-git-commit: 82ff222d22255b9c99de76111d25d4a3cf6f2d5c
workflow-type: tm+mt
source-wordcount: '1376'
ht-degree: 22%

---

# [!DNL The Trade Desk] 接続

## 概要 {#overview}

この宛先コネクタを使用して、プロファイルデータを [!DNL The Trade Desk] に送信します。 このコネクタは、[!DNL The Trade Desk] のファーストパーティエンドポイントにデータを送信します。 Adobe Experience Platformと [!DNL The Trade Desk] の統合では、[!DNL The Trade Desk] サードパーティのエンドポイントへのデータの書き出しはサポートされていません。

[!DNL The Trade Desk] は、広告購入者がディスプレイ、ビデオ、モバイルなどの在庫ソースをまたいで、リターゲティングやオーディエンスをターゲットにしたデジタルキャンペーンを実行するためのセルフサービスプラットフォームです。

プロファイルデータを [!DNL The Trade Desk] に送信するには、最初に宛先に接続する必要があります。このページの次の節で説明しています。

## ユースケース {#use-cases}

マーケターは、[!DNL Trade Desk IDs] またはデバイス ID から作成されたオーディエンスを使用して、リターゲティングやオーディエンスターゲット設定のデジタルキャンペーンを作成できるようにしたいと考えています。

## サポートされている ID {#supported-identities}

[!DNL The Trade Desk] では、以下の表に示す ID に基づいたオーディエンスのアクティベーションをサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

宛先でサポートされている ID[!DNL The Trade Desk] 以下に示します。 これらの ID を使用して、オーディエンスをアクティブ化して [!DNL The Trade Desk] ーザーに対してアクティブ化できます。

以下の表に示すすべての ID は、アクティベーション時に事前設定され、自動的にマッピングされます。 アクティブ化ワークフローでこれらのマッピングを手動で設定する必要はありません。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | GOOGLE ADVERTISING ID | GAID がプロファイルに存在する場合にアクティブ化されます。 |
| IDFA | Apple の広告主 ID | IDFA がプロファイルに存在する場合にアクティブ化されます。 |
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。詳しくは、[ECID](/help/identity-service/features/ecid.md) に関する次のドキュメントを参照してください。 |
| [!DNL Tradedesk] | [!DNL TDID] プラットフォームでの [!DNL The Trade Desk] | プロファイルに ECID があり、Experience Platformに ECID と Trade デスク ID のマッピングが存在する場合にアクティブ化されます。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの接触チャネル | ○ | このカテゴリには、[!DNL Segmentation Service] を通じて生成されたオーディエンス以外のすべてのオーディエンスの接触チャネルが含まれます。 [&#x200B; 様々なオーディエンスのオリジン &#x200B;](/help/segmentation/ui/audience-portal.md#customize) について確認する。 次に例を示します。 <ul><li> csv ファイルからExperience Platformへのカスタムアップロードオーディエンス [&#x200B; 読み込み &#x200B;](../../../segmentation/ui/audience-portal.md#import-audience)</li><li> 類似オーディエンス、 </li><li> 連合オーディエンス、 </li><li> Adobe Journey Optimizerなど、他のExperience Platform アプリで生成されたオーディエンス。 </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスデータタイプでサポートされるオーディエンス：

| オーディエンスデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [&#x200B; 人物オーディエンス &#x200B;](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルに基づき、マーケティングキャンペーンの対象となる人物のグループを指定できます。 | 頻繁な購入、買い物かごの放棄 |
| [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) | × | アカウントベースのマーケティング戦略では、特定の組織内の個人をターゲットに設定します。 | B2B マーケティング |
| [&#x200B; 見込み客オーディエンス &#x200B;](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないものの、ターゲットオーディエンスと特性を共有する個人をターゲットに設定します。 | サードパーティデータを使用した予測 |
| [&#x200B; データセットの書き出し &#x200B;](/help/catalog/datasets/overview.md) | × | Adobe Experience Platform Data Lake に保存された構造化データのコレクション。 | レポート、データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | オーディエンスのすべてのメンバーを宛先に書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

前提条件は、オーディエンスアクティベーションに使用する ID タイプによって異なります。

**モバイル ID のアクティベーションの場合のみ**、前提条件はありません。 顧客の ID （GAID や IDFA）を収集して管理している限り、オーディエンスのアクティブ化を開始でき [!DNL The Trade Desk] す。

**[!DNL The Trade Desk]** に基づく cookie ベースのターゲティングの場合、ECID と [!DNL Trade Desk ID] の間のマッピングが確立されていることを確認します。 これを行うには、以下の手順を実行します。

1. **ID 同期機能を有効にする**:[!DNL The Trade Desk ID] のアクティベーションを初めて設定する場合で、これまで（Adobe Audience Managerなどのアプリケーションを使用して）Experience Cloud ID サービスで [ID 同期機能 &#x200B;](https://experienceleague.adobe.com/ja/docs/id-service/using/id-service-api/methods/idsync) を有効にしたことがない場合は、Adobe Consultingまたはカスタマーケアに連絡して ID 同期を有効にしてもらってください。
   * 以前にAudience Managerで [!DNL The Trade Desk] 統合を設定している場合、既存の ID 同期は自動的にExperience Platformに引き継がれます。

2. **Web ページの実装**:Web ページにコードを実装して、[!DNL The Trade Desk ID] とAdobe ECID 間のマッピングを作成します。 これにより、Experience Platformは Trade Desk ID を顧客プロファイルに関連付けることができます。

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

### 事前設定済みマッピング {#preconfigured-mappings}

>[!CONTEXTUALHELP]
>id="platform_destinations_required_mappings_ttd"
>title="事前設定済みのマッピングセット"
>abstract="これら 4 つのマッピングセットは事前設定されています。Trade Desk に対してデータをアクティブ化する場合、アクティブ化されたオーディエンスに対して選定されたプロファイルには、必ずしも 4 つの ID すべてがプロファイルに存在している必要はありません。これは、この宛先が、ここに示すいずれかのターゲット ID で動作するためです。 <br> Trade Desk ID に基づく cookie ベースのターゲティングの場合、プロファイルに存在する ECID と、Trade Desk ID と ECID の間の ID 同期マッピングが必要です。"
>additional-url="https://experienceleague.adobe.com/jp/docs/experience-platform/destinations/catalog/advertising/tradedesk#preconfigured-mappings" text="詳しくは、事前設定されたマッピングを参照してください。"

オーディエンスアクティベーションワークフローでは、次の ID マッピングが **事前設定され、自動的に入力されます**。

* GAID （Google Advertising ID）
* IDFA （広告主のApple ID）
* ECID （Experience Cloud ID）
* [!DNL The Trade Desk ID]

![&#x200B; 必須マッピングを示すスクリーンショット &#x200B;](../../assets/catalog/advertising/tradedesk/mandatory-mappings.png)

これらのマッピングはグレー表示され、読み取り専用です。 この手順では、何も設定する必要はありません。 「**[!UICONTROL Next]**」を選択して続行します。

Experience Platformは、アクティベーションワークフローでマッピングされたオーディエンスに属する各プロファイルを、サポートされているすべての id タイプで自動的にチェックし、存在する id を使用してプロファイルをアクティベートします。

### アクティベーションタイプ別の ID 要件

**モバイル ID アクティベーション（GAID/IDFA）:** が GAID または IDFA のみのプロファイルはアクティベーションに十分です。 追加の ID や前提条件は不要です。

**Cookie ベースのターゲティング（[!DNL Trade Desk ID]）:** 次の両方が必要です。

* プロファイルに存在する ECID
* [!DNL Trade Desk ID] と ECID の間の ID 同期マッピング（[&#x200B; 前提条件 &#x200B;](#prerequisites) の節で説明されているように設定されます）

**複数の ID の動作：** プロファイルにサポートされている複数の ID が含まれている場合、[!DNL The Trade Desk] に対して各 ID が個別にアクティブ化されます。 これにより、オーディエンスのアクティベーションで最大限のリーチと柔軟性を確保します。

### アクティブ化の例

* **モバイル ID プロファイル：** GAID や IDFA を持つプロファイルは、それぞれの広告 ID を使用してアクティブ化されます。 プロファイルに GAID と IDFA の両方が含まれている場合、各 ID は別々にアクティブ化されます。
* **Cookie ベースのプロファイル：** ECID とそれに対応する [!DNL Trade Desk ID] マッピングを持つプロファイルは、Cookie ベースのターゲティングに Trade Desk ID を使用してアクティブ化されます。
* **ECID のみのプロファイル：** ECID のみを持ち、[!DNL Trade Desk ID] マッピングが存在しないプロファイルは **書き出されません**。 ECID だけでは有効化には不十分です。

## 書き出したデータ {#exported-data}

データがに正常に [!DNL The Trade Desk] の宛先に書き出されたかどうかを確認するには、[!DNL The Trade Desk] アカウントを確認します。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。
