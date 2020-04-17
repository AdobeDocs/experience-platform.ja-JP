---
title: リアルタイム顧客データプロファイルでのプライバシー
seo-title: リアルタイム顧客データプロファイルでのプライバシー
description: リアルタイム顧客データプロファイルを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
seo-description: リアルタイム顧客データプロファイルを使用すると、プライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。
translation-type: tm+mt
source-git-commit: a1161630c8edae107b784f32ee20af225f9f8c46

---


# Real-time CDP のプライバシー

リアルタイム顧客データプラットフォーム（Real-time CDP）は、マーケティング担当者が複数の大規模法人システムからデータを統合し、顧客の特定、理解、関与を促進するのに役立ちます。アドビでは、基本的なデザイン原則として消費者データのプライバシーを保持し、マーケティング担当者が顧客のデータのプライバシーを管理するのに役立つ様々なコントロールを提供しています。

ほとんどの Real-time CDP 機能は、Adobe Experience Platform によって動作します。このドキュメントでは、Real-time CDP がサポートする各種プライバシー強化テクノロジーに関する情報と、詳細を含む Experience Platform のドキュメントへのリンクを提供します。

## プライバシーサービス

Adobe Experience Platform プライバシーサービスを使用すると、GDPR（EU 一般データ保護規則）やカリフォルニア州消費者プライバシー法（CCPA）などのプライバシー規制に準拠したデータ操作を維持するプロセスを合理化できます。Real-time CDP は、データ収集とストレージに Experience Platform の機能を活用するので、GDPR と CCPA に対するアクセス要求と削除要求は、プラットフォーム内で管理する必要があります。このサービスの概要について詳しくは、『[プライバシーサービスの概要](../../privacy-service/home.md)』ドキュメントを参照してください。

個々の GDPR および CCPA データ主体アクセス要求を送信して顧客データにアクセスし、削除する方法は 2 つあります。

* [プライバシーサービスの UI](https://gdprui.cloud.adobe.io/) を使用して、ビジュアルワークスペース内でアクセス要求と削除要求を作成および監視します。手順については、『[プライバシーサービスの UI チュートリアル](../../privacy-service/ui/overview.md)』を参照してください。
* [プライバシーサービス API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/privacy-service.yaml) を使用して、RESTful API 呼び出しを介してアクセス要求と削除要求を管理します。手順については、『[プライバシーサービス API のチュートリアル](../../privacy-service/api/getting-started.md)』を参照してください。

<!-- (Capability will not be available for November GA) 
## Opt-out capabilities

Real-time CDP provides two types of consumer opt-out capabilities:

1. **General opt-out**: (Waiting on info)
1. **Segment-level opt-out of sale**: Opt-out of sale requests are captured using the Profile Privacy mixin (see the section on "Handling opt-out requests" in the [Real-time Customer Profile overview](../../profile/home.md) for more information). Using this, you can exclude users who have opted out from a segment using boolean logic ("AND NOT") in the segment predicate.
-->

## 次の手順

このドキュメントでは、Real-time CDP のプライバシー機能について簡単に説明します。アクセス要求や削除要求を送信する際のベストプラクティスと手順について詳しくは、[プライバシーサービスのドキュメント](../../privacy-service/home.md)を参照してください。