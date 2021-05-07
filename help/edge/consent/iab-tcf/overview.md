---
title: Adobe Experience PlatformWeb SDKでのIAB TCF 2.0のサポート
description: Adobe Experience PlatformWeb SDKを使用してIAB TCF 2.0の同意の環境設定をサポートする方法を学びます。
keywords: 同意；setConsent;プロファイルプライバシーフィールドグループ；エクスペリエンスイベントプライバシーフィールドグループ；プライバシーフィールドグループ；IAB TCF 2.0；リアルタイムCDP；リアルタイム顧客データプロファイル
exl-id: 78e728f4-1604-40bf-9e21-a056024bbc98
translation-type: tm+mt
source-git-commit: 7d7502b238f96eda1a15b622ba10bbccc289b725
workflow-type: tm+mt
source-wordcount: '883'
ht-degree: 0%

---

# Adobe Experience PlatformWeb SDKでのIAB TCF 2.0のサポート

Adobe Experience PlatformWeb SDKは、Interactive Advertising Bureau Transparency &amp; Consent Frameworkバージョン2.0(IAB TCF 2.0)をサポートしています。 このガイドは、リアルタイム顧客データプラットフォーム、Audience Manager、エクスペリエンスイベント、Adobe Analytics、エクスペリエンスエッジとの統合により、IAB TCF 2.0をサポートするための要件を示しています。

さらに、IAB TCF 2.0とAdobe Experience Platform Launchの統合/統合の方法を学習する際には、次のガイドを参照してください。

- [Adobe Experience Platform Launchと](./with-launch.md)
- [Adobe Experience Platform Launchなし](./without-launch.md)

## はじめに

IAB TCF 2.0を使用してWeb SDKを実装するには、エクスペリエンスデータモデル(XDM)とエクスペリエンスのイベントについて、十分に理解している必要があります。 開始を行う前に、次のドキュメントを確認してください。

- [Experience Data Model(XDM)システム概要](../../../xdm/home.md):標準化と相互運用性は、Adobe Experience Platformの主な概念です。[!DNL Experience Data Model (XDM)]Adobeによって推進されるのは、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

## Experience Platform統合

SDKを使用してAdobe Experience Platformに同意データを送信するには、次の情報が必要です。

- スキーマが[!DNL XDM Individual Profile]クラスに基づいており、TCF 2.0の同意フィールドが含まれているデータセット。[!DNL Real-time Customer Profile]での使用が有効になります。
- Platformと、前述のプロファイル対応データセットを使用して設定されたエッジ設定。

必要なデータセットとエッジ設定を作成する手順については、[TCF 2.0 compliance](../../../landing/governance-privacy-security/consent/iab/overview.md)のガイドを参照してください。

## Audience Manager統合

Adobe Audience Manager(AAM)は、IAB TCF 2.0のサポートを含みます。IAB TCF 2.0では、顧客のプライバシー選択を評価、尊重し、下流のパートナーに転送できます。<!--For more information, read the documentation on [Sending Data to Audience Manager](../audience-manager/audience-manager-overview.md).-->

>[!TIP]
>
>Adobe Experience PlatformWeb SDKを使用してAudience Managerと統合するには、Adobe Audience Managerに転送するエッジ設定があることを確認します。

## エクスペリエンスイベントとAdobe Analyticsの統合

リアルタイムCDPとAudience Managerのオーディエンスは、お客様の現在の同意の好みを追跡しますが、エクスペリエンスイベントは、イベントが収集されたときにアクティブだったお客様の同意の好みを保持できます。

イベントの同意情報を収集するには、次の情報が必要です。

- [!DNL XDM Experience Event]クラスに基づくデータセット。[!DNL Experience Event]プライバシースキーマフィールドグループを持ちます。
- 上記の[!DNL XDM Experience Event]データセットを使用して設定されたエッジ設定。

XDMエクスペリエンスイベントをAnalyticsヒットに変換する方法について詳しくは、[Analyticsの概要](../../data-collection/adobe-analytics/analytics-overview.md)のドキュメントを参照し、開始を参照してください。

## Adobe Experience PlatformWeb SDKの統合

以下の節では、IAB TCF 2.0とAdobe Experience PlatformWeb SDKの主な統合ポイントについて説明します。

>[!NOTE]
>
>リアルタイムCDPまたはAudience Managerがセットアップされていなくても、IAB TCF 2.0をWeb SDKと統合できます。 同意の環境設定を使用して、エクスペリエンスイベントの収集を制御し、ID Cookieを設定できます。

### 既定の同意

既定の同意は、既に顧客に保存されている同意の優先順位がない場合に使用されます。 つまり、デフォルトの同意オプションで、Adobe Experience PlatformWeb SDKの動作を制御し、お客様の地域に基づいて変更を行うことができます。

たとえば、GDPR(General Data Protection Regulation)の管轄区域外のお客様がいる場合、デフォルトの同意は`in`に設定できますが、GDPRの管轄区域内では`pending`に設定できます。 お客様の同意管理プラットフォーム(CMP)がお客様の地域を検出し、IAB TCF 2.0にフラグ`gdprApplies`を提供する場合があります。このフラグは、デフォルトの同意を設定するために使用できます。

デフォルトの同意について詳しくは、SDK設定ドキュメントの[デフォルトの同意の節](../../fundamentals/configuring-the-sdk.md#default-consent)を参照してください。

### 変更時の同意の設定

Adobe Experience PlatformWeb SDKには`setConsent`コマンドがあり、IAB TCF 2.0を使用するすべてのAdobeサービスに対して、お客様の同意を伝えます。リアルタイムCDPと統合する場合は、お客様のプロファイルを更新します。 Audience Managerとの統合を行っている場合は、顧客情報が更新されます。 また、この呼び出しを行うと、将来のエクスペリエンスイベントの送信を許可するかどうかを制御する、オールオアナッシングの同意プリファレンスを持つcookieも設定されます。 同意が変更されるたびにこのアクションが呼び出されることを意図しています。 今後のページ読み込みでは、エクスペリエンスエッジの同意cookieが読み取られ、エクスペリエンスイベントを送信できるかどうか、およびID cookieを設定できるかどうかが判断されます。

Audience ManagerのIAB TCF 2.0統合と同様、Experience Edgeでは、次の目的に対する明示的な同意をユーザーが提供した場合に、同意が得られます。

- **目的1：デバイスに情報を** 保存/アクセスする
- **目的10:** 製品の開発と改善
- **特別な目的1：セキュリティの** 確保、不正の防止、デバッグの実施。（IAB TCF規則では、これは常に同意する）
- **Adobeベンダーの権限：Adobe** の同意（ベンダー565）

`setConsent`コマンドの詳細については、[サポートする同意](../../consent/supporting-consent.md)のドキュメントを参照してください。

### エクスペリエンスイベントへの同意の追加

Adobe Experience PlatformWeb SDKには、エクスペリエンスイベントを収集する`sendEvent`コマンドがあります。 エクスペリエンスイベントまたはAdobe Analyticsとの統合時に、すべてのエクスペリエンスイベントの同意を希望する場合は、すべての`sendEvent`コマンドに同意情報を追加する必要があります。

`sendEvent`コマンドの詳細については、[トラッキングイベント](../../fundamentals/tracking-events.md)のドキュメントを参照してください。

## 次の手順

IAB Transparency &amp; Consent Framework 2.0の基本的な理解ができたので、IAB TCF 2.0 [をAdobe Experience Platform Launch](./with-launch.md)と組み合わせて、またはAdobe Experience Platform Launch](./without-launch.md)なしで[使用する方法に関するガイドを参照してください。
