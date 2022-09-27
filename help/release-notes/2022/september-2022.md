---
title: Adobe Experience Platformリリースノート 2022 年 9 月
description: Adobe Experience Platformの 2022 年 9 月のリリースノート。
source-git-commit: 1890bd9dbce6a217951c28fc2fb3fec2634e714d
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 38%

---


# Adobe Experience Platform リリースノート

**リリース日：2022 年 9 月 28 日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [ID サービス](#identity-service)
- [ソース](#sources)

## ID サービス {#identity-service}

関連するデジタルエクスペリエンスを提供するには、顧客を完全に理解する必要があります。これは、顧客データが異なる複数のシステムに断片化され、各顧客が複数の「ID」を持つように見える場合に、より難しくなります。

Adobe Experience Platform ID サービスは、デバイスやシステム間で ID を結び付けることで、顧客とその行動をより良く把握でき、効果的な個人のデジタルエクスペリエンスをリアルタイムで提供します。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| データセット削除のサポート | ID サービスで、 [カタログサービス API](https://developer.adobe.com/experience-platform-apis/references/catalog/)、UI、またはデータの衛生状態。 次のガイドを読む： [UI でのデータセットの削除](../../catalog/datasets/user-guide.md#delete-a-dataset) を参照してください。 |

ID サービスの詳細については、 [ID サービスの概要](../../identity-service/home.md).

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Audience Managerセグメント母集団がリアルタイム顧客プロファイルに与える影響 | サイズの大きいAudience Managerセグメント母集団の取り込みは、Audience Managerソースを使用して初めてプラットフォームにAudience Managerセグメントを送信する際に、合計プロファイル数に直接影響します。 つまり、すべてのセグメントを選択すると、ライセンス使用権限を超えてプロファイル数がカウントされる可能性があります。 詳しくは、 [Audience Managerソースの概要](../../sources/connectors/adobe-applications/audience-manager.md). ライセンスの使用方法については、 [ライセンス使用状況ダッシュボードの使用](../../dashboards/guides/license-usage.md). |

ソースの詳細については、 [ソースの概要](../../sources/home.md).