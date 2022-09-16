---
title: Adobe Experience Platform Web SDK での IAB TCF 2.0 のサポート
description: Adobe Experience Platform Web SDK を使用して、IAB TCF 2.0 の同意設定をサポートする方法について説明します。
keywords: 同意；setConsent；プロファイルプライバシーフィールドグループ；エクスペリエンスイベントプライバシーフィールドグループ；プライバシーフィールドグループ；IAB TCF 2.0；リアルタイム CDP；リアルタイム顧客データプロファイル
exl-id: 78e728f4-1604-40bf-9e21-a056024bbc98
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '864'
ht-degree: 0%

---

# Adobe Experience Platform Web SDK での IAB TCF 2.0 のサポート

Adobe Experience Platform Web SDK は、Interactive Advertising Bureau の Transparency &amp; Consent Framework、バージョン 2.0(IAB TCF 2.0) をサポートしています。 このガイドでは、Real-time Customer Data Platform、Audience Manager、Experience Events、Adobe Analytics、Experience Edge との統合により、Adobe Experience Platform Web SDK で IAB TCF 2.0 をサポートするための要件を示します。

また、タグの有無に関わらず IAB TCF 2.0 を統合する方法を学ぶのに役立つガイドが以下に用意されています。

- [タグ付き](./with-launch.md)
- [タグなし](./without-launch.md)

## はじめに

IAB TCF 2.0 を使用した Web SDK を実装するには、エクスペリエンスデータモデル (XDM) とエクスペリエンスイベントに関して、十分に理解している必要があります。 開始する前に、次のドキュメントを確認してください。

- [エクスペリエンスデータモデル (XDM) システムの概要](../../../xdm/home.md):標準化と相互運用性は、Adobe Experience Platformの背後にある重要な概念です。 [!DNL Experience Data Model (XDM)]は、Adobeに基づいて推進され、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

## Experience Platform統合

SDK を使用してAdobe Experience Platformに同意データを送信するには、以下が必要です。

- スキーマがに基づくデータセット [!DNL XDM Individual Profile] クラスおよびには TCF 2.0 の同意フィールドが含まれ、での使用が有効になっています。 [!DNL Real-time Customer Profile].
- 上記の Platform とプロファイル対応データセットを使用して設定されたデータストリーム。

詳しくは、 [TCF 2.0 への準拠](../../../landing/governance-privacy-security/consent/iab/overview.md) 必要なデータセットとデータストリームの作成手順を参照してください。

## Audience Manager統合

Adobe Audience Manager(AAM) には、顧客のプライバシー選択を評価、尊重し、ダウンストリームパートナーに転送できる IAB TCF 2.0 のサポートが含まれています。 <!--For more information, read the documentation on [Sending Data to Audience Manager](../audience-manager/audience-manager-overview.md).-->

>[!TIP]
>
>Adobe Experience Platform Web SDK を使用してAudience Managerと統合するには、Adobe Audience Managerに転送するためのデータストリームが設定されていることを確認してください。

## Experience Events とAdobe Analyticsの統合

リアルタイム CDP とAudience Managerのオーディエンスは顧客の現在の同意設定を追跡するのに対して、Experience Events は、イベントが収集された際にアクティブだった顧客の同意設定を保持できます。

イベントに関する同意情報を収集するには、以下が必要です。

- に基づくデータセット [!DNL XDM Experience Event] クラス、 [!DNL Experience Event] プライバシースキーマフィールドグループ。
- データストリームは [!DNL XDM Experience Event] データセットを上に置きます。

XDM エクスペリエンスイベントを Analytics ヒットに変換する方法について詳しくは、まず [Analytics の概要](../../data-collection/adobe-analytics/analytics-overview.md) ドキュメント。

## Adobe Experience Platform Web SDK との統合

以下の節では、IAB TCF 2.0 とAdobe Experience Platform Web SDK の間の主な統合ポイントについて説明します。

>[!NOTE]
>
>リアルタイム CDP またはAudience Managerが設定されていなくても、IAB TCF 2.0 を Web SDK と統合できます。 同意の環境設定を使用して、Experience Events の収集を制御し、ID Cookie を設定できます。

### デフォルトの同意

お客様に対して同意設定が既に保存されていない場合は、デフォルトの同意が使用されます。 つまり、デフォルトの同意オプションで、Adobe Experience Platform Web SDK の動作を制御し、お客様の地域に基づいて変更できます。

例えば、EU 一般データ保護規則 (GDPR) の管轄区域外のお客様がいる場合、デフォルトの同意は次のように設定できます。 `in`ただし、GDPR の管轄区域内では、デフォルトの同意が `pending`. お客様の同意管理プラットフォーム (CMP) が顧客の地域を検出し、フラグを提供する場合があります `gdprApplies` を IAB TCF 2.0 に追加しました。このフラグを使用して、デフォルトの同意を設定できます。

デフォルトの同意について詳しくは、 [デフォルトの同意セクション](../../fundamentals/configuring-the-sdk.md#default-consent) （ SDK 設定ドキュメント内）。

### 変更時の同意の設定

Adobe Experience Platform Web SDK には、 `setConsent` コマンドを使用して、IAB TCF 2.0 を使用するすべてのAdobe サービスに顧客の同意設定を伝えます。Real-time CDP と統合している場合は、顧客のプロファイルが更新されます。 Audience Managerと統合すると、顧客の情報が更新されます。 また、このを呼び出すと、今後の Experience イベントの送信を許可するかどうかを制御する、すべてまたは何も指定しない同意設定が Cookie に設定されます。 このアクションは、同意が変更されるたびに呼び出されることを意図しています。 今後のページの読み込みでは、Experience Edge の同意 Cookie が読み取られて、Experience Events を送信できるかどうか、および ID Cookie を設定できるかどうかが決定されます。

Audience Managerの IAB TCF 2.0 統合と同様、Experience Edge は、顧客が次の目的に対して明示的に同意した場合に同意を提供します。

- **目的 1:** デバイス上での情報の保存/アクセス
- **目的 10:** 製品の開発と改善
- **特別な目的 1:** セキュリティの確保、不正の防止、デバッグ。 （IAB TCF 規制に従い、これは常に同意されます）
- **Adobeベンダー権限：** Adobeの同意（ベンダー 565）

詳しくは、 `setConsent` コマンドを使用する場合は、次のドキュメントを読んでください： [同意のサポート](../../consent/supporting-consent.md).

### エクスペリエンスイベントへの同意の追加

Adobe Experience Platform Web SDK には、 `sendEvent` エクスペリエンスイベントを収集するコマンドを使用します。 Experience Events またはAdobe Analyticsとの統合時に、すべてのエクスペリエンスイベントに対する同意設定を希望する場合は、 `sendEvent` コマンドを使用します。

詳しくは、 `sendEvent` コマンドを使用する場合は、次のドキュメントを読んでください： [イベントの追跡](../../fundamentals/tracking-events.md).

## 次の手順

これで、IAB Transparency &amp; Consent Framework 2.0 に関する基本的な知識が整いました。IAB TCF 2.0 の使用に関するガイドのいずれかを参照してください。 [タグ付き](./with-launch.md) または [タグなし](./without-launch.md).
