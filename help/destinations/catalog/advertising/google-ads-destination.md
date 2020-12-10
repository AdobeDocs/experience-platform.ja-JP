---
keywords: Google ads;google ads;google adwords;Google AdWords;Google Adwords
title: Google 広告の宛先
seo-title: Google 広告の宛先
description: Google 広告（旧称 Google AdWords）は、テキストベースの検索、グラフィック表示、YouTube ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスです。
seo-description: Google 広告（旧称 Google AdWords）は、テキストベースの検索、グラフィック表示、YouTube ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスです。
translation-type: tm+mt
source-git-commit: 7129a375b1bf4623f78989ed75fcd2bb5dad4a02
workflow-type: tm+mt
source-wordcount: '663'
ht-degree: 26%

---


# [!DNL Google Ads] 宛先

## 概要

[!DNL Google Ads](旧称 [!DNL Google AdWords])は、企業がテキストベースの検索、グラフィックディスプレイ、 [!DNL YouTube] ビデオ、アプリ内モバイルディスプレイにわたってペイパークリック広告を行えるようにするオンライン広告サービスです。

## 宛先の仕様

Note the following details that are specific to [!DNL Google Ads] destinations:

* You can send the following [identities](../../../identity-service/namespaces.md) to [!DNL Google Ads] destinations: Google cookie ID, IDFA, GAID, Roku IDs, Microsoft IDs, and Amazon Fire TV IDs.
* Activated audiences are created programmatically in the [!DNL Google] platform.
*  Real-time CDP には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。統合を検証し、オーディエンスターゲット設定サイズについて理解するには、Google でのオーディエンス数を参照します。

>[!IMPORTANT]
>
>If you are looking to create your first destination with [!DNL Google Ads] and have not enabled the [ID sync functionality](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html) in Experience Cloud ID Service in the past (with Audience Manager or other applications), please reach out to Adobe Consulting or Customer Care to enable ID syncs. 以前にAudience ManagerでGoogle統合を設定した場合、Real-time CDPに繰り越しを設定したID同期。

### 書き出しタイプ {#export-type}

**セグメントエクスポート** — セグメント(オーディエンス)のすべてのメンバーをGoogleのエクスポート先にエクスポートします。

## 前提条件

### 既存の [!DNL Google Ads] アカウント

>[!IMPORTANT]
>
> [!DNL Google] では、サードパーティベンダーとの新しい [!DNL Google Ads] cookie統合が廃止されました。 次の節の許可リスト手順を実行するには、との既存の統合が必要で [!DNL Google Ads]す。 その結果、を使用するための推奨される方法は、 [!DNL Google Ads][!DNL Google Customer Match] 統合を設定することです。 統合の作成の詳細については、 [!DNL Google Customer Match] 接続の作成に関するチュートリアルを参照してくだ [[!DNL Google Customer Match]](./google-customer-match.md) さい。

### 許可リスト

>[!NOTE]
>
>この許可リストは、リアルタイムCDPで最初の [!DNL Google Ads] 宛先を設定する前に必須です。 Please ensure the allow list process described below has been completed by [!DNL Google] before creating a destination.

リアルタイムCDPで [!DNL Google Ads][!DNL Google] 宛先を作成する前に、許可されているデータプロバイダのリストにAdobeを置くため、お客様のアカウントを許可リストに追加するために、に問い合わせる必要があります。 Contact [!DNL Google] and provide the following information:

* **アカウントID** :これは、AdobeのアカウントID [!DNL Google]です。 アドビカスタマーケアまたは Adobe 担当者に問い合わせて、この ID を取得してください。
* **顧客ID** :これは、Adobeの顧客アカウントID [!DNL Google]です。 この ID を取得するには、アドビカスタマーケアまたはアドビの担当者に問い合わせてください。
* アカウントのタイプ：**AdWords**
* **Google AdWords ID** :これはお客様のIDで [!DNL Google]す。 通常、ID の形式は 123-456-7890 です。

## 宛先の設定

**[!UICONTROL 接続]** / **[!UICONTROL 宛先]**、を選択し、「 [!DNL Google Ads]設定 ****」を選択します。

![Google 広告の宛先への接続](../../assets/catalog/advertising/google-ads-destination/catalog.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに **[!UICONTROL 「アクティブ化]** 」ボタンが表示されます。 「 **[!UICONTROL アクティブ化]** 」と「 **[!UICONTROL 設定]**」の違いについて詳しくは、表示先ワークスペースのドキュメントの「 [カタログ](../../ui/destinations-workspace.md#catalog) 」セクションを参照してください。

In the **Setup** step of the create destination workflow, fill in the [!UICONTROL Basic Information] for the destination.

![Google 広告の基本情報](../../assets/catalog/advertising/google-ads-destination/setup.png)

* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントのタイプ]**：AdWords は利用可能な唯一のオプションです。
* **[!UICONTROL アカウントID]**:アカウントIDをに入力し [!DNL Google Ads]ます。 通常、ID の形式は 123-456-7890 です。
* **[!UICONTROL マーケティングの使用例]**:マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 Adobe定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](../../../rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 個々のAdobe定義マーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](../../../data-governance/policies/overview.md#core-actions)。

## Activate segments to [!DNL Google Ads]

For instructions on how to activate segments to [!DNL Google Ads], see [Activate Data to Destinations](../../ui/activate-destinations.md).

## 書き出されたデータ

データが正常に宛先にエクスポートされたかどうかを確認するには、アカウントを確認して [!DNL Google Ads] く [!DNL Google Ads] ださい。 アクティベーションに成功すると、オーディエンスがアカウントに入力されます。