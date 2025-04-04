---
title: Data Distiller認証 API ガイド
description: Data Distiller Authorization API を使用して、SQL を介した安全な接続のためにネットワークベースの IP 制限を適用する方法を説明します。 この API を使用して、Adobe Experience Platform データのデータアクセス制御を強化します。
role: Developer
exl-id: bcc5ea0e-cb6d-4c7b-bf9f-a0336f76c4c8
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 2%

---

# Data Distiller認証 API ガイド

>[!AVAILABILITY]
>
>この機能は、Data Distiller アドオンを購入したお客様が利用できます。 詳しくは、アドビ担当者にお問い合わせください。

Data Distiller Authorization API を使用して、IP ベースの制限を適用します。 これらの測定値を適用すると、承認されたネットワークとクライアントマシンのみがAdobe Experience Platformの SQL を介してデータにアクセスできるようになります。 これらの制御により、厳しいセキュリティ基準を満たしながら、リアルタイムのアクセス監視とアラートを提供できます。

この API を使用すると、SQL インターフェイスを介してデータにアクセスするための IP 制限を設定、適用、監視できます。 このドキュメントでは、API のコア機能、エンドポイント関数および今後の機能の概要を説明します。

## 主な特長

次の機能を使用すると、IP ベースのアクセス制限を定義し、アクセス試行を監視し、クエリサービスのネットワークセキュリティ設定をカスタマイズできます。

- **ネットワークベースのデータアクセス制御を定義**：クエリサービスアクセスの許可される IP 範囲を指定します。 この制限は、SQL データベース接続（Business Intelligence（BI）ツール、データベースクライアント、JDBC などのプログラミングインターフェイスを使用して確立された接続など）に特に当てはまります。
- **包括的な監視とアラートの有効化**：拒否された接続を含むすべてのアクセス試行は、ログに記録され、[Adobe Experience Platform監査ログ ](../../landing/governance-privacy-security/audit-logs/overview.md) に送信されて、リアルタイムトラッキングが行われます。 この機能を使用して、アクセスパターンを監視し、潜在的なセキュリティ侵害を検出します。
- **柔軟な IP 制限を設定**：許可する IP を、個々の IP ブロック形式と CIDR ブロック形式の両方で指定します。 これらの設定はサンドボックスごとに適用され、特定のセキュリティニーズに合わせてネットワーク制限を調整できます。

## 監査と監視の機能

安全なデータアクセス手法をサポートするために、クエリサービスは、Experience Platformにアクセスまたはアクセスを試みるすべてのクライアント IP をログに記録します。 拒否された接続を含む監査イベントは、Experience Platform監査ログに送信されます。 これにより、次のことが可能になります。

- **リアルタイム監視**: IP ベースのアクセスパターンを追跡して、コンプライアンスを確保します。
- **権限のないアクセスに関するアラート**：権限のない IP からのアクセス試行を識別して応答します。

詳しくは、[ 監査ログの概要 ](../../landing/governance-privacy-security/audit-logs/overview.md) を参照してください。

## 次の手順

Data Distiller Authorization API の基本を学ぶには、[ はじめる前に ](./getting-started.md) を参照して、必要なヘッダーや API 呼び出し規則などの基本的な設定手順を確認してください。 次に、安全なデータアクセスの設定と管理に関する [IP アクセス ](./ip-access.md) および [IP 検証 ](./validate.md) のエンドポイント固有のガイドを参照します。

統合、テスト、探索を容易にする標準化された機械読み取り可能な形式を表示するには、[Data Distiller認証 OpenAPI リファレンスドキュメント ](https://developer.adobe.com/experience-platform-apis/references/data-distiller-auth/) を参照してください。

返される各データセットの様々な応答パラメーターについて詳しくは、[ データセット API 開発者向けドキュメント ](https://developer.adobe.com/experience-platform-apis/references/catalog/#tag/Datasets/operation/listDatasets) を参照してください。
