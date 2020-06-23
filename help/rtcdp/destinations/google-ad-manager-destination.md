---
title: Google Ad Manager の宛先
seo-title: Google Ad Manager の宛先
description: 'Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。 '
seo-description: 'Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。 '
translation-type: tm+mt
source-git-commit: db2084024f7c25cb1f914f9b8da35298691fd95f
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 36%

---


# Google Ad Manager の宛先

## 概要

Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。

## 宛先の仕様

Google Ad Managerの表示先に固有の次の詳細に注意してください。

* 次の [IDをGoogle Ad Managerの送信先に送信できます](../../identity-service/namespaces.md) 。 **Google cookie ID、IDFA、GAID、Roku ID、Microsoft ID、Amazon Fire TV**。
* アクティブ化されたオーディエンスは、Google プラットフォームでプログラム的に作成されます。
* Adobe Real-time CDP には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。Googleでオーディエンス数を参照して統合を検証し、オーディエンスのターゲット設定サイズを把握する。

>[!IMPORTANT]
>
>Google Ad Managerでの最初のリンク先を作成する場合で、以前(Audience Managerや他のExperience Cloudを使用して)IDサービスで [](https://docs.adobe.com/content/help/ja-JP/id-service/using/id-service-api/methods/idsync.html) ID同期機能を有効にしていない場合は、アドビのコンサルティングまたはカスタマーケアにご連絡ください。 以前にAudience ManagerでGoogle統合を設定した場合、設定したID同期はAdobe Real-time CDPに繰り越します。

## 前提条件

### 許可リスト

>[!NOTE]
>
>この許可リストは、Adobe Real-time CDPで最初のGoogle Ad Managerの宛先を設定する前に必須です。 リンク先を作成する前に、Googleが以下に説明する許可リストプロセスを完了していることを確認してください。

Adobe Real-time CDPでGoogle Ad Managerの宛先を作成する前に、Googleに問い合わせて、許可されているデータプロバイダーのリストを有効にし、お客様のアカウントを許可リストに追加する必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：これは、アドビの Google アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **顧客 ID**：これは、アドビの Google 顧客アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **ネットワークID** : これは、Google Ad Managerを使用するアカウントです
* **オーディエンスリンクID** : これは、Google Ad Managerを使用するアカウントです
* アカウントの種類。**Google** またはAdXの **購入者によるDFP**。

## 宛先の作成

1. In **[!UICONTROL Connections > Destinations]**, select Google Ad Manager, and select **[!UICONTROL Create destination]**.
   ![Google Ad Managerのリンク先への接続](/help/rtcdp/destinations/assets/google-1-destination.png)

2. 宛先を作成ワークフローの **セットアップ** 手順で、宛先の [!UICONTROL 基本情報] を入力します。 <br>

   ![Google Ad Managerの基本情報](/help/rtcdp/destinations/assets/google-1-destination-setup-step.png)
* **[!UICONTROL 名前]**:この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   * DoubleClick `DFP by Google` を発行者に使用
   * Google AdX - `AdX buyer` を使用
* **[!UICONTROL アカウント ID]**：Google アカウントの ID を入力します。ネットワークIDまたはオーディエンスリンクIDを指定できます。 通常、これは8桁のIDです。
* **[!UICONTROL マーケティングの使用例]**: マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 アドビ定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](/help/rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 アドビが定義した個々のマーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](/help/data-governance/policies/overview.md#core-actions)。

> [!NOTE]
>
> Google Ad Managerの表示先を設定する場合は、Googleのアカウントマネージャーまたはアドビの担当者に問い合わせて、お持ちのアカウントのタイプを確認してください。

## Google Ad Managerへのセグメントのアクティブ化

For instructions on how to activate segments to Google Ad Manager, see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).