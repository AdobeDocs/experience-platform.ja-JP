---
title: Marketo Engage先
description: Marketo Engageは、マーケティング、広告、分析、コマースに対応する、エンドツーエンドのカスタマーエクスペリエンス管理(CXM)ソリューションの1つです。 CRMのリード管理や顧客エンゲージメントから、アカウントベースのマーケティングや売上高アトリビューションに至るまで、アクティビティを自動化および管理できます。
source-git-commit: 9b1c805f0717d0ed2c5759420d20abf5dcdeaabc
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 4%

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

## 設定 {#set-up}

宛先[の設定方法に関する説明は、](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/static-lists/push-an-adobe-experience-cloud-segment-to-a-marketo-static-list.html?lang=en)を参照してください。

## データの使用とガバナンス {#data-usage-governance}

すべての[!DNL Adobe Experience Platform]宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform]によるデータガバナンスの強制方法について詳しくは、「[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html)」を参照してください。
