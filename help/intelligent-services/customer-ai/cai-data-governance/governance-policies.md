---
keywords: Experience Platform;ユーザーガイド;顧客 AI;人気のトピック;アクセス制御;モデルの作成;
feature: Customer AI
title: 顧客 AI のガバナンスポリシー
description: Adobe Experience Platform には、収集したエクスペリエンスデータを確信を持って制御できるいくつかのサービスおよびツールが用意されています。
source-git-commit: 66d20dc1141ff33211635ba74d320350f8b27fb7
workflow-type: ht
source-wordcount: '186'
ht-degree: 100%

---


# ガバナンスポリシー

モデルを作成しモデルの設定を送信するワークフローが完了したら、違反があるかどうかを[ポリシーの適用](/help/data-governance/enforcement/auto-enforcement.md)が確認します。 ポリシー違反が発生した場合は、1 つ以上のポリシーに違反したことを示すポップオーバーが表示されます。 これは、Platform 内のデータ操作とマーケティングアクションがデータ使用ポリシーに準拠していることを確認するためです。

![ポリシー違反に関する情報を表示するポップオーバー](../images/user-guide/policy-violation-popover-cai.png)。

ポップオーバーは、違反に関する具体的な情報を提供します。 これらの違反は、設定ワークフローに直接関係しないポリシー設定やその他の測定を通じて解決できます。 例えば、特定のフィールドをデータサイエンスの目的で使用できるようにラベルを変更できます。 また、モデル設定そのものを変更して、ラベルが付いている場合は使用されないようにすることもできます。 [ポリシー](/help/data-governance/policies/overview.md)の設定方法について詳しくは、ドキュメントを参照してください。