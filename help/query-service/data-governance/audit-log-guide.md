---
title: クエリサービス監査ログの統合
description: クエリサービス監査ログは、問題のトラブルシューティングや企業のデータ管理ポリシーおよび規制要件への準拠のための監査証跡を形成するために、様々なユーザーアクションの記録を保持します。 このチュートリアルでは、クエリサービスに固有の監査ログ機能の概要を説明します。
exl-id: 5fdc649f-3aa1-4337-965f-3f733beafe9d
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '935'
ht-degree: 8%

---

# [!DNL Query Service] 監査ログの統合

Adobe Experience Platform [!DNL Query Service] 監査ログ統合は、クエリ関連のユーザーアクションのレコードを提供します。 監査ログは、企業のデータ管理ポリシーや規制要件のトラブルシューティングと順守に不可欠なツールです。 この機能を使用すると、様々なイベントタイプのアクションログを返し、レコードをフィルタリングして書き出すことができます。 ログにはExperience Platform UI または [Audit Query API](https://www.adobe.io/experience-platform-apis/references/audit-query/) 経由でアクセスでき、CSV または JSON ファイル形式でダウンロードできます。

監査ログのユーザーインターフェイスについて詳しくは、[&#x200B; 監査ログの概要ドキュメント &#x200B;](../../landing/governance-privacy-security/audit-logs/overview.md) を参照してください。 Experience Platform API を呼び出す方法について詳しくは、[&#x200B; 監査ログ API ガイド &#x200B;](../../landing/api-guide.md) を参照してください。

## 前提条件

Experience Platform UI 内で監査ログダッシュボードを表示するには、[!DNL Data Governance]&#x200B;[!UICONTROL &#x200B; ユーザーアクティビティログを表示 &#x200B;] 権限が有効になっている必要があります。 この権限は、Adobe [Admin Console](https://adminconsole.adobe.com/) を使用して有効にできます。この権限を有効にするための管理者権限がない場合は、組織の管理者に問い合わせてください。 [Admin Console を使用した権限の追加に関する完全な手順](../../access-control/home.md)については、アクセス制御に関するドキュメントを参照してください。

## [!DNL Query Service] 監査ログのカテゴリ {#audit-log-categories}

[!DNL Query Service] が提供する監査ログのカテゴリは次のとおりです。

| カテゴリ | 説明 |
|---|---|
| [!UICONTROL クエリ] | このカテゴリを使用すると、クエリの実行を監査できます。 |
| [!UICONTROL &#x200B; クエリテンプレート &#x200B;] | このカテゴリを使用すると、クエリテンプレートに対して実行される様々なアクション（作成、更新、削除）を監査できます。 |
| [!UICONTROL &#x200B; スケジュールされたクエリ &#x200B;] | このカテゴリを使用すると、[!DNL Query Service] 内で作成、更新または削除されたスケジュールを監査できます。 |

## [!DNL Query Service] 監査ログの実行 {#perform-an-audit-log}

[!DNL Query Service] のアクティビティの監査を実行するには、左側のナビゲーションから **[!UICONTROL 監査]** を選択し、次にファネルアイコン（![A フィルターアイコンを選択します。](/help/images/icons/filter.png)）を選択して、結果を絞り込むのに役立つフィルターコントロールのリストを表示します。

![&#x200B; 左側のナビゲーションに「監査」が表示され、フィルターコントロールがハイライト表示されたExperience Platform UI 監査ログダッシュボード。](../images/audit-log/filter-controls.png)

[!UICONTROL &#x200B; 監査 &#x200B;] ダッシュボード [!UICONTROL &#x200B; アクティビティログ &#x200B;] タブでは、記録されたすべてのExperience Platform アクションを [!DNL Query Service] のいずれかのカテゴリでフィルタリングできます。 ログ結果は、実行期間、実行されたアクションや機能、クエリを実行したユーザーに基づいて、さらにフィルタリングできます。 詳しくは、監査ログのドキュメント [&#x200B; カテゴリ、アクション、ユーザー、ステータスに基づいてログをフィルタリングする方法の完全な手順 &#x200B;](../../landing/governance-privacy-security/audit-logs/overview.md#managing-audit-logs-in-the-ui) を参照してください。

返される監査ログデータには、選択したフィルター条件を満たすすべてのクエリに関する次の情報が含まれます。

| 列の名前 | 説明 |
|---|---|
| [!UICONTROL &#x200B; タイムスタンプ &#x200B;] | `month/day/year hour:minute AM/PM` 形式で実行されたアクションの正確な日時。 |
| [!UICONTROL &#x200B; アセット名 &#x200B;] | 「[!UICONTROL &#x200B; アセット名 &#x200B;] フィールドの値は、フィルターとして選択したカテゴリによって異なります。 [!UICONTROL &#x200B; スケジュール済みクエリ &#x200B;] カテゴリを使用する場合、これは **スケジュール名** です。 [!UICONTROL &#x200B; クエリテンプレート &#x200B;] カテゴリを使用する場合、これは **テンプレート名** です。 [!UICONTROL &#x200B; クエリ &#x200B;] カテゴリを使用する場合、これは **セッション ID** です |
| [!UICONTROL カテゴリ] | このフィールドは、フィルタードロップダウンで選択したカテゴリに一致します。 |
| [!UICONTROL アクション] | 作成、削除、更新、実行のいずれかを指定できます。 使用できるアクションは、フィルターとして選択したカテゴリによって異なります。 |
| [!UICONTROL ユーザー] | このフィールドには、クエリを実行したユーザー ID が表示されます。 |

![&#x200B; フィルタリングされたアクティビティログがハイライト表示された監査ダッシュボード。](../images/audit-log/filtered-activity.png)

>[!NOTE]
>
>ログ結果を CSV または JSON ファイル形式でダウンロードすると、監査ログダッシュボードにデフォルトで表示されるクエリの詳細よりも多くのクエリが提供されます。

## 詳細パネル

監査ログ結果の行を選択して、画面の右側にある詳細パネルを開きます。

![&#x200B; 詳細パネルがハイライト表示された「監査」ダッシュボードの「アクティビティログ」タブ &#x200B;](../images/audit-log/details-panel.png)

詳細パネルを使用すると、[!UICONTROL &#x200B; アセット ID] と [!UICONTROL &#x200B; イベントステータス &#x200B;] を確認できます。

[!UICONTROL &#x200B; アセット ID] の値は、監査で使用されたカテゴリに応じて変わります。

* [!UICONTROL &#x200B; クエリ &#x200B;] カテゴリを使用する場合、[!UICONTROL &#x200B; アセット ID] は **セッション ID** です。
* [!UICONTROL &#x200B; クエリテンプレート &#x200B;] カテゴリを使用する場合、[!UICONTROL &#x200B; アセット ID] は **テンプレート ID** であり、先頭に `[!UICONTROL templateID:]` が付きます。
* [!UICONTROL &#x200B; スケジュール済みクエリ &#x200B;] カテゴリを使用する場合、[!UICONTROL &#x200B; アセット ID] は **スケジュール ID** で、先頭に `[!UICONTROL scheduleID:]` が付きます。

[!UICONTROL &#x200B; イベントステータス &#x200B;] の値は、監査で使用されたカテゴリに応じて変わります。

* [!UICONTROL &#x200B; クエリ &#x200B;] カテゴリを使用する場合、[!UICONTROL &#x200B; イベントステータス &#x200B;] フィールドには、そのセッション内でユーザーが実行したすべての **クエリ ID** のリストが表示されます。
* [!UICONTROL &#x200B; クエリテンプレート &#x200B;] カテゴリを使用する場合、[!UICONTROL &#x200B; イベントステータス &#x200B;] フィールドには、イベントステータスのプレフィックスとして **テンプレート名** が表示されます。
* [!UICONTROL &#x200B; スケジュールをクエリ &#x200B;] カテゴリを使用する場合、[!UICONTROL &#x200B; イベントステータス &#x200B;] フィールドには、イベントステータスのプレフィックスとして **スケジュール名** が表示されます。

## [!DNL Query Service] 監査ログカテゴリで使用可能なフィルター {#available-filters}

使用可能なフィルターは、ドロップダウンで選択したカテゴリによって異なります。 次の表に、[[!DNL Query Service]  監査ログカテゴリ &#x200B;](#audit-log-categories) で使用できるフィルターの詳細を示します。

| フィルター | 説明 |
|---|---|
| カテゴリ | 使用可能なカテゴリの完全なリストについては、[[!DNL Query Service]  監査ログカテゴリ &#x200B;](#audit-log-categories) の節を参照してください。 |
| アクション | [!DNL Query Service] 監査カテゴリを参照する場合、update は **既存フォームの変更**、delete は **スケジュールまたはテンプレートの削除**、create は **新しいスケジュールまたはテンプレートの作成**、execute は **クエリの実行** です。 |
| ユーザー | ユーザーでフィルタリングするには、完全なユーザー ID （例：johndoe@acme.com）を入力します。 |
| ステータス | [!UICONTROL &#x200B; 許可 &#x200B;]、[!UICONTROL &#x200B; 成功 &#x200B;]、[!UICONTROL &#x200B; 失敗 &#x200B;] オプションでは、「ステータス」または「イベントステータス」に基づいてログがフィルタリングされるのに対して、[!UICONTROL &#x200B; 拒否 &#x200B;] オプションでは **すべて** のログがフィルタリングされます。 |
| 日付 | 結果をフィルターする日付範囲を定義する開始日および／または終了日を選択します。 |

## 次の手順

このドキュメントでは、[!DNL Query Service] 監査ログ機能と、その機能を使用して [!DNL Query Service] ユーザーのアクションをフィルタリングする方法について詳しく説明します。

トラブルシューティングのために [!DNL Query Service] 監査ログ機能を使用している場合は、[&#x200B; トラブルシューティングガイド &#x200B;](../troubleshooting-guide.md) を読むことをお勧めします。
