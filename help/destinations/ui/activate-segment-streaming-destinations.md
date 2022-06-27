---
keywords: セグメントストリーミング先のアクティブ化；セグメントストリーミング先のアクティブ化；データのアクティブ化
title: ストリーミングセグメント書き出し宛先に対するオーディエンスデータの有効化
type: Tutorial
description: セグメントをセグメントストリーミング宛先にマッピングして、Adobe Experience Platformでのオーディエンスデータをアクティブ化する方法について説明します。
exl-id: bb61a33e-38fc-4217-8999-9eb9bf899afa
source-git-commit: a6fe0f5a0c4f87ac265bf13cb8bba98252f147e0
workflow-type: tm+mt
source-wordcount: '832'
ht-degree: 36%

---

# ストリーミングセグメント書き出し宛先に対するオーディエンスデータの有効化

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

## 概要 {#overview}

この記事では、Adobe Experience Platformのセグメントストリーミング宛先でオーディエンスデータをアクティブ化するために必要なワークフローについて説明します。

## 前提条件 {#prerequisites}

宛先へのデータをアクティベートするには、正常に[宛先に接続する](./connect-destination.md)必要があります。まだ接続していない場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照し、使用する宛先を設定します。

## 宛先を選択 {#select-destination}

1. **[!UICONTROL 接続／宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![「宛先カタログ」タブ](../assets/ui/activate-segment-streaming-destinations/catalog-tab.png)

1. 以下に示す画像のように、セグメントをアクティベートする宛先に対応するカードで「**[!UICONTROL セグメントのアクティベート]**」を選択します。

   ![ボタンの有効化](../assets/ui/activate-segment-streaming-destinations/activate-segments-button.png)

1. セグメントをアクティベートするために使用する宛先接続を選択し、「**[!UICONTROL 次へ]**」を選択します。

   ![宛先を選択](../assets/ui/activate-segment-streaming-destinations/select-destination.png)

1. 次のセクションの「[セグメントを選択](#select-segments)」に移動します。

## セグメントを選択 {#select-segments}

セグメント名の左側にあるチェックボックスを使用して、宛先に対してアクティベートするセグメントを選択し、「**[!UICONTROL 次へ]**」を選択します。

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
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-segment-streaming-destinations.html#apply-transformation" text="詳しくは、ドキュメントを参照してください"

ハッシュ化されていないソース属性を、宛先でハッシュ化される必要があるターゲット属性にマッピングする場合 ( 例： `email_lc_sha256` または `phone_sha256`)、 **変換を適用** アクティブ化時にAdobe Experience Platformがソース属性を自動的にハッシュ化するオプションが追加されました。

![ID マッピング](../assets/ui/activate-segment-streaming-destinations/mapping-summary.png)

## セグメントの書き出しをスケジュールする {#scheduling}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_enddate"
>title="終了日"
>abstract="セグメントスケジュールの終了日の追加は使用できません。"
>additional-url="https://www.adobe.com/go/destinations-activate-segment-scheduling-en" text="詳しくは、ドキュメントを参照してください"

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
>このステップでは、Adobe Experience Platform はデータ使用ポリシーの違反がないかを確認します。ポリシーに違反した場合の例を次に示します。違反を解決するまで、セグメントのアクティベーションワークフローを完了することはできません。ポリシー違反の解決方法については、データガバナンスに関するドキュメントの[ポリシーの適用](../../rtcdp/privacy/data-governance-overview.md#enforcement)を参照してください。

![データポリシー違反](../assets/common/data-policy-violation.png)

ポリシー違反が検出されていない場合は、「**[!UICONTROL 完了]**」を選択して選択内容を確定し、宛先へのデータの送信を開始します。

![レビュー](../assets/ui/activate-segment-streaming-destinations/review.png)

## セグメントのアクティベーションを検証 {#verify}

次を確認します。 [宛先の監視に関するドキュメント](../../dataflows/ui/monitor-destinations.md) を参照してください。

<!-- 
For [!DNL Facebook Custom Audience], a successful activation means that a [!DNL Facebook] custom audience would be created programmatically in [[!UICONTROL Facebook Ads Manager]](https://www.facebook.com/adsmanager/manage/). Segment membership in the audience would be added and removed as users are qualified or disqualified for the activated segments.

>[!TIP]
>
>The integration between Adobe Experience Platform and [!DNL Facebook] supports historical audience backfills. All historical segment qualifications are sent to [!DNL Facebook] when you activate the segments to the destination.
-->
