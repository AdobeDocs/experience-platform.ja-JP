---
title: Marketo Engage先
description: Marketo Engageは、マーケティング、広告、分析、コマースに対応する、エンドツーエンドのカスタマーエクスペリエンス管理(CXM)ソリューションの1つです。 CRMのリード管理や顧客エンゲージメントから、アカウントベースのマーケティングや売上高アトリビューションに至るまで、アクティビティを自動化および管理できます。
exl-id: 5ae5f114-47ba-4ff6-8e42-f8f43eb079f7
source-git-commit: 1f18e07af7ef0d90f882fa668c5659330bce5960
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 3%

---

# （ベータ版）Marketo Engage先 {#beta-marketo-engage-destination}

>[!IMPORTANT]
>
>Adobe Experience PlatformのMarketo Engageの宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

Marketo Engageは、マーケティング、広告、分析、コマースに対応する、エンドツーエンドのカスタマーエクスペリエンス管理(CXM)ソリューションの1つです。 CRMのリード管理や顧客エンゲージメントから、アカウントベースのマーケティングや売上高アトリビューションに至るまで、アクティビティを自動化および管理できます。

セグメントコネクタを使用すると、マーケターは、Adobe Experience Platformで作成したセグメントを静的リストとしてMarketoにプッシュできます。

## サポートされるID {#supported-identities}

| ターゲットID | 説明 |
|---|---|
| ECID | ECIDを表す名前空間。 この名前空間は、次のエイリアスでも参照できます。&quot;Adobe Marketing Cloud ID&quot;、&quot;Adobe Experience Cloud ID&quot;、&quot;Adobe Experience Platform ID&quot;。 詳しくは、[ECID](/help/identity-service/ecid.md)に関する次のドキュメントを参照してください。 |
| メール | 電子メールアドレスを表す名前空間。 このタイプの名前空間は、多くの場合、1人の人に関連付けられるので、異なるチャネルをまたいでその人を識別するために使用できます。 |

## 書き出しタイプ {#export-type}

セグメントのエクスポート — セグメントの宛先で使用されている識別子（名前、電話番号など）を持つセグメント（オーディエンス）のすべてのMarketo Engageをエクスポートします。

## 宛先の設定とセグメントのアクティブ化 {#set-up}

宛先の設定方法とセグメントのアクティブ化方法について詳しくは、Marketoドキュメントの「[Adobe Experience PlatformセグメントをMarketo静的リストにプッシュする](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/static-lists/push-an-adobe-experience-cloud-segment-to-a-marketo-static-list.html?lang=en)」を参照してください。

<!--

## Connect to the destination {#connect}

To connect to this destination, follow the steps described in the [destination configuration tutorial](../../ui/connect-destination.md).

-->

## データの使用とガバナンス {#data-usage-governance}

すべての[!DNL Adobe Experience Platform]宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform]によるデータガバナンスの強制方法について詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html)を参照してください。

<!--

## Activate segments to this destination {#activate}

See [Activate audience data to streaming segment export destinations](../../ui/activate-segment-streaming-destinations.md) for instructions on activating audience segments to this destination.

-->