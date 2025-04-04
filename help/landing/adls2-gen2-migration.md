---
title: Gen2 への Data Lake の移行
description: Adobe Experience Platformでの Data Lake から Gen2 への移行によって提供される新機能について説明します。
exl-id: 56d9c77a-d7eb-498d-994f-b15d150dedb7
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 0%

---

# Adobe Experience Platform Data Lake を Gen2 に移行

Adobe Experience Platformは Gen2 Data Lake に移行されています。 これは、Experience Platform ユーザーにジオリージョンレプリケーション、ロールベースのアクセス制御（RBAC）の微細化、スケーリングの改善などのメリットをもたらす新世代のデータレイクです。

## ユーザーへの影響

Adobeが Data Lake を Gen1 から Gen 2 に移行する間、ユーザーは Data Lake から **読み取り** できますが、Data Lake に **書き込み** するすべての機能に影響が及びます。 影響を受ける機能のリストを以下に示します。

- **ソース**：ソースおよび様々なデータ取り込みワークフローから受信するデータは遅延します。 移行が完了すると、ユーザーにデータが表示されます。
- **クエリサービス**: ユーザーはクエリを実行できますが、クエリの出力をデータセットに書き込むことはできません。
- **リアルタイム顧客プロファイル**:**バッチ** 取り込みを通じてプロファイルストアに取り込まれたデータは、移行中は使用できません。 ただし、**ストリーミング** 取り込みによって取り込まれたデータは、移行中も使用できます。 また、移行中はプロファイルの書き出しを使用することはできません。
- **Data Science Workspace**:Data Science Workspaceからの書き込みが失敗します。
- **セグメント化サービス**:**バッチ** セグメント化から派生したオーディエンスは、移行中にアクティブ化できません。 **ストリーミング** セグメント化から派生するオーディエンスは影響を受けません。
- **Customer Journey Analytics**: バッチが Data Lake に取り込まれていないので、Customer Journey Analytics レポートのデータが古くなっている可能性があり、移行中に更新されません。

## Experience Platform ユーザーへの通信

Adobeがシステム管理者に連絡して、移行の影響について詳細に話し合い、特定の組織に対する移行日時を確認します。
