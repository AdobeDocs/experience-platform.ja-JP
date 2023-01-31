---
keywords: プロファイルリクエストの宛先のアクティブ化、データのアクティブ化、プロファイルリクエストの宛先
title: プロファイルリクエスト宛先に対するオーディエンスデータの有効化
type: Tutorial
description: セグメントをプロファイルリクエストの宛先にマッピングして、Adobe Experience Platformでのオーディエンスデータをアクティブ化する方法について説明します。
exl-id: cd7132eb-4047-4faa-a224-47366846cb56
source-git-commit: 9bde403338187409892d76de68805535de03d59f
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 23%

---

# プロファイルリクエスト宛先に対するオーディエンスデータの有効化

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

## 概要 {#overview}

この記事では、Adobe Experience Platformプロファイルリクエストの宛先でオーディエンスデータをアクティブ化するために必要なワークフローについて説明します。 と一緒に使用する場合 [エッジセグメント化](../../segmentation/ui/edge-segmentation.md)を使用すると、これらの宛先によって、Web プロパティでの同じページおよび次のページのパーソナライゼーションの使用例を可能にします。 詳細を表示 [同じページおよび次のページのパーソナライゼーションの使用例の有効化](/help/destinations/ui/configure-personalization-destinations.md).

プロファイルリクエストの宛先の例は、 [Adobe Target](../../destinations/catalog/personalization/adobe-target-connection.md) そして [カスタムパーソナライゼーション](../../destinations/catalog/personalization/custom-personalization.md) 接続。

## 前提条件 {#prerequisites}

宛先へのデータをアクティベートするには、正常に[宛先に接続する](./connect-destination.md)必要があります。まだおこなっていない場合は、 [宛先カタログ](../catalog/overview.md)、サポートされるパーソナライゼーションの宛先を参照し、使用する宛先を設定します。

### セグメント結合ポリシー {#merge-policy}

現在、プロファイルリクエストの宛先は、 [エッジ上のアクティブな結合ポリシー](../../segmentation/ui/segment-builder.md#merge-policies) をデフォルトとして設定します。

## 宛先の選択 {#select-destination}

1. **[!UICONTROL 接続／宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![「宛先カタログ」タブ](../assets/ui/activate-segment-streaming-destinations/catalog-tab.png)

1. 選択 **[!UICONTROL セグメントのアクティブ化]** を選択します。

   ![ボタンの有効化](../assets/ui/activate-profile-request-destinations/activate-segments-button.png)

1. セグメントをアクティベートするために使用する宛先接続を選択し、「**[!UICONTROL 次へ]**」を選択します。

   ![宛先を選択](../assets/ui/activate-profile-request-destinations/select-destination.png)

1. 次のセクションの「[セグメントを選択](#select-segments)」に移動します。

## セグメントを選択 {#select-segments}

セグメント名の左側にあるチェックボックスを使用して、宛先に対してアクティベートするセグメントを選択し、「**[!UICONTROL 次へ]**」を選択します。

![セグメントを選択](../assets/ui/activate-profile-request-destinations/select-segments.png)

## （ベータ版）属性のマッピング {#map-attributes}

>[!IMPORTANT]
>
>マッピング手順。 [Adobe Target](/help/destinations/catalog/personalization/adobe-target-connection.md) および [汎用パーソナライゼーションの宛先](/help/destinations/catalog/personalization/custom-personalization.md)は現在ベータ版です。お客様の組織はまだアクセスできない可能性があります。 このドキュメントは変更される場合があります。

ユーザーのパーソナライゼーションユースケースを有効にする属性を選択します。 つまり、属性の値が変更された場合、または属性がプロファイルに追加された場合、そのプロファイルはセグメントのメンバーになり、パーソナライゼーションの宛先に対してアクティブ化されます。

属性の追加はオプションで、属性を選択せずに次の手順に進み、同じページと次のページのパーソナライゼーションを有効にすることができます。 この手順で属性を追加しない場合、パーソナライゼーションは、プロファイルのセグメントメンバーシップと ID マップの認定に基づいておこなわれます。

![属性が選択されたマッピング手順を示す画像](../assets/ui/activate-profile-request-destinations/mapping-step.png)

### ソース属性を選択 {#select-source-attributes}

ソース属性を追加するには、 **[!UICONTROL 新しいフィールドを追加]** ～に対する支配 **[!UICONTROL ソースフィールド]** 列を検索し、以下に示すように、目的の XDM 属性フィールドに移動します。

![マッピング手順でターゲット属性を選択する方法を示す画面記録](../assets/ui/activate-profile-request-destinations/mapping-step-select-attribute.gif)

### ターゲット属性を選択 {#select-target-attributes}

>[!NOTE]
>
>ソース属性の選択のみが必要な宛先と、ソース属性とターゲット属性の両方が必要な宛先があります。
>
>現在、 [Adobe Target V2](../catalog/personalization/adobe-target-connection.md) 宛先にはソース属性のみが必要ですが、 [属性を含むカスタムパーソナライゼーション](../catalog/personalization/custom-personalization.md) には、source 属性と target 属性の両方が必要です。

ターゲット属性を追加するには、 **[!UICONTROL 新しいフィールドを追加]** ～に対する支配 **[!UICONTROL ターゲットフィールド]** 列を開き、ソース属性をマッピングするカスタム属性名を入力します。

![マッピング手順で XDM 属性を選択する方法を示す画面の記録](../assets/ui/activate-profile-request-destinations/mapping-step-select-target-attribute.gif)

## セグメントの書き出しをスケジュールする {#scheduling}

デフォルトでは、 [!UICONTROL セグメントスケジュール] ページには、現在のアクティベーションフローで選択した新しく選択されたセグメントのみが表示されます。

![新しいセグメント](../assets/ui/activate-profile-request-destinations/new-segments.png)

宛先に対してアクティブ化されているすべてのセグメントを表示するには、フィルタリングオプションを使用して、 **[!UICONTROL 新しいセグメントのみを表示]** フィルター。

![すべてのセグメント](../assets/ui/activate-profile-request-destinations/all-segments.png)

の **[!UICONTROL セグメントスケジュール]** ページで、各セグメントを選択し、 **[!UICONTROL 開始日]** および **[!UICONTROL 終了日]** セレクター：宛先にデータを送信する際の時間間隔を設定します。

![セグメントスケジュール](../assets/ui/activate-profile-request-destinations/segment-schedule.png)

選択 **[!UICONTROL 次へ]** 行く [!UICONTROL レビュー] ページ。

## レビュー {#review}

「**[!UICONTROL レビュー]**」ページには、選択内容の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

![レビューステップの選択の概要。](../assets/ui/activate-profile-request-destinations/review.png)

### 同意ポリシーの評価 {#consent-policy-evaluation}

組織がを購入した場合 **Adobeヘルスケアシールド** または **Adobeプライバシーとセキュリティシールド**&#x200B;を選択します。 **[!UICONTROL 該当する同意ポリシーの表示]** を使用して、適用される同意ポリシーと、その結果としてアクティベーションに含まれるプロファイルの数を確認します。 詳細 [同意ポリシーの評価](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) を参照してください。

### データ使用ポリシーのチェック {#data-usage-policy-checks}

内 **[!UICONTROL レビュー]** 手順の後、Experience Platformは、データ使用ポリシーの違反を確認します。 ポリシーに違反した場合の例を次に示します。違反を解決するまで、セグメントのアクティベーションワークフローを完了することはできません。ポリシー違反の解決方法については、 [データ使用ポリシー違反](/help/data-governance/enforcement/auto-enforcement.md#data-usage-violation) （データガバナンスに関するドキュメントの節）。

![データポリシー違反](../assets/common/data-policy-violation.png)

### セグメントのフィルタリング {#filter-segments}

また、この手順では、ページで使用可能なフィルターを使用して、このワークフローの一部としてスケジュールまたはマッピングが更新されたセグメントのみを表示できます。 また、表示するテーブル列を切り替えることもできます。

![レビューステップで使用可能なセグメントフィルターを示す画面記録。](/help/destinations/assets/ui/activate-profile-request-destinations/filter-segments-review-step.gif)

選択が完了し、ポリシー違反が検出されなかった場合は、「 」を選択します。 **[!UICONTROL 完了]** をクリックして選択を確定し、宛先へのデータの送信を開始します。

<!--

Commenting out this part since destination monitoring is not available currently for the Adobe Target and Custom Personalization destinations.

## Verify segment activation {#verify}

Check the [destination monitoring documentation](../../dataflows/ui/monitor-destinations.md) for detailed information on how to monitor the flow of data to your destinations.

-->