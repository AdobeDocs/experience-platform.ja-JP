---
keywords: 宛先の接続；宛先接続；宛先の接続方法
title: 新しい宛先接続の作成
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platformで宛先に接続する手順を示します
exl-id: 56d7799a-d1da-4727-ae79-fb2c775fe5a5
source-git-commit: f4721d3f114357b25517e4e66f1f626f82621c34
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 3%

---

# 新しい宛先接続の作成

## 概要 {#overview}

オーディエンスデータを宛先に送信する前に、宛先プラットフォームへの接続を設定する必要があります。 この記事では、Adobe Experience Platformユーザーインターフェイスを使用して新しい宛先を設定する方法について説明します。

## 新しい宛先接続の作成 {#setup}

1. **[!UICONTROL 接続]** > **[!UICONTROL 宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![カタログページ](../assets/ui/connect-destinations/catalog.png)

1. 宛先への接続が既にあるかどうかに応じて、宛先カードに「**[!UICONTROL Set up]**」ボタンと「**[!UICONTROL Activate segments]**」ボタンが表示されます。 **[!UICONTROL Activate segments]**&#x200B;と&#x200B;**[!UICONTROL Set up]**&#x200B;の違いについて詳しくは、宛先ワークスペースのドキュメントの「[Catalog](../ui/destinations-workspace.md#catalog)」の節を参照してください。

   使用可能なボタンに応じて、「**[!UICONTROL Set up]**」または「**[!UICONTROL Activate segments]**」を選択します。

   ![カタログページ](../assets/ui/connect-destinations/set-up.png)

   ![セグメントのアクティブ化](../assets/ui/connect-destinations/activate-segments.png)

1. 「**[!UICONTROL 設定]**」を選択した場合は、次の手順に進みます。

   「**[!UICONTROL セグメントをアクティブ化]**」を選択した場合は、既存の宛先接続のリストが表示されます。

   「**[!UICONTROL 新しい宛先を設定]**」を選択します。

   ![新しい宛先の設定](../assets/ui/connect-destinations/configure-new-destination.png)

1. 宛先プラットフォーム接続の詳細を入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

   >[!NOTE]
   >
   >以下の画像は説明用にのみ使用されています。 宛先の接続の詳細は、宛先によって異なります。 宛先の接続の詳細について詳しくは、各[宛先カタログ](../catalog/overview.md)ページの&#x200B;**接続パラメーター**&#x200B;の節を参照してください（例：[Googleカスタマーマッチ](..//catalog/advertising/google-customer-match.md#parameters)）。

   ![宛先に接続](../assets/ui/connect-destinations/connect-destination.png)

1. 「**[!UICONTROL Next]**」を選択します。

   ![宛先に接続](../assets/ui/connect-destinations/next.png)

1. 宛先に書き出すデータに適用できるマーケティングアクションを選択します。 マーケティングアクションは、宛先にデータを書き出す目的を示します。 Adobe定義のマーケティングアクションから選択することも、独自のマーケティングアクションを作成することもできます。 マーケティングアクションについて詳しくは、「[データ使用ポリシーの概要](../../data-governance/policies/overview.md)」ページを参照してください。

   ![マーケティングアクションの選択](../assets/ui/connect-destinations/governance.png)

1. 「**[!UICONTROL 保存して終了]**」を選択して宛先設定を保存するか、「**[!UICONTROL 次へ]**」を選択してオーディエンスデータ[アクティベーションフロー](activation-overview.md)に進みます。