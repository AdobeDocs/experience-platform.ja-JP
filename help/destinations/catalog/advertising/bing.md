---
keywords: 広告；bing;
title: Microsoft Bing 接続
description: Microsoft Bing の接続先を使用すると、ディスプレイ広告、検索、ネイティブを含むMicrosoft Advertising ネットワーク全体でリターゲティングとオーディエンスターゲットのデジタルキャンペーンを実行できます。
exl-id: e1c0273b-7e3c-4d77-ae14-d1e528ca0294
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '688'
ht-degree: 49%

---

# [!DNL Microsoft Bing] 接続 {#bing-destination}

## 概要 {#overview}

[!DNL Microsoft Bing] の宛先を使用して、[!DNL Display Advertising]、[!DNL Search]、[!DNL Native] を含む [!DNL Microsoft Advertising Network] 全体にプロファイルデータを送信します。

[!DNL Microsoft Bing] の宛先は、Microsoftに *[!DNL Custom Audiences]* を作成します。 これらは、[Microsoft Advertisingのドキュメント ](https://help.ads.microsoft.com/#apex/ads/en/56892/1-500) に記載されているように、[!DNL Microsoft Search Network] と [!DNL Audience Network] （[!DNL Native] /[!DNL Display] /[!DNL Programmatic]）の両方で使用できます。

プロファイルデータを [!DNL Microsoft Bing] に送信するには、まず宛先に接続する必要があります。

## ユースケース {#use-cases}

マーケターは、[!DNL Microsoft Advertising IDs] ークフローに基づいて作成されたオーディエンスを使用し、あらゆるチャネルにわたるディスプレイまたは検索広告を通じてユーザーをターゲティングで [!DNL Microsoft Advertising] るようになりたいと考えています。

## サポートされている ID {#supported-identities}

[!DNL Microsoft Bing] では、以下の表に示す ID に基づいたオーディエンスのアクティベーションをサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ID | 説明 |
|---|---|
| MAID | MICROSOFT ADVERTISING ID |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

**[!DNL Audience Export]** - オーディエンスのすべてのメンバーを [!DNL Microsoft Bing] の宛先に書き出します。

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | オーディエンスのすべてのメンバーを [!DNL Microsoft Bing] の宛先に書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>[!DNL Microsoft Bing] での最初の宛先を作成しようとしており、これまで（Adobe Audience Managerなどのアプリケーションを使用して）Experience Cloud ID サービスで [ID 同期機能 ](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) を有効にしたことがない場合は、Adobe Consultingまたはカスタマーケアに連絡して ID 同期を有効にしてもらってください。 以前にAudience Managerで [!DNL Microsoft Bing] 統合を設定していた場合、設定した ID 同期はExperience Platformに引き継がれます。

宛先を設定する際には、次の情報を指定する必要があります。

* [!UICONTROL &#x200B; アカウント ID]：整数フォーマットの [!DNL Bing Ads CID] です。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 宛先の詳細の入力 {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**：お使いの [!DNL Bing Ads Customer ID] （CID）。 CID は整数で、[!DNL Microsoft Advertising] にログインしたときに URL 内に表示されます。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!CONTEXTUALHELP]
>id="platform_destinations_bing_mapping_id"
>title="マッピング ID"
>abstract="選択したセグメントをマッピングする、数値の Bing オーディエンス ID を入力します。指定された[!UICONTROL マッピング ID] が Bing 宛先のオーディエンス ID に対応していない場合、Bing アカウントの期待されるオーディエンスデータは表示されません。"

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

[ オーディエンススケジュール ](../../ui/activate-segment-streaming-destinations.md#scheduling) 手順では、「[!UICONTROL &#x200B; マッピング ID]」フィールドにオーディエンス名を手動でマッピングする必要があります。 これにより、オーディエンスメタデータが [!DNL Bing] に正しく渡されます。

![ オーディエンス名を Bing マッピング ID にマッピングする方法の例を示すオーディエンススケジュール画面を示す UI 画像。](../../assets/catalog/advertising/bing/mapping-id.png)

## 書き出したデータ {#exported-data}

データがに正常に [!DNL Microsoft Bing] の宛先に書き出されたかどうかを確認するには、[!DNL Microsoft Bing Ads] アカウントを確認します。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。
