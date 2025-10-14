---
title: 「Auditor」タブ
description: Adobe Experience Platform Debuggerの「Auditor」タブを使用してAdobe Experience Cloud実装をテストする方法を説明します。
keywords: デバッガー；experience platform デバッガー拡張機能；chrome；拡張機能；auditor;dtm;target
exl-id: 409094f8-a7d9-45f7-ba12-b5e6250abc0f
source-git-commit: df1a67e4b6f3d2eaeaba2b8d3c9b1588ee0b1461
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 32%

---

# 「Auditor」タブ

Adobe Experience Platform Debuggerとして、「**[!UICONTROL Auditor]**」タブを使用して、ページに対して一連の監査テストを実行できます。

この機能を使用するには：

1. 左側のナビゲーションで **[!UICONTROL Auditor]** を選択します。
1. 「**[!UICONTROL Auditor テストを実行]**」を選択します。 テストが完了すると、結果が以下に表示されます。

![&#x200B; 「Auditor」タブに表示されるテスト結果のスクリーンショット &#x200B;](../images/auditor-results.png)

結果リストには、テストとその結果が表示され、問題を解決するための提案が示されます。

## テスト結果の解釈

各テストには重み付けが付けられ、テストスコアは割り当てられた重み付けと等しくなります。 5 の重みでテストに合格すると、5 ポイントを獲得できます。

| スコア | 説明 |
| --- | --- |
| 0 | 注意が必要な問題をアラートで通知しますが、スコアには影響しません。 |
| 1 | は最適化を推奨します。 データの正確性には影響しません。 |
| 2 | このテストに失敗すると、Adobe Experience Cloudの最新の機能や修正点にアクセスできなくなります。 |
| 3 | 効率をテストし、実装がベストプラクティスに従っているかどうかを確認します。 |
| 4 | 失敗は、信頼性の低いデータを収集している可能性があることを意味します。 |
| 5 | 失敗すると、データが失われる可能性があります。 |

すべてのテストが合格または不合格になります。 テスト条件に対する準拠または違反をテストするので、一部準拠している場合でも部分的にスコアが提供されるわけではありません。例えば、テストでアドビソリューションの最新バージョンを確認し、1 つ前のバージョンを使用していることがわかった場合、5 つ前のバージョンを使用している場合と同じスコアが付けられます。最新バージョンには、パフォーマンスの向上とバグ修正が含まれているので、最新バージョンにすることをお勧めします。

レベル 4 または 5 の結果を修正することを&#x200B;**強くお勧めします**。

レベル 1 から 3 の結果を修正することを&#x200B;**お勧めします**。

## サポートされるAdobeテクノロジ

Auditor 機能は、次のAdobeテクノロジを評価できます。

* Adobe Advertising Cloud DSP
* Adobe Advertising Cloud Search
* Adobe Analytics
* Adobe Experience Cloud ID サービス
* Adobe Target
* タグ（旧称Adobe Experience Platform Launch）

## テストルーブリック

この機能で提供されるテストルーブリックについて詳しくは、次のドキュメントを参照してください。

* [タグの整合性](./tag-consistency.md)
* [タグの有無](./tag-presence.md)
* [設定](./configuration.md)
* [アラート](./alerts.md)
