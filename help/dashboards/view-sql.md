---
title: insight SQL を表示
description: プロファイル、オーディエンス、宛先およびカスタマイズされたインサイトの背後にある SQL を表示し、クエリエディターを通じてクエリをオンデマンドで実行します。
exl-id: fd728926-c113-4593-92b1-916a02d09d41
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---

# insight SQL を表示

[!UICONTROL SQL を表示 ] 機能を使用して、プロファイル、オーディエンス、宛先およびカスタマイズされたインサイトの背後にある SQL を表示し、クエリエディターを通じてオンデマンドでクエリを実行します。 40 を超える既存のインサイトの SQL からインスピレーションを得て、ビジネスニーズに基づいてExperience Platform データから独自のインサイトを導き出す新しいクエリを作成します。

## ダッシュボードの「概要」に移動します。 {#navigate-to-overview}

選択したダッシュボードを開くには、**[!UICONTROL プロファイル]**、**[!UICONTROL オーディエンス]**、**[!UICONTROL 宛先]** のいずれかを左側のナビゲーションから選択します。 次に、ワークスペースが自動的に表示されない場合は、タブオプションから **[!UICONTROL 概要]** を選択します。

または、左側のナビゲーションから **[!UICONTROL ダッシュボード]** を選択し、続いてカスタムダッシュボードの名前を選択します。 ユーザー定義ダッシュボードの概要が表示されます。

![[!UICONTROL  プロファイル ]、[!UICONTROL  オーディエンス ]、[!UICONTROL  宛先 ]、および [!UICONTROL  ダッシュボード ] がハイライト表示されたExperience Platform UI](./images/view-sql/dashboard-navigation.png)

## SQL を表示切替スイッチ {#toggle}

プロファイル、オーディエンス、宛先、ユーザー定義の各ダッシュボードの概要から、この機能を有効または無効にする切替スイッチを使用できます。

>[!NOTE]
>
>[!UICONTROL SQL を表示 ] 切替スイッチを有効にした場合、この機能を無効にするまでは、グローバルフィルターおよびウィジェットレベルのフィルターを変更できません。

![ 「[!UICONTROL SQL を表示 ]」切替スイッチがハイライト表示されています。](./images/view-sql/view-sql-toggle.png)

個々のinsightに [!UICONTROL SQL を表示 ] テキストを表示する切替スイッチを有効にします。

![ 「SQL を表示 [!UICONTROL  がハイライト表示されたinsight]](./images/view-sql/insight-view-sql.png)

**[!UICONTROL SQL を表示]** を選択して、ウィジェットの SQL を含むダイアログを開きます。

## SQL ダイアログ {#sql-dialog}

insightのタイトルと、それを生成する SQL を含むダイアログが表示されます。

>[!TIP]
>
>コピーアイコン（![ コピーアイコンを選択すると、SQL 文全体をクリップボードにコピーできます。](/help/images/icons/copy.png)）を選択します。

![SQL 文がハイライト表示されたinsightダイアログ ](./images/view-sql/sql-dialog.png)

「**[!UICONTROL SQL を実行]**」を選択して、クエリが事前入力されたクエリエディターを開きます。

![ 「[!UICONTROL SQL を実行 ] がハイライト表示されたinsight ダイアログ ](./images/view-sql/run-sql.png)

## 既存の SQL を編集 {#edit-sql}

クエリエディターが表示されます。 レポートのニーズに合った方法で、ステートメントを編集し、platform データをクエリできるようになりました。 新しいクエリテンプレートを適切な名前で保存します。

![ 選択したinsight SQL が事前入力されたクエリエディター。](./images/view-sql/edit-sql.png)

## 次の手順

このドキュメントでは、標準ダッシュボードまたはユーザー定義ダッシュボード内のinsightの SQL にアクセスする方法を確認しました。 [Real-Time Customer Data Platform Insights データモデルのドキュメント ](./data-models/cdp-insights-data-model-b2c.md) を読むことをお勧めします。 このドキュメントには、マーケティングおよび KPI のニーズに合わせてカスタマイズされた、Real-Time CDP レポート用の SQL テンプレートのカスタマイズに関するインサイトが含まれています。
