---
title: Google 広告の宛先
seo-title: Google 広告の宛先
description: Google 広告（旧称 Google AdWords）は、テキストベースの検索、グラフィック表示、YouTube ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスです。
seo-description: Google 広告（旧称 Google AdWords）は、テキストベースの検索、グラフィック表示、YouTube ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスです。
translation-type: tm+mt
source-git-commit: dc5ee796ca390d06fc8e08bd6c30e88a0d96dd53
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 33%

---


# Google 広告の宛先

## 概要

Google 広告（旧称 Google AdWords）は、テキストベースの検索、グラフィック表示、YouTube ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスです。

## 宛先の仕様

Google広告の表示先に固有の次の詳細に注意してください。

* 次の [IDをGoogle Adsの送信先に送信できます](../../identity-service/namespaces.md) 。 **Google cookie ID、IDFA、GAID、Roku ID、Microsoft ID、Amazon Fire TV**。
* アクティブ化されたオーディエンスは、Google プラットフォームでプログラム的に作成されます。
* Adobe Real-time CDP には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。Googleでオーディエンス数を参照して統合を検証し、オーディエンスのターゲット設定サイズを把握する。

>[!IMPORTANT]
>
>Google Adsでの最初のリンク先を作成する場合で、 [](https://docs.adobe.com/content/help/ja-JP/id-service/using/id-service-api/methods/idsync.html) ID同期機能を以前(オーディエンスマネージャーや他のアプリケーションを使用)Experience Cloud IDサービスで有効にしていない場合は、アドビのコンサルティングまたはカスタマーケアにご連絡ください。 Google統合を以前にオーディエンスマネージャーで設定している場合、設定したID同期はAdobe Real-time CDPに繰り越されます。

## 前提条件

### 既存のGoogle広告アカウント

Googleは、サードパーティベンダーとの新しいGoogle広告統合を一時停止しました。 次の節の許可リスト手順を実行し、Adobe Real-time CDPでGoogle Adsの宛先を作成するには、Google Adsとの既存の統合が必要です。

### リストを許可

>[!NOTE]
>
>許可リストは、Adobe Real-time CDPで最初のGoogle広告の宛先を設定する前に必須です。 宛先を作成する前に、以下に説明するリストの許可プロセスがGoogleによって完了していることを確認してください。

Adobe Real-time CDPでGoogle Adsの配信先を作成する前に、Googleに問い合わせて、許可されているデータプロバイダーのリストを有効にし、アカウントを許可リストに追加する必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：これは、アドビの Google アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **顧客 ID**：これは、アドビの Google 顧客アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* アカウントの種類： **AdWords**
* **Google AdWords ID** : これはGoogleのIDです。 通常、IDの形式は123～456～7890です。

## 宛先の作成

1. In **[!UICONTROL Connections > Destinations]**, select Google Ads, and select **[!UICONTROL Create destination]**.
   ![Google広告のリンク先の接続](/help/rtcdp/destinations/assets/google-2-destination.png)

2. In the Create destination workflow, fill in the [!UICONTROL Basic Information] for the destination. <br>

   ![Google広告の基本情報](/help/rtcdp/destinations/assets/google-2-basic-information.png)
* **[!UICONTROL 名前]**:この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントタイプ]**: AdWordsは唯一の使用可能なオプションです。
* **[!UICONTROL アカウントID]**: アカウントIDに「Google広告」と入力します。 通常、IDの形式は123～456～7890です。

## Google広告に対するセグメントのアクティブ化

For instructions on how to activate segments to Google Ads, see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).

