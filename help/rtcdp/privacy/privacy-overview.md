---
keywords: データガバナンス rtcdp、rtcdp データガバナンス、リアルタイム顧客データプロファイルデータガバナンス、プライバシー rtcdp、rtcdp プライバシー
title: Real-time Customer Data Platformのプライバシー
description: Adobe Real-time Customer Data Platformを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
exl-id: bcb0e42e-4549-4952-bb69-5534aee353f8
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 6%

---

# Real-time Customer Data Platformのプライバシー

[!DNL Adobe Real-Time Customer Data Platform] ([!DNL Real-Time CDP]) は、マーケターが複数の大規模法人システムからデータを統合し、顧客の識別、理解、関与を促進するのに役立ちます。 アドビでは、基本的なデザイン原則として消費者データのプライバシーを保持し、マーケティング担当者が顧客のデータのプライバシーを管理するのに役立つ様々なコントロールを提供しています。

～の大部分 [!DNL Real-Time CDP] 機能はAdobe Experience Platformが提供します。 このドキュメントでは、 [!DNL Real-Time CDP]、リンクあり [!DNL Experience Platform] ドキュメントを参照してください。

## 顧客のアクセス要求および削除要求の遵守

法的プライバシー規制 ( [!DNL General Data Protection Regulation] (GDPR) および [!DNL California Consumer Privacy Act] (CCPA) は、お客様が自分から収集した個人データへのアクセス、またはその削除を要求する権利を付与します。 次以降 [!DNL Real-Time CDP] 活用 [!DNL Experience Platform] データ収集および保存機能、お客様の個人データへのアクセスおよび削除要求は、以下の場所で管理する必要があります。 [!DNL Platform]. 概要については、 [Adobe Experience Platform Privacy Service](../../privacy-service/home.md) を参照してください。

>[!IMPORTANT]
>
> Adobe Experience Platform Privacy Service for Adobe Marketo Engageを通じて送信されたプライバシーリクエストは、Real-Time CDP B2B のお客様にのみ適用されます。

## オプトアウト機能

[!DNL Real-Time CDP] を使用すると、顧客は自分の個人データをセグメントの使用例に含めないようにオプトアウトできます。 顧客のオプトアウト設定は、 [!DNL Real-time Customer Profile]を適用するには、セグメントの述語でブール論理 (「AND NOT」) を使用してセグメントからオプトアウトしたユーザーを除外します。

次のドキュメントを参照してください： [オプトアウト要求の遵守](../../segmentation/consents.md) ( Adobe Experience Platformセグメント化サービスのドキュメント ) を参照してください。

## IAB TCF 2.0 のサポート

[!DNL Real-Time CDP] は、Adobe Experience Platformに基づいて構築され、これは登録済みの [ベンダーリスト](https://iabeurope.eu/vendor-list-tcf-v2-0/) の [!DNL Transparency & Consent Framework (TCF)]( [!DNL Interactive Advertising Bureau (IAB)]. TCF 2.0 の要件に準拠して、Platform を使用すると、詳細な顧客同意データを収集し、保存された顧客プロファイルに統合できます。 その後、使用事例に応じて、特定のプロファイルが書き出されたオーディエンスセグメントに含まれるかどうかに、この同意データを考慮に入れることができます。

概要については、 [Experience Platformでの IAB TCF 2.0 のサポート](../../landing/governance-privacy-security/consent/iab/overview.md) を参照してください。

## 次の手順

このドキュメントでは、のプライバシー機能に関する簡単な概要を説明しました [!DNL Real-Time CDP]. 各機能の詳細については、このガイド全体にリンクされているドキュメントを参照してください。
