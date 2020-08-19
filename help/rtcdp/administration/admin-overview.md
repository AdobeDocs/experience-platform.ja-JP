---
keywords: rtcdp administration overview;administration overview
title: 管理の概要
seo-title: リアルタイム CDP 管理の概要
description: 'このドキュメントでは、Adobe Experience Platform によるリアルタイム顧客データプロファイルの管理機能の概要を説明します。 '
seo-description: SEO 説明
translation-type: tm+mt
source-git-commit: 15323134f0c626cad2c4e90b3e1c0662cf7e57dd
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 69%

---


# リアルタイム CDP 管理の概要

This document provides an overview of the administration capabilities of [!DNL Real-time Customer Data Platform], powered by Adobe Experience Platform.

[!DNL Experience Platform]管理者は、 を使用して、ユーザーのロールベースのアクセス制御を管理したり、アプリケーション開発用の仮想サンドボックスを管理したりできます。

The following sections provide introductions to the central components of [!DNL Experience Platform] administration capabilities, and includes links to [!DNL Experience Platform] documentation where more detailed information is provided.

## アクセス制御

アクセス制御は、[Adobe Admin Console](http://adminconsole.adobe.com) でおこないます。This functionality leverages product profiles in [!DNL Admin Console], allowing you to link users with permissions and sandboxes. この機能を使用すると、管理者は、定義されたユーザーのセットに対して、特定のリアルタイム CDP 機能に対するアクセスを許可または制限できます。

アクセス制御について詳しくは、 ドキュメントの「[アクセス制御の概要](../../access-control/home.md)」を参照してください。[!DNL Experience Platform]

>[!IMPORTANT]
>UI での表示を可能にするなど、リアルタイム CDP 機能へのアクセス権付与の詳細なガイドについては、[アクセス制御ユーザガイド](../../access-control/ui/overview.md)（特に製品プロファイルの詳細と追加サービスを管理する手順）に従ってください。

## サンドボックス

Adobe Experience Platform（および拡張機能によるリアルタイム CDP）は、デジタルエクスペリエンスアプリケーションをグローバルに拡張するために構築されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。

To address this need, Adobe Experience Platform provides &quot;sandboxes&quot;, enabling you to partition a single [!DNL Platform] instance into separate virtual environments that can be used to develop and evolve digital experience applications.

サンドボックスについて詳しくは、 ドキュメントの[サンドボックスの概要](../../sandboxes/home.md)を参照してください。[!DNL Experience Platform]