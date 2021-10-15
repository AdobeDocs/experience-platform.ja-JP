---
keywords: データガバナンス rtcdp、rtcdp データガバナンス、リアルタイム顧客データプロファイルデータガバナンス、プライバシー rtcdp、rtcdp プライバシー
title: Real-time Customer Data Platformのプライバシー
seo-title: Privacy in Real-time Customer Data Platform
description: Real-time Customer Data Platformを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
seo-description: Real-time Customer Data Platform allows you to streamline the process of keeping your data operations compliant with privacy regulations.
exl-id: bcb0e42e-4549-4952-bb69-5534aee353f8
source-git-commit: f193787ac27e30c69d25418656ae9c59c89622dc
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 7%

---

# Real-time Customer Data Platformのプライバシー

[!DNL Real-time Customer Data Platform] ([!DNL Real-time CDP]) は、マーケターが複数のエンタープライズシステムからデータを統合し、顧客の識別、理解、関与を促進するのに役立ちます。アドビでは、基本的なデザイン原則として消費者データのプライバシーを保持し、マーケティング担当者が顧客のデータのプライバシーを管理するのに役立つ様々なコントロールを提供しています。

[!DNL Real-time CDP] 機能の大部分は、Adobe Experience Platformによって動作します。 このドキュメントでは、[!DNL Real-time CDP] がサポートする様々なプライバシー強化テクノロジーに関する情報と、詳細については [!DNL Experience Platform] ドキュメントへのリンクを提供します。

## 顧客のアクセス要求および削除要求の遵守

[!DNL General Data Protection Regulation](GDPR) や [!DNL California Consumer Privacy Act](CCPA) などの法的プライバシー規制により、お客様は、収集した個人データへのアクセスを要求したり、そのデータを削除したりする権利を与えられます。 [!DNL Real-time CDP] は [!DNL Experience Platform] 機能をデータ収集と保存に利用するので、個人データに対する顧客のアクセス要求と削除要求は [!DNL Platform] 内で管理する必要があります。 詳しくは、[Adobe Experience Platform Privacy Service](../../privacy-service/home.md) の概要を参照してください。

## オプトアウト機能

[!DNL Real-time CDP] では、セグメント化の使用例に個人データを含めないようにオプトアウトできます。お客様のオプトアウト設定は [!DNL Real-time Customer Profile] によって取り込まれ、保存されます。セグメントの述語で Boolean ロジック (「AND NOT」) を使用してセグメントからオプトアウトしたユーザーを除外することで、適用できます。

詳しくは、 Adobe Experience Platformセグメント化サービスのドキュメントの [ オプトアウトリクエストの処理 ](../../segmentation/consents.md) に関するドキュメントを参照してください。

## IAB TCF 2.0 のサポート

[!DNL Real-time CDP] は、Adobe Experience Platform( で説明されているように、の登録ベン [ダ](https://iabeurope.eu/vendor-list-tcf-v2-0/) ーリストの [!DNL Transparency & Consent Framework (TCF)]一部 ) を基に構築されていま [!DNL Interactive Advertising Bureau (IAB)]す。TCF 2.0 の要件に準拠した Platform を使用すると、詳細な顧客同意データを収集し、保存された顧客プロファイルに統合できます。 その後、この同意データは、エクスポートされたオーディエンスセグメントに特定のプロファイルが含まれるかどうかに関係なく、使用例に応じて考慮できます。

詳しくは、Experience Platform](../../landing/governance-privacy-security/consent/iab/overview.md) の [IAB TCF 2.0 のサポートの概要を参照してください。

## 次の手順

このドキュメントでは、[!DNL Real-time CDP] のプライバシー機能について簡単に説明しました。 各機能の詳細については、このガイド全体でリンクされているドキュメントを参照してください。
