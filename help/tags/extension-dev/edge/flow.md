---
title: Edge 拡張機能のフロー
description: Adobe Experience Platform のエッジ拡張機能のコンポーネントが、実行時にどのように相互作用するかについて説明します。
exl-id: 99058e22-3e14-4ec6-858e-bb1c1fafdb7c
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 85%

---

# Edge 拡張機能のフロー

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

エッジ拡張機能の各条件、アクション、およびデータ要素のタイプには、ユーザーが設定を変更できるビューと、これらのユーザー定義の設定に基づいて動作するライブラリモジュールの両方が用意されています。

次の概要図に示すように、拡張機能のアクションタイプビューは、Adobe Experience Platform と統合されたアプリケーションの iframe 内に表示されます。次に、ビューを使用して設定を変更し、Experience Platform内に保存します。 タグランタイムライブラリを構築する場合、エッジノードにデプロイされるランタイムライブラリには、拡張機能のアクションタイプライブラリモジュールとユーザー定義の設定の両方が含まれます。Experience Platformのユーザー定義設定は、実行時にライブラリモジュールに挿入されます。

![拡張機能のフロー図](../images/flow/edge/event-processing-flow.png)

次の図に、ルール処理フロー内のイベント、条件、およびアクション間のリンクを示します。

![ルール処理のフロー図](../images/flow/edge/rule-processing-flow.png)

ルール処理フローには、次のフェーズが含まれます。

1. `settings` および `trigger` メソッドは、起動時にイベントライブラリモジュールに対して指定されます。
1. イベントライブラリモジュールがイベントの発生を判断すると、イベントライブラリモジュールは `trigger` を呼び出します。
1. Experience Platformは、ルールの条件タイプライブラリモジュールに `settings` を渡し、このモジュールで条件が評価されます。
1. 各条件タイプは、条件が true と評価されるかどうかを返します。
1. すべての条件が満たされると、ルールのアクションが実行されます。
