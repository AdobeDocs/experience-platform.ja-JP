---
title: Marketo Engage先
description: Marketo Engageは、マーケティング、広告、分析、コマースに対する唯一のエンドツーエンドのカスタマーエクスペリエンス管理 (CXM) ソリューションです。 CRM リード管理や顧客エンゲージメントから、アカウントベースのマーケティングや売上高属性に至るアクティビティを自動化および管理できます。
exl-id: 5ae5f114-47ba-4ff6-8e42-f8f43eb079f7
source-git-commit: 9c5a5a49385baa7377ebdc806fd22918c39ad0b2
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 2%

---

# Marketo Engage先 {#beta-marketo-engage-destination}

## 概要 {#overview}

Marketo Engageは、マーケティング、広告、分析、コマースに対する唯一のエンドツーエンドのカスタマーエクスペリエンス管理 (CXM) ソリューションです。 CRM リード管理や顧客エンゲージメントから、アカウントベースのマーケティングや売上高属性に至るアクティビティを自動化および管理できます。

宛先を使用すると、マーケターは、Adobe Experience Platformで作成したセグメントを静的リストとして表示されるMarketoにプッシュできます。

## サポートされる ID {#supported-identities}

| ターゲット ID | 説明 |
|---|---|
| ECID | ECID を表す名前空間。 この名前空間は、次のエイリアスからも参照できます。&quot;Adobe Marketing Cloud ID&quot;、&quot;Adobe Experience Cloud ID&quot;、&quot;Adobe Experience Platform ID&quot;。 次のドキュメントを参照してください： [ECID](/help/identity-service/ecid.md) を参照してください。 |
| メール | 電子メールアドレスを表す名前空間。 このタイプの名前空間は、多くの場合、1 人のユーザーに関連付けられているので、様々なチャネルをまたいでそのユーザーを識別するために使用できます。 |

>[!NOTE]
>
>内 [マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) 「宛先のアクティブ化」ワークフローの「 *必須* ID をマッピングするには、および *オプション* 属性をマッピングします。 「 ID 名前空間」タブから電子メールや ECID をマッピングすることは、Marketoで人物が一致するようにするために最も重要な作業です。 「E メールのマッピング」を選択すると、最も高い一致率が確保されます。

## 書き出しタイプ {#export-type}

セグメントの書き出し — セグメントの宛先で使用される識別子（電子メール、ECID）を使用して、セグメント（オーディエンス）のすべてのMarketo Engageを書き出します。

## 宛先の設定とセグメントのアクティブ化 {#set-up}

宛先の設定方法とセグメントのアクティブ化方法について詳しくは、 [Adobe Experience PlatformセグメントをMarketo静的リストにプッシュ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/static-lists/push-an-adobe-experience-cloud-segment-to-a-marketo-static-list.html?lang=en) (Marketoドキュメント ) を参照してください。

<!--

## Connect to the destination {#connect}

To connect to this destination, follow the steps described in the [destination configuration tutorial](../../ui/connect-destination.md).

-->

## データの使用とガバナンス {#data-usage-governance}

すべて [!DNL Adobe Experience Platform] の宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja).

<!--

## Activate segments to this destination {#activate}

See [Activate audience data to streaming segment export destinations](../../ui/activate-segment-streaming-destinations.md) for instructions on activating audience segments to this destination.

-->
