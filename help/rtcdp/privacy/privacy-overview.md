---
keywords: データガバナンス rtcdp、rtcdp データガバナンス、カスタマーデータプロファイルデータガバナンス、プライバシー、プライバシー rtcdp、rtcdp privacy、rtcdp プライバシー
title: リアルタイムの顧客データプラットフォームにおけるプライバシー
seo-title: Privacy in Real-time Customer Data Platform
description: リアルタイムカスタマーデータプラットフォームを使用すると、データ操作がプライバシー規制に準拠している状態を維持するためのプロセスを効率化できます。
seo-description: Real-time Customer Data Platform allows you to streamline the process of keeping your data operations compliant with privacy regulations.
exl-id: bcb0e42e-4549-4952-bb69-5534aee353f8
source-git-commit: e3519817559dca372e228ed19bba4f9adf279768
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 6%

---

# リアルタイムの顧客データプラットフォームにおけるプライバシー

[!DNL Real-time Customer Data Platform] ( [!DNL Real-time CDP] ) マーケティング担当者は、複数のエンタープライズシステムからデータを取得することによって、それらのデータをより的確に特定し、理解し、その業務に役立てることができます。 アドビでは、基本的なデザイン原則として消費者データのプライバシーを保持し、マーケティング担当者が顧客のデータのプライバシーを管理するのに役立つ様々なコントロールを提供しています。

機能の大部分 [!DNL Real-time CDP] は、Adobe エクスペリエンスプラットフォームによって実現されています。 このドキュメントでは、でサポートされるプライバシー強化テクノロジに関する情報を提供し [!DNL Real-time CDP] ます。詳細については、ドキュメントへのリンクを参照 [!DNL Experience Platform] してください。

## お客様のアクセスおよび削除要求について

(GDPR) や (ccpa) のような法律上の法律上の規制によって、ユーザーに対して、自分が [!DNL General Data Protection Regulation] [!DNL California Consumer Privacy Act] 収集した個人データへのアクセスを許可したり、削除したりすることができます。 で [!DNL Real-time CDP] は [!DNL Experience Platform] 、データの収集と格納に関する機能を活用しているので、お客様から個人用データにアクセスして削除する要求はによって管理さ [!DNL Platform] れます。 詳しくは、『 [ Adobe エクスペリエンス Platform プライバシーサービス』の概要を参照してください ](../../privacy-service/home.md) 。

>[!IMPORTANT]
>
> アドビシステムズ社の adobe Marketo 用プラットフォームプライバシーサービスを通じて送信されたプライバシー要求は、リアルタイム CDP B2B ユーザーのみに適用されます。

## オプトアウト機能

[!DNL Real-time CDP] では、お客様が個人データを使用して、用途に区分を追加することはできません。 顧客のオプトアウトの設定は、によってキャプチャされて保存され [!DNL Real-time Customer Profile] ます。また、segment 述語にブールロジック (&quot;AND NOT&quot;) を使用して、セグメントからオプトインしたユーザーを除外することもできます。

詳細については、オプトアウト要求について詳しくは、『 [ ](../../segmentation/consents.md) Adobe 経験 Platform セグメンテーションサービス』を参照してください。

## IAB TCF 2.0 サポート

[!DNL Real-time CDP] は、で説明しているように、の登録されているベンダーリストの一部である Adobe エクスペリエンス Platform 上に構築され [ ](https://iabeurope.eu/vendor-list-tcf-v2-0/) てい [!DNL Transparency & Consent Framework (TCF)] [!DNL Interactive Advertising Bureau (IAB)] ます。 TCF 2.0 要件に適合するように、プラットフォームによって、詳細な顧客同意データを収集し、格納されている顧客プロファイルに組み込むことができます。 このような承認データは、用途によっては、エクスポートされた対象ユーザーセグメントに特定のプロファイルを含めるかどうかを決定することができます。

詳しくは、詳細については、 [ iab TCF 2.0 サポートの概要を参照してください ](../../landing/governance-privacy-security/consent/iab/overview.md) 。

## 次の手順

このドキュメントでは、のプライバシー機能について簡単に説明しました [!DNL Real-time CDP] 。 各機能について詳しくは、リンク先のドキュメントを参照してください。
