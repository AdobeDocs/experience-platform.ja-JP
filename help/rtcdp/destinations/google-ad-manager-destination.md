---
title: Google Ad Manager の宛先
seo-title: Google Ad Manager の宛先
description: 'Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。 '
seo-description: 'Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。 '
translation-type: tm+mt
source-git-commit: 6f680a60c88bc5fee6ce9cb5a4f314c4b9d02249
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 32%

---


# [!DNL Google Ad Manager Destination]

## 概要

[!DNL Google Ad Manager]（以前は for Publishers または と呼ばれていました）は の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。[!DNL DoubleClick][!DNL DoubleClick AdX][!DNL Google]

## 宛先の仕様

Note the following details that are specific to [!DNL Google Ad Manager] destinations:

* 次の [IDを宛先に送信でき](../../identity-service/namespaces.md)[!DNL Google Ad Manager] ます。 **Google cookie ID、IDFA、GAID、Roku ID、Microsoft ID、Amazon Fire TV**。
* Activated audiences are created programmatically in the [!DNL Google] platform.
* Adobe Real-time CDP には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。Googleでオーディエンス数を参照して統合を検証し、オーディエンスのターゲット設定サイズを把握する。

>[!IMPORTANT]
>
>最初の宛先を作成する場合 [!DNL Google Ad Manager] は、以前(Audience Managerや他のExperience Cloudを使用)にアドビの [](https://docs.adobe.com/content/help/ja-JP/id-service/using/id-service-api/methods/idsync.html) IDサービスでID同期機能を有効にしていない限り、アドビのコンサルティングまたはカスタマーケアにご連絡いただき、ID同期を有効にしてください。 以前にAudience Managerで [!DNL Google] 統合を設定した場合、設定したID同期はAdobe Real-time CDPに繰り越します。

## 前提条件

### 許可リスト

>[!NOTE]
>
>許可リストは、Adobe Real-time CDPで最初の [!DNL Google Ad Manager] 宛先を設定する前に必須です。 宛先を作成する前に、次に説明する許可リストプロセスがによって完了しているこ [!DNL Google] とを確認してください。

Adobe Real-time CDPで [!DNL Google Ad Manager] 宛先を作成する前に、アドビに連絡して、許可されているデータプロバイダーのリスト [!DNL Google] に加え、お客様のアカウントを許可リストに追加する必要があります。 Contact [!DNL Google] and provide the following information:

* **アカウントID** : これは、を使用するアドビのアカウントIDで [!DNL Google]す。 このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **顧客ID** : これは、アドビの顧客アカウントID [!DNL Google]です。 このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **ネットワーク ID**：これは のアカウントです。[!DNL Google Ad Manager]
* **オーディエンスリンク ID**：これは のアカウントです。[!DNL Google Ad Manager]
* アカウントの種類。**Google** またはAdXの **購入者によるDFP**。

## 宛先の作成

1. **[!UICONTROL 接続/宛先]**&#x200B;を選択し、「宛先を [!DNL Google Ad Manager]作成 ****」を選択します。
   ![Google Ad Managerのリンク先への接続](/help/rtcdp/destinations/assets/google-1-destination.png)

2. 宛先を作成ワークフローの **セットアップ** 手順で、宛先の [!UICONTROL 基本情報] を入力します。 <br>

   ![Google Ad Managerの基本情報](/help/rtcdp/destinations/assets/google-1-destination-setup-step.png)
* **[!UICONTROL 名前]**:この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   * 発行者 `DFP by Google` に対 [!DNL DoubleClick] して使用
   * Use `AdX buyer` for [!DNL Google AdX]
* **[!UICONTROL アカウントID]**: アカウントIDをに入力し [!DNL Google]ます。 ネットワークIDまたはオーディエンスリンクIDを指定できます。 通常、これは8桁のIDです。
* **[!UICONTROL マーケティングの使用例]**: マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 アドビ定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](/help/rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 アドビが定義した個々のマーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](/help/data-governance/policies/overview.md#core-actions)。

> [!NOTE]
>
> 発行先を設定する場合は、担当のアドビ担当者 [!DNL Google Ad Manager][!DNL Google Account Manager] またはアドビ担当者にご相談のうえ、お持ちのアカウントのタイプをご確認ください。

## Activate segments to [!DNL Google Ad Manager]

For instructions on how to activate segments to [!DNL Google Ad Manager], see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).