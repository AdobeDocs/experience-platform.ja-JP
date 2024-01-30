---
title: Insight SQL を表示
description: プロファイル、オーディエンス、宛先、カスタマイズされたインサイトの背後に SQL を表示し、クエリエディターを使用してクエリをオンデマンドで実行します。
source-git-commit: be90cf38970a54431f48799bf506fb0a20ec0166
workflow-type: tm+mt
source-wordcount: '409'
ht-degree: 0%

---

# インサイト SQL を表示

以下を使用します。 [!UICONTROL SQL を表示] の機能を使用して、プロファイル、オーディエンス、宛先、カスタマイズされたインサイトの背後に SQL を表示し、クエリエディターを使用してクエリをオンデマンドで実行できます。 40 を超える既存のインサイトの SQL からインスピレーションを得て、ビジネスニーズに基づいて Platform データから一意のインサイトを引き出す新しいクエリを作成します。

## ダッシュボードの概要に移動します。 {#navigate-to-overview}

選択したダッシュボードを開くには、次のいずれかを選択します。 **[!UICONTROL プロファイル]**, **[!UICONTROL オーディエンス]**&#x200B;または **[!UICONTROL 宛先]** をクリックします。 次の選択 **[!UICONTROL 概要]** ワークスペースが自動的に表示されない場合は、「 」タブオプションから選択します。

または、 **[!UICONTROL ダッシュボード]** 左側のナビゲーションに、カスタムダッシュボードの名前を入力します。 ユーザー定義のダッシュボードの概要が表示されます。

![とのExperience PlatformUI [!UICONTROL プロファイル], [!UICONTROL オーディエンス], [!UICONTROL 宛先]、および [!UICONTROL ダッシュボード] ハイライト表示されました。](./images/view-sql/dashboard-navigation.png)

## SQL 切り替えを表示 {#toggle}

プロファイル、オーディエンス、宛先、ユーザー定義の各ダッシュボードの概要から、この機能の有効/無効を切り替えることができます。

>[!NOTE]
>
>以下を有効にした場合、 [!UICONTROL SQL を表示] 切り替え可能にすると、この機能を無効にするまで、グローバルフィルタとウィジェットレベルフィルタを変更できません。

![The [!UICONTROL SQL を表示] ハイライト表示を切り替えます。](./images/view-sql/view-sql-toggle.png)

切り替えを有効にして表示 [!UICONTROL SQL を表示] 個々のインサイトに関するテキスト。

![を使用したインサイト [!UICONTROL SQL を表示] ハイライト表示されました。](./images/view-sql/insight-view-sql.png)

選択 **[!UICONTROL SQL を表示]** をクリックして、ウィジェットの SQL を含むダイアログを開きます。

## SQL ダイアログ {#sql-dialog}

インサイトのタイトルと、そのインサイトを生成する SQL を含むダイアログが表示されます。

>[!TIP]
>
>コピーアイコン (![コピーアイコン。](./images/view-sql/copy-icon.png)) をクリックします。

![SQL 文がハイライト表示されたインサイトダイアログ。](./images/view-sql/sql-dialog.png)

選択 **[!UICONTROL SQL を実行]** をクリックすると、クエリが事前に入力された状態でクエリエディターが開きます。

![とのインサイトダイアログ [!UICONTROL SQL を実行] ハイライト表示されました。](./images/view-sql/run-sql.png)

## 既存の SQL を編集 {#edit-sql}

クエリエディターが表示されます。 これで、レポートのニーズに合わせて、ステートメントを編集し、プラットフォームデータに対してクエリを実行できるようになりました。 新しいクエリテンプレートを適切な名前で保存します。

![選択した Insight SQL が事前入力されたクエリエディター。](./images/view-sql/edit-sql.png)

## 次の手順

このドキュメントでは、標準のダッシュボードまたはユーザー定義のダッシュボードで、SQL にアクセスしてインサイトを得る方法を説明しました。 まだおこなっていない場合は、 [Real-time Customer Data Platform Insights データモデルドキュメント](./cdp-insights-data-model.md). このドキュメントには、マーケティングおよび KPI のニーズに合わせて調整されたReal-Time CDPレポートの SQL テンプレートのカスタマイズに関するインサイトが含まれています。
