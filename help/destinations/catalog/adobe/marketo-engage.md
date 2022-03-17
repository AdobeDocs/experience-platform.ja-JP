---
title: Marketo Engage の宛先
description: Marketo Engageは、マーケティング、広告、分析、コマースに対する唯一のエンドツーエンドのカスタマーエクスペリエンス管理 (CXM) ソリューションです。 CRM リード管理や顧客エンゲージメントから、アカウントベースのマーケティングや売上高属性に至るアクティビティを自動化および管理できます。
exl-id: 5ae5f114-47ba-4ff6-8e42-f8f43eb079f7
source-git-commit: b1945d42b82b549985d848071762fa6ee2451368
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 10%

---

# Marketo Engage先 {#beta-marketo-engage-destination}

## 概要 {#overview}

Marketo Engageは、マーケティング、広告、分析、コマースに対する唯一のエンドツーエンドのカスタマーエクスペリエンス管理 (CXM) ソリューションです。 CRM リード管理や顧客エンゲージメントから、アカウントベースのマーケティングや売上高属性に至るアクティビティを自動化および管理できます。

 の宛先を利用することで、マーケターは、Adobe Experience Platform で作成したセグメントを Marketo にプッシュできます。それらのセグメントは静的リストとして表示されます。

## サポートされる ID {#supported-identities}

| ターゲット ID | 説明 |
|---|---|
| ECID | ECID を表す名前空間。 この名前空間は、次のエイリアスからも参照できます。&quot;Adobe Marketing Cloud ID&quot;、&quot;Adobe Experience Cloud ID&quot;、&quot;Adobe Experience Platform ID&quot;。 次のドキュメントを参照してください： [ECID](/help/identity-service/ecid.md) を参照してください。 |
| メール | 電子メールアドレスを表す名前空間。 このタイプの名前空間は、多くの場合、1 人のユーザーに関連付けられているので、様々なチャネルをまたいでそのユーザーを識別するために使用できます。 |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>内 [マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) 「宛先のアクティブ化」ワークフローの「 *必須* ID をマッピングするには、および *オプション* 属性をマッピングします。 「 ID 名前空間」タブから電子メールや ECID をマッピングすることは、Marketoで人物が一致するようにするために最も重要な作業です。 「E メールのマッピング」を選択すると、最も高い一致率が確保されます。

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | セグメントの宛先で使用されている識別子（E メール、ECID）を使用して、セグメント（オーディエンス）のすべてのMarketo Engageを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## 宛先の設定とセグメントのアクティブ化 {#set-up}

宛先の設定方法とセグメントのアクティブ化方法について詳しくは、 [Adobe Experience PlatformセグメントをMarketo静的リストにプッシュ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/static-lists/push-an-adobe-experience-cloud-segment-to-a-marketo-static-list.html?lang=en) (Marketoドキュメント ) を参照してください。

次のビデオでは、Marketoの宛先を設定し、セグメントをアクティブ化する手順についても説明します。

>[!NOTE]
>
>Experience Platformのユーザーインターフェイスは頻繁に更新され、このビデオの録画以降に変更された可能性があります。 最新情報については、上記にリンクされたガイドを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/338248?quality=12)

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
