---
keywords: 広告；ビング
title: Microsoft Bing 接続
description: Microsoft Bing の接続先を使用すると、Microsoft Display Advertising をまたいで、再ターゲティングとオーディエンスにターゲットを絞ったデジタルキャンペーンを実行できます。
exl-id: e1c0273b-7e3c-4d77-ae14-d1e528ca0294
source-git-commit: aec9708680c2a4cb3c70af12f95c67ec37b2e129
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 64%

---

# [!DNL Microsoft Bing] 接続 {#bing-destination}

## 概要 {#overview}

この [!DNL Microsoft Bing] の宛先は、次にプロファイルデータを送信する際に役立ちます： [!DNL Microsoft Display Advertising].

プロファイルデータをに送信するには [!DNL Microsoft Bing]の場合、最初に宛先に接続する必要があります。

## ユースケース {#use-cases}

マーケターは、 [!DNL Microsoft Advertising IDs] をまたいで、ディスプレイ広告を介してユーザーをターゲットにします。 [!DNL Microsoft Advertising] チャネル。

## サポートされている ID {#supported-identities}

[!DNL Microsoft Bing] では、以下の表で説明する ID のアクティベーションをサポートしています。[ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 |
|---|---|
| MAID | Microsoft Advertising ID |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

**[!DNL Segment Export]**  — セグメント（オーディエンス）のすべてのメンバーを [!DNL Microsoft Bing] 宛先。

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントの書き出し]** | セグメント（オーディエンス）のすべてのメンバーをに書き出しています [!DNL Microsoft Bing] 宛先。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>を使用して最初の宛先を作成する場合は、 [!DNL Microsoft Bing] 有効にしていない [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) 以前のExperience CloudID サービス (Adobe Audience Managerや他のアプリケーションを使用 ) では、Adobeコンサルティングまたはカスタマーケアに問い合わせて、ID 同期を有効にしてください。 以前に Audience Manager で [!DNL Microsoft Bing] 統合を設定していた場合、設定した ID 同期は Platform に引き継がれます。

宛先を設定する際は、次の情報を指定する必要があります。

* [!UICONTROL アカウント ID]:こちらは [!DNL Bing Ads CID]（整数形式）

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**:お使いの [!DNL Bing Ads Customer ID] (CID) を使用します。 CID は整数で、にログインする際に URL に含まれます。 [!DNL Microsoft Advertising].

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!CONTEXTUALHELP]
>id="platform_destinations_bing_mapping_id"
>title="マッピング ID"
>abstract="選択したセグメントをマッピングする、数値の Bing セグメント ID を入力します。指定された[!UICONTROL マッピング ID] が Bing 宛先のセグメント ID に対応していない場合、Bing アカウントの期待されるオーディエンスデータは表示されません。"

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントをアクティブ化する手順は、[ストリーミングセグメント書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

内 [セグメントスケジュール](../../ui/activate-segment-streaming-destinations.md#scheduling) 手順を実行する場合は、 [!UICONTROL マッピング ID] フィールドに入力します。 これにより、セグメントメタデータがに正しく渡されます。 [!DNL Bing].

![セグメント名を Bing マッピング ID にマッピングする方法の例を示す、セグメントスケジュール画面を示す UI 画像。](../../assets/catalog/advertising/bing/mapping-id.png)

## 書き出したデータ {#exported-data}

データがに正常に [!DNL Microsoft Bing] の宛先に書き出されたかどうかを確認するには、[!DNL Microsoft Bing Ads] アカウントを確認します。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。
