---
title: IAB Transparency & Consent Framework 2.0の概要
seo-title: Interactive Advertising Bureau Transparency & Consent Framework 2.0からのAdobe Experience PlatformWeb SDKの同意の環境設定のサポート
description: Experience PlatformWeb SDKを使用してIAB TCF 2.0の同意の環境設定をサポートする方法を説明します。
seo-description: Experience PlatformWeb SDKを使用してIAB TCF 2.0の同意の環境設定をサポートする方法を説明します。
keywords: consent;setConsent;Profile Privacy Mixin;Experience Event Privacy Mixin;Privacy Mixin;IAB TCF 2.0;Real-time CDP;Real-time Customer Data Profile
translation-type: tm+mt
source-git-commit: 3f70e7fdd5888018f3814d1446042e96d2e53304
workflow-type: tm+mt
source-wordcount: '936'
ht-degree: 0%

---


# IAB Transparency &amp; Consent Framework 2.0の概要

Adobe Experience PlatformWeb SDKは、Interactive Advertising Bureau Transparency &amp; Consent Frameworkバージョン2.0(IAB TCF 2.0)をサポートしています。 このガイドは、リアルタイム顧客データプラットフォーム、Audience Manager、エクスペリエンスイベント、Adobe Analytics、エクスペリエンスエッジとの統合により、IAB TCF 2.0をサポートするための要件を示しています。

さらに、IAB TCF 2.0とAdobe Experience Platform Launchの統合/統合の方法を学習する際には、次のガイドを参照してください。

- [Adobe Experience Platform Launchと](./with-launch.md)
- [Adobe Experience Platform Launchなし](./without-launch.md)

## はじめに

IAB TCF 2.0を使用してWeb SDKを実装するには、エクスペリエンスデータモデル(XDM)とエクスペリエンスのイベントについて、十分に理解している必要があります。 開始を行う前に、次のドキュメントを確認してください。

- [Experience Data Model(XDM)システム概要](../../../xdm/home.md):標準化と相互運用性は、Adobe Experience Platformの主な概念です。 [!DNL Experience Data Model (XDM)]Adobeによって推進されるのは、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

## リアルタイム顧客データプラットフォーム統合

Real-time Customer Data Platform(Real-time CDP)は、Adobe Experience Platform上に構築されており、複数のエンタープライズ・ソースから既知の匿名データを統合できます。 これにより、すべてのチャネルとデバイスにわたって、パーソナライズされた顧客体験をリアルタイムで提供するために使用できる顧客プロファイルを作成できます。 SDKを介してリアルタイムCDPに同意データを送信するには、以下が必要です。

- プロファイルのプライバシーミックスインを使用し、での使用を有効にした、 [!DNL XDM Individual Profile] クラスに基づくデータセット [!DNL Real-time Customer Profile]。
- Real-time CDPと、前述のプロファイルデータセットを使用して設定されたエッジ構成。

必要なデータセットの作成方法については、TCF 2.0の同意を取り込むためのデータセットの [作成に関するチュートリアルを参照してください](../../../rtcdp/privacy/iab/dataset-preparation.md) 。

エッジ設定の作成手順については、 [IAB TCF 2.0準拠の概要](../../../rtcdp/privacy/privacy-overview.md) （英語）を参照してください。

## Audience Manager統合

Adobe Audience Manager(AAM)は、IAB TCF 2.0のサポートを含みます。IAB TCF 2.0では、顧客のプライバシー選択を評価、尊重、ダウンストリームのパートナーに転送できます。 <!--For more information, read the documentation on [Sending Data to Audience Manager](../audience-manager/audience-manager-overview.md).-->

>[!TIP]
>
>Adobe Experience PlatformWeb SDKを使用してAudience Managerと統合するには、Adobe Audience Managerに転送するエッジ設定があることを確認します。

## エクスペリエンスイベントとAdobe Analyticsの統合

リアルタイムCDPとAudience Managerのオーディエンスは、お客様の現在の同意の好みを追跡しますが、エクスペリエンスイベントは、イベントが収集されたときにアクティブだったお客様の同意の好みを保持できます。

イベントの同意情報を収集するには、次の情報が必要です。

- プライバシーミックスインを含む、 [!DNL XDM Experience Event][!DNL Experience Event] クラスに基づくデータセット。
- 上記のデータセットを使用したエッジ設定 [!DNL XDM Experience Event] 。

XDMエクスペリエンスのイベントをAnalyticsヒットに変換する方法について詳しくは、 [Analyticsの概要](../../data-collection/adobe-analytics/analytics-overview.md) 「」ドキュメントを参照して開始してください。

## Adobe Experience PlatformWeb SDKの統合

以下の節では、IAB TCF 2.0とAdobe Experience PlatformWeb SDKの主な統合ポイントについて説明します。

>[!NOTE]
>
>リアルタイムCDPまたはAudience Managerがセットアップされていなくても、IAB TCF 2.0をWeb SDKと統合できます。 同意の環境設定を使用して、エクスペリエンスイベントの収集を制御し、ID Cookieを設定できます。

### 既定の同意

既定の同意は、既に顧客に保存されている同意の優先順位がない場合に使用されます。 つまり、デフォルトの同意オプションで、Adobe Experience PlatformWeb SDKの動作を制御し、お客様の地域に基づいて変更を行うことができます。

たとえば、GDPR(General Data Protection Regulation)の管轄区域外のお客様がいる場合、デフォルトの同意はに設定できますが、GDPRの管轄区域内では、デフォルトの同意はに設定でき `in``pending`ます。 クラウド管理プラットフォーム(CMP)が顧客の地域を検出し、IAB TCF 2.0にフラグを提供する場合があります。このフラグは、デフォルトの同意を設定するために使用できます。 `gdprApplies`

デフォルトの同意について詳しくは、SDK設定ドキュメントの [デフォルトの同意の節](../../fundamentals/configuring-the-sdk.md#default-consent) を参照してください。

### 変更時の同意の設定

Adobe Experience PlatformWeb SDKには、IAB TCF 2.0を使用するすべてのAdobeサービスに対して、お客様の同意を伝える `setConsent` コマンドがあります。Real-time CDPと統合する場合は、お客様のプロファイルを更新します。 Audience Managerとの統合を行っている場合は、顧客情報が更新されます。 また、この呼び出しを行うと、将来のエクスペリエンスイベントの送信を許可するかどうかを制御する、オールオアナッシングの同意プリファレンスを持つcookieも設定されます。 同意が変更されるたびにこのアクションが呼び出されることを意図しています。 今後のページ読み込みでは、エクスペリエンスエッジの同意cookieが読み取られ、エクスペリエンスイベントを送信できるかどうか、およびID cookieを設定できるかどうかが判断されます。

Audience ManagerのIAB TCF 2.0統合と同様、Experience Edgeでは、次の目的に対する明示的な同意をユーザーが提供した場合に、同意が得られます。

- **目的1:** デバイス上の情報の保存/アクセス
- **目的10:** 製品の開発と改善
- **特別な目的1:** セキュリティの確保、不正の防止、デバッグ。 （IAB TCF規則では、これは常に同意する）
- **Adobeベンダーの権限：** Adobeの同意（ベンダ565）

コマンドの詳細については、 `setConsent` サポートする同意に関するドキュメントを参照して [ください](../../consent/supporting-consent.md)。

### エクスペリエンスイベントへの同意の追加

Adobe Experience PlatformWeb SDKには、エクスペリエンスイベントを収集する `sendEvent` コマンドがあります。 エクスペリエンスイベントまたはAdobe Analyticsとの統合時に、すべてのエクスペリエンスイベントの同意を希望する場合は、すべての `sendEvent` コマンドに同意情報を追加する必要があります。

この `sendEvent` コマンドの詳細については、 [イベントの](../../fundamentals/tracking-events.md)追跡に関するドキュメントを参照してください。

## 次の手順

IAB Transparency &amp; Consent Framework 2.0の基本的な理解ができたので、IAB TCF 2.0をAdobe Experience Platform Launch [と併用するか、Adobe Experience Platform Launch](./with-launch.md) なしで使用する方法に関するガイドを参照してください [](./without-launch.md)。
