---
keywords: personalization; target; destination; personalizationo destinations; configure personalization destinations; same page; next page;
title: Configure personalization destinations for same-page and next-page personalization
type: Tutorial
seo-title: Configure personalization destinations for same-page and next-page personalization.
description: 同じページと次のページのパーソナライゼーション用にパーソナライゼーションの宛先を設定する方法について説明します。
seo-description: Configure personalization destinations for same-page and next-page personalization.
exl-id: 7d7b6869-bd59-4766-a044-f449396f6524
source-git-commit: dd9493077706b102467493e90b363ac202550eee
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 1%

---

# 同じページと次のページのパーソナライゼーション用にパーソナライゼーションの宛先を設定

## 概要 {#overview}

Adobe Experience Platform使用 [エッジセグメント化](../../segmentation/ui/edge-segmentation.md) を使用すれば、顧客はオーディエンスセグメントを高い規模でリアルタイムに作成およびターゲット設定できます。

この機能は、同じページおよび次のページのパーソナライゼーションの使用例を設定する場合に役立ちます。

この記事では、これらの使用例でExperience Platformとパーソナライゼーションの宛先を設定する手順を順を追って説明します。

## 手順 1:Experience PlatformWeb SDK のデータストリームの設定 {#configure-datastream}

The first step in configuring your personalization use case is to configure a [!DNL Web SDK datastream].

次に示す手順に従います： [データストリーム設定](../../edge/fundamentals/datastreams.md) ドキュメント。

## Step 2: Configure your personalization destination {#configure-destination}

データストリームを設定したら、パーソナライゼーションの宛先の設定を開始できます。

フォロー： [宛先接続の作成チュートリアル](../ui/connect-destination.md) 新しい宛先接続の作成方法に関する詳細な手順については、を参照してください。

設定する宛先に応じて、宛先固有の前提条件と関連情報については、次の記事を参照してください。

* [Adobe Target接続](../catalog/personalization/adobe-target-connection.md)
* [Custom personalization connection](../catalog/personalization/custom-personalization.md)

## Step 3: Create an [!DNL Active-On-Edge] merge policy {#create-merge-policy}

宛先接続を作成したら、 [!DNL Active-On-Edge] 結合ポリシー。

次の手順に従います。 [結合ポリシーの作成](../../profile/merge-policies/ui-guide.md#create-a-merge-policy)を有効にし、必ず **[!UICONTROL エッジ上のアクティブな結合ポリシー]** 切り替え

## Step 4: Create a new segment in Platform {#create-segment}

After you have created the [!DNL Active-On-Edge] merge policy, you must create a new segment in Platform.

フォロー： [セグメントビルダー](../../segmentation/ui/segment-builder.md) 新しいセグメントを作成する際のガイドを参照し、必ず [割り当てる](../../segmentation/ui/segment-builder.md#merge-policies) の [!DNL Active-On-Edge] 手順 3 で作成した結合ポリシーです。

## 手順 5:宛先へのセグメントをアクティブ化します。

設定プロセスの最後の手順では、手順 4 で作成したセグメントを、手順 2 で作成した宛先に対してアクティブ化します。

これをおこなうには、次の手順に従います [activation チュートリアル](../ui/activate-profile-request-destinations.md).

## 設定の検証 {#validate-configuration}

上記の手順に正常に従うと、パーソナライゼーションの宛先に新しいセグメントが表示されます。
