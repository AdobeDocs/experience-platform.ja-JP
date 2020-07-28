---
title: リアルタイム顧客データプロファイルでのプライバシー
seo-title: リアルタイム顧客データプロファイルでのプライバシー
description: リアルタイム顧客データプロファイルを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
seo-description: リアルタイム顧客データプロファイルを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
translation-type: tm+mt
source-git-commit: b96286f6a06f0583b45343a513ee64f0025d79a7
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 61%

---


# リアルタイム CDP のプライバシー

[!DNL Real-time Customer Data Platform] （リアルタイムCDP）は、マーケティング担当者が複数のエンタープライズ・システムからデータを統合し、顧客の識別、理解、関与性を高めるのに役立ちます。 アドビでは、基本的なデザイン原則として消費者データのプライバシーを保持し、マーケティング担当者が顧客のデータのプライバシーを管理するのに役立つ様々なコントロールを提供しています。

ほとんどのリアルタイム CDP 機能は、Adobe Experience Platform によって動作します。This document provides information about the various privacy enhancement technologies supported by Real-time CDP, with links to [!DNL Experience Platform] documentation for more information.

## [!DNL Privacy Service]

Adobe Experience Platform [!DNL Privacy Service] により、GDPR [!DNL General Data Protection Regulation] や [!DNL California Consumer Privacy Act] CCPAなどのプライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。 Since Real-time CDP leverages [!DNL Experience Platform] capabilities for data collection and storage, the access and delete requests for GDPR and CCPA should be managed within [!DNL Platform]. このサービスの概要について詳しくは、『[Privacy Service の概要](../../privacy-service/home.md)』ドキュメントを参照してください。

個々の GDPR および CCPA データ主体アクセス要求を送信して顧客データにアクセスし、削除する方法は 2 つあります。

* Use the [!DNL Privacy Service UI](https://gdprui.cloud.adobe.io/) to create and monitor access and delete requests within a visual workspace. 手順については、『[Privacy Service の UI チュートリアル](../../privacy-service/ui/overview.md)』を参照してください。
* Use the [!DNL Privacy Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/privacy-service.yaml) to manage access and delete requests with RESTful API calls. 手順については、『[Privacy Service API のチュートリアル](../../privacy-service/api/getting-started.md)』を参照してください。

<!-- (Capability will not be available for November GA) 
## Opt-out capabilities

Real-time CDP provides two types of consumer opt-out capabilities:

1. **General opt-out**: (Waiting on info)
1. **Segment-level opt-out of sale**: Opt-out of sale requests are captured using the Profile Privacy mixin (see the section on "Handling opt-out requests" in the [Real-time Customer Profile overview](../../profile/home.md) for more information). Using this, you can exclude users who have opted out from a segment using boolean logic ("AND NOT") in the segment predicate.
-->

## 次の手順

このドキュメントでは、リアルタイム CDP のプライバシー機能について簡単に説明します。アクセス要求や削除要求を送信する際のベストプラクティスと手順について詳しくは、[Privacy Service のドキュメント](../../privacy-service/home.md)を参照してください。