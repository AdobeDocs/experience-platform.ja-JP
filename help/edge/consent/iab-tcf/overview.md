---
title: Adobe Experience Platform Web SDKでのIAB TCF 2.0のサポート
description: Adobe Experience Platform Web SDKを使用して、IAB TCF 2.0の同意設定をサポートする方法について説明します。
keywords: 同意；setConsent；プロファイルプライバシーフィールドグループ；エクスペリエンスイベントプライバシーフィールドグループ；プライバシーフィールドグループ；IAB TCF 2.0；リアルタイムCDP；リアルタイム顧客データプロファイル
exl-id: 78e728f4-1604-40bf-9e21-a056024bbc98
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '864'
ht-degree: 0%

---

# Adobe Experience Platform Web SDKでのIAB TCF 2.0のサポート

Adobe Experience Platform Web SDKは、Interactive Advertising Bureau Transparency &amp; Consent Frameworkバージョン2.0(IAB TCF 2.0)をサポートしています。 このガイドでは、リアルタイム顧客データプラットフォーム、Audience Manager、Experience Events、Adobe Analytics、Experience Edgeと統合した、Adobe Experience Platform Web SDKを通じてIAB TCF 2.0をサポートするための要件を示します。

さらに、タグの有無に関わらずIAB TCF 2.0を統合する方法を学ぶのに役立つガイドを次に示します。

- [タグ付き](./with-launch.md)
- [タグなし](./without-launch.md)

## はじめに

IAB TCF 2.0を使用してWeb SDKを実装するには、エクスペリエンスデータモデル(XDM)とエクスペリエンスイベントに関する十分な知識が必要です。 開始する前に、次のドキュメントを確認してください。

- [エクスペリエンスデータモデル(XDM)システムの概要](../../../xdm/home.md):標準化と相互運用性は、Adobe Experience Platformの背後にある重要な概念です。[!DNL Experience Data Model (XDM)]は、Adobe主導で、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

## Experience Platform統合

SDKを使用してAdobe Experience Platformに同意データを送信するには、以下が必要です。

- [!DNL XDM Individual Profile]クラスに基づくスキーマを持ち、TCF 2.0同意フィールドを含むデータセット。[!DNL Real-time Customer Profile]で使用可能。
- Platformと、前述のプロファイル対応データセットを使用して設定されたデータストリーム。

必要なデータセットとデータストリームの作成手順については、[TCF 2.0準拠](../../../landing/governance-privacy-security/consent/iab/overview.md)のガイドを参照してください。

## Audience Manager統合

Adobe Audience Manager(AAM)には、顧客のプライバシー選択を評価、遵守およびダウンストリームパートナーに転送できるIAB TCF 2.0のサポートが含まれています。<!--For more information, read the documentation on [Sending Data to Audience Manager](../audience-manager/audience-manager-overview.md).-->

>[!TIP]
>
>Adobe Experience Platform Web SDKを使用してAudience Managerと統合するには、Adobe Audience Managerに転送するためのデータストリームが設定されていることを確認してください。

## エクスペリエンスイベントとAdobe Analyticsの統合

リアルタイムCDPとAudience Managerのオーディエンスは、顧客の現在の同意設定を追跡しますが、エクスペリエンスイベントは、イベントが収集された際にアクティブだった顧客の同意設定を保持できます。

イベントの同意情報を収集するには、以下が必要です。

- [!DNL XDM Experience Event]クラスに基づくデータセットで、[!DNL Experience Event]プライバシースキーマフィールドがグループ化されています。
- 上記の[!DNL XDM Experience Event]データセットで設定されたデータストリーム。

XDMエクスペリエンスイベントをAnalyticsヒットに変換する方法について詳しくは、まず[Analyticsの概要](../../data-collection/adobe-analytics/analytics-overview.md)ドキュメントをお読みください。

## Adobe Experience Platform Web SDKとの統合

以下の節では、IAB TCF 2.0とAdobe Experience Platform Web SDKの主な統合ポイントについて説明します。

>[!NOTE]
>
>リアルタイムCDPまたはAudience Managerが設定されていなくても、IAB TCF 2.0をWeb SDKと統合できます。 同意の環境設定を使用して、エクスペリエンスイベントの収集を制御し、ID Cookieを設定できます。

### デフォルトの同意

デフォルトの同意は、顧客に対して同意設定が保存されていない場合に使用されます。 つまり、デフォルトの同意オプションで、Adobe Experience Platform Web SDKの動作を制御し、お客様の地域に基づいて変更を加えることができます。

例えば、EU一般データ保護規則(GDPR)の管轄区域内にないお客様がいる場合、デフォルトの同意は`in`に設定できますが、GDPRの管轄区域内では、デフォルトの同意は`pending`に設定できます。 同意管理プラットフォーム(CMP)が顧客の地域を検出し、IAB TCF 2.0にフラグ`gdprApplies`を提供する場合があります。このフラグを使用して、デフォルトの同意を設定できます。

デフォルトの同意について詳しくは、SDK設定ドキュメントの[デフォルトの同意に関する節](../../fundamentals/configuring-the-sdk.md#default-consent)を参照してください。

### 変更時の同意の設定

Adobe Experience Platform Web SDKには`setConsent`コマンドがあり、IAB TCF 2.0を使用して、顧客の同意設定をすべてのAdobeサービスに伝えます。リアルタイムCDPと統合する場合は、顧客のプロファイルが更新されます。 Audience Managerと統合すると、顧客の情報が更新されます。 また、このを呼び出すと、今後のエクスペリエンスイベントの送信を許可するかどうかを制御する、すべてまたは何も指定しない同意設定がCookieに設定されます。 このアクションは、同意が変更されるたびに呼び出されることを意図しています。 今後のページ読み込みでは、Experience Edgeの同意Cookieが読み取られ、エクスペリエンスイベントを送信できるかどうか、およびID Cookieを設定できるかどうかを判断します。

Audience ManagerのIAB TCF 2.0統合と同様、Experience Edgeも、顧客が次の目的に対して明示的に同意した場合に同意を提供します。

- **目的1:** デバイス上に情報を保存し、その情報にアクセスする
- **目的10:** 製品の開発と改善
- **特別な目的1:** セキュリティの確保、不正の防止、デバッグ。（IAB TCF規制に従い、これは常に同意されます）
- **Adobeベンダーの権限：** Adobeの同意（ベンダー565）

`setConsent`コマンドの詳細については、[同意](../../consent/supporting-consent.md)のサポートに関するドキュメントを参照してください。

### エクスペリエンスイベントへの同意の追加

Adobe Experience Platform Web SDKには、エクスペリエンスイベントを収集する`sendEvent`コマンドがあります。 エクスペリエンスイベントまたはAdobe Analyticsとの統合中で、すべてのエクスペリエンスイベントに同意設定を行う場合は、すべての`sendEvent`コマンドに同意情報を追加する必要があります。

`sendEvent`コマンドの詳細については、[トラッキングイベント](../../fundamentals/tracking-events.md)に関するドキュメントを参照してください。

## 次の手順

IAB Transparency &amp; Consent Framework 2.0に関する基本的な知識が整ったので、IAB TCF 2.0 [をタグ](./with-launch.md)または[をタグ](./without-launch.md)なしで使用する方法に関するガイドを参照してください。
