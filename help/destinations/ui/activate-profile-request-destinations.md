---
keywords: プロファイルリクエストの宛先のアクティブ化、データのアクティブ化、プロファイルリクエストの宛先
title: プロファイルリクエスト宛先に対するオーディエンスデータの有効化
type: Tutorial
seo-title: Activate audience data to profile request destinations
description: セグメントをプロファイルリクエストの宛先にマッピングして、Adobe Experience Platformでのオーディエンスデータをアクティブ化する方法について説明します。
seo-description: Learn how to activate the audience data you have in Adobe Experience Platform by mapping segments to profile request destinations.
exl-id: cd7132eb-4047-4faa-a224-47366846cb56
source-git-commit: 0b094e635e6d22e58e5aa79a374df0879167a833
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 56%

---

# プロファイルリクエスト宛先に対するオーディエンスデータの有効化

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

## 概要 {#overview}

この記事では、Adobe Experience Platformプロファイルリクエストの宛先でオーディエンスデータをアクティブ化するために必要なワークフローについて説明します。 プロファイルリクエストの宛先の例は、 [Adobe Target](../../destinations/catalog/personalization/adobe-target-connection.md) そして [カスタムパーソナライゼーション](../../destinations/catalog/personalization/custom-personalization.md) 接続。

## 前提条件 {#prerequisites}

宛先へのデータをアクティベートするには、正常に[宛先に接続する](./connect-destination.md)必要があります。まだ接続していない場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照し、使用する宛先を設定します。

### セグメント結合ポリシー {#merge-policy}

現在、プロファイルリクエストの宛先は、 [エッジ上のアクティブな結合ポリシー](../../segmentation/ui/segment-builder.md#merge-policies) をデフォルトとして設定します。

## 宛先を選択 {#select-destination}

1. **[!UICONTROL 接続／宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![「宛先カタログ」タブ](../assets/ui/activate-segment-streaming-destinations/catalog-tab.png)

1. 以下に示す画像のように、セグメントをアクティベートする宛先に対応するカードで「**[!UICONTROL セグメントのアクティベート]**」を選択します。

   ![ボタンの有効化](../assets/ui/activate-profile-request-destinations/activate-segments-button.png)

1. セグメントをアクティベートするために使用する宛先接続を選択し、「**[!UICONTROL 次へ]**」を選択します。

   ![宛先を選択](../assets/ui/activate-profile-request-destinations/select-destination.png)

1. 次のセクションの「[セグメントを選択](#select-segments)」に移動します。

## セグメントを選択 {#select-segments}

セグメント名の左側にあるチェックボックスを使用して、宛先に対してアクティベートするセグメントを選択し、「**[!UICONTROL 次へ]**」を選択します。

![セグメントを選択](../assets/ui/activate-profile-request-destinations/select-segments.png)

## セグメントの書き出しをスケジュールする {#scheduling}

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
>このステップでは、Adobe Experience Platform はデータ使用ポリシーの違反がないかを確認します。ポリシーに違反した場合の例を次に示します。違反を解決するまで、セグメントのアクティベーションワークフローを完了することはできません。ポリシー違反の解決方法については、データガバナンスに関するドキュメントの[ポリシーの適用](../../rtcdp/privacy/data-governance-overview.md#enforcement)を参照してください。

![データポリシー違反](../assets/common/data-policy-violation.png)

ポリシー違反が検出されていない場合は、「**[!UICONTROL 完了]**」を選択して選択内容を確定し、宛先へのデータの送信を開始します。

![レビュー](../assets/ui/activate-profile-request-destinations/review.png)

<!--

Commenting out this part since destination monitoring is not available currently for the Adobe Target and Custom Personalization destinations.

## Verify segment activation {#verify}

Check the [destination monitoring documentation](../../dataflows/ui/monitor-destinations.md) for detailed information on how to monitor the flow of data to your destinations.

-->