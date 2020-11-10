---
keywords: data governance rtcdp;rtcdp data governance;real time customer data profile data governance;privacy rtcdp;rtcdp privacy
title: リアルタイム顧客データプロファイルでのプライバシー
seo-title: リアルタイム顧客データプロファイルでのプライバシー
description: リアルタイム顧客データプロファイルを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
seo-description: リアルタイム顧客データプロファイルを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 21%

---


# プライバシー [!DNL Real-time CDP]

[!DNL Real-time Customer Data Platform] ([!DNL Real-time CDP])マーケティング担当者が複数のエンタープライズシステムからのデータを統合し、顧客の識別、把握、関与をより的確に行えるようにします。 アドビでは、基本的なデザイン原則として消費者データのプライバシーを保持し、マーケティング担当者が顧客のデータのプライバシーを管理するのに役立つ様々なコントロールを提供しています。

The majority of [!DNL Real-time CDP] capabilities are powered by Adobe Experience Platform. This document provides information about the various privacy enhancement technologies supported by [!DNL Real-time CDP], with links to [!DNL Experience Platform] documentation for more information.

## 顧客のアクセスおよび削除要求の実行

(GDPR)や [!DNL General Data Protection Regulation][!DNL California Consumer Privacy Act] (CCPA)などの法的プライバシー規制により、お客様は、お客様から収集した個人データへのアクセスを要求したり、そのデータを削除したりする権利を得ることができます。 データの収集とストレージに [!DNL Real-time CDP] 機能を活用するため、個人データのアクセスと削除を求める顧客の要求は、内で管理する必要があり [!DNL Experience Platform][!DNL Platform]ます。 See the overview on [Adobe Experience Platform Privacy Service](../../privacy-service/home.md) for more information.

## オプトアウト機能

[!DNL Real-time CDP] セグメント化の使用例に個人データオプトアウトを含めることができます。 顧客のオプトアウトプリファレンスは、によってキャプチャおよび保存され [!DNL Real-time Customer Profile]、セグメントの述語にBooleanロジック(「AND NOT」)を使用してセグメントからオプトアウトしたユーザーを除外すると、適用できます。

詳しくは、Adobe Experience Platform Segmentation Serviceドキュメントで、オプトアウトリクエストの [実行に関するドキュメントを参照してください](../../segmentation/honoring-opt-outs.md) 。

## （IAB TCF 2.0のサポート）

[!DNL Real-time CDP] は、 [(IAB)で概要を説明しているように、の登録](https://iabeurope.eu/vendor-list-tcf-v2-0/) ベンダーリスト [!DNL Transparency & Consent Framework (TCF)][!DNL Interactive Advertising Bureau] に含まれています。 TCF 2.0の要件に準拠して、詳細な顧客の同意データを収集 [!DNL Real-time CDP] し、それを保存されている顧客プロファイルに統合できます。 その後、この同意データは、使用事例に応じて、特定のプロファイルが書き出されたオーディエンスセグメントに含まれているかどうかに織り込まれます。

詳細については、 [IAB TCF 2.0サポートの概要 [!DNL Real-time CDP]](./iab/overview.md) ()を参照してください。

## 次の手順

This document provided a brief introduction to the privacy capabilities of [!DNL Real-time CDP]. 各機能の詳細については、本ガイド全体にリンクされたドキュメントを参照してください。