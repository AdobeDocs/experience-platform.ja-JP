---
title: Google AdsDestination
seo-title: Google広告の表示先
description: Google Ads（旧称Google AdWords）は、企業がテキストベースの検索、グラフィック表示、YouTubeビデオ、アプリ内モバイルディスプレイにわたってクリックあたりの広告をペイパークリックできるオンライン広告サービスです。
seo-description: Google Ads（旧称Google AdWords）は、企業がテキストベースの検索、グラフィック表示、YouTubeビデオ、アプリ内モバイルディスプレイにわたってクリックあたりの広告をペイパークリックできるオンライン広告サービスです。
translation-type: tm+mt
source-git-commit: 336aa90cf1e059a92a36dd0ef3222ef6a6f5123b

---


# Google広告の表示先

## 概要

Google Ads（旧称Google AdWords）は、企業がテキストベースの検索、グラフィック表示、YouTubeビデオ、アプリ内モバイルディスプレイにわたってクリックあたりの広告をペイパークリックできるオンライン広告サービスです。

## 宛先の仕様

Google広告の表示先に固有の次の詳細に注意してください。

* 次のIDをGoogle広告の送 [信先](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/identity_namespace_overview/identity_namespace_overview.md) に送信できます。 **Google cookie ID、IDFA、GAID、Roku ID、Microsoft ID、Amazon Fire TV ID**。
* アクティブ化されたオーディエンスは、Google プラットフォームでプログラム的に作成されます。
* Adobe Real-time CDP には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。統合を検証し、オーディエンスのターゲット設定のサイズを理解するには、Googleでのオーディエンス数を参照します。

>[!IMPORTANT]
>
>Google Adsを使用して最初のリンク先を作成する場合で、以前(オーディエンスマネージャーなどのアプリケーションを使用して)Experience Cloud IDサービスで [](https://docs.adobe.com/content/help/en/id-service/using/id-service-api/methods/idsync.html) ID同期機能を有効にしていない場合は、アドビのコンサルティングまたはカスタマーケアに連絡してID同期を有効にしてください。 以前にオーディエンスマネージャーでGoogle統合を設定していた場合、設定したID同期はAdobe Real-time CDPに引き継がれます。

## 前提条件

### 既存のGoogle広告アカウント

Googleは、サードパーティベンダーとの新しいGoogle広告統合を一時停止しました。 次の節のホワイトリストの手順を実行し、Adobe Real-time CDPでGoogle Adの宛先を作成するには、Google Adとの既存の統合が必要です。

### ホワイトリスト

>[!NOTE]
>
>ホワイトリストは、Adobe Real-time CDPで最初のGoogle広告の宛先を設定する前に必須です。 リンク先を作成する前に、Googleが以下に説明するホワイトリストのプロセスを完了していることを確認してください。

Adobe Real-time CDPでGoogle広告の宛先を作成する前に、Googleに連絡して、Adobeをデータプロバイダーとしてホワイトリストに登録するように依頼し、お客様のアカウントをホワイトリストに登録する必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：これは、アドビの Google アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **顧客 ID**：これは、アドビの Google 顧客アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* アカウントの種類：AdWords ****
* **Google AdWords ID** :これはGoogleのIDです。 通常、IDの形式は123～456～7890です。

## 宛先の作成

1. で、「Google **[!UICONTROL Connections > Destinations]**&#x200B;広告」を選択し、を選択しま **[!UICONTROL Create destination]**す。
   ![Google広告のリンク先への接続](/help/rtcdp/destinations/assets/google-2-destination.png)

2. 宛先を作成ワークフローで、宛先のを [!UICONTROL Basic Information] 入力します。
   ![Google広告の基本情報](/help/rtcdp/destinations/assets/google-2-basic-information.png)
* **[!UICONTROL Name]**:この宛先の優先名を入力します。
* **[!UICONTROL Description]**: オプション. 例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL Account Type]**:AdWordsは唯一のオプションです。
* **[!UICONTROL Account ID]**:アカウントIDにGoogle広告を入力します。 通常、IDの形式は123～456～7890です。

## Google広告へのセグメントのアクティブ化

For instructions on how to activate segments to Google Ads, see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).

