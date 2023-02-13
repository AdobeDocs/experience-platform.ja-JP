---
keywords: データガバナンス rtcdp;rtcdp データガバナンス;リアルタイム顧客データプロファイルデータガバナンス;プライバシー rtcdp;rtcdp プライバシー
title: Real-time Customer Data Platform のプライバシー
description: Adobe Real-time Customer Data Platform を使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
exl-id: bcb0e42e-4549-4952-bb69-5534aee353f8
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: ht
source-wordcount: '387'
ht-degree: 100%

---

# Real-time Customer Data Platform のプライバシー

[!DNL Adobe Real-Time Customer Data Platform]（[!DNL Real-Time CDP]）は、マーケターが複数の大規模法人システムからデータを統合し、顧客の特定、理解、関与を促進するのに役立ちます。アドビでは、基本的なデザイン原則として消費者データのプライバシーを保持し、マーケティング担当者が顧客のデータのプライバシーを管理するのに役立つ様々なコントロールを提供しています。

ほとんどの [!DNL Real-Time CDP] 機能は、Adobe Experience Platform によって動作します。このドキュメントでは、[!DNL Real-Time CDP] がサポートする各種プライバシー強化テクノロジーに関する情報と、詳細を含む [!DNL Experience Platform] のドキュメントへのリンクを提供します。

## 顧客のアクセスリクエストおよび削除リクエストの遵守

[!DNL General Data Protection Regulation]（GDPR）や [!DNL California Consumer Privacy Act]（CCPA）などの法的なプライバシー規制では、顧客から収集した個人データへのアクセスまたは削除をリクエストする権利が顧客に与えられています。[!DNL Real-Time CDP] はデータの収集と保存に [!DNL Experience Platform] の機能を活用しているので、顧客の個人データへのアクセスと削除のリクエストは [!DNL Platform] 内で管理する必要があります。詳しくは、[Adobe Experience Platform Privacy Service](../../privacy-service/home.md) に関する概要を参照してください。

>[!IMPORTANT]
>
> Adobe Marketo Engage 向け Adobe Experience Platform Privacy Service を通じて送信されるプライバシーリクエストは、Real-Time CDP B2B の顧客にのみ適用されます。

## オプトアウト機能

[!DNL Real-Time CDP] を使用すると、顧客は個人データをセグメント化のユースケースに含めることをオプトアウトできます。顧客のオプトアウト環境設定は [!DNL Real-Time Customer Profile] によってキャプチャおよび保存され、セグメント述語でブーリアン論理（「AND NOT」）を使用してセグメントからオプトアウトしたユーザーを除外することによって適用できます。

詳しくは、Adobe Experience Platform セグメント化サービスのドキュメントで、[オプトアウトリクエストの遵守](../../segmentation/consents.md)に関するドキュメントを参照してください。

## IAB TCF 2.0 のサポート

[!DNL Real-Time CDP] は、[!DNL Interactive Advertising Bureau (IAB)] で説明しているように、[!DNL Transparency & Consent Framework (TCF)] の登録済み[ベンダーリスト](https://iabeurope.eu/vendor-list-tcf-v2-0/)の一部である Adobe Experience Platform に基づいて作成されています。TCF 2.0 要件に準拠して、Platform を使用すると、詳細な顧客同意データを収集し、保存されている顧客プロファイルに統合できます。この同意データは、ユースケースに応じて、特定のプロファイルが書き出されたオーディエンスセグメントに含まれているかどうかの要因となります。

詳しくは、[Experience Platform での IAB TCF 2.0 のサポート](../../landing/governance-privacy-security/consent/iab/overview.md)に関する概要を参照してください。

## 次の手順

このドキュメントでは、[!DNL Real-Time CDP] のプライバシー機能について簡単に説明します。各機能について詳しくは、このガイド全体にリンクされているドキュメントを参照してください。
