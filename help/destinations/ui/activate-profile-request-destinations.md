---
keywords: プロファイルリクエストの宛先のアクティブ化、データのアクティブ化、プロファイルリクエストの宛先
title: プロファイルリクエストの宛先に対するオーディエンスデータのアクティブ化
type: Tutorial
seo-title: Activate audience data to profile request destinations
description: セグメントをプロファイルリクエストの宛先にマッピングして、Adobe Experience Platformでのオーディエンスデータをアクティブ化する方法について説明します。
seo-description: Learn how to activate the audience data you have in Adobe Experience Platform by mapping segments to profile request destinations.
exl-id: cd7132eb-4047-4faa-a224-47366846cb56
source-git-commit: 388a061c87cfe9acda177ed71ed9f6017c8c2f4c
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 9%

---

# プロファイルリクエストの宛先に対するオーディエンスデータのアクティブ化

## 概要 {#overview}

この記事では、Adobe Experience Platformプロファイルリクエストの宛先でオーディエンスデータをアクティブ化するために必要なワークフローについて説明します。 プロファイルリクエストの宛先の例は、 [Adobe Target](../../destinations/catalog/personalization/adobe-target-connection.md) そして [カスタムパーソナライゼーション](../../destinations/catalog/personalization/custom-personalization.md) 接続。

## 前提条件 {#prerequisites}

宛先に対してデータをアクティブ化するには、次の条件を満たしている必要があります。 [宛先に接続されている](./connect-destination.md). まだおこなっていない場合は、 [宛先カタログ](../catalog/overview.md)、サポートされている宛先を参照し、使用する宛先を設定します。

### セグメント結合ポリシー {#merge-policy}

現在、プロファイルリクエストの宛先は、 [エッジ上のアクティブな結合ポリシー](../../segmentation/ui/segment-builder.md#merge-policies) をデフォルトとして設定します。

## 宛先を選択 {#select-destination}

1. に移動します。 **[!UICONTROL 接続/宛先]**&#x200B;をクリックし、 **[!UICONTROL カタログ]** タブをクリックします。

   ![「宛先カタログ」タブ](../assets/ui/activate-segment-streaming-destinations/catalog-tab.png)

1. 選択 **[!UICONTROL セグメントのアクティブ化]** を選択します。

   ![ボタンの有効化](../assets/ui/activate-profile-request-destinations/activate-segments-button.png)

1. セグメントのアクティブ化に使用する宛先接続を選択し、「 」を選択します。 **[!UICONTROL 次へ]**.

   ![宛先を選択](../assets/ui/activate-profile-request-destinations/select-destination.png)

1. 次のセクションに移動： [セグメントを選択](#select-segments).

## セグメントを選択 {#select-segments}

セグメント名の左側にあるチェックボックスを使用して、宛先に対してアクティブ化するセグメントを選択し、「 」を選択します。 **[!UICONTROL 次へ]**.

![セグメントを選択](../assets/ui/activate-profile-request-destinations/select-segments.png)

## セグメントの書き出しをスケジュール {#scheduling}

デフォルトでは、 [!UICONTROL セグメントスケジュール] ページには、現在のアクティベーションフローで選択した新しく選択されたセグメントのみが表示されます。

![新しいセグメント](../assets/ui/activate-profile-request-destinations/new-segments.png)

宛先に対してアクティブ化されているすべてのセグメントを表示するには、フィルタリングオプションを使用して、 **[!UICONTROL 新しいセグメントのみを表示]** フィルター。

![すべてのセグメント](../assets/ui/activate-profile-request-destinations/all-segments.png)

の **[!UICONTROL セグメントスケジュール]** ページで、各セグメントを選択し、 **[!UICONTROL 開始日]** および **[!UICONTROL 終了日]** セレクター：宛先にデータを送信する際の時間間隔を設定します。

![セグメントスケジュール](../assets/ui/activate-profile-request-destinations/segment-schedule.png)

選択 **[!UICONTROL 次へ]** 行く [!UICONTROL レビュー] ページ。

## レビュー {#review}

「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>この手順では、Adobe Experience Platformはデータ使用ポリシーの違反を確認します。 次に、ポリシーに違反した場合の例を示します。 違反を解決するまで、セグメントのアクティベーションワークフローを完了することはできません。 ポリシー違反の解決方法については、 [ポリシーの適用](../../rtcdp/privacy/data-governance-overview.md#enforcement) （データガバナンスに関するドキュメントの節）。

![データポリシー違反](../assets/common/data-policy-violation.png)

ポリシー違反が検出されていない場合は、「 」を選択します。 **[!UICONTROL 完了]** をクリックして選択を確定し、宛先へのデータの送信を開始します。

![レビュー](../assets/ui/activate-profile-request-destinations/review.png)

<!--

Commenting out this part since destination monitoring is not available currently for the Adobe Target and Custom Personalization destinations.

## Verify segment activation {#verify}

Check the [destination monitoring documentation](../../dataflows/ui/monitor-destinations.md) for detailed information on how to monitor the flow of data to your destinations.

-->