---
title: Google Ad Managerの表示先
seo-title: Google Ad Managerの表示先
description: 'Google Ad Manager（以前はDoubleClick for PublishersまたはDoubleClick AdXと呼ばれていました）は、Googleの広告提供プラットフォームで、発行者はビデオやモバイルアプリを通じてWebサイト上の広告の表示を管理する手段を提供します。 '
seo-description: 'Google Ad Manager（以前はDoubleClick for PublishersまたはDoubleClick AdXと呼ばれていました）は、Googleの広告提供プラットフォームで、発行者はビデオやモバイルアプリを通じてWebサイト上の広告の表示を管理する手段を提供します。 '
translation-type: tm+mt
source-git-commit: 3e510c891c84fb3dc1632bd1182ef1e010ea898f

---


# Google Ad Managerの表示先

## 概要

Google Ad Manager（以前はDoubleClick for PublishersまたはDoubleClick AdXと呼ばれていました）は、Googleの広告提供プラットフォームで、発行者はビデオやモバイルアプリを通じてWebサイト上の広告の表示を管理する手段を提供します。

## 宛先の仕様

Google Ad Managerの表示先に固有の次の詳細に注意してください。

* 次のIDをGoogle Ad Managerの送 [信先](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/identity_namespace_overview/identity_namespace_overview.md) に送信できます。 **Google cookie ID、IDFA、GAID、Roku ID、Microsoft ID、Amazon Fire TV ID**。
* アクティブ化されたオーディエンスは、Google プラットフォームでプログラム的に作成されます。
* Adobe Real-time CDP には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。統合を検証し、オーディエンスのターゲット設定のサイズを理解するには、Googleでオーディエンスのカウントを参照します。

>[!IMPORTANT]
>
>Google Ad Managerでの最初のリンク先を作成し、以前（Audience Managerなどのアプリケーションで）Experience Cloud IDサービスで [](https://docs.adobe.com/content/help/en/id-service/using/id-service-api/methods/idsync.html) ID同期機能を有効にしていない場合は、アドビのコンサルティングまたはカスタマーケアに連絡して、ID同期を有効にしてください。 以前にAudience ManagerでGoogle統合を設定していた場合、設定したID同期はAdobe Real-time CDPに引き継がれます。

## 前提条件

### ホワイトリスト

>[!NOTE]
>
>ホワイトリストは、Adobe Real-time CDPで最初のGoogle Ad Managerの宛先を設定する前に必須です。 リンク先を作成する前に、Googleが以下に説明するホワイトリストのプロセスを完了していることを確認してください。

Adobe Real-time CDPでGoogle Ad Managerの宛先を作成する前に、Googleに問い合わせて、データプロバイダーとしてホワイトリストに登録するようにアドビに求め、お客様のアカウントをホワイトリストに登録する必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：これは、アドビの Google アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **顧客 ID**：これは、アドビの Google 顧客アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **ネットワークID** :これは、Google Ad Managerを使用するお客様のアカウントです
* **オーディエンスリンクID** :これは、Google Ad Managerを使用するお客様のアカウントです
* アカウントの種類。**Googleまたは** AdXの購入者によるDFP ****。

## 宛先を作成

1. で、「 **[!UICONTROL Connections > Destinations]** Google Ad Manager」を選択し、を選択しま **[!UICONTROL Create destination]**す。
   ![Google Ad Managerの宛先への接続](/help/rtcdp/destinations/assets/google-1-destination.png)

2. 宛先の作成ウィザードで、宛先の基本情報を入力します。
   ![Google Ad Managerの基本情報](/help/rtcdp/destinations/assets/google-1-basic-information.png)
* **名前**:この宛先の名前を入力します。
* **説明**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **アカウントの種類**：Google のアカウントに応じて、次のオプションを選択します。
   * 発行者に対 `DFP by Google` してDoubleClickを使用
   * Google AdX - `AdX buyer` を使用
* **アカウント ID**：Google アカウントの ID を入力します。これは、ネットワークIDまたはオーディエンスリンクIDです。 通常、これは8桁のIDです。

>[!NOTE]
>
>Google Ad Managerを設定する際は、Googleのアカウントマネージャーまたはアドビの担当者にお問い合わせの上、お持ちのアカウントの種類をご確認ください。

## Google Ad Managerでのセグメントのアクティブ化

For instructions on how to activate segments to Google Ad Manager, see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).