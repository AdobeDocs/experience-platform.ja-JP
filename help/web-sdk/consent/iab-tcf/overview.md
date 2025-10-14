---
title: Adobe Experience Platform Web SDKでの IAB TCF 2.0 のサポート
description: Adobe Experience Platform web SDKを使用して IAB TCF 2.0 同意環境設定をサポートする方法を説明します
keywords: 同意；setConsent；プロファイルプライバシーフィールドグループ；Experience Event Privacy フィールドグループ；プライバシーフィールドグループ；IAB TCF 2.0;Real-Time CDP;
exl-id: 78e728f4-1604-40bf-9e21-a056024bbc98
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '863'
ht-degree: 0%

---

# Adobe Experience Platform Web SDKでの IAB TCF 2.0 のサポート

Adobe Experience Platform Web SDKでは、Interactive Advertising Bureau Transparency &amp; Consent Framework Version 2.0 （IAB TCF 2.0）がサポートされています。 このガイドでは、Adobe Real-Time Customer Data Platform、Audience Manager、エクスペリエンスイベント、Adobe Analytics、Edge Networkと統合された、Adobe Experience Platform web SDKを使用した IAB TCF 2.0 のサポート要件について説明します。

さらに、タグを使用する場合と使用しない場合の IAB TCF 2.0 の統合方法を学ぶうえで役に立つガイドが以下にあります。

- [タグ付き](./with-tags.md)
- [タグなし](./without-tags.md)

## はじめに

IAB TCF 2.0 で Web SDKを実装するには、エクスペリエンスデータモデル（XDM）とエクスペリエンスイベントに関する十分な知識が必要です。 開始する前に、次のドキュメントを確認してください。

- [&#x200B; エクスペリエンスデータモデル（XDM）システムの概要 &#x200B;](../../../xdm/home.md)：標準化と相互運用性は、Adobe Experience Platformを支える重要な概念です。 Adobeが推進する [!DNL Experience Data Model (XDM)] は、カスタマーエクスペリエンスデータを標準化し、カスタマーエクスペリエンス（顧客体験）管理のスキーマを定義する取り組みです。

## Experience Platformの統合

SDKを使用してAdobe Experience Platformに同意データを送信するには、次が必要です。

- スキーマが [!DNL XDM Individual Profile] クラスに基づいており、[!DNL Real-Time Customer Profile] で使用できるよう有効化された TCF 2.0 同意フィールドが含まれるデータセット。
- Experience Platformと前述のプロファイル対応データセットで設定されたデータストリーム。

必要なデータセットとデータストリームの作成方法については、[TCF 2.0 への準拠 &#x200B;](../../../landing/governance-privacy-security/consent/iab/overview.md) に関するガイドを参照してください。

## Audience Managerの統合

Adobe Audience Manager（AAM）には、IAB TCF 2.0 のサポートが含まれています。これにより、お客様のプライバシーに関する選択肢を評価、順守、ダウンストリームのパートナーに転送できます。<!--For more information, read the documentation on [Sending Data to Audience Manager](../audience-manager/audience-manager-overview.md).-->

>[!TIP]
>
>Adobe Experience Platform Web SDKを通じてAudience Managerと統合するには、Adobe Audience Managerに転送するようにデータストリームが設定されていることを確認します。

## Experience Events とAdobe Analyticsの統合

Real-Time CDPとAudience Managerのオーディエンスは、顧客の現在の同意環境設定を追跡しますが、Experience Events は、イベントが収集された際にアクティブだった顧客の同意環境設定を保持できます。

イベントに関する同意情報を収集するには、次の操作が必要です。

- [!DNL Experience Event] プライバシースキーマフィールドグループを持つ、[!DNL XDM Experience Event] クラスに基づくデータセット。
- 上記の [!DNL XDM Experience Event] データセットで設定されたデータストリーム。

XDM エクスペリエンスイベントを Analytics ヒットに変換する方法について詳しくは、[Web SDKを使用したAdobe Analyticsへのデータ送信 &#x200B;](/help/web-sdk/use-cases/adobe-analytics.md) を参照してください。

## Adobe Experience Platform Web SDKの統合

以下の節では、IAB TCF 2.0 とAdobe Experience Platform web SDKの間の主な統合ポイントについて説明します。

>[!NOTE]
>
>Real-Time CDPやAudience Managerをセットアップしていなくても、IAB TCF 2.0 を web SDKと統合できます。 同意環境設定を使用すると、エクスペリエンスイベントの収集と ID cookie の設定を制御できます。

### デフォルトの同意

顧客に同意設定が既に保存されていない場合は、デフォルトの同意が使用されます。 つまり、デフォルトの同意オプションが、Adobe Experience Platform Web SDKの動作を制御し、顧客の地域に基づいて変化する可能性があります。

例えば、お客様が EU 一般データ保護規則（GDPR）の管轄権外にある場合、デフォルトの同意は `in` に設定されますが、GDPR の管轄権内にある場合はデフォルトの同意を `pending` に設定できます。 お客様の同意管理プラットフォーム（CMP）が顧客の地域を検出し、IAB TCF 2.0 に `gdprApplies` するフラグを提供している場合があります。このフラグは、デフォルトの同意を設定するために使用できます。 詳細は、[`defaultConsent`](/help/web-sdk/commands/configure/defaultconsent.md) を参照してください。

### 変更時の同意の設定

Adobe Experience Platform Web SDKには `setConsent` コマンドがあり、IAB TCF 2.0 を使用するすべてのAdobe サービスに顧客の同意環境設定を伝えます。Real-Time CDPと統合している場合は、顧客プロファイルが更新されます。 をAudience Managerと統合すると、顧客情報が更新されます。 これを呼び出すと、今後のエクスペリエンスイベントの送信を許可するかどうかを制御する、all-or-nothing の同意環境設定を持つ Cookie も設定されます。 このアクションは、同意が変更されるたびに呼び出されることを目的としています。 今後のページ読み込み時に、Edge Network同意 Cookie が読み取られ、エクスペリエンスイベントを送信できるかどうか、および ID Cookie を設定できるかどうかを判断します。

Audience Managerの IAB TCF 2.0 統合と同様に、Edge Networkは、お客様が次の目的に明示的に同意している場合に同意を与えます。

- **目的 1:** デバイス上の情報の保存やアクセス
- **目的 10:** 製品の開発・改良
- **特別な目的 1:** セキュリティの確保、詐欺の防止、デバッグ。 （IAB TCF 規制に従い、これは常に同意されます）
- **Adobe仕入先のアクセス許可：** Adobeの同意（仕入先 565）

`setConsent` コマンドについて詳しくは、[setConsent](../../../web-sdk/commands/setconsent.md) に関する専用の Web SDK ドキュメントを参照してください。

### エクスペリエンスイベントへの同意の追加

Adobe Experience Platform Web SDKには、エクスペリエンスイベントを収集する [`sendEvent`](/help/web-sdk/commands/sendevent/overview.md) コマンドがあります。 エクスペリエンスイベントまたはAdobe Analyticsと統合し、すべてのエクスペリエンスイベントで同意環境設定が必要な場合は、すべての `sendEvent` コマンドに同意情報を追加します。

## 次の手順

これで、IAB Transparency &amp; Consent Framework 2.0 の基本的な理解が整いました。IAB TCF 2.0 の使用に関するガイド [&#x200B; タグ付き &#x200B;](./with-tags.md) または [&#x200B; タグなし &#x200B;](./without-tags.md) のいずれかを参照してください。
