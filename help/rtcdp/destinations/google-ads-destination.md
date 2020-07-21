---
title: Google 広告の宛先
seo-title: Google 広告の宛先
description: Google 広告（旧称 Google AdWords）は、テキストベースの検索、グラフィック表示、YouTube ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスです。
seo-description: Google 広告（旧称 Google AdWords）は、テキストベースの検索、グラフィック表示、YouTube ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスです。
translation-type: tm+mt
source-git-commit: 6f680a60c88bc5fee6ce9cb5a4f314c4b9d02249
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 20%

---


# [!DNL Google Ads] 宛先

## 概要

[!DNL Google Ads](旧称 [!DNL Google AdWords])は、企業がテキストベースの検索、グラフィックディスプレイ、 [!DNL YouTube] ビデオ、アプリ内モバイルディスプレイにわたってペイパークリック広告を行えるようにするオンライン広告サービスです。

## 宛先の仕様

Note the following details that are specific to [!DNL Google Ads] destinations:

* 次の [IDを宛先に送信でき](../../identity-service/namespaces.md)[!DNL Google Ads] ます。 **Google cookie ID、IDFA、GAID、Roku ID、Microsoft ID、Amazon Fire TV**。
* Activated audiences are created programmatically in the [!DNL Google] platform.
* Adobe Real-time CDP には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。Googleでオーディエンス数を参照して統合を検証し、オーディエンスのターゲット設定サイズを把握する。

>[!IMPORTANT]
>
>最初の宛先を作成する場合 [!DNL Google Ads] は、以前(Audience Managerや他のExperience Cloudを使用)にアドビの [](https://docs.adobe.com/content/help/ja-JP/id-service/using/id-service-api/methods/idsync.html) IDサービスでID同期機能を有効にしていない限り、アドビのコンサルティングまたはカスタマーケアにご連絡いただき、ID同期を有効にしてください。 以前にAudience ManagerでGoogle統合を設定した場合、設定したID同期はAdobe Real-time CDPに繰り越します。

## 前提条件

### 既存の [!DNL Google Ads] アカウント

[!DNL Google] は、サードパーティベンダーとの新しい [!DNL Google Ads] 統合を一時停止しています。 次の節の許可リスト手順を実行し、Adobe Real-time CDP [!DNL Google Ads] で [!DNL Google Ads] 宛先を作成するには、との既存の統合が必要です。

### 許可リスト

>[!NOTE]
>
>許可リストは、Adobe Real-time CDPで最初の [!DNL Google Ads] 宛先を設定する前に必須です。 宛先を作成する前に、次に説明する許可リストプロセスがによって完了しているこ [!DNL Google] とを確認してください。

Adobe Real-time CDPで [!DNL Google Ads] 宛先を作成する前に、アドビに連絡して、許可されているデータプロバイダーのリスト [!DNL Google] に加え、お客様のアカウントを許可リストに追加する必要があります。 Contact [!DNL Google] and provide the following information:

* **アカウントID** : これは、を使用するアドビのアカウントIDで [!DNL Google]す。 このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **顧客ID** : これは、アドビの顧客アカウントID [!DNL Google]です。 このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* アカウントの種類： **AdWords**
* **Google AdWords ID** : これはお客様のIDで [!DNL Google]す。 通常、IDの形式は123～456～7890です。

## 宛先の作成

1. **[!UICONTROL 接続/宛先]**&#x200B;を選択し、「宛先を [!DNL Google Ads]作成 ****」を選択します。
   ![Google広告のリンク先の接続](/help/rtcdp/destinations/assets/google-2-destination.png)

2. 宛先を作成ワークフローの **セットアップ** 手順で、宛先の [!UICONTROL 基本情報] を入力します。 <br>

   ![Google広告の基本情報](/help/rtcdp/destinations/assets/google-2-destination-setup-step.png)
* **[!UICONTROL 名前]**:この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントタイプ]**: AdWordsは唯一の使用可能なオプションです。
* **[!UICONTROL アカウントID]**: アカウントIDをに入力し [!DNL Google Ads]ます。 通常、IDの形式は123～456～7890です。
* **[!UICONTROL マーケティングの使用例]**: マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 アドビ定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](/help/rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 アドビが定義した個々のマーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](/help/data-governance/policies/overview.md#core-actions)。

## Activate segments to [!DNL Google Ads]

For instructions on how to activate segments to [!DNL Google Ads], see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).

