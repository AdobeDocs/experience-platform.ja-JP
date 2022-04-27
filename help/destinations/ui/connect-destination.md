---
keywords: 宛先の接続；宛先の接続；宛先の接続方法
title: 新しい宛先接続の作成
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platformで宛先に接続する手順を示します
exl-id: 56d7799a-d1da-4727-ae79-fb2c775fe5a5
source-git-commit: 729c0724c7af88bb69c9d68a45d58c3575c90828
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 6%

---

# 新しい宛先接続の作成

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

## 概要 {#overview}

オーディエンスデータを宛先に送信する前に、宛先プラットフォームへの接続を設定する必要があります。 この記事では、Adobe Experience Platformユーザーインターフェイスを使用して新しい宛先を設定する方法について説明します。

## 新しい宛先接続の作成 {#setup}

1. に移動します。 **[!UICONTROL 接続]** > **[!UICONTROL 宛先]**&#x200B;をクリックし、 **[!UICONTROL カタログ]** タブをクリックします。

   ![カタログページ](../assets/ui/connect-destinations/catalog.png)

1. 宛先への既存の接続があるかどうかに応じて、 **[!UICONTROL 設定]** または **[!UICONTROL セグメントのアクティブ化]** ボタンをクリックします。 の違いに関する詳細 **[!UICONTROL セグメントのアクティブ化]** および **[!UICONTROL 設定]**（を参照） [カタログ](../ui/destinations-workspace.md#catalog) の節を参照してください。

   次のいずれかを選択 **[!UICONTROL 設定]** または **[!UICONTROL セグメントのアクティブ化]**（使用可能なボタンに応じて異なります）。

   ![カタログページ](../assets/ui/connect-destinations/set-up.png)

   ![セグメントのアクティブ化](../assets/ui/connect-destinations/activate-segments.png)

1. 選択した場合 **[!UICONTROL 設定]**、次の手順に進みます。

   選択した場合 **[!UICONTROL セグメントのアクティブ化]**&#x200B;に設定すると、既存の宛先接続のリストが表示されるようになります。

   選択 **[!UICONTROL 新しい宛先の設定]**.

   ![新しい宛先の設定](../assets/ui/connect-destinations/configure-new-destination.png)

1. 宛先プラットフォーム接続の詳細を入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

   >[!NOTE]
   >
   >以下の画像は説明用にのみ使用されています。 宛先の接続の詳細は、宛先間で異なります。 宛先の接続の詳細について詳しくは、 **接続パラメーター** 各 [宛先カタログ](../catalog/overview.md) ページ ( 例： [Google Customer Match](..//catalog/advertising/google-customer-match.md#parameters)) をクリックします。

   ![宛先に接続](../assets/ui/connect-destinations/connect-destination.png)

1. （オプション）サブスクライブする宛先データフローアラートを選択します。 データフローを作成する際にアラートをサブスクライブして、フロー実行のステータス、成功または失敗に関するアラートメッセージを受け取ることができます。 詳しくは、 [コンテキスト内宛先アラートを購読](alerts.md) 宛先のデータフローアラートの詳細を参照してください。

   ![コンテキスト内宛先アラートのサブスクリプションオプションを示す UI 画像](../assets/ui/connect-destinations/subscribe-to-alerts.png)

1. 「**[!UICONTROL Next]**」を選択します。

   ![宛先に接続](../assets/ui/connect-destinations/next.png)

1. 宛先に書き出すデータに適用できるマーケティングアクションを選択します。 マーケティングアクションは、宛先にデータを書き出す意図を示します。 Adobe定義のマーケティングアクションから選択することも、独自のマーケティングアクションを作成することもできます。 マーケティングアクションについて詳しくは、 [データ使用ポリシーの概要](../../data-governance/policies/overview.md) ページ。

   ![マーケティングアクションを選択](../assets/ui/connect-destinations/governance.png)

1. 選択 **[!UICONTROL 保存して終了]** 宛先の設定を保存するには、「 」を選択します。 **[!UICONTROL 次へ]** オーディエンスデータに進むには [活性化フロー](activation-overview.md).