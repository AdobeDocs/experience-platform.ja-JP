---
title: LiveRamp 識別子に基づいてキュレーションされた宛先に対するオーディエンスのアクティブ化
type: Tutorial
description: LiveRamp RampID を使用して、Adobe Experience Platformからコネクテッドな TV やオーディオの宛先へのオーディエンスやその他の統合機能をアクティブ化する方法について説明します。
exl-id: 37e5bab9-588f-40b3-b65b-68f1a4b868f1
source-git-commit: c2e308b5e743f07062be9a34e23c4bc700b27463
workflow-type: tm+mt
source-wordcount: '660'
ht-degree: 1%

---

# LiveRamp 識別子に基づいてキュレーションされた宛先に対するオーディエンスのアクティブ化

Adobe Real-Time CDPと [!DNL LiveRamp] の統合を使用すると、以下に示すような接続された TV やオーディオの宛先など、アクティベーションに [!DNL [LiveRamp RampID]](https://docs.liveramp.com/connect/en/interpreting-rampid,-liveramp-s-people-based-identifier.html) を使用する、キュレートされた宛先リストに対してオーディエンスをアクティブ化できます。

>[!IMPORTANT]
>
>を取り込む必要はありません。また、Experience Platformインターフェイスで LiveRamp Ramp RampID を使用して作業する必要もありません。
>
> 公式の [LiveRamp ドキュメント ](https://docs.liveramp.com/connect/en/identity-and-identifier-terms-and-concepts.html#known-identifiers) に記載されているように、PII ベースの識別子、既知の識別子、カスタム ID など、Real-Time CDPから ID を書き出すことができます。 その後、これらの ID は、アクティブ化プロセス [!DNL LiveRamp RampIDs] さらにダウンストリームで照合されます。


* [[!DNL 4C Insights]](#insights)
* [[!DNL Acast]](#acast)
* [[!DNL Ampersand.tv]](#ampersand-tv)
* [[!DNL Captify]](#captify)
* [[!DNL Cardlytics]](#cardlytics)
* [[!DNL Disney (Hulu/ESPN/ABC)]](#disney)
* [[!DNL iHeartMedia]](#iheartmedia)
* [[!DNL Index Exchange]](#index-exchange)
* [[!DNL Magnite CTV Platform]](#magnite)
* [[!DNL Magnite DV+ (Rubicon Project)]](#magnite-dv)
* [[!DNL Nexxen]](#nexxen)
* [[!DNL One Fox]](#fox)
* [[!DNL Pandora]](#pandora)
* [[!DNL Reddit]](#reddit)
* [[!DNL Roku]](#roku)
* [[!DNL Spotify]](#spotify)
* [[!DNL Taboola]](#taboola)
* [[!DNL TargetSpot]](#targetspot)
* [[!DNL Teads]](#teads)
* [[!DNL WB Discovery]](#wb-discovery)

この記事では、Real-Time CDP UI から直接、Real-Time CDPから上記の宛先に対してオーディエンスをアクティブ化するために必要なワークフローについて説明します。

## 有効化ワークフロー {#workflow}

以下の画像に示すように、2 つの手順を実行し、[LiveRamp - オンボーディング ](../catalog/advertising/liveramp-onboarding.md) および [LiveRamp – 配布 ](../catalog/advertising/liveramp-distribution.md) 宛先を使用して、接続された TV およびオーディオの宛先に対してオーディエンスをアクティブ化します。

![LiveRamp を通じて、Real-Time CDPからキュレーションされた宛先にオーディエンスをアクティブ化するワークフローを示す図。](../assets/ui/activate-curated-destinations-liveramp/workflow-diagram.png){width="1920" zoomable="yes"}

まず、オーディエンスをReal-Time CDPから [[!DNL LiveRamp - Onboarding]](../catalog/advertising/liveramp-onboarding.md) の宛先に CSV ファイルとして書き出します。

オーディエンスを書き出したら、[[!DNL LiveRamp - Distribution]](../catalog/advertising/liveramp-distribution.md) の宛先を使用してアクティブ化します。

>[!TIP]
>
>このプロセスにより、アクティベーションのために [!DNL LiveRamp] アカウントにログインしなくても、Real-Time CDP UI から直接、[[!DNL Roku]](../catalog/advertising/liveramp-distribution.md#roku)、[[!DNL Disney]](../catalog/advertising/liveramp-distribution.md#disney) などの宛先に対してオーディエンスをアクティブ化できます。

### ビデオチュートリアル {#video}

このページで説明するワークフローのエンドツーエンドの説明については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3425367)

### 手順 1:[!DNL LiveRamp - Onboarding] の宛先を介して、Experience Platformから LiveRamp にオーディエンスを送信する {#onboarding}

LiveRamp RampID に基づいてキュレーションされた宛先に対してオーディエンスをアクティブ化するには、まず、オーディエンスをExperience Platformから [!DNL LiveRamp]**に** 書き出す」必要があります。

これを行うには、**[!DNL LiveRamp - Onboarding]** の宛先を使用します。

![LiveRamp - オンボーディングの宛先カードを示すExperience PlatformUI 画像 ](../assets/ui/activate-curated-destinations-liveramp/liveramp-onboarding-catalog.png)

[!DNL LiveRamp - Onboarding] の宛先を設定し、宛先からオーディエンスを書き出す方法については、[[!DNL LiveRamp - Onboarding]](../catalog/advertising/liveramp-onboarding.md) のExperience Platformドキュメントを参照してください。

>[!IMPORTANT]
>
>ファイルを [!DNL LiveRamp - Onboarding] の宛先に書き出す場合、Platform では各 [ 結合ポリシー ID](../../profile/merge-policies/overview.md) に対して 1 つの CSV ファイルを生成します。 LiveRamp へのデータ書き出しを検証する方法について詳しくは、[[!DNL LiveRamp - Onboarding]](../catalog/advertising/liveramp-onboarding.md) 宛先ドキュメントを参照してください。


オーディエンスを LiveRamp に正常に書き出したら、[ 手順 2](#distribution) に進みます。

>[!TIP]
>
>[ 手順 2](#distribution) に移動する前に、オーディエンスが LiveRamp に正常に書き出されたことを [ 検証 ](../catalog/advertising/liveramp-onboarding.md#exported-data) します。 [ 宛先データフローの監視 ](../../dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) に関するドキュメントを参照し、[[!DNL LiveRamp - Onboarding]](../catalog/advertising/liveramp-onboarding.md#exported-data) の特定の監視の詳細をお読みください。

### 手順 2:[!DNL LiveRamp - Distribution] の宛先を介して、接続された TV およびオーディオの宛先に対してオンボーディングされたオーディエンスをアクティブ化する {#distribution}

オーディエンスが LiveRamp に正常に書き出されたことを [ 検証 ](../catalog/advertising/liveramp-onboarding.md#exported-data) したら、[[!DNL Roku]](../catalog/advertising/liveramp-distribution.md#roku)、[[!DNL Disney]](../catalog/advertising/liveramp-distribution.md#disney) などの目的の宛先に対してオーディエンスをアクティブ化します。

**[!DNL LiveRamp - Distribution]** の宛先を使用して、（手順 1](#onboarding) で書き出した [ オーディエンスをアクティブ化します。

![LiveRamp – 配布先カードを示すExperience PlatformUI 画像 ](../assets/ui/activate-curated-destinations-liveramp/liveramp-distribution-catalog.png)

**[!DNL LiveRamp - Distribution]** の宛先を設定し、[ 手順 1](#onboarding) で書き出したオーディエンスをアクティブ化する方法については、[[!DNL LiveRamp - Distribution]](../catalog/advertising/liveramp-distribution.md) の宛先のドキュメントを参照してください。

>[!IMPORTANT]
>
>**[!DNL LiveRamp - Distribution]** 宛先の **オーディエンス選択** 手順では、*手順 1](#onboarding) で [LiveRamp - オンボーディング ](../catalog/advertising/liveramp-onboarding.md) 宛先に書き出した* まったく同じオーディエンス [ を選択する必要があります。

**[!DNL LiveRamp - Distribution]** の宛先を設定する場合、使用するダウンストリーム宛先（Roku、Disney など）ごとに専用の接続を作成する必要があります。

>[!TIP]
>
>宛先に名前を付ける場合、Adobeでは次の形式をお勧めします。`LiveRamp - Downstream Destination Name` この命名パターンにより、宛先ワークスペースの [ 参照 ](../ui/destinations-workspace.md#browse) タブで宛先をすばやく識別することができます。
><br>
>例：`LiveRamp - Roku`。

![ 複数の LiveRamp 宛先を示す Platform UI のスクリーンショット。](../assets/ui/activate-curated-destinations-liveramp/liveramp-naming.png)

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

[[!DNL LiveRamp - Onboarding]](../catalog/advertising/liveramp-onboarding.md) ースの宛先へのオーディエンスの書き出しが成功したことを検証するには、[ 宛先データフローの監視 ](../../dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) に関するドキュメントを参照し、[[!DNL LiveRamp - Onboarding]](../catalog/advertising/liveramp-onboarding.md#exported-data) の特定の監視の詳細を確認します。

選択した広告プラットフォーム（Roku、Disney など）へのオーディエンスのアクティベーションが成功したことを検証するには、宛先プラットフォームアカウントにログインし、アクティベーション指標を確認します。
