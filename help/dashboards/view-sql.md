---
title: Insight SQL を表示
description: プロファイル、オーディエンス、宛先およびカスタマイズされたインサイトの背後にある SQL を表示し、クエリエディターを通じてクエリをオンデマンドで実行します。
exl-id: fd728926-c113-4593-92b1-916a02d09d41
source-git-commit: e0af1f0110ceb514a5b249c42a05bf780ea834c6
workflow-type: tm+mt
source-wordcount: '409'
ht-degree: 0%

---

# インサイト SQL を表示

の使用 [!UICONTROL SQL を表示] プロファイル、オーディエンス、宛先およびカスタマイズされたインサイトの背後にある SQL を表示し、クエリエディターを通じてオンデマンドでクエリを実行する機能。 40 を超える既存のインサイトの SQL からインスピレーションを得て、ビジネスニーズに基づいて Platform データから独自のインサイトを導き出す新しいクエリを作成します。

## ダッシュボードの「概要」に移動します。 {#navigate-to-overview}

選択したダッシュボードを開くには、次のいずれかを選択します **[!UICONTROL プロファイル]**, **[!UICONTROL オーディエンス]**、または **[!UICONTROL 宛先]** 左側のナビゲーションから。 次に選択 **[!UICONTROL 概要]** ワークスペースが自動的に表示されない場合は、タブのオプションから。

または、以下を選択します。 **[!UICONTROL ダッシュボード]** 左側のナビゲーションから、カスタムダッシュボードの名前に続いて。 ユーザー定義ダッシュボードの概要が表示されます。

![のExperience PlatformUI [!UICONTROL プロファイル], [!UICONTROL オーディエンス], [!UICONTROL 宛先]、および [!UICONTROL ダッシュボード] ハイライト表示](./images/view-sql/dashboard-navigation.png)

## SQL を表示切替スイッチ {#toggle}

プロファイル、オーディエンス、宛先、ユーザー定義の各ダッシュボードの概要から、この機能を有効または無効にする切替スイッチを使用できます。

>[!NOTE]
>
>を有効にする場合 [!UICONTROL SQL を表示] 切り替え：この機能を無効にするまでは、グローバルフィルターとウィジェットレベルのフィルターを変更できません。

![この [!UICONTROL SQL を表示] 切り替えがハイライト表示されています。](./images/view-sql/view-sql-toggle.png)

表示する切り替えを有効にします [!UICONTROL SQL を表示] 個々のインサイトに関するテキスト。

![～の洞察 [!UICONTROL SQL を表示] ハイライト表示](./images/view-sql/insight-view-sql.png)

を選択 **[!UICONTROL SQL を表示]** ウィジェットの SQL を含むダイアログを開きます。

## SQL ダイアログ {#sql-dialog}

インサイトのタイトルと、そのインサイトを生成する SQL を含んだダイアログが表示されます。

>[!TIP]
>
>コピーアイコン（![「コピー」アイコン](./images/view-sql/copy-icon.png)）を選択します。

![SQL 文がハイライト表示されたインサイトダイアログ](./images/view-sql/sql-dialog.png)

を選択 **[!UICONTROL SQL を実行]** クエリが事前入力された状態でクエリエディターを開きます。

![とのインサイトダイアログ [!UICONTROL SQL を実行] ハイライト表示](./images/view-sql/run-sql.png)

## 既存の SQL を編集 {#edit-sql}

クエリエディターが表示されます。 レポートのニーズに合った方法で、ステートメントを編集し、platform データをクエリできるようになりました。 新しいクエリテンプレートを適切な名前で保存します。

![選択したインサイト SQL が事前入力されたクエリエディター。](./images/view-sql/edit-sql.png)

## 次の手順

このドキュメントでは、標準ダッシュボードまたはユーザー定義ダッシュボード内のインサイトに対して SQL にアクセスする方法を確認しました。 まだ読んでいない場合は、を読むことをお勧めします。 [Real-time Customer Data Platform Insights データモデルドキュメント](./data-models/cdp-insights-data-model-b2c.md). このドキュメントには、マーケティングおよび KPI のニーズに合わせてカスタマイズされた、Real-Time CDP レポート用の SQL テンプレートのカスタマイズに関するインサイトが含まれています。
