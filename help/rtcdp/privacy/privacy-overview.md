---
title: リアルタイム顧客データプロファイルでのプライバシー
seo-title: リアルタイム顧客データプロファイルでのプライバシー
description: リアルタイム顧客データプロファイルを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
seo-description: リアルタイム顧客データプロファイルを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
translation-type: tm+mt
source-git-commit: 5d3bedd97208d9eed3977a35e16a10f4864aedd9

---


# リアルタイムCDPのプライバシー

リアルタイム顧客データプラットフォーム(Real-time CDP)は、マーケティング担当者が複数のエンタープライズシステムからデータを統合し、顧客の特定、理解、関与を促進するのに役立ちます。 アドビは、基本的なデザイン原則として消費者データのプライバシーを保持し、マーケティング担当者が顧客のデータのプライバシーを管理するのに役立つ様々なコントロールを提供しています。

ほとんどのリアルタイムCDP機能は、Adobe Experience Platformによって動作します。 このドキュメントでは、Real-time CDPがサポートする各種プライバシー拡張テクノロジーに関する情報と、詳細についてExperience Platformのドキュメントへのリンクを提供します。

## プライバシーサービス

Adobe Experience Platform Privacy Serviceを使用すると、GDPR(General Data Protection Regulation)やCalifornia Consumer Privacy Act(CCPA)などのプライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。 Real-time CDPは、データ収集とストレージにExperience Platformの機能を活用するので、GDPRとCCPAに対するアクセスと削除のリクエストは、プラットフォーム内で管理する必要があります。 このサービスの [概要について詳しくは](https://www.adobe.io/apis/experiencecloud/gdpr/docs/alldocs.html#!api-specification/markdown/narrative/technical_overview/privacy_service_overview/privacy_service_overview.md) 、「プライバシーサービスの概要」のドキュメントを参照してください。

個々のGDPRおよびCCPAデータのサブジェクトリクエストを送信して顧客データにアクセスし、削除する方法は2つあります。

* プライバシーサ [ービスのUIを使用して](https://gdprui.cloud.adobe.io/) 、ビジュアルワークスペース内でアクセス要求と削除要求を作成および監視します。 手順につ [いては、プライバシーサービスのUI](https://www.adobe.io/apis/experiencecloud/gdpr/docs/alldocs.html#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md) （チュートリアル）を参照してください。
* RESTful API呼び出しを使用 [して](https://www.adobe.io/apis/experiencecloud/gdpr/api-reference.html) 、アクセスおよび削除のリクエストを管理するには、プライバシーサービスAPIを使用します。 手順につ [いては、プライバシーサービスAPI](https://www.adobe.io/apis/experiencecloud/gdpr/docs/alldocs.html#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_api_tutorial.md) （英語のみ）のチュートリアルを参照してください。

<!-- (Capability will not be available for November GA) 
## Opt-out capabilities

Real-time CDP provides two types of consumer opt-out capabilities:

1. **General opt-out**: (Waiting on info)
1. **Segment-level opt-out of sale**: Opt-out of sale requests are captured using the Profile Privacy mixin (see the section on "Handling opt-out requests" in the [Real-time Customer Profile overview](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md) for more information). Using this, you can exclude users who have opted out from a segment using boolean logic ("AND NOT") in the segment predicate.
-->

## 次の手順

このドキュメントでは、リアルタイムCDPのプライバシー機能について簡単に説明します。 アクセス/削除のリクエストを送信する際のベストプラクティスと手順について詳しくは、プライバシーサービスのドキュメントを参照 [してください](https://www.adobe.io/apis/experiencecloud/gdpr/docs.html)。