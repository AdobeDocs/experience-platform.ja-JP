---
keywords: セグメントのストリーミング宛先のアクティブ化；セグメントのストリーミング宛先のアクティブ化；データのアクティブ化
title: ストリーミングセグメントの書き出し先に対するオーディエンスデータのアクティブ化
type: Tutorial
seo-title: ストリーミングセグメントの書き出し先に対するオーディエンスデータのアクティブ化
description: セグメントをセグメントストリーミング宛先にマッピングして、Adobe Experience Platformで保有するオーディエンスデータをアクティブ化する方法を説明します。
seo-description: セグメントをセグメントストリーミング宛先にマッピングして、Adobe Experience Platformで保有するオーディエンスデータをアクティブ化する方法を説明します。
source-git-commit: c3e273c66ffe0542258e5418104e0bcf154f5235
workflow-type: tm+mt
source-wordcount: '740'
ht-degree: 6%

---


# ストリーミングセグメントの書き出し先に対するオーディエンスデータのアクティブ化

## 概要 {#overview}

この記事では、Adobe Experience Platformのセグメントストリーミング宛先でオーディエンスデータをアクティブ化するために必要なワークフローについて説明します。

## 前提条件 {#prerequisites}

宛先へのデータをアクティブ化するには、宛先](./connect-destination.md)に[接続している必要があります。 まだの場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照して、使用する宛先を設定します。

## 宛先の選択 {#select-destination}

1. **[!UICONTROL 接続/宛先]**&#x200B;に移動し、「**[!UICONTROL 参照]**」タブを選択します。

   ![「宛先の参照」タブ](../assets/ui/activate-segment-streaming-destinations/browse-tab.png)

1. 次の図に示すように、セグメントをアクティブ化する宛先に対応する「**[!UICONTROL Add segments]**」ボタンを選択します。

   ![ボタンの有効化](../assets/ui/activate-segment-streaming-destinations/activate-buttons-browse.png)

1. 次のセクションに移動して[セグメントを選択します](#select-segments)。

## セグメントの選択 {#select-segments}

セグメント名の左側にあるチェックボックスを使用して、宛先に対してアクティブ化するセグメントを選択し、「**[!UICONTROL 次へ]**」を選択します。

![セグメントの選択](../assets/ui/activate-segment-streaming-destinations/select-segments.png)

## 属性とIDのマッピング {#mapping}

>[!IMPORTANT]
>
>この手順は、一部のセグメントストリーミング宛先にのみ適用されます。 宛先に&#x200B;**[!UICONTROL マッピング]**&#x200B;手順がない場合は、「[セグメントのエクスポートをスケジュール](#scheduling)」にスキップします。

一部のセグメントストリーミング宛先では、宛先のターゲットIDとしてマッピングするために、ソース属性またはID名前空間を選択する必要があります。

1. **[!UICONTROL マッピング]**&#x200B;ページで、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。

   ![新しいマッピングの追加](../assets/ui/activate-segment-streaming-destinations/add-new-mapping.png)

1. **[!UICONTROL ソースフィールド]**&#x200B;エントリの右側にある矢印を選択します。

   ![ソースフィールドの選択](../assets/ui/activate-segment-streaming-destinations/select-source-field.png)

1. **[!UICONTROL ソースフィールド]**&#x200B;を選択ページで、**[!UICONTROL 属性を選択]**&#x200B;または&#x200B;**[!UICONTROL ID名前空間を選択]**&#x200B;オプションを使用して、使用可能なソースフィールドの2つのカテゴリを切り替えます。 使用可能な[!DNL XDM]プロファイル属性とID名前空間から、宛先にマッピングするプロファイル属性を選択し、「****」を選択します。

   ![ソースフィールドページを選択](../assets/ui/activate-segment-streaming-destinations/source-field-page.png)

1. 「**[!UICONTROL ターゲットフィールド]**」エントリの右側にあるボタンを選択します。

   ![ターゲットフィールドの選択](../assets/ui/activate-segment-streaming-destinations/select-target-field.png)

1. **[!UICONTROL ターゲットフィールド]**&#x200B;を選択ページで、ソースフィールドのマッピング先のターゲットID名前空間を選択し、「**[!UICONTROL 選択]**」を選択します。

   ![ターゲットフィールドページを選択](../assets/ui/activate-segment-streaming-destinations/target-field-page.png)

1. マッピングをさらに追加するには、手順1 ～ 5を繰り返します。

### 変換の適用 {#apply-transformation}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_applytransformation"
>title="変換の適用"
>abstract="非ハッシュ化のソースフィールドを使用する場合は、このオプションをオンにして、Adobe Experience Platformでアクティベーション時に自動的にハッシュ化するようにします。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-segment-streaming-destinations.html?lang=en#apply-transformation" text="詳しくは、ドキュメントを参照してください。"

ハッシュ化されていないソース属性を、宛先でハッシュ化が必要なターゲット属性にマッピングする場合(例：`email_lc_sha256`または`phone_sha256`)で、「**変換を適用**」オプションをオンにして、Adobe Experience Platformがアクティブ化時にソース属性を自動的にハッシュ化するようにします。

![IDマッピング](/help/destinations/assets/ui/activate-segment-streaming-destinations/mapping-summary.png)


## スケジュールセグメントの書き出し {#scheduling}

1. **[!UICONTROL セグメントスケジュール]**&#x200B;ページで、各セグメントを選択し、**[!UICONTROL 開始日]**&#x200B;および&#x200B;**[!UICONTROL 終了日]**&#x200B;セレクターを使用して、宛先にデータを送信する時間間隔を設定します。

   ![セグメントスケジュール](../assets/ui/activate-segment-streaming-destinations/segment-schedule.png)

   * 一部の宛先では、カレンダーセレクターの下にあるドロップダウンメニューを使用して、各セグメントの&#x200B;**[!UICONTROL オーディエンスの接触チャネル]**&#x200B;を選択する必要があります。 宛先にこのセレクターが含まれていない場合は、この手順をスキップします。

      ![マッピング ID](../assets/ui/activate-segment-streaming-destinations/origin-of-audience.png)

   * 一部の宛先では、[!DNL Platform]セグメントをターゲットの宛先の対応するセグメントに手動でマッピングする必要があります。 それには、各セグメントを選択し、宛先プラットフォームから対応するセグメントIDを「**[!UICONTROL マッピングID]**」フィールドに入力します。 宛先にこのフィールドが含まれていない場合は、この手順をスキップします。

      ![マッピング ID](../assets/ui/activate-segment-streaming-destinations/mapping-id.png)

   * 一部の宛先では、[!DNL IDFA]または[!DNL GAID]セグメントをアクティブ化する際に、**[!UICONTROL アプリID]**&#x200B;を入力する必要があります。 宛先にこのフィールドが含まれていない場合は、この手順をスキップします。

      ![アプリ ID](../assets/ui/activate-segment-streaming-destinations/destination-appid.png)

1. **[!UICONTROL 次へ]**&#x200B;を選択して、[!UICONTROL レビュー]ページに移動します。

## レビュー {#review}

「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>この手順では、Adobe Experience Platformはデータ使用ポリシーの違反を確認します。 次に、ポリシーに違反する例を示します。 違反を解決するまで、セグメントのアクティベーションワークフローを完了することはできません。 ポリシー違反の解決方法について詳しくは、データガバナンスのドキュメントの節の「[ポリシーの適用](../../rtcdp/privacy/data-governance-overview.md#enforcement)」を参照してください。

![データポリシー違反](../assets/common/data-policy-violation.png)

ポリシー違反が検出されなかった場合は、「**[!UICONTROL 完了]**」を選択して選択内容を確認し、宛先へのデータの送信を開始します。

![レビュー](../assets/ui/activate-segment-streaming-destinations/review.png)

## セグメントのアクティベーションの検証 {#verify}

宛先アカウントを確認します。 アクティブ化に成功した場合、オーディエンスは宛先プラットフォームに入力されます。

<!-- 
For [!DNL Facebook Custom Audience], a successful activation means that a [!DNL Facebook] custom audience would be created programmatically in [[!UICONTROL Facebook Ads Manager]](https://www.facebook.com/adsmanager/manage/). Segment membership in the audience would be added and removed as users are qualified or disqualified for the activated segments.

>[!TIP]
>
>The integration between Adobe Experience Platform and [!DNL Facebook] supports historical audience backfills. All historical segment qualifications are sent to [!DNL Facebook] when you activate the segments to the destination.
-->
