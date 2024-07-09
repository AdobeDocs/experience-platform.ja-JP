---
keywords: 広告；bing;
title: Microsoft Bing 接続
description: Microsoft Bing の接続先を使用すると、ディスプレイ広告、検索、ネイティブを含むMicrosoft Advertising ネットワーク全体でリターゲティングとオーディエンスターゲットのデジタルキャンペーンを実行できます。
exl-id: e1c0273b-7e3c-4d77-ae14-d1e528ca0294
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 52%

---

# [!DNL Microsoft Bing] 接続 {#bing-destination}

## 概要 {#overview}

の使用 [!DNL Microsoft Bing] 全体へのプロファイルデータの送信先 [!DNL Microsoft Advertising Network]を含む [!DNL Display Advertising], [!DNL Search]、および [!DNL Native].

この [!DNL Microsoft Bing] 宛先の作成 *[!DNL Custom Audiences]* Microsoftで。 これらは、次の場所から利用できます [!DNL Microsoft Search Network] および [!DNL Audience Network] （[!DNL Native] /[!DNL Display] /[!DNL Programmatic]）に一覧表示されます。 [Microsoft Advertising ドキュメント](https://help.ads.microsoft.com/#apex/ads/en/56892/1-500).

プロファイルデータをに送信する [!DNL Microsoft Bing]最初に宛先に接続する必要があります。

## ユースケース {#use-cases}

マーケターは、から構築されたオーディエンスを使用できるようにしたいと考えています [!DNL Microsoft Advertising IDs] でのディスプレイまたは検索広告を介したユーザーのターゲット設定 [!DNL Microsoft Advertising] チャネル：

## サポートされている ID {#supported-identities}

[!DNL Microsoft Bing] では、以下の表に示す ID に基づいたオーディエンスのアクティベーションがサポートされています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ID | 説明 |
|---|---|
| MAID | MICROSOFT ADVERTISING ID |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

**[!DNL Audience Export]** - オーディエンスのすべてのメンバーをにエクスポートしています [!DNL Microsoft Bing] の宛先。

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | オーディエンスのすべてのメンバーをに書き出します。 [!DNL Microsoft Bing] の宛先。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>を使用した最初の宛先を作成する場合 [!DNL Microsoft Bing] を有効にしていません。 [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) 過去のExperience CloudID サービス（Adobe Audience Managerまたはその他のアプリケーションを使用）では、Adobe Consultingまたはカスタマーケアに連絡して、ID 同期を有効にしてもらってください。 以前に Audience Manager で [!DNL Microsoft Bing] 統合を設定していた場合、設定した ID 同期は Platform に引き継がれます。

宛先を設定する際には、次の情報を指定する必要があります。

* [!UICONTROL アカウント ID]：これはです [!DNL Bing Ads CID]（整数フォーマット）。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 宛先の詳細の入力 {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**：あなたの [!DNL Bing Ads Customer ID] （CID）。 CID は整数で、にログインしたときに URL 内に表示されます。 [!DNL Microsoft Advertising].

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
>データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

が含まれる [オーディエンススケジュール](../../ui/activate-segment-streaming-destinations.md#scheduling) 手順では、でオーディエンス名を手動でマッピングする必要があります [!UICONTROL マッピング ID] フィールド。 これにより、オーディエンスメタデータがに正しく渡されます [!DNL Bing].

![オーディエンス名を Bing マッピング ID にマッピングする方法の例を示すオーディエンススケジュール画面を示す UI 画像。](../../assets/catalog/advertising/bing/mapping-id.png)

## 書き出したデータ {#exported-data}

データがに正常に [!DNL Microsoft Bing] の宛先に書き出されたかどうかを確認するには、[!DNL Microsoft Bing Ads] アカウントを確認します。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。
