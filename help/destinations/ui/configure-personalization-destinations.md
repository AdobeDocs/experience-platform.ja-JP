---
keywords: パーソナライゼーション；ターゲット；宛先；パーソナライゼーションの宛先；パーソナライゼーションの宛先の設定同じページ；次のページ
title: 同じページと次のページのパーソナライゼーション用にパーソナライゼーションの宛先を設定
type: Tutorial
seo-title: Configure personalization destinations for same-page and next-page personalization.
description: 同じページと次のページのパーソナライゼーション用にパーソナライゼーションの宛先を設定する方法について説明します。
seo-description: Configure personalization destinations for same-page and next-page personalization.
exl-id: 7d7b6869-bd59-4766-a044-f449396f6524
source-git-commit: a990e829c8ba034f31b883360495513f3f5b4cfc
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# 同じページと次のページのパーソナライゼーション用にパーソナライゼーションの宛先を設定

Adobe Experience Platform使用 [エッジセグメント化](../../segmentation/ui/edge-segmentation.md) を使用すれば、顧客はオーディエンスセグメントを高い規模でリアルタイムに作成およびターゲット設定できます。

この機能は、同じページおよび次のページのパーソナライゼーションの使用例を設定する場合に役立ちます。

この記事では、これらの使用例でExperience Platformとパーソナライゼーションの宛先を設定する手順を順を追って説明します。

さらに、エンドツーエンドの設定プロセスの概要については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/340091/)

>[!NOTE]
>
>Experience Platformのユーザーインターフェイスは頻繁に更新され、このビデオの録画以降に変更された可能性があります。 最新の情報については、次の節で説明する設定手順を参照してください。

## 手順 1:データ収集 UI でのデータストリームの設定 {#configure-datastream}

パーソナライゼーションの宛先を設定する最初の手順は、Experience PlatformWeb SDK のデータストリームを設定することです。 これは、データ収集 UI でおこないます。

データストリームを設定する場合、 **[!UICONTROL Adobe Experience Platform]** 両方を確実にする **[!UICONTROL エッジセグメント化]** および **[!UICONTROL パーソナライズ機能の宛先]** が選択されている。

![データストリーム設定](../assets/ui/configure-personalization-destinations/datastream-config.png)

データストリームの設定方法の詳細については、 [Platform Web SDK ドキュメント](../../edge/fundamentals/datastreams.md).

## 手順 2:パーソナライゼーションの宛先の設定 {#configure-destination}

データストリームを設定したら、パーソナライゼーションの宛先の設定を開始できます。

フォロー： [宛先接続の作成チュートリアル](../ui/connect-destination.md) 新しい宛先接続の作成方法に関する詳細な手順については、を参照してください。

設定する宛先に応じて、宛先固有の前提条件と関連情報については、次の記事を参照してください。

* [Adobe Target接続](../catalog/personalization/adobe-target-connection.md)
* [カスタムパーソナライゼーション接続](../catalog/personalization/custom-personalization.md)

## 手順 3:の作成 [!DNL Active-On-Edge] 結合ポリシー {#create-merge-policy}

宛先接続を作成したら、 [!DNL Active-On-Edge] 結合ポリシー。

次の手順に従います。 [結合ポリシーの作成](../../profile/merge-policies/ui-guide.md#create-a-merge-policy)を有効にし、必ず **[!UICONTROL エッジ上のアクティブな結合ポリシー]** 切り替え

## 手順 4:Platform での新しいセグメントの作成 {#create-segment}

次の [!DNL Active-On-Edge] 結合ポリシーの場合、Platform で新しいセグメントを作成する必要があります。

フォロー： [セグメントビルダー](../../segmentation/ui/segment-builder.md) 新しいセグメントを作成する際のガイドを参照し、必ず [割り当てる](../../segmentation/ui/segment-builder.md#merge-policies) の [!DNL Active-On-Edge] 手順 3 で作成した結合ポリシーです。

## 手順 5:宛先へのセグメントをアクティブ化します。

設定プロセスの最後の手順では、手順 4 で作成したセグメントを、手順 2 で作成した宛先に対してアクティブ化します。

これをおこなうには、次の手順に従います [activation チュートリアル](../ui/activate-profile-request-destinations.md).

## 設定の検証 {#validate-configuration}

上記の手順に正常に従うと、パーソナライゼーションの宛先に新しいセグメントが表示されます。
