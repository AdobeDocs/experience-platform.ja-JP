---
keywords: データ・ガバナンスrtcdp;rtcdpデータ・ガバナンス；リアルタイム・カスタマー・データ・プロファイル・データ・ガバナンス；プライバシーrtcdp;rtcdpプライバシー
title: リアルタイム顧客データプラットフォームでのプライバシー
seo-title: リアルタイム顧客データプラットフォームでのプライバシー
description: リアルタイム顧客データプラットフォームを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
seo-description: リアルタイム顧客データプラットフォームを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
translation-type: tm+mt
source-git-commit: 8d403e73a804953f9584d6a72f945d4444e65d11
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 6%

---


# リアルタイム顧客データプラットフォームでのプライバシー

[!DNL Real-time Customer Data Platform] ([!DNL Real-time CDP])マーケティング担当者が複数のエンタープライズシステムからのデータを統合し、顧客の識別、把握、関与をより的確に行えるようにします。アドビでは、基本的なデザイン原則として消費者データのプライバシーを保持し、マーケティング担当者が顧客のデータのプライバシーを管理するのに役立つ様々なコントロールを提供しています。

[!DNL Real-time CDP]の機能の大半はAdobe Experience Platformが動力を持っています。 このドキュメントでは、[!DNL Real-time CDP]がサポートする各種のプライバシー拡張テクノロジーに関する情報と、[!DNL Experience Platform]ドキュメントへのリンクを提供しています。

## 顧客のアクセスおよび削除要求の実行

[!DNL General Data Protection Regulation](GDPR)や[!DNL California Consumer Privacy Act](CCPA)などの法的プライバシー規制は、お客様に、お客様が収集した個人データへのアクセスを要求したり、それらから削除する権利を与える。 [!DNL Real-time CDP]はデータの収集とストレージに[!DNL Experience Platform]機能を活用するので、個人データのアクセスと削除のリクエストは[!DNL Platform]内で管理する必要があります。 詳しくは、[Adobe Experience Platform Privacy Service](../../privacy-service/home.md)の概要を参照してください。

## オプトアウト機能

[!DNL Real-time CDP] セグメント化の使用例に個人データオプトアウトを含めることができます。顧客のオプトアウト設定は[!DNL Real-time Customer Profile]によってキャプチャおよび保存され、セグメントの述語にBooleanロジック(「AND NOT」)を使用してセグメントからオプトアウトしたユーザーを除外すると、適用できます。

詳しくは、Adobe Experience Platform Segmentation Serviceドキュメントの[オプトアウトリクエスト](../../segmentation/honoring-opt-outs.md)に関するドキュメントを参照してください。

## （IAB TCF 2.0のサポート）

[!DNL Real-time CDP] は、に示すように、に登録された [ベンダー](https://iabeurope.eu/vendor-list-tcf-v2-0/) リストの一部であるAdobe Experience Platform [!DNL Transparency & Consent Framework (TCF)]に基づいて構築され [!DNL Interactive Advertising Bureau (IAB)]ます。TCF 2.0の要件に準拠したプラットフォームを使用すると、詳細な顧客の同意データを収集し、保存されている顧客プロファイルに統合できます。 その後、この同意データは、使用事例に応じて、特定のプロファイルが書き出されたオーディエンスセグメントに含まれているかどうかに織り込まれます。

詳しくは、Experience Platform](../../landing/governance-privacy-security/consent/iab/overview.md)の[IAB TCF 2.0サポートの概要を参照してください。

## 次の手順

このドキュメントは[!DNL Real-time CDP]のプライバシー機能の簡単な紹介を提供しました。 各機能の詳細については、本ガイド全体にリンクされたドキュメントを参照してください。