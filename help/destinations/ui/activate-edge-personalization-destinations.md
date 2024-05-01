---
title: エッジパーソナライゼーションの宛先に対してオーディエンスをアクティブ化する
description: 同じページと次のページのパーソナライゼーションのユースケースに対して、Adobe Experience Platformからエッジパーソナライゼーションの宛先へのオーディエンスをアクティブ化する方法を説明します。
type: Tutorial
exl-id: cd7132eb-4047-4faa-a224-47366846cb56
source-git-commit: c113d9615a276af67714f38b8325e69737b23964
workflow-type: tm+mt
source-wordcount: '1957'
ht-degree: 15%

---


# エッジパーソナライゼーションの宛先に対してオーディエンスをアクティブ化する

## 概要 {#overview}

Adobe Experience Platformが使用する [エッジのセグメント化](../../segmentation/ui/edge-segmentation.md) ～と共に [エッジの宛先](/help/destinations/destination-types.md#edge-personalization-destinations) お客様が大規模かつリアルタイムでオーディエンスを作成およびターゲット設定できるようにします。 この機能は、同じページおよび次のページのパーソナライゼーションのユースケースを設定するのに役立ちます。

エッジ宛先の例は次のとおりです [Adobe Target](../../destinations/catalog/personalization/adobe-target-connection.md) および [カスタムパーソナライゼーション](../../destinations/catalog/personalization/custom-personalization.md) 接続。

>[!NOTE]
>
>条件 [Adobe Target接続の設定](../catalog/personalization/adobe-target-connection.md) *なし* データストリーム ID を使用した場合、この記事で説明されているユースケースはサポートされていません。 データストリームがない場合は、次のセッションのパーソナライゼーションのユースケースのみがサポートされます。

>[!IMPORTANT]
> 
> * データをアクティブ化してを有効にするには [マッピングステップ](#mapping) ワークフローで、次が必要です **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions).
> * を経由せずにデータをアクティブ化するには [マッピングステップ](#mapping) ワークフローで、次が必要です **[!UICONTROL 宛先の表示]**, **[!UICONTROL マッピングを使用しないセグメントのアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions).
>* エクスポートする *id*、が必要です **[!UICONTROL ID グラフの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png "宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。"){width="100" zoomable="yes"}
> 
> [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この記事では、Adobe Experience Platform Edge 宛先に対してオーディエンスをアクティブ化するために必要なワークフローについて説明します。 と併用した場合 [エッジのセグメント化](../../segmentation/ui/edge-segmentation.md) およびオプション [プロファイル属性のマッピング](#mapping)これらの宛先により、web プロパティおよびモバイルプロパティで、同じページおよび次のページのパーソナライゼーションのユースケースが可能になります。

エッジパーソナライゼーション用にAdobe Target接続を設定する方法の概要については、以下のビデオをご覧ください。

>[!NOTE]
>
>Adobe Experience Platform のユーザーインターフェイスは頻繁に更新され、このビデオが録画された後に変更されている可能性があります。 最新の情報については、以下の節で説明する設定手順を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/3418799/?quality=12&learn=on)

Adobe Targetとカスタムパーソナライゼーションの宛先に対してオーディエンスとプロファイル属性を共有する方法の概要については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3419036/?quality=12&learn=on)

## ユースケース {#use-cases}

Adobe TargetなどのAdobeのパーソナライゼーションソリューションや、独自のパーソナライゼーションパートナープラットフォーム（例： [!DNL Optimizely], [!DNL Pega]）に加えて、独自のシステム（社内の CMS など）を使用して、を介してより深い顧客パーソナライゼーションエクスペリエンスを強化します [カスタムパーソナライゼーション](../catalog/personalization/custom-personalization.md) の宛先。 これらすべてを、Experience PlatformEdge Networkのデータ収集およびセグメント化機能も活用して行います。

以下に説明するユースケースには、サイトのパーソナライゼーションとターゲットを設定したオンサイト広告の両方が含まれます。

これらのユースケースを可能にするには、オーディエンス情報とプロファイル属性情報の両方をExperience Platformから取得し、その情報を以下のどちらかに送信する、迅速かつ合理化された方法が必要です [Adobe Target](../catalog/personalization/adobe-target-connection.md) または [カスタムパーソナライゼーション](../catalog/personalization/custom-personalization.md) Experience PlatformUI での接続。

### 同じページのパーソナライズ機能 {#same-page}

ユーザーが web サイトのページにアクセスします。 現在のページ訪問情報（参照 URL、ブラウザー言語、埋め込み製品情報など）を使用して、次のアクションや決定（パーソナライゼーションなど）を選択するには、 [カスタムパーソナライゼーション](../catalog/personalization/custom-personalization.md) Adobe以外のプラットフォーム用の接続（例： [!DNL Pega], [!DNL Optimizely] など）。

### 次のページのパーソナライゼーション {#next-page}

ユーザーが web サイトのページ A にアクセスします。 このインタラクションに基づいて、ユーザーは一連のオーディエンスに対して選定されました。 次に、ユーザーがページ A からページ B に移動するリンクをクリックします。ページ A での以前のインタラクション中にユーザーが選定したオーディエンスは、現在の web サイト訪問によって決定されたプロファイルの更新と共に、次のアクションまたは決定（例えば、訪問者に表示する広告バナー、A/B テストの場合は表示するページのバージョン）の強化に使用されます。

### 次のセッションのパーソナライズ機能 {#next-session}

ユーザーが web サイトの複数のページにアクセスします。 これらのインタラクションに基づいて、ユーザーは一連のオーディエンスに対して選定されました。 次に、ユーザーは現在の閲覧セッションを終了します。

次の日に、ユーザーは同じ顧客 web サイトに戻ります。 訪問したすべての web サイトページとの以前のインタラクション中に選定したオーディエンスは、現在の web サイト訪問によって決定されたプロファイルの更新と共に、次のアクション/決定（例えば、訪問者に表示する広告バナー、A/B テストの場合は表示するページのバージョン）の選択に使用されます。

### ホームページバナーのパーソナライズ {#home-page-banner}

あるホームレンタルや販売会社が、Adobe Experience Platformでのオーディエンスの選定に基づいて、バナー付きのホームページをパーソナライズしたいと考えています。 会社は、パーソナライズされたエクスペリエンスを体験できるオーディエンスを選択し、それらのオーディエンスを Target オファーのターゲティング条件としてAdobe Targetに送信できます。

## 前提条件 {#prerequisites}

### データ収集 UI でのデータストリームの設定 {#configure-datastream}

パーソナライゼーションの宛先の設定における最初の手順は、Experience Platform Web SDK のデータストリームを設定することです。 これは Data Collection UI で行います。

データストリームを設定する際に、「**[!UICONTROL Adobe Experience Platform]**」で「**[!UICONTROL エッジセグメンテーション]**」と「**[!UICONTROL パーソナライゼーションの宛先]**」の両方が選択されていることを確認します。

>[!TIP]
>
>2024 年 4 月リリース以降、次の場合に「エッジセグメント化」チェックボックスをオンにする必要はありません [Adobe Targetへの接続の設定](/help/destinations/catalog/personalization/adobe-target-connection.md). この場合、 [次のセッションのパーソナライズ機能](#next-session) は、利用できるパーソナライゼーションのユースケースのみです。

![エッジセグメント化とパーソナライゼーションの宛先がハイライト表示されたデータストリーム設定。](../assets/ui/activate-edge-personalization-destinations/datastream-config.png)

データストリームの設定方法の詳細については、[Platform Web SDK ドキュメント](../../datastreams/configure.md#aep)に記載されている手順に従ってください。

### を作成 [!DNL Active-On-Edge] 結合ポリシー {#create-merge-policy}

宛先接続を作成したら、 [!DNL Active-On-Edge] 結合ポリシー。 この [!DNL Active-On-Edge] 結合ポリシーは、オーディエンスが常に評価されるようにします [端に](../../segmentation/ui/edge-segmentation.md) およびは、リアルタイムおよび次のページのパーソナライゼーションのユースケースで使用できます。

>[!IMPORTANT]
>
>現在、エッジ宛先では、 [アクティブオンエッジ結合ポリシー](../../segmentation/ui/segment-builder.md#merge-policies) をデフォルトとして設定します。 異なる結合ポリシーを使用するオーディエンスをエッジ宛先にマッピングした場合、それらのオーディエンスは評価されません。

[結合ポリシーの作成](../../profile/merge-policies/ui-guide.md#create-a-merge-policy)の手順に従い、「**[!UICONTROL エッジでアクティブ化結合ポリシー]**」切り替えスイッチを必ず有効にします。

### Platform での新しいオーディエンスの作成 {#create-audience}

を作成したら [!DNL Active-On-Edge] 結合ポリシーの場合、Platform で新しいオーディエンスを作成する必要があります。

に従う [audience builder](../../segmentation/ui/segment-builder.md) 新しいオーディエンスを作成し、次のことを確認するためのガイド [割り当て](../../segmentation/ui/segment-builder.md#merge-policies) この [!DNL Active-On-Edge] 前の手順で作成した結合ポリシー。

### 宛先接続の作成 {#connect-destination}

データストリームを設定したら、パーソナライゼーションの宛先の設定を開始できます。

新しい宛先接続の作成方法に関する詳細な手順については、[宛先接続の作成チュートリアル](../ui/connect-destination.md)に従ってください。

設定する宛先に応じて、次の記事で宛先固有の前提条件と関連情報について確認してください。

* [Adobe Target 接続](../catalog/personalization/adobe-target-connection.md#parameters)
* [カスタムパーソナライゼーション接続](../catalog/personalization/custom-personalization.md##parameters)

## 宛先の選択 {#select-destination}

前提条件を満たしたら、同じページと次のページのパーソナライゼーションに使用するエッジパーソナライゼーションの宛先を選択できます。

1. **[!UICONTROL 接続／宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![Experience PlatformUI でハイライト表示された「宛先カタログ」タブ。](../assets/ui/activate-edge-personalization-destinations/catalog-tab.png)

1. を選択 **[!UICONTROL オーディエンスをアクティベート]** 次の画像に示すように、オーディエンスをアクティベートするパーソナライゼーションの宛先に対応するカードで。

   ![カタログの宛先カードでハイライト表示されたオーディエンスコントロールをアクティブ化。](../assets/ui/activate-edge-personalization-destinations/activate-audiences-button.png)

1. オーディエンスをアクティベートするために使用する宛先接続を選択してから、を選択します **[!UICONTROL 次]**.

   ![アクティベーションワークフローの宛先ステップを選択します。](../assets/ui/activate-edge-personalization-destinations/select-destination.png)

1. 次のセクションに移動： [オーディエンスを選択](#select-audiences).

## オーディエンスを選択 {#select-audiences}

オーディエンス名の左側にあるチェックボックスを使用して、宛先に対してアクティベートするオーディエンスを選択し、次に「」を選択します **[!UICONTROL 次]**.

宛先に対してアクティブ化するオーディエンスを選択するには、オーディエンス名の左側にあるチェックボックスを使用したあと、「」を選択します。 **[!UICONTROL 次]**.

接触チャネルに応じて、複数のタイプのオーディエンスから選択できます。

* **[!UICONTROL セグメント化サービス]**:Segmentation Service によってExperience Platform内で生成されたオーディエンス。 を参照してください。 [セグメント化ドキュメント](../../segmentation/ui/overview.md) を参照してください。
* **[!UICONTROL カスタムアップロード]**:Experience Platform以外で生成され、CSV ファイルとして Platform にアップロードされたオーディエンス。 外部オーディエンスについて詳しくは、のドキュメントを参照してください。 [オーディエンスのインポート](../../segmentation/ui/overview.md#import-audience).
* その他のAdobeソリューションから生成される、次のようなオーディエンスのタイプ [!DNL Audience Manager].

![複数のオーディエンスがハイライト表示された、アクティベーションワークフローのオーディエンス選択手順。](../assets/ui/activate-edge-personalization-destinations/select-audiences.png)

## 属性のマッピング {#mapping}

>[!IMPORTANT]
>
>プロファイル属性には、機密データが含まれている場合があります。 このデータを保護するには、 **[!UICONTROL カスタムパーソナライゼーション]** 宛先では、を使用する必要があります [Edge Networkサーバー API](../../server-api/overview.md) 属性ベースのパーソナライゼーションの宛先を設定する場合。 すべての Server API 呼び出しは、 [認証済みコンテキスト](../../server-api/authentication.md).
>
><br>既に統合に Web SDK または Mobile SDK を使用している場合は、サーバーサイド統合を追加することで、Server API を介して属性を取得できます。
>
><br>上記の要件に従わない場合、パーソナライゼーションはオーディエンスメンバーシップのみに基づきます。

ユーザーに対してパーソナライゼーションのユースケースを有効にする対象に基づいて属性を選択します。 つまり、属性の値が変更された場合や属性がプロファイルに追加された場合、そのプロファイルはオーディエンスのメンバーになり、パーソナライゼーションの宛先に対してアクティブ化されます。

属性の追加はオプションです。引き続き次の手順に進み、属性を選択せずに同じページおよび次のページのパーソナライゼーションを有効にすることができます。 この手順で属性を追加しない場合でも、プロファイルのオーディエンスメンバーシップおよび ID マップの選定に基づいて、パーソナライゼーションが実行されます。

![属性が選択されたマッピングステップを示す画像](../assets/ui/activate-edge-personalization-destinations/mapping-step.png)

### ソース属性を選択 {#select-source-attributes}

ソース属性を追加するには、 **[!UICONTROL 新しいフィールドを追加]** に対する制御 **[!UICONTROL ソースフィールド]** 列を選択し、目的の XDM 属性フィールドを検索するか、移動します（下図を参照）。

![マッピングステップでターゲット属性を選択する方法を示す画面録画。](../assets/ui/activate-edge-personalization-destinations/mapping-step-select-attribute.gif)

### ターゲット属性を選択 {#select-target-attributes}

ターゲット属性を追加するには、 **[!UICONTROL 新しいフィールドを追加]** に対する制御 **[!UICONTROL ターゲットフィールド]** ソース属性のマッピング先となるカスタム属性名を列に入力します。

>[!NOTE]
>
>ターゲット属性の選択は、 [カスタムパーソナライゼーション](../catalog/personalization/custom-personalization.md) 宛先プラットフォームでわかりやすい名前のフィールドマッピングをサポートするためのアクティベーションワークフローです。

![マッピングステップで XDM 属性を選択する方法を示す画面録画](../assets/ui/activate-edge-personalization-destinations/mapping-step-select-target-attribute.gif)

## オーディエンスの書き出しのスケジュール {#scheduling}

デフォルトでは、 [!UICONTROL オーディエンススケジュール] ページには、現在のアクティベーションフローで選択した、新しく選択したオーディエンスのみが表示されます。

宛先に対してアクティブ化されているすべてのオーディエンスを表示するには、フィルタリングオプションを使用して、 **[!UICONTROL 新しいオーディエンスのみを表示]** フィルター。

![ハイライト表示されたすべてのオーディエンスフィルター。](../assets/ui/activate-edge-personalization-destinations/all-audiences.png)

日 **[!UICONTROL オーディエンススケジュール]** ページで各オーディエンスを選択してから、 **[!UICONTROL 開始日]** および **[!UICONTROL 終了日]** セレクター：宛先にデータを送信する時間間隔を設定します。

![開始日と終了日がハイライト表示された、アクティベーションワークフローのオーディエンススケジュールステップ。](../assets/ui/activate-edge-personalization-destinations/audience-schedule.png)

を選択 **[!UICONTROL 次]** に移動します [!UICONTROL レビュー] ページ。

## レビュー {#review}

「**[!UICONTROL レビュー]**」ページには、選択内容の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

![レビュー手順の選択の概要。](../assets/ui/activate-edge-personalization-destinations/review.png)

### 同意ポリシーの評価 {#consent-policy-evaluation}

組織で **Adobe Healthcare Shield** または **Adobe Privacy &amp; Security Shield** を購入した場合、**[!UICONTROL 適用可能な同意ポリシーを表示]**&#x200B;を選択すると、どの同意ポリシーが適用され、その結果、いくつのプロファイルがアクティベーションに含まれるかを確認することができます。詳細を読む [同意ポリシーの評価](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) を参照してください。

### データ使用ポリシーのチェック {#data-usage-policy-checks}

が含まれる **[!UICONTROL レビュー]** また、Experience Platformはデータ使用ポリシーの違反がないかどうかを確認します。 ポリシーに違反した場合の例を次に示します。違反を解決するまで、Audience Activation ワークフローを完了することはできません。 ポリシー違反の解決方法については、以下を参照してください [データ使用ポリシーの違反](/help/data-governance/enforcement/auto-enforcement.md#data-usage-violation) データガバナンスに関するドキュメントの節を参照してください。

![データポリシー違反の例。](../assets/common/data-policy-violation.png)

### オーディエンスのフィルタリング {#filter-audiences}

この手順では、ページで使用可能なフィルターを使用して、このワークフローの一環としてスケジュールまたはマッピングが更新されたオーディエンスのみを表示できます。 また、表示するテーブル列を切り替えることもできます。

![レビューステップで使用可能なオーディエンスフィルターを示す画面録画。](../assets/ui/activate-edge-personalization-destinations/filter-audiences-review-step.gif)

選択内容に満足し、ポリシー違反が検出されていない場合は、を選択します **[!UICONTROL 終了]** をクリックして選択内容を確認し、宛先へのデータの送信を開始します。

<!--

Commenting out this part since destination monitoring is not available currently for the Adobe Target and Custom Personalization destinations.

## Verify audience activation {#verify}

Check the [destination monitoring documentation](../../dataflows/ui/monitor-destinations.md) for detailed information on how to monitor the flow of data to your destinations.

-->
