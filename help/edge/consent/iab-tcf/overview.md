---
title: Adobe Experience Platform Web SDK での IAB TCF 2.0 のサポート
description: Adobe Experience Platform Web SDK を使用して、IAB TCF 2.0 の同意設定をサポートする方法を説明します
keywords: 同意；setConsent；プロファイルプライバシーフィールドグループ；エクスペリエンスイベントプライバシーフィールドグループ；プライバシーフィールドグループ；IAB TCF 2.0；リアルタイム CDP；リアルタイム顧客データプロファイル
exl-id: 78e728f4-1604-40bf-9e21-a056024bbc98
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '864'
ht-degree: 0%

---

# Adobe Experience Platform Web SDK での IAB TCF 2.0 のサポート

Adobe Experience Platform Web SDK は、Interactive Advertising Bureau の Transparency &amp; Consent Framework バージョン 2.0(IAB TCF 2.0) をサポートしています。 このガイドでは、Real-time Customer Data Platform、Audience Manager、Experience Event、Adobe Analytics、および Experience Edge との統合により、Adobe Experience Platform Web SDK を介して IAB TCF 2.0 をサポートするための要件を示します。

また、タグの有無に関わらず、IAB TCF 2.0 を統合する方法を学ぶのに役立つガイドを次に示します。

- [タグ付き](./with-launch.md)
- [タグなし](./without-launch.md)

## はじめに

IAB TCF 2.0 を使用して Web SDK を実装するには、エクスペリエンスデータモデル (XDM) とエクスペリエンスイベントに関する十分な知識が必要です。 開始する前に、次のドキュメントを確認してください。

- [エクスペリエンスデータモデル (XDM) システムの概要](../../../xdm/home.md):標準化と相互運用性は、Adobe Experience Platformの背後にある重要な概念です。[!DNL Experience Data Model (XDM)]は、Adobe主導で、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

## Experience Platform統合

SDK を使用してAdobe Experience Platformに同意データを送信するには、以下が必要です。

- [!DNL XDM Individual Profile] クラスに基づくスキーマを持ち、TCF 2.0 同意フィールドを含むデータセット。[!DNL Real-time Customer Profile] で使用できます。
- Platform と前述のプロファイル対応データセットを使用して設定されたデータストリーム。

必要なデータセットとデータストリームの作成手順については、[TCF 2.0 準拠 ](../../../landing/governance-privacy-security/consent/iab/overview.md) のガイドを参照してください。

## Audience Manager統合

Adobe Audience Manager(AAM) には、顧客のプライバシー選択を評価、遵守、ダウンストリームパートナーに転送できる IAB TCF 2.0 のサポートが含まれています。<!--For more information, read the documentation on [Sending Data to Audience Manager](../audience-manager/audience-manager-overview.md).-->

>[!TIP]
>
>Adobe Experience Platform Web SDK を使用してAudience Managerと統合するには、Adobe Audience Managerに転送するデータストリームが設定されていることを確認してください。

## エクスペリエンスイベントとAdobe Analyticsの統合

リアルタイム CDP とAudience Managerのオーディエンスは顧客の現在の同意設定を追跡するのに対して、Experience Events は、イベントの収集時にアクティブだった顧客の同意設定を保持できます。

イベントに関する同意情報を収集するには、以下が必要です。

- [!DNL XDM Experience Event] クラスに基づくデータセットで、[!DNL Experience Event] プライバシースキーマフィールドがグループ化されています。
- 上記の [!DNL XDM Experience Event] データセットで設定されたデータストリーム。

XDM エクスペリエンスイベントを Analytics ヒットに変換する方法の詳細については、まず [Analytics の概要 ](../../data-collection/adobe-analytics/analytics-overview.md) ドキュメントをお読みください。

## Adobe Experience Platform Web SDK との統合

以下の節では、IAB TCF 2.0 とAdobe Experience Platform Web SDK の間の主な統合ポイントについて説明します。

>[!NOTE]
>
>リアルタイム CDP またはAudience Managerが設定されていなくても、IAB TCF 2.0 を Web SDK と統合できます。 同意設定を使用して、エクスペリエンスイベントの収集と ID Cookie の設定を制御できます。

### デフォルトの同意

デフォルトの同意は、顧客に対して同意設定が既に保存されていない場合に使用されます。 つまり、デフォルトの同意オプションで、Adobe Experience Platform Web SDK の動作を制御し、お客様の地域に基づいて変更を加えることができます。

例えば、EU 一般データ保護規則 (GDPR) の管轄区域内にない顧客がいる場合、デフォルトの同意は `in` に設定できますが、GDPR の管轄区域内では、デフォルトの同意は `pending` に設定できます。 お客様の同意管理プラットフォーム (CMP) が顧客の地域を検出し、IAB TCF 2.0 にフラグ `gdprApplies` を提供する場合があります。このフラグを使用して、デフォルトの同意を設定できます。

デフォルトの同意について詳しくは、SDK 設定ドキュメントの [ デフォルトの同意に関する節 ](../../fundamentals/configuring-the-sdk.md#default-consent) を参照してください。

### 変更時の同意の設定

Adobe Experience Platform Web SDK には `setConsent` コマンドがあり、IAB TCF 2.0 を使用するすべてのAdobe サービスに、お客様の同意設定を伝えます。リアルタイム CDP と統合している場合は、お客様のプロファイルが更新されます。 Audience Managerと統合すると、顧客の情報が更新されます。 また、このを呼び出すと、今後のエクスペリエンスイベントの送信を許可するかどうかを制御する、すべてまたは何も指定しない同意設定が Cookie に設定されます。 このアクションは、同意が変更されるたびに呼び出されることを意図しています。 今後のページ読み込みでは、Experience Edge の同意 Cookie が読み取られて、Experience Events を送信できるかどうか、および ID Cookie を設定できるかどうかが判断されます。

Audience Managerの IAB TCF 2.0 統合と同様、Experience Edge は、顧客が次の目的に対して明示的に同意した場合に同意を提供します。

- **目的 1:** デバイス上での情報の保存および/またはアクセス
- **目的 10:** 製品の開発と改善
- **特別な目的 1:** セキュリティの確保、不正の防止、デバッグ。（IAB TCF 規制に従い、これは常に同意されます）
- **Adobeベンダー権限：** Adobeに対する同意（ベンダー 565）

`setConsent` コマンドの詳細については、[ 同意のサポート ](../../consent/supporting-consent.md) に関するドキュメントを参照してください。

### エクスペリエンスイベントへの同意の追加

Adobe Experience Platform Web SDK には、エクスペリエンスイベントを収集する `sendEvent` コマンドがあります。 Experience Events またはAdobe Analyticsとの統合中で、すべてのエクスペリエンスイベントに対する同意設定を希望する場合は、すべての `sendEvent` コマンドに同意情報を追加する必要があります。

`sendEvent` コマンドの詳細については、[ トラッキングイベント ](../../fundamentals/tracking-events.md) に関するドキュメントを参照してください。

## 次の手順

IAB Transparency &amp; Consent Framework 2.0 に関する基本的な理解が整ったので、タグ ](./with-launch.md) またはタグ [ なしで IAB TCF 2.0 [ を使用する方法に関するガイドを参照してください。](./without-launch.md)
