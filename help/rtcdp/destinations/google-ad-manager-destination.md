---
keywords: google ad manager;google ad;doubleclick;DoubleClick AdX;DoubleClick;Google Ad Manager;Google ad manager
title: Google Ad Manager の宛先
seo-title: Google Ad Manager の宛先
description: 'Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。 '
seo-description: 'Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。 '
translation-type: tm+mt
source-git-commit: d20b558a6f4518be74cd5969c50a5db310370c08
workflow-type: tm+mt
source-wordcount: '690'
ht-degree: 40%

---


# [!DNL Google Ad Manager Destination]

## 概要

[!DNL Google Ad Manager]（以前は for Publishers または と呼ばれていました）は の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。[!DNL DoubleClick][!DNL DoubleClick AdX][!DNL Google]

## 宛先の仕様

Note the following details that are specific to [!DNL Google Ad Manager] destinations:

* You can send the following [identities](../../identity-service/namespaces.md) to [!DNL Google Ad Manager] destinations: **Google cookie ID, IDFA, GAID, Roku IDs, Microsoft IDs, Amazon Fire TV IDs**.
* Activated audiences are created programmatically in the [!DNL Google] platform.
* アドビのリアルタイム CDP には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。統合を検証し、オーディエンスターゲット設定サイズについて理解するには、Google でのオーディエンス数を参照します。

>[!IMPORTANT]
>
>If you are looking to create your first destination with [!DNL Google Ad Manager] and have not enabled the [ID sync functionality](https://docs.adobe.com/content/help/ja-JP/id-service/using/id-service-api/methods/idsync.html) in Experience Cloud ID Service in the past (with Audience Manager or other applications), please reach out to Adobe Consulting or Customer Care to enable ID syncs. If you had previously set up [!DNL Google] integrations in Audience Manager, the ID syncs you had set up carry over to Adobe Real-time CDP.

### 書き出しタイプ {#export-type}

**セグメントエクスポート** — セグメント(オーディエンス)のすべてのメンバーをGoogleのエクスポート先にエクスポートします。

## 前提条件

### 許可リスト

>[!NOTE]
>
>許可リストは、AdobeReal-time CDPで最初の [!DNL Google Ad Manager] 宛先を設定する前に必須です。 Please ensure the allow list process described below has been completed by [!DNL Google] before creating a destination.

AdobeReal-time CDPで [!DNL Google Ad Manager][!DNL Google] 宛先を作成する前に、Adobeを許可されたデータプロバイダのリストに置くため、およびアカウントを許可リストに追加するために、に連絡する必要があります。 Contact [!DNL Google] and provide the following information:

* **アカウントID** :これは、AdobeのアカウントID [!DNL Google]です。 アドビカスタマーケアまたは Adobe 担当者に問い合わせて、この ID を取得してください。
* **顧客ID** :これは、Adobeの顧客アカウントID [!DNL Google]です。 アドビカスタマーケアまたは Adobe 担当者に問い合わせて、この ID を取得してください。
* **ネットワーク ID**：これは のアカウントです。[!DNL Google Ad Manager]
* **オーディエンスリンク ID**：これは のアカウントです。[!DNL Google Ad Manager]
* アカウントの種類。Google DFP または AdX 購入者。

## 宛先の設定

1. **[!UICONTROL 接続]** / **[!UICONTROL 宛先]**、を選択し、「 **[!DNL Google Ad Manager]**&#x200B;設定 ****」を選択します。
   ![Google Ad Manager の宛先の接続](/help/rtcdp/destinations/assets/google-1-destination.png)

   >[!NOTE]
   >
   >この宛先との接続が既に存在する場合は、宛先カードに **[!UICONTROL 「アクティブ化]** 」ボタンが表示されます。 「 **[!UICONTROL アクティブ化]** 」と「 **[!UICONTROL 設定]**」の違いについて詳しくは、表示先ワークスペースのドキュメントの「 [カタログ](/help/rtcdp/destinations/destinations-workspace.md#catalog) 」セクションを参照してください。

2. In the **Setup** step of the create destination workflow, fill in the [!UICONTROL Basic Information] for the destination. <br>

   ![Google Ad Manager の基本情報](/help/rtcdp/destinations/assets/google-1-destination-setup-step.png)
* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   *  for Publishers - `DFP by Google` を使用[!DNL DoubleClick]
   * 使用 `AdX buyer` する対象 [!DNL Google AdX]
* **[!UICONTROL アカウントID]**:アカウントIDをに入力し [!DNL Google]ます。 これは、ネットワーク ID またはオーディエンスリンク ID です。通常、これは 8 桁の ID です。
* **[!UICONTROL マーケティングの使用例]**:マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 Adobe定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](/help/rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 個々のAdobe定義マーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](/help/data-governance/policies/overview.md#core-actions)。

>[!NOTE]
>
> When setting up a [!DNL Google Ad Manager] destination please work with your [!DNL Google Account Manager] or Adobe representative to understand which account type you have.

## Activate segments to [!DNL Google Ad Manager]

For instructions on how to activate segments to [!DNL Google Ad Manager], see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).

## 書き出されたデータ

データが正常に宛先にエクスポートされたかどうかを確認するには、アカウントを確認して [!DNL Google Ad Manager] く [!DNL Google Ad Manager] ださい。 アクティベーションに成功すると、オーディエンスがアカウントに入力されます。