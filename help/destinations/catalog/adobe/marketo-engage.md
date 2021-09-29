---
title: Marketo Engage先
description: Marketo Engageは、マーケティング、広告、分析、コマースに対応する、エンドツーエンドのカスタマーエクスペリエンス管理 (CXM) ソリューションのみです。 CRM のリード管理や顧客エンゲージメントから、アカウントベースのマーケティングや売上高アトリビューションに至るアクティビティを自動化および管理できます。
exl-id: 5ae5f114-47ba-4ff6-8e42-f8f43eb079f7
source-git-commit: 6ea82e0589843f15b1486e1242aa68ef8e2fe9d3
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 5%

---

# （ベータ版）Marketo Engage先 {#beta-marketo-engage-destination}

>[!IMPORTANT]
>
>Adobe Experience PlatformのMarketo Engageの宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

Marketo Engageは、マーケティング、広告、分析、コマースに対応する、エンドツーエンドのカスタマーエクスペリエンス管理 (CXM) ソリューションのみです。 CRM のリード管理や顧客エンゲージメントから、アカウントベースのマーケティングや売上高アトリビューションに至るアクティビティを自動化および管理できます。

宛先を指定すると、マーケターは、Adobe Experience Platformで作成したセグメントを静的リストとして表示されるMarketoにプッシュできます。

## サポートされる ID {#supported-identities}

| ターゲット ID | 説明 |
|---|---|
| ECID | ECID を表す名前空間。 この名前空間は、次のエイリアスでも参照できます。&quot;Adobe Marketing Cloud ID&quot;、&quot;Adobe Experience Cloud ID&quot;、&quot;Adobe Experience Platform ID&quot;。 詳しくは、[ECID](/help/identity-service/ecid.md) の次のドキュメントを参照してください。 |
| メール | 電子メールアドレスを表す名前空間。 このタイプの名前空間は、多くの場合、1 人のユーザーに関連付けられているので、異なるチャネルをまたいでそのユーザーを識別するために使用できます。 |

## 書き出しタイプ {#export-type}

セグメントのエクスポート — セグメントの宛先で使用されている識別子（電子メール、ECID）を使用して、セグメント（オーディエンス）のすべてのMarketo Engageをエクスポートします。

## 宛先の設定とセグメントのアクティブ化 {#set-up}

宛先を設定し、セグメントをアクティブ化する方法について詳しくは、Marketoドキュメントの「[Adobe Experience PlatformセグメントをMarketo静的リストにプッシュする ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/static-lists/push-an-adobe-experience-cloud-segment-to-a-marketo-static-list.html?lang=en)」を参照してください。

<!--

## Connect to the destination {#connect}

To connect to this destination, follow the steps described in the [destination configuration tutorial](../../ui/connect-destination.md).

-->

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] の宛先はすべて、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform] によるデータガバナンスの強制方法について詳しくは、[ データガバナンスの概要 ](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja) を参照してください。

<!--

## Activate segments to this destination {#activate}

See [Activate audience data to streaming segment export destinations](../../ui/activate-segment-streaming-destinations.md) for instructions on activating audience segments to this destination.

-->