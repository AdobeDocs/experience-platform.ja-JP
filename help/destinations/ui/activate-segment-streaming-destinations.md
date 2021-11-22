---
keywords: セグメントストリーミング先のアクティブ化；セグメントストリーミング先のアクティブ化；データのアクティブ化
title: ストリーミングセグメント書き出しの宛先に対するオーディエンスデータのアクティブ化
type: Tutorial
seo-title: Activate audience data to streaming segment export destinations
description: セグメントをセグメントストリーミング宛先にマッピングして、Adobe Experience Platformでのオーディエンスデータをアクティブ化する方法について説明します。
seo-description: Learn how to activate the audience data you have in Adobe Experience Platform by mapping segments to segment streaming destinations.
exl-id: bb61a33e-38fc-4217-8999-9eb9bf899afa
source-git-commit: 822276890b6ebed922d359f8dece58d8c90dea24
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 8%

---

# ストリーミングセグメント書き出しの宛先に対するオーディエンスデータのアクティブ化

## 概要 {#overview}

この記事では、Adobe Experience Platformのセグメントストリーミング宛先でオーディエンスデータをアクティブ化するために必要なワークフローについて説明します。

## 前提条件 {#prerequisites}

宛先に対してデータをアクティブ化するには、次の条件を満たしている必要があります。 [宛先に接続されている](./connect-destination.md). まだおこなっていない場合は、 [宛先カタログ](../catalog/overview.md)、サポートされている宛先を参照し、使用する宛先を設定します。

## 宛先を選択 {#select-destination}

1. に移動します。 **[!UICONTROL 接続/宛先]**&#x200B;をクリックし、 **[!UICONTROL カタログ]** タブをクリックします。

   ![「宛先カタログ」タブ](../assets/ui/activate-segment-streaming-destinations/catalog-tab.png)

1. 選択 **[!UICONTROL セグメントのアクティブ化]** を選択します。

   ![ボタンの有効化](../assets/ui/activate-segment-streaming-destinations/activate-segments-button.png)

1. セグメントのアクティブ化に使用する宛先接続を選択し、「 」を選択します。 **[!UICONTROL 次へ]**.

   ![宛先を選択](../assets/ui/activate-segment-streaming-destinations/select-destination.png)

1. 次のセクションに移動： [セグメントを選択](#select-segments).

## セグメントを選択 {#select-segments}

セグメント名の左側にあるチェックボックスを使用して、宛先に対してアクティブ化するセグメントを選択し、「 」を選択します。 **[!UICONTROL 次へ]**.

![セグメントを選択](../assets/ui/activate-segment-streaming-destinations/select-segments.png)

## 属性と ID のマッピング {#mapping}

>[!IMPORTANT]
>
>この手順は、一部のセグメントストリーミング宛先にのみ適用されます。 宛先に **[!UICONTROL マッピング]** ステップ、スキップ [セグメントの書き出しをスケジュール](#scheduling).

一部のセグメントストリーミングの宛先では、宛先のターゲット ID としてマッピングするために、ソース属性または ID 名前空間を選択する必要があります。

1. 内 **[!UICONTROL マッピング]** ページ、選択 **[!UICONTROL 新しいマッピングを追加]**.

   ![新しいマッピングを追加](../assets/ui/activate-segment-streaming-destinations/add-new-mapping.png)

1. 右側の矢印を選択します。 **[!UICONTROL ソースフィールド]** エントリ。

   ![ソースフィールドを選択](../assets/ui/activate-segment-streaming-destinations/select-source-field.png)

1. 内 **[!UICONTROL ソースフィールドを選択]** ページで、 **[!UICONTROL 属性を選択]** または **[!UICONTROL ID 名前空間を選択]** 使用可能なソースフィールドの 2 つのカテゴリを切り替えるためのオプション。 使用可能な [!DNL XDM] プロファイル属性と id 名前空間で、宛先にマッピングする名前空間を選択し、「 」を選択します **[!UICONTROL 選択]**.

   ![ソースフィールドページを選択](../assets/ui/activate-segment-streaming-destinations/source-field-page.png)

1. の右側にあるボタンを選択します。 **[!UICONTROL ターゲットフィールド]** エントリ。

   ![ターゲットフィールドを選択](../assets/ui/activate-segment-streaming-destinations/select-target-field.png)

1. 内 **[!UICONTROL ターゲットフィールドを選択]** ページで、ソースフィールドをマッピングするターゲット id 名前空間を選択し、「 」を選択します。 **[!UICONTROL 選択]**.

   ![ターゲットフィールドページを選択](../assets/ui/activate-segment-streaming-destinations/target-field-page.png)

1. さらにマッピングを追加するには、手順 1 ～ 5 を繰り返します。

### 変換を適用 {#apply-transformation}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_applytransformation"
>title="変換を適用"
>abstract="このオプションは、ハッシュ化されていないソースフィールドを使用する場合に、Adobe Experience Platformでアクティベーション時に自動的にハッシュ化するように、オンにします。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-segment-streaming-destinations.html?lang=en#apply-transformation" text="詳しくは、ドキュメントを参照してください。"

ハッシュ化されていないソース属性を、宛先でハッシュ化される必要があるターゲット属性にマッピングする場合 ( 例： `email_lc_sha256` または `phone_sha256`)、 **変換を適用** アクティブ化時にAdobe Experience Platformがソース属性を自動的にハッシュ化するオプションが追加されました。

![ID マッピング](../assets/ui/activate-segment-streaming-destinations/mapping-summary.png)

## セグメントの書き出しをスケジュール {#scheduling}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_enddate"
>title="終了日"
>abstract="セグメントスケジュールの終了日の追加は使用できません。"
>additional-url="https://www.adobe.com/go/destinations-activate-segment-scheduling-en" text="詳しくは、ドキュメントを参照してください。"

デフォルトでは、 [!UICONTROL セグメントスケジュール] ページには、現在のアクティベーションフローで選択した新しく選択されたセグメントのみが表示されます。

![新しいセグメント](../assets/ui/activate-segment-streaming-destinations/new-segments.png)

宛先に対してアクティブ化されているすべてのセグメントを表示するには、フィルタリングオプションを使用して、 **[!UICONTROL 新しいセグメントのみを表示]** フィルター。

![すべてのセグメント](../assets/ui/activate-segment-streaming-destinations/all-segments.png)

1. の **[!UICONTROL セグメントスケジュール]** ページで、各セグメントを選択し、 **[!UICONTROL 開始日]** および **[!UICONTROL 終了日]** セレクター：宛先にデータを送信する際の時間間隔を設定します。

   ![セグメントスケジュール](../assets/ui/activate-segment-streaming-destinations/segment-schedule.png)

   * 一部の宛先では、 **[!UICONTROL オーディエンスの起源]** カレンダーセレクターの下にあるドロップダウンメニューを使用して、セグメントごとに 宛先にこのセレクターが含まれていない場合は、この手順をスキップします。

      ![マッピング ID](../assets/ui/activate-segment-streaming-destinations/origin-of-audience.png)

   * 一部の宛先では、手動でマッピングする必要があります [!DNL Platform] セグメントをターゲットとする宛先の対応するセグメントに追加します。 これをおこなうには、各セグメントを選択し、 **[!UICONTROL マッピング ID]** フィールドに入力します。 宛先にこのフィールドが含まれていない場合は、この手順をスキップします。

      ![マッピング ID](../assets/ui/activate-segment-streaming-destinations/mapping-id.png)

   * 一部の宛先では、 **[!UICONTROL アプリ ID]** 有効化時 [!DNL IDFA] または [!DNL GAID] セグメント。 宛先にこのフィールドが含まれていない場合は、この手順をスキップします。

      ![アプリ ID](../assets/ui/activate-segment-streaming-destinations/destination-appid.png)

1. 選択 **[!UICONTROL 次へ]** 行く [!UICONTROL レビュー] ページ。

## レビュー {#review}

「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>この手順では、Adobe Experience Platformはデータ使用ポリシーの違反を確認します。 次に、ポリシーに違反した場合の例を示します。 違反を解決するまで、セグメントのアクティベーションワークフローを完了することはできません。 ポリシー違反の解決方法については、 [ポリシーの適用](../../rtcdp/privacy/data-governance-overview.md#enforcement) （データガバナンスに関するドキュメントの節）。

![データポリシー違反](../assets/common/data-policy-violation.png)

ポリシー違反が検出されていない場合は、「 」を選択します。 **[!UICONTROL 完了]** をクリックして選択を確定し、宛先へのデータの送信を開始します。

![レビュー](../assets/ui/activate-segment-streaming-destinations/review.png)

## セグメントのアクティベーションを検証 {#verify}

次を確認します。 [宛先の監視に関するドキュメント](../../dataflows/ui/monitor-destinations.md) を参照してください。

<!-- 
For [!DNL Facebook Custom Audience], a successful activation means that a [!DNL Facebook] custom audience would be created programmatically in [[!UICONTROL Facebook Ads Manager]](https://www.facebook.com/adsmanager/manage/). Segment membership in the audience would be added and removed as users are qualified or disqualified for the activated segments.

>[!TIP]
>
>The integration between Adobe Experience Platform and [!DNL Facebook] supports historical audience backfills. All historical segment qualifications are sent to [!DNL Facebook] when you activate the segments to the destination.
-->
