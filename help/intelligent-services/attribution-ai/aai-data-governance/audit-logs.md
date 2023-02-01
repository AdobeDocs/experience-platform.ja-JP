---
keywords: インサイト；attribution ai;attribution ai インサイト；AAI クエリサービス；アトリビューションクエリ；アトリビューションスコア
title: 監査ログのAttribution AIの概要
description: 監査ログをAttribution AIで表示および管理する方法について説明します。
source-git-commit: a68d4634c6341f27673fdd70d96f7e214032b5a9
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 38%

---

# 監査ログ

システムで実行されるアクティビティの透明性と可視性を高めるために、Attribution AIワークフロー内のユーザーアクティビティが監査ログに取り込まれ、Attribution AIモデルに対するユーザー主導の変更を把握できるようになりました。 これらのログは、問題のトラブルシューティングに役立つ監査証跡を形成し、企業のデータ管理ポリシーおよび規制要件に効果的に準拠するのに役立ちます。  HIPAA(Health Insurance Portability and Accountability Act) に従事し、Attribution AIまたは顧客 AI を通じて、許可された機密データを作成、受け取り、保守、または送信する場合、BAA をAdobeおよびライセンス付与のヘルスケア・シールドを使用して実行する責任があります。

基本的に、監査ログでは、誰が どのアクションを、いつ実行したかがわかります。ログに記録される各アクションには、アクションのタイプ、日時、アクションを実行したユーザーの電子メール ID、アクションのタイプに関連する追加の属性を示すメタデータが含まれます。これは、ユーザーが実行した作成、更新および削除アクションをAttribution AI内で追跡します。

<!-- [The audit logs selected in the Attribution AI workspace](../../../attribution-ai/aai-data-governance/images/data-governance/audit-logs-cai.png) -->

## 監査ログへのアクセス

組織に対してこの機能が有効になっている場合、アクティビティの発生に応じて監査ログが自動的に収集されます。ログ収集を手動で有効にする必要はありません。

監査ログを表示および書き出すには、Adobe コンソールで監査ログのアクセスアクセス制御権限を付与されている必要があります。Attribution AI機能の個々の権限を管理する方法については、 [アクセス制御ドキュメント](../aai-data-governance/access-controls.md).

