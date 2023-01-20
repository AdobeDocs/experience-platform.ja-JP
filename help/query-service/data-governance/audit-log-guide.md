---
title: クエリサービス監査ログの統合
description: クエリサービスの監査ログは、様々なユーザー操作の記録を保持し、問題のトラブルシューティングや、企業データ管理ポリシーおよび規制要件への準拠に関する監査証跡を形成します。 このチュートリアルでは、クエリサービスに固有の監査ログ機能の概要を説明します。
exl-id: 5fdc649f-3aa1-4337-965f-3f733beafe9d
source-git-commit: cde7c99291ec34be811ecf3c85d12fad09bcc373
workflow-type: tm+mt
source-wordcount: '935'
ht-degree: 8%

---

# [!DNL Query Service] 監査ログの統合

ザAdobe Experience Platform [!DNL Query Service] 監査ログ統合は、クエリ関連のユーザーアクションの記録を提供します。 監査ログは、企業のデータ管理ポリシーや規制要件のトラブルシューティングや準拠をおこなうための重要なツールです。 この機能を使用すると、多くのイベントタイプのアクションログを返し、レコードをフィルタリングおよびエクスポートできます。 ログには、Platform UI または [監査クエリ API](https://www.adobe.io/experience-platform-apis/references/audit-query/) CSV または JSON ファイル形式でダウンロードしたもの。

監査ログのユーザーインターフェイスについて詳しくは、 [監査ログの概要ドキュメント](../../landing/governance-privacy-security/audit-logs/overview.md). Platform API への呼び出しについて詳しくは、 [監査ログ API ガイド](../../landing/api-guide.md).

## 前提条件

次を持っている必要があります： [!DNL Data Governance] [!UICONTROL ユーザーアクティビティログを表示] 権限を有効にして、Platform UI 内で監査ログダッシュボードを表示できるようにしました。 この権限は、Adobe [Admin Console](https://adminconsole.adobe.com/) を使用して有効にできます。この権限を有効にするための管理者権限がない場合は、組織の管理者に問い合わせてください。 [Admin Console を使用した権限の追加に関する完全な手順](../../access-control/home.md)については、アクセス制御に関するドキュメントを参照してください。

## [!DNL Query Service] 監査ログカテゴリ {#audit-log-categories}

次の方法で提供される監査ログカテゴリ [!DNL Query Service] は次のようになります。

| カテゴリ | 説明 |
|---|---|
| [!UICONTROL クエリ] | このカテゴリでは、クエリの実行を監査できます。 |
| [!UICONTROL クエリテンプレート] | このカテゴリでは、クエリテンプレートで実行される様々なアクション（作成、更新、削除）を監査できます。 |
| [!UICONTROL スケジュール済みクエリ] | このカテゴリでは、内で作成、更新または削除されたスケジュールを監査できます [!DNL Query Service]. |

## の実行 [!DNL Query Service] 監査ログ {#perform-an-audit-log}

監査を実行するには [!DNL Query Service] アクティビティ、選択 **[!UICONTROL 監査]** 左のナビゲーションから、ファネルアイコン (![フィルターアイコン。](../images/audit-log/filter.png)) をクリックして、結果を絞り込むのに役立つフィルターコントロールのリストを表示します。

![左側のナビゲーションに「監査」が表示された Platform UI 監査ログダッシュボードと、フィルターコントロールがハイライト表示されています。](../images/audit-log/filter-controls.png)

次の [!UICONTROL 監査] dashboard [!UICONTROL アクティビティログ] 」タブでは、記録されたすべての Platform アクションを、 [!DNL Query Service] カテゴリ。 ログの結果は、実行期間、実行されたアクション/関数、またはクエリを実行したユーザーに基づいて、さらにフィルタリングできます。 詳しくは、監査ログのドキュメントを参照してください。 [カテゴリ、アクション、ユーザーおよびステータスに基づいてログをフィルタリングする方法について詳しく説明します](../../landing/governance-privacy-security/audit-logs/overview.md#managing-audit-logs-in-the-ui).

返される監査ログデータには、選択したフィルター条件を満たすすべてのクエリに関する次の情報が含まれます。

| 列名 | 説明 |
|---|---|
| [!UICONTROL タイムスタンプ] | アクションが `month/day/year hour:minute AM/PM` 形式 |
| [!UICONTROL アセット名] | の値 [!UICONTROL アセット名] 「 」フィールドは、フィルターとして選択したカテゴリに応じて異なります。 を使用する場合、 [!UICONTROL スケジュール済みクエリ] これはカテゴリです **スケジュール名**. を使用する場合、 [!UICONTROL クエリテンプレート] カテゴリ、これは **テンプレート名**. を使用する場合、 [!UICONTROL クエリ] カテゴリ、これは **session ID** |
| [!UICONTROL カテゴリ] | このフィールドは、フィルタードロップダウンで選択したカテゴリと一致します。 |
| [!UICONTROL アクション] | 作成、削除、更新、実行のいずれかを指定できます。 使用可能なアクションは、フィルターとして選択したカテゴリによって異なります。 |
| [!UICONTROL ユーザー] | このフィールドは、クエリを実行したユーザー ID を提供します。 |

![フィルターが適用されたアクティビティログが強調表示された「監査」ダッシュボード。](../images/audit-log/filtered-activity.png)

>[!NOTE]
>
>ログ結果を CSV または JSON ファイル形式でダウンロードすると、監査ログダッシュボードにデフォルトで表示されるよりも多くのクエリの詳細が表示されます。

## 詳細パネル

監査ログの結果の行を選択して、画面の右側にある詳細パネルを開きます。

![詳細パネルが強調表示された監査ダッシュボードの「アクティビティログ」タブ](../images/audit-log/details-panel.png)

詳細パネルを使用して、 [!UICONTROL アセット ID] そして [!UICONTROL イベントステータス].

の値 [!UICONTROL アセット ID] 監査で使用されるカテゴリに応じて変化します。

* を使用する場合、 [!UICONTROL クエリ] カテゴリ [!UICONTROL アセット ID] が  **session ID**.
* を使用する場合、 [!UICONTROL クエリテンプレート] カテゴリ [!UICONTROL アセット ID] が **template ID** という名前で、 `[!UICONTROL templateID:]`.
* を使用する場合、 [!UICONTROL スケジュール済みクエリ] カテゴリ [!UICONTROL アセット ID] が  **スケジュール ID** という名前で、 `[!UICONTROL scheduleID:]`.

の値 [!UICONTROL イベントステータス] 監査で使用されるカテゴリに応じて変化します。

* を使用する場合、 [!UICONTROL クエリ] カテゴリ [!UICONTROL イベントステータス] フィールドには、 **クエリ ID** は、そのセッション内でユーザーによって実行されました。
* を使用する場合、 [!UICONTROL クエリテンプレート] カテゴリ [!UICONTROL イベントステータス] フィールドに **テンプレート名** イベントステータスのプレフィックスとして。
* を使用する場合、 [!UICONTROL クエリスケジュール] カテゴリ [!UICONTROL イベントステータス] フィールドに **スケジュール名** イベントステータスのプレフィックスとして。

## 使用可能なフィルター [!DNL Query Service] 監査ログカテゴリ {#available-filters}

使用可能なフィルターは、ドロップダウンで選択したカテゴリによって異なります。 次の表に、使用可能なフィルターの詳細を示します。 [[!DNL Query Service] 監査ログカテゴリ](#audit-log-categories).

| フィルター | 説明 |
|---|---|
| カテゴリ | 詳しくは、 [[!DNL Query Service] 監査ログカテゴリ](#audit-log-categories) セクションを参照してください。 |
| アクション | 参照元が [!DNL Query Service] 監査カテゴリ、更新は **既存のフォームへの変更**、削除は **スケジュールまたはテンプレートの削除**、 **新しいスケジュールまたはテンプレートの作成**、および execute **クエリの実行**. |
| ユーザー | ユーザーでフィルタリングするには、完全なユーザー ID( 例：johndoe@acme.com) を入力します。 |
| ステータス | この [!UICONTROL 許可], [!UICONTROL 成功]、および [!UICONTROL 失敗] オプションでは、「ステータス」または「イベントステータス」に基づいてログをフィルタリングしますが、 [!UICONTROL 拒否] オプションは除外されます **すべて** ログ。 |
| 日付 | 結果をフィルターする日付範囲を定義する開始日および／または終了日を選択します。 |

## 次の手順

このドキュメントを読むと、 [!DNL Query Service] 監査ログ機能と、それを使用して [!DNL Query Service] ユーザーアクション。

を使用している場合、 [!DNL Query Service] トラブルシューティングの目的で監査ログ機能を使用する場合は、 [トラブルシューティングガイド](../troubleshooting-guide.md).
