---
keywords: プロファイルリクエストの宛先のアクティブ化、データのアクティブ化、プロファイルリクエストの宛先
title: プロファイルリクエストの宛先に対するオーディエンスデータのアクティブ化（ベータ版）
type: Tutorial
seo-title: Activate audience data to profile request destinations
description: セグメントをプロファイルリクエストの宛先にマッピングして、Adobe Experience Platformでのオーディエンスデータをアクティブ化する方法について説明します。
seo-description: Learn how to activate the audience data you have in Adobe Experience Platform by mapping segments to profile request destinations.
exl-id: cd7132eb-4047-4faa-a224-47366846cb56
source-git-commit: dd9493077706b102467493e90b363ac202550eee
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 9%

---

# プロファイルリクエストの宛先に対するオーディエンスデータのアクティブ化

## 概要 {#overview}

この記事では、Adobe Experience Platformプロファイルリクエストの宛先でオーディエンスデータをアクティブ化するために必要なワークフローについて説明します。 プロファイルリクエストの宛先の例は、 [Adobe Target](../../destinations/catalog/personalization/adobe-target-connection.md) そして [カスタムパーソナライゼーション](../../destinations/catalog/personalization/custom-personalization.md) 接続。

## 前提条件 {#prerequisites}

To activate data to destinations, you must have successfully [connected to a destination](./connect-destination.md). If you haven&#39;t done so already, go to the [destinations catalog](../catalog/overview.md), browse the supported destinations, and configure the destination that you want to use.

### Segment merge policy {#merge-policy}

現在、プロファイルリクエストの宛先は、 [デフォルトの結合ポリシー](../../segmentation/ui/segment-builder.md#merge-policies).

## 宛先を選択 {#select-destination}

1. Go to **[!UICONTROL Connections > Destinations]**, and select the **[!UICONTROL Catalog]** tab.

   ![「宛先カタログ」タブ](../assets/ui/activate-segment-streaming-destinations/catalog-tab.png)

1. 選択 **[!UICONTROL セグメントのアクティブ化]** を選択します。

   ![Activate buttons](../assets/ui/activate-profile-request-destinations/activate-segments-button.png)

1. セグメントのアクティブ化に使用する宛先接続を選択し、「 」を選択します。 **[!UICONTROL 次へ]**.

   ![宛先を選択](../assets/ui/activate-profile-request-destinations/select-destination.png)

1. 次のセクションに移動： [セグメントを選択](#select-segments).

## セグメントを選択 {#select-segments}

セグメント名の左側にあるチェックボックスを使用して、宛先に対してアクティブ化するセグメントを選択し、「 」を選択します。 **[!UICONTROL 次へ]**.

![Select segments](../assets/ui/activate-profile-request-destinations/select-segments.png)

## セグメントの書き出しをスケジュール {#scheduling}

デフォルトでは、 [!UICONTROL セグメントスケジュール] ページには、現在のアクティベーションフローで選択した新しく選択されたセグメントのみが表示されます。

![新しいセグメント](../assets/ui/activate-profile-request-destinations/new-segments.png)

To see all the segments being activated to your destination, use the filtering option and disable the **[!UICONTROL Show new segments only]** filter.

![All segments](../assets/ui/activate-profile-request-destinations/all-segments.png)

On the **[!UICONTROL Segment schedule]** page, select each segment, then use the **[!UICONTROL Start date]** and **[!UICONTROL End date]** selectors to configure the time interval for sending data to your destination.

![Segment schedule](../assets/ui/activate-profile-request-destinations/segment-schedule.png)

Select **[!UICONTROL Next]** to go to the [!UICONTROL Review] page.

## レビュー {#review}

「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>この手順では、Adobe Experience Platformはデータ使用ポリシーの違反を確認します。 Shown below is an example where a policy is violated. 違反を解決するまで、セグメントのアクティベーションワークフローを完了することはできません。 ポリシー違反の解決方法については、 [ポリシーの適用](../../rtcdp/privacy/data-governance-overview.md#enforcement) （データガバナンスに関するドキュメントの節）。

![data policy violation](../assets/common/data-policy-violation.png)

ポリシー違反が検出されていない場合は、「 」を選択します。 **[!UICONTROL 完了]** をクリックして選択を確定し、宛先へのデータの送信を開始します。

![レビュー](../assets/ui/activate-profile-request-destinations/review.png)

## セグメントのアクティベーションを検証 {#verify}

Check the [destination monitoring documentation](../../dataflows/ui/monitor-destinations.md) for detailed information on how to monitor the flow of data to your destinations.
