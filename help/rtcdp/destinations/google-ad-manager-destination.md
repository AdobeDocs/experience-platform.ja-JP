---
title: Google Ad Manager の宛先
seo-title: Google Ad Manager の宛先
description: 'Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。 '
seo-description: 'Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。 '
translation-type: tm+mt
source-git-commit: 121ae74e9c352b1f6fc12093d815e711ebd817b8
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 41%

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
>Google Ad Managerでの最初のリンク先を作成する場合で、以前(オーディエンスマネージャーなどのアプリケーションを使用して)Experience Cloud IDサービスで [](https://docs.adobe.com/content/help/ja-JP/id-service/using/id-service-api/methods/idsync.html) ID同期機能を有効にしていない場合は、アドビのコンサルティングまたはカスタマーケアにご連絡ください。 Google統合を以前にオーディエンスマネージャーで設定している場合、設定したID同期はAdobe Real-time CDPに繰り越されます。

## 前提条件

### ホワイトリスト

>[!NOTE]
>
>ホワイトリストは、Adobe Real-time CDPでGoogle Ad Managerの最初の宛先を設定する前に必須です。 リンク先を作成する前に、Googleが以下に説明するホワイトリスト登録プロセスを完了していることを確認してください。

Adobe Real-time CDPでGoogle Ad Managerの宛先を作成する前に、Googleに連絡して、Adobeをデータプロバイダーとしてホワイトリストに登録するように依頼し、ホワイトリストに登録するアカウントを取得する必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：これは、アドビの Google アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **顧客 ID**：これは、アドビの Google 顧客アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **ネットワークID** : これは、Google Ad Managerを使用するアカウントです
* **オーディエンスリンクID** : これは、Google Ad Managerを使用するアカウントです
* アカウントの種類。**Google** またはAdXの **購入者によるDFP**。

## 宛先の作成

1. In **[!UICONTROL Connections > Destinations]**, select Google Ad Manager, and select **[!UICONTROL Create destination]**.
   ![Google Ad Managerのリンク先への接続](/help/rtcdp/destinations/assets/google-1-destination.png)

2. In the Create destination workflow, fill in the [!UICONTROL Basic Information] for the destination. <br>
   ![Google Ad Managerの基本情報](/help/rtcdp/destinations/assets/google-1-basic-information.png)
* **[!UICONTROL 名前]**:この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   * DoubleClick `DFP by Google` を発行者に使用
   * Google AdX - `AdX buyer` を使用
* **[!UICONTROL アカウント ID]**：Google アカウントの ID を入力します。ネットワークIDまたはオーディエンスリンクIDを指定できます。 通常、これは8桁のIDです。

>[!NOTE]
>
>Google Ad Managerの表示先を設定する場合は、Googleのアカウントマネージャーまたはアドビの担当者に問い合わせて、お持ちのアカウントのタイプを確認してください。

## Google Ad Managerへのセグメントのアクティブ化

For instructions on how to activate segments to Google Ad Manager, see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).