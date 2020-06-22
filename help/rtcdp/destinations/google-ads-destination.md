---
title: Google 広告の宛先
seo-title: Google 広告の宛先
description: Google 広告（旧称 Google AdWords）は、テキストベースの検索、グラフィック表示、YouTube ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスです。
seo-description: Google 広告（旧称 Google AdWords）は、テキストベースの検索、グラフィック表示、YouTube ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスです。
translation-type: tm+mt
source-git-commit: 3c598454a868139b7604c5c7ca2b98fa0f1bb961
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 29%

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
>Google Adsでの最初のリンク先を作成する場合で、以前(Audience Managerやその他のExperience Cloudを含む)、 [ID同期機能を有効にしていない場合は](https://docs.adobe.com/content/help/ja-JP/id-service/using/id-service-api/methods/idsync.html) 、アドビのコンサルティングまたはカスタマーケアにご連絡いただき、ID同期を有効にしてください。 以前にAudience ManagerでGoogle統合を設定した場合、設定したID同期はAdobe Real-time CDPに繰り越します。

## 前提条件

### 既存のGoogle広告アカウント

Googleは、サードパーティベンダーとの新しいGoogle広告統合を一時停止しました。 次のセクションの許可リスト手順を実行し、Adobe Real-time CDPでGoogle Adsの宛先を作成するには、Google Adsとの既存の統合が必要です。

### 許可リスト

>[!NOTE]
>
>この許可リストは、Adobe Real-time CDPで最初のGoogle広告の宛先を設定する前に必須です。 リンク先を作成する前に、Googleが以下に説明する許可リストプロセスを完了していることを確認してください。

Adobe Real-time CDPでGoogle Adsの宛先を作成する前に、Googleに問い合わせて、許可されているデータプロバイダーのリストを有効にし、アカウントを許可リストに追加するようにAdobeに求める必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：これは、アドビの Google アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **顧客 ID**：これは、アドビの Google 顧客アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* アカウントの種類： **AdWords**
* **Google AdWords ID** : これはGoogleのIDです。 通常、IDの形式は123～456～7890です。

## 宛先の作成

1. In **[!UICONTROL Connections > Destinations]**, select Google Ads, and select **[!UICONTROL Create destination]**.
   ![Google広告のリンク先の接続](/help/rtcdp/destinations/assets/google-2-destination.png)

2. 宛先を作成ワークフローの **セットアップ** 手順で、宛先の [!UICONTROL 基本情報] を入力します。 <br>
   ![Google広告の基本情報](/help/rtcdp/destinations/assets/google-ads-setup-step.png)
* **[!UICONTROL 名前]**:この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントタイプ]**: AdWordsは唯一の使用可能なオプションです。
* **[!UICONTROL アカウントID]**: アカウントIDに「Google広告」と入力します。 通常、IDの形式は123～456～7890です。
* **[!UICONTROL マーケティングの使用例]**: マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 アドビ定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](/help/rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 アドビが定義した個々のマーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](/help/data-governance/policies/overview.md#core-actions)。

## Google広告に対するセグメントのアクティブ化

For instructions on how to activate segments to Google Ads, see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).

