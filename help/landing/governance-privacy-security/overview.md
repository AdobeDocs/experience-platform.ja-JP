---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: ガバナンス、プライバシー、セキュリティの概要
description: Adobe Experience Platform には、ビジネスプラクティス、法的義務および開発プロセスに準拠するために、収集されたエクスペリエンスデータを確信を持って制御できるサービスやツールがいくつか用意されています。
feature: Data Governance,Privacy
exl-id: 1ab5a436-c5dd-4e7a-aba1-549f0613f224
source-git-commit: c0eb5b5c3a1968cae2bc19b7669f70a97379239b
workflow-type: tm+mt
source-wordcount: '857'
ht-degree: 21%

---

# Adobe Experience Platformにおけるガバナンス、プライバシー、セキュリティ

Adobe Experience Platformでは、データを取り込み、分析、最適化およびアクション実行して、カスタマーエクスペリエンスを大幅に強化できます。 このデータは広大で複雑、そして信じられないほど価値があります。 データ操作の性質、ビジネスが運営される法的管轄区域、データ使用に関する組織ポリシーに応じて、ビジネス上の利益を保護するために、カスタマーエクスペリエンスデータの収集と使用を慎重に制御および監視する必要があります。

Experience Platformでは、ビジネスプラクティス、法的義務および開発プロセスに準拠するために、収集されたエクスペリエンスデータを確信を持って制御できるサービスやツールをいくつか提供しています。 以下の節では、これらの各サービスの概要と、詳細を含むのドキュメントへのリンクを示します。

サービスは次の 3 つのドメインに分類できます。

* [データガバナンス](#governance)
* [プライバシー](#privacy)
* [セキュリティ](#security)

## データガバナンス {#governance}

データガバナンスは、Experience Platformのあらゆる機能と相互に関係する不可欠な概念です。 データガバナンスは、Platform を介したジャーニー全体を通じてデータを制御し、理解する能力を表します。 これには、データ品質、データ系列、データのカタログ化などを維持管理することが必要になります。

### Adobe Experience Platform のデータガバナンス {#data-governance}

Adobe Experience Platform データガバナンスを使用すると、Platform サービスとして、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへのコンプライアンスを確保できます。 Experience Platform内の様々なレベルで重要な役割を果たします（データ使用ラベル、データ使用ポリシー、ポリシー適用、データ系列など）。

詳しくは、[データガバナンスの概要](../../data-governance/home.md)を参照してください。

### カタログとデータセット {#catalog}

カタログサービスは、Platform 内でデータの場所と系列を記録するシステムです。 Experience Platform に取得されるすべてのデータはファイルとディレクトリとしてデータレイクに保存されますが、カタログには、参照や監視のために、これらのファイルとディレクトリのメタデータと説明が保持されます。

カタログは、取り込んだデータをデータセットにまとめ、各データセットには、含んだデータのラベル付けと分類に使用できるメタデータが含まれています。

サービスについて詳しくは、[ カタログサービスの概要 ](../../catalog/home.md) を参照してください。 Experience Platformでデータセットを管理する方法については、[ データセットの概要 ](../../catalog/datasets/overview.md) を参照してください。

## プライバシー {#privacy}

プライバシーは、ビジネス、議員、顧客にとって重要な問題です。 お客様から収集された個人情報は、ほぼすべてのExperience Platformワークフローの中心にあるため、Platform はこれらの取り組みをサポートするサービスを提供します。

### Adobe Experience Platform Privacy Service {#privacy-service}

EU 一般データ保護規則（GDPR）やカリフォルニア州消費者プライバシー法（CCPA）などの法的なプライバシー規制では、管轄内の市民に、収集および保存した個人データにアクセスして削除する権利が付与されています。

Adobe Experience Platform Privacy Serviceは、これらのリクエストの管理に役立つ RESTful API とユーザーインターフェイスを提供します。 Privacy Serviceを使用すると、Adobe Experience Cloud アプリケーションから非公開または個人的な顧客データに対するアクセスまたは削除のリクエストを送信でき、法的および組織のプライバシー規制の自動的な準拠が容易になります。

詳しくは、[Privacy Serviceの概要 ](../../privacy-service/home.md) を参照してください。

### 同意処理 {#consent}

多くの法的プライバシー規制では、データ収集、パーソナライゼーション、その他のマーケティングのユースケースに関して、アクティブで特定の同意に対する要件が導入されています。 これらの要件を満たすために、Experience Platformでは、個々の顧客プロファイルで同意情報を取得し、それらの環境設定をダウンストリームの Platform ワークフローで各顧客のデータがどのように使用されるかを決定する要因として使用できます。

Adobe標準を使用して顧客の同意データと環境設定データを処理する方法については、[Experience Platformでの同意処理 ](./consent/adobe/overview.md) の概要を参照してください。

IAB Transparency and Consent Framework （TCF） 2.0 に従って顧客の同意データを処理する方法については、[Platform での IAB TCF 2.0 のサポート ](./consent/iab/overview.md) の概要を参照してください。

## セキュリティ {#security}

データの整合性とセキュリティは、ビジネスにとって不可欠であり、このリスクには、業界をリードするセキュリティ機能が必要です。 この課題を満たすために、Platform にはデータ操作を保護するのに役立つツールがいくつか用意されています。

### データ暗号化

すべての Platform データは、送信時および保存時に暗号化されます。 詳しくは、[Platform でのデータ暗号化 ](./encryption.md) のドキュメントを参照してください。

### アクセス制御 {#access-control}

Experience Platformは、Adobe Admin Consoleを使用して、様々な Platform 機能に対する役割ベースのアクセス制御を提供します。 この機能は、Admin Console の製品プロファイルを利用して、ユーザーを権限およびサンドボックスにリンクします。

詳しくは、「[アクセス制御の概要](../../access-control/home.md)」を参照してください。

### サンドボックス {#sandboxes}

Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。

開発の柔軟性のニーズに対応するために、Experience Platformにはサンドボックスが用意されています。サンドボックスでは、単一の Platform インスタンスを別々の仮想環境に分割するので、ユーザー独自の開発ライフサイクルに基づいてデジタルエクスペリエンスアプリケーションを発展させることができます。

詳しくは、[サンドボックスの概要](../../sandboxes/home.md)を参照してください。

## 次の手順

このドキュメントでは、データガバナンス、プライバシー、セキュリティに関連する様々な Platform サービスとツールの概要を説明しました。 これらの機能の詳細については、このガイド全体にリンクされているドキュメントを参照してください。
