---
keywords: データガバナンスrtcdp;rtcdpデータガバナンス；リアルタイム顧客データプロファイルデータガバナンス；プライバシーrtcdp;rtcdpプライバシー
title: リアルタイム顧客データプラットフォームでのプライバシー
seo-title: リアルタイム顧客データプラットフォームでのプライバシー
description: リアルタイムの顧客データプラットフォームを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
seo-description: リアルタイムの顧客データプラットフォームを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
exl-id: bcb0e42e-4549-4952-bb69-5534aee353f8
source-git-commit: f193787ac27e30c69d25418656ae9c59c89622dc
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 6%

---

# リアルタイム顧客データプラットフォームでのプライバシー

[!DNL Real-time Customer Data Platform] ([!DNL Real-time CDP])は、マーケターが複数のエンタープライズシステムからデータを統合し、顧客をより深く識別、理解し、惹きつけるのに役立ちます。アドビでは、基本的なデザイン原則として消費者データのプライバシーを保持し、マーケティング担当者が顧客のデータのプライバシーを管理するのに役立つ様々なコントロールを提供しています。

[!DNL Real-time CDP]機能の大部分はAdobe Experience Platformによって動作します。 このドキュメントでは、[!DNL Real-time CDP]がサポートする様々なプライバシー強化テクノロジーに関する情報と、詳細については[!DNL Experience Platform]のドキュメントへのリンクを提供します。

## 顧客のアクセス要求および削除要求の処理

[!DNL General Data Protection Regulation](GDPR)や[!DNL California Consumer Privacy Act](CCPA)などの法的プライバシー規制は、お客様に対し、収集した個人データへのアクセスを要求したり、そのデータを削除したりする権利を与えます。 [!DNL Real-time CDP]はデータの収集と保存に[!DNL Experience Platform]機能を活用するので、個人データに対する顧客のアクセス要求と削除要求は[!DNL Platform]内で管理する必要があります。 詳しくは、[Adobe Experience Platform Privacy Service](../../privacy-service/home.md)の概要を参照してください。

## オプトアウト機能

[!DNL Real-time CDP] では、個人データをセグメント化の使用例に含めないようオプトアウトできます。お客様のオプトアウト設定は[!DNL Real-time Customer Profile]によってキャプチャおよび保存され、セグメントの述語でBooleanロジック(「AND NOT」)を使用してセグメントからオプトアウトしたユーザーを除外することで、適用できます。

詳しくは、 Adobe Experience Platformセグメント化サービスのドキュメントの[オプトアウトリクエストの処理](../../segmentation/consents.md)に関するドキュメントを参照してください。

## IAB TCF 2.0のサポート

[!DNL Real-time CDP] は、Adobe Experience Platform(で説明されているように、の登録ベ [ンダ](https://iabeurope.eu/vendor-list-tcf-v2-0/) ーリストの [!DNL Transparency & Consent Framework (TCF)]一部)を基に構築されていま [!DNL Interactive Advertising Bureau (IAB)]す。TCF 2.0の要件に準拠したPlatformを使用すると、詳細な顧客同意データを収集し、保存した顧客プロファイルに統合できます。 その後、この同意データは、エクスポートされたオーディエンスセグメントに特定のプロファイルが含まれるかどうかに関係なく、使用例に応じて考慮できます。

詳しくは、Experience Platform](../../landing/governance-privacy-security/consent/iab/overview.md)の[IAB TCF 2.0のサポートの概要を参照してください。

## 次の手順

このドキュメントでは、[!DNL Real-time CDP]のプライバシー機能について簡単に説明しました。 各機能の詳細については、このガイド全体でリンクされているドキュメントを参照してください。
