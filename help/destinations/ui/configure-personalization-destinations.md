---
keywords: パーソナライゼーション; ターゲット; 宛先; パーソナライゼーションの宛先; パーソナライゼーションの宛先の設定; 同じページ; 次のページ;
title: 同じページと次のページのパーソナライズのためのパーソナライズ機能宛先の設定
type: Tutorial
seo-title: Configure personalization destinations for same-page and next-page personalization.
description: 同じページと次のページのパーソナライゼーション用にパーソナライゼーションの宛先を設定する方法について説明します。
seo-description: Configure personalization destinations for same-page and next-page personalization.
exl-id: 7d7b6869-bd59-4766-a044-f449396f6524
source-git-commit: fb0d8aedbb88aad8ed65592e0b706bd17840406b
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 100%

---

# 同じページと次のページのパーソナライズのためのパーソナライズ機能宛先の設定

顧客が大規模かつリアルタイムで、オーディエンスセグメントを作成およびターゲット設定できるよう、Adobe Experience Platform では[エッジのセグメント化](../../segmentation/ui/edge-segmentation.md)を使用しています。

この機能は、同じページおよび次のページのパーソナライゼーションのユースケースを設定するのに役立ちます。

この記事では、これらのユースケースで Adobe Experience Platform およびパーソナライゼーションの宛先を設定する方法を、手順に沿って説明します。

さらに、以下のビデオを視聴して、エンドツーエンドの設定プロセスの概要について確認してください。

>[!VIDEO](https://video.tv.adobe.com/v/340091/)

>[!NOTE]
>
>Adobe Experience Platform のユーザーインターフェイスは頻繁に更新され、このビデオが録画された後に変更されている可能性があります。 最新の情報については、以下の節で説明する設定手順を参照してください。

## 手順 1：Data Collection UI でのデータストリームの設定 {#configure-datastream}

パーソナライゼーションの宛先の設定における最初の手順は、Experience Platform Web SDK のデータストリームを設定することです。 これは Data Collection UI で行います。

データストリームを設定する際に、「**[!UICONTROL Adobe Experience Platform]**」で「**[!UICONTROL エッジセグメンテーション]**」と「**[!UICONTROL パーソナライゼーションの宛先]**」の両方が選択されていることを確認します。

![データストリーム設定](../assets/ui/configure-personalization-destinations/datastream-config.png)

データストリームの設定方法の詳細については、[Platform Web SDK ドキュメント](../../edge/datastreams/overview.md)に記載されている手順に従ってください。

## 手順 2：パーソナライゼーションの宛先の設定 {#configure-destination}

データストリームを設定したら、パーソナライゼーションの宛先の設定を開始できます。

新しい宛先接続の作成方法に関する詳細な手順については、[宛先接続の作成チュートリアル](../ui/connect-destination.md)に従ってください。

設定する宛先に応じて、次の記事で宛先固有の前提条件と関連情報について確認してください。

* [Adobe Target 接続](../catalog/personalization/adobe-target-connection.md)
* [カスタムパーソナライゼーション接続](../catalog/personalization/custom-personalization.md)

## 手順 3：[!DNL Active-On-Edge] 結合ポリシーの作成 {#create-merge-policy}

宛先接続を作成したら、[!DNL Active-On-Edge] 結合ポリシーを作成する必要があります。

[結合ポリシーの作成](../../profile/merge-policies/ui-guide.md#create-a-merge-policy)の手順に従い、「**[!UICONTROL エッジでアクティブ化結合ポリシー]**」切り替えスイッチを必ず有効にします。

## 手順 4：Platform での新しいセグメントの作成 {#create-segment}

[!DNL Active-On-Edge] 結合ポリシーを作成したら、Platform で新しいセグメントを作成する必要があります。

[セグメントビルダー](../../segmentation/ui/segment-builder.md)ガイドに従って新しいセグメントを作成し、手順 3 で作成した [!DNL Active-On-Edge] 結合ポリシーに必ず[割り当てます](../../segmentation/ui/segment-builder.md#merge-policies)。

## 手順 5：宛先に対するセグメントのアクティブ化

設定プロセスの最後の手順では、手順 4 で作成したセグメントを、手順 2 で作成した宛先に対してアクティブ化します。

これを行うには、[アクティブ化のチュートリアル](../ui/activate-profile-request-destinations.md)の手順に従います。

## 設定の検証 {#validate-configuration}

上記の手順を正常に実行すると、パーソナライゼーションの宛先に新しいセグメントが表示されます。
