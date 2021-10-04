---
keywords: プロファイルリクエストの宛先のアクティブ化、データのアクティブ化、プロファイルリクエストの宛先のアクティブ化
title: プロファイルリクエストの宛先に対するオーディエンスデータのアクティブ化（ベータ版）
type: Tutorial
seo-title: Activate audience data to profile request destinations
description: セグメントをプロファイルリクエストの宛先にマッピングして、Adobe Experience Platformでのオーディエンスデータをアクティブ化する方法を説明します。
seo-description: Learn how to activate the audience data you have in Adobe Experience Platform by mapping segments to profile request destinations.
exl-id: cd7132eb-4047-4faa-a224-47366846cb56
source-git-commit: ba27484655438df654a1e062309ddd30638f62a5
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 10%

---

# プロファイルリクエストの宛先に対するオーディエンスデータのアクティブ化（ベータ版）

## 概要 {#overview}

>[!IMPORTANT]
>
>Adobe Experience Platformのプロファイルリクエストの宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

この記事では、Adobe Experience Platformプロファイルリクエストの宛先でオーディエンスデータをアクティブ化するために必要なワークフローについて説明します。 プロファイルリクエストの宛先の例としては、[Adobe Target](../../destinations/catalog/personalization/adobe-target-connection.md) 接続と [ カスタムパーソナライゼーション ](../../destinations/catalog/personalization/custom-personalization.md) 接続があります。

## 前提条件 {#prerequisites}

宛先に対してデータをアクティブ化するには、宛先 ](./connect-destination.md) に [ 正常に接続されている必要があります。 まだの場合は、[ 宛先カタログ ](../catalog/overview.md) に移動し、サポートされている宛先を参照して、使用する宛先を設定します。

### セグメント結合ポリシー {#merge-policy}

現在、プロファイルリクエストの宛先は、[ デフォルトの結合ポリシー ](../../segmentation/ui/segment-builder.md#merge-policies) を使用するセグメントのアクティブ化のみをサポートしています。

## 宛先の選択 {#select-destination}

1. **[!UICONTROL 接続/宛先]** に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![「宛先カタログ」タブ](../assets/ui/activate-segment-streaming-destinations/catalog-tab.png)

1. 次の図に示すように、セグメントをアクティブ化する宛先に対応するカードで「**[!UICONTROL セグメントをアクティブ化]**」を選択します。

   ![ボタンの有効化](../assets/ui/activate-profile-request-destinations/activate-segments-button.png)

1. セグメントのアクティブ化に使用する宛先接続を選択し、「**[!UICONTROL 次へ]**」を選択します。

   ![宛先の選択](../assets/ui/activate-profile-request-destinations/select-destination.png)

1. 次のセクションに移動して [ セグメントを選択します ](#select-segments)。

## セグメントの選択 {#select-segments}

セグメント名の左側にあるチェックボックスを使用して、宛先に対してアクティブ化するセグメントを選択し、「**[!UICONTROL 次へ]**」を選択します。

![セグメントの選択](../assets/ui/activate-profile-request-destinations/select-segments.png)

## スケジュールセグメントの書き出し {#scheduling}

デフォルトでは、[!UICONTROL  セグメントスケジュール ] ページには、現在のアクティベーションフローで選択した新しく選択したセグメントのみが表示されます。

![新しいセグメント](../assets/ui/activate-profile-request-destinations/new-segments.png)

宛先に対してアクティブ化されているすべてのセグメントを表示するには、フィルタリングオプションを使用して、「**[!UICONTROL Show new segments only]**」フィルターを無効にします。

![すべてのセグメント](../assets/ui/activate-profile-request-destinations/all-segments.png)

**[!UICONTROL セグメントスケジュール]** ページで、各セグメントを選択し、**[!UICONTROL 開始日]** および **[!UICONTROL 終了日]** セレクターを使用して、宛先にデータを送信する時間間隔を設定します。

![セグメントスケジュール](../assets/ui/activate-profile-request-destinations/segment-schedule.png)

**[!UICONTROL 次へ]** を選択して、[!UICONTROL  レビュー ] ページに移動します。

## レビュー {#review}

「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>この手順では、Adobe Experience Platformはデータ使用ポリシーの違反を確認します。 次に、ポリシーに違反する例を示します。 違反を解決するまで、セグメントのアクティベーションワークフローを完了することはできません。 ポリシー違反の解決方法について詳しくは、データガバナンスのドキュメントの節の [ ポリシーの適用 ](../../rtcdp/privacy/data-governance-overview.md#enforcement) を参照してください。

![データポリシー違反](../assets/common/data-policy-violation.png)

ポリシー違反が検出されなかった場合は、「**[!UICONTROL 完了]**」を選択して選択内容を確認し、宛先へのデータの送信を開始します。

![レビュー](../assets/ui/activate-profile-request-destinations/review.png)

## セグメントのアクティベーションの検証 {#verify}

宛先へのデータのフローを監視する方法の詳細については、[ 宛先の監視に関するドキュメント ](../../dataflows/ui/monitor-destinations.md) を参照してください。
