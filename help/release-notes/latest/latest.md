---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2023年10月のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: 4ab89ef7cabc9d808fd9dab24b6dbe3fe23e53f3
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 47%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年10月25日（PT）**

 Experience Platform の既存の機能に対するアップデート：

- [データ収集](#data-collection)
- [サンドボックス](#sandboxes)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| 拡張機能 | [!DNL Meta] コンバージョン API の機能強化 | 次の 3 つの機能強化がおこなわれました： [メタ変換 API](/help/tags/extensions/server/meta/overview.md) 拡張子： <ul><li>との統合 [[!DNL Meta Business Extension (MBE)]](/help/tags/extensions/server/meta/overview.md#integration-with-meta-business-extension-mbe)：コンバージョン API 統合用のピクセル ID とアクセストークンをAdobeと共有でき、シームレスなログイン操作を作成します。</li><li>との統合 [[!DNL Event Match Quality Score (EMQ)]](/help/tags/extensions/server/meta/overview.md#integration-with-event-quality-match-score-emq)：目的のアクションを完了し、配信された広告にアクションをリンクし直す可能性の高い人に広告を配信できます。</li><li>との統合 [[!DNL LiveRamp (Alpha)]](/help/tags/extensions/server/meta/overview.md#integration-with-liveramp-alpha):PII を直接パートナーやメタと共有する必要がなく、CIP フィールドに LiveRamp の RampID を渡すことができます。 </li></ul> |

データ収集について詳しくは、[データ収集の概要](../../tags/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対処するため、Experience Platformは、1 つの Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展を支援するサンドボックスを提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| サンドボックスツール | サンドボックスツール機能を使用すると、サンドボックス間の設定の精度を向上させ、サンドボックス間でサンドボックス設定をシームレスに書き出し、読み込むことができます。 サンドボックスツール機能を使用すると、異なるオブジェクトを選択してパッケージにエクスポートできます。 詳しくは、 [サンドボックスツール UI ガイド](../../sandboxes/ui/sandbox-tooling.md). |

サンドボックスについて詳しくは、 [サンドボックスの概要](../../sandboxes/home.md) を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| アカウントオーディエンス（限定 GA） | Real-time Customer Data Platform B2B Edition で、アカウントのセグメント化を使用して、ユーザーベースのオーディエンスからアカウントベースのオーディエンスに至る、マーケティングセグメント化エクスペリエンスを完全に簡単かつ洗練化できるようになりました。 この機能の詳細については、 [アカウントオーディエンスの概要](../../segmentation/ui/account-audiences.md). |

セグメント化サービスの詳細については、「[セグメント化サービスの概要](../../segmentation/home.md)」を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| データランディングゾーンの認証を更新しました | 資格情報の表示時に、データランディングゾーンの指定された有効期限が表示されるようになりました。 アプリケーションでトークンを引き続き使用するには、有効期限の前にトークンを更新する必要があります。 指定した有効期限より前に手動でトークンを更新しない場合、次回資格情報を取得する際に自動的に更新され、新しいトークンが提供されます。 詳しくは、 [データランディングゾーンの使用](../../sources/tutorials/ui/create/cloud-storage/data-landing-zone.md). |

{style="table-layout:auto"}

ソースの詳細については、 [ソースの概要](../../sources/home.md).
