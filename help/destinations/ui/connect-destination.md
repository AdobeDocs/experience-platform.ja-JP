---
keywords: 宛先の接続；宛先接続；宛先の接続方法
title: 新しい宛先接続の作成
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platformで宛先に接続する手順を示します
exl-id: 56d7799a-d1da-4727-ae79-fb2c775fe5a5
source-git-commit: 1c67d9eb1b7762ecfcad5b0b7db5c317621f144e
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 1%

---

# 新しい宛先接続の作成

## 概要 {#overview}

オーディエンスデータを宛先に送信する前に、宛先プラットフォームへの接続を設定する必要があります。 この記事では、Adobe Experience Platformユーザーインターフェイスを使用して新しい宛先を設定する方法について説明します。

## 新しい宛先接続の作成 {#setup}

### 宛先の選択 {#select-destination}

1. **[!UICONTROL 接続]** > **[!UICONTROL 宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![カタログページ](../assets/ui/connect-destinations/catalog.png)

1. 宛先への接続が既にあるかどうかに応じて、宛先カードに「**[!UICONTROL Configure]**」ボタンと「**[!UICONTROL Activate]**」ボタンが表示されます。 **[!UICONTROL アクティブ化]**&#x200B;と&#x200B;**[!UICONTROL 設定]**&#x200B;の違いについて詳しくは、宛先ワークスペースのドキュメントの[カタログ](../ui/destinations-workspace.md#catalog)の節を参照してください。 使用可能なボタンに応じて、「**[!UICONTROL 設定]**」または「**[!UICONTROL アクティブ化]**」を選択します。

   ![カタログページ](../assets/ui/connect-destinations/set-up.png)

   ![セグメントのアクティブ化](../assets/ui/connect-destinations/activate-segments.png)

<!-- 1. If you selected **[!UICONTROL Set up]**, skip this step. If you selected **[!UICONTROL Activate segments]**, you can now see a list of the existing destination connections. Select **[!UICONTROL Configure new destination]**.

   ![Configure new destination](../assets/ui/connect-destinations/configure-new-destination.png) -->

### アカウントの手順 {#account}

「**[!UICONTROL 新しいアカウント]**」を選択して、宛先への新しい接続を設定します。 または、宛先への接続を既に設定している場合は、「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続を選択します。

アカウント手順で入力する必要がある資格情報は、宛先と認証の種類によって異なります。

* クラウドストレージの宛先の場合、ストレージの場所に接続するためのExperience Platformの資格情報を指定する必要があります。

   ![クラウドストレージの宛先のアカウントタイプの選択](../assets/ui/connect-destinations/new-account-cloud-storage.png)

* facebookやその他の複数のソーシャルおよび広告の宛先の場合は、「 **[!UICONTROL 新しいアカウント]** 」を選択し、「 **[!UICONTROL 宛先に接続]** 」を選択します。 これにより、宛先のログインページが表示され、宛先にExperience Platformを接続できます。

   ![ソーシャルの宛先のアカウントタイプの選択](../assets/ui/connect-destinations/new-account.png)

>[!IMPORTANT]
>
>この手順で必要なパラメーターの詳細については、各宛先カタログページの&#x200B;**[!UICONTROL 接続パラメーター]**&#x200B;の節を参照してください（例：[Azure Blob](../catalog/cloud-storage/azure-blob.md#parameters)には接続文字列が必要）。

### 認証手順 {#authentication}

宛先プラットフォーム接続の詳細を入力し、「**[!UICONTROL 宛先を作成]**」を選択します。

1. 宛先に書き出すデータに適用できるマーケティングアクションを選択します。 マーケティングアクションは、宛先にデータを書き出す目的を示します。 Adobe定義のマーケティングアクションから選択することも、独自のマーケティングアクションを作成することもできます。 マーケティングアクションについて詳しくは、「[データ使用ポリシーの概要](../../data-governance/policies/overview.md)」ページを参照してください。

   >[!IMPORTANT]
   >
   >以下の画像は説明用にのみ使用されています。 宛先の接続の詳細は、宛先によって異なります。 宛先の接続の詳細について詳しくは、各[宛先カタログ](../catalog/overview.md)ページの&#x200B;**[!UICONTROL 接続パラメーター]**&#x200B;の節を参照してください（例：[Googleカスタマーマッチ](../catalog/advertising/google-customer-match.md#parameters)）。

   ![宛先に接続](../assets/ui/connect-destinations/connect-destination.png)

1. 「**[!UICONTROL 保存して終了]**」を選択して宛先設定を保存するか、「**[!UICONTROL 次へ]**」を選択してオーディエンスデータ[アクティベーションフロー](activate-destinations.md)に進みます。