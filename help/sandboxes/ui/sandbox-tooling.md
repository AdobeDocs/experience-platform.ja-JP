---
title: サンドボックスツール
description: サンドボックス間でのサンドボックス設定のシームレスな書き出しと読み込みをおこないます。
exl-id: f1199ab7-11bf-43d9-ab86-15974687d182
source-git-commit: 0aaba1d1ae47908ea92e402b284438accb4b4731
workflow-type: tm+mt
source-wordcount: '1821'
ht-degree: 9%

---

# [!BADGE ベータ版] サンドボックスツール

>[!IMPORTANT]
>
>The **サンドボックスツール** 以下に説明する機能は、一部のベータ版のお客様のみが利用できます。

>[!NOTE]
>
>サンドボックスツールは、 [!DNL Real-Time Customer Data Platform] および [!DNL Journey Optimizer] 開発サイクル効率及び構成精度を向上させる。<br><br>サンドボックスツール機能を使用するには、次の 2 つの役割ベースのアクセス制御権限が必要です。<br>- `manage-sandbox` または `view-sandbox`<br>- `manage-package`

サンドボックス全体の設定精度を向上させ、サンドボックスツール機能を使用して、サンドボックス間でサンドボックス設定をシームレスに書き出し、読み込みます。 サンドボックスツールを使用すると、実装プロセスの価値を高める時間を短縮し、サンドボックス全体で成功した設定を移動できます。

サンドボックスツール機能を使用すると、異なるオブジェクトを選択してパッケージにエクスポートできます。 パッケージは、1 つのオブジェクトまたは複数のオブジェクトで構成できます。 <!--or an entire sandbox.-->パッケージに含まれるオブジェクトは、同じサンドボックスからのものである必要があります。

## サンドボックスツールでサポートされるオブジェクト {#supported-objects}

サンドボックスツール機能を使用すると、書き出し機能を利用できます [!DNL Adobe Real-Time Customer Data Platform] および [!DNL Adobe Journey Optimizer] オブジェクトをパッケージに含めます。

### Real-time Customer Data Platformオブジェクト {#real-time-cdp-objects}

次の表にリストを示します。 [!DNL Adobe Real-Time Customer Data Platform] サンドボックスツールで現在サポートされているオブジェクト：

| Platform | オブジェクト | 詳細 |
| --- | --- | --- |
| 顧客データプラットフォーム | ソース | ソースアカウント資格情報は、セキュリティ上の理由から、ターゲットサンドボックスにレプリケートされないので、手動で更新する必要があります。 ソースのデータフローは、デフォルトでドラフトステータスでコピーされます。 |
| 顧客データプラットフォーム | オーディエンス | 次の項目のみ **[!UICONTROL 顧客オーディエンス]** type **[!UICONTROL セグメント化サービス]** はサポートされています。 同意およびガバナンスの既存のラベルは、同じインポートジョブでコピーされます。 |
| 顧客データプラットフォーム | ID | ターゲットサンドボックスで作成する際、Adobe標準 ID 名前空間の重複は自動的に排除されます。 オーディエンスは、和集合スキーマでオーディエンスルールのすべての属性が有効な場合にのみコピーできます。 必要なスキーマを移動し、統合プロファイルに対して最初に有効にする必要があります。 |
| 顧客データプラットフォーム | スキーマ | 同意およびガバナンスの既存のラベルは、同じインポートジョブでコピーされます。 スキーマ統合プロファイルのステータスは、そのままソースサンドボックスからコピーされます。 スキーマがソースサンドボックスで統合プロファイルに対して有効になっている場合、すべての属性が和集合スキーマに移動されます。 スキーマ関係のエッジケースは、パッケージに含まれていません。 |
| 顧客データプラットフォーム | データセット | デフォルトでは、統合プロファイル設定が無効な状態でデータセットがコピーされます。 |

次のオブジェクトはインポートされますが、ドラフトまたは無効のステータスになっています。

| 機能 | オブジェクト | ステータス |
| --- | --- | --- |
| インポートステータス | ソースのデータフロー | ドラフト |
| インポートステータス | ジャーニー | ドラフト |
| 統合プロファイル | データセット | 統合プロファイルが無効です |
| ポリシー | データガバナンスポリシー | 無効 |

### Adobe Journey Optimizerオブジェクト {#abobe-journey-optimizer-objects}

次の表にリストを示します。 [!DNL Adobe Journey Optimizer] サンドボックスツールと制限に対して現在サポートされているオブジェクト：

| Platform | オブジェクト | 詳細 |
| --- | --- | --- |
| [!DNL Adobe Journey Optimizer] | オーディエンス | オーディエンスは、ジャーニーオブジェクトの依存オブジェクトとしてコピーできます。 「新しいオーディエンスを作成」を選択するか、ターゲットサンドボックスで既存のオーディエンスを再利用できます。 |
| [!DNL Adobe Journey Optimizer] | スキーマ | ジャーニーで使用されるスキーマは、依存オブジェクトとしてコピーできます。 新しいスキーマを作成するか、ターゲットサンドボックスで既存のスキーマを再利用するかを選択できます。 |
| [!DNL Adobe Journey Optimizer] | メッセージ | ジャーニーで使用されるメッセージは、依存オブジェクトとしてコピーできます。 「ジャーニー」フィールドで使用されるチャネルアクションアクティビティは、メッセージ内のパーソナライゼーションに使用され、完全性がチェックされません。 コンテンツブロックはコピーされません。 |
| [!DNL Adobe Journey Optimizer] | ジャーニー - キャンバスの詳細 | キャンバス上のジャーニーの表現には、ジャーニー内のオブジェクト（条件、アクション、イベント、読み取りオーディエンスなど、コピーされるオーディエンスなど）が含まれます。 ジャンプアクティビティはコピーから除外されます。 |
| [!DNL Adobe Journey Optimizer] | イベント | ジャーニーで使用されるイベントとイベントの詳細がコピーされます。 常にターゲットサンドボックスに新しいバージョンが作成されます。 |
| [!DNL Adobe Journey Optimizer] | アクション | ジャーニーで使用されるアクションとアクションの詳細がコピーされます。常にターゲットサンドボックスに新しいバージョンが作成されます。 |

サーフェス（例えば、プリセット）はコピーされません。 メッセージのタイプとサーフェス名に基づいて、宛先サンドボックスで最も近い一致が自動的に選択されます。 ターゲットサンドボックスにサーフェスが見つからない場合、サーフェスのコピーは失敗し、メッセージのコピーは失敗します。これは、設定にサーフェスを使用する必要があるためです。 この場合、コピーを機能させるには、メッセージの右側のチャンネルに少なくとも 1 つのサーフェスを作成する必要があります。

ジャーニーの書き出し時に、カスタム ID タイプが依存オブジェクトとしてサポートされない。

## パッケージへのオブジェクトのエクスポート {#export-objects}

>[!NOTE]
>
>すべての書き出しアクションは監査ログに記録されます。

>[!CONTEXTUALHELP]
>id="platform_sandbox_tooling_exit_package"
>title="パッケージを保存して終了"
>abstract="パッケージを終了して保存するには、「戻る」オプションを使用するだけで済みます。"

>[!CONTEXTUALHELP]
>id="platform_sandbox_tooling_remove_object"
>title="オブジェクトを削除"
>abstract="ユーザーは行を選択し、削除オプション（選択時に使用可能）を使用して、行を削除する必要があります。"

>[!CONTEXTUALHELP]
>id="platform_sandbox_package_expiry"
>title="パッケージの有効期限設定"
>abstract="日付は今日から 90 日に設定されています。この日付は、パッケージが公開されるまで変更され続けます。あるユーザーが翌日ドラフトステータスのパッケージにアクセスすると、日付は 1 日後に移動します（ユーザーが設定している場合を除く）。"

>[!CONTEXTUALHELP]
>id="platform_sandbox_tooling_package_status"
>title="パッケージステータス"
>abstract="デフォルトでは、ステータスはドラフトに設定されています。パッケージが公開されると、ステータスが「公開済み」に変わります。パッケージが公開された後は、変更することはできません。"

>[!NOTE]
>
>パッケージのインポートは、オブジェクトにアクセスする権限を持っている場合にのみ実行できます。

この例では、スキーマを書き出してパッケージに追加するプロセスについて説明します。 同じプロセスを使用して、データセット、ジャーニーなど、他のオブジェクトを書き出すことができます。

### 新しいパッケージにオブジェクトを追加 {#add-object-to-new-package}

選択 **[!UICONTROL スキーマ]** 左のナビゲーションから、 **[!UICONTROL 参照]** 」タブに追加します。使用可能なスキーマの一覧が表示されます。 次に、省略記号 (`...`) をクリックし、ドロップダウンにコントロールを表示します。 選択 **[!UICONTROL パッケージに追加]** をドロップダウンから選択します。

![スキーマのリスト。 [!UICONTROL パッケージに追加] コントロール。](../images/ui/sandbox-tooling/add-to-package.png)

次から： **[!UICONTROL パッケージに追加]** ダイアログで、 **[!UICONTROL 新しいパッケージを作成]** オプション。 次を提供： [!UICONTROL 名前] （パッケージとオプション） [!UICONTROL 説明]を選択し、「 **[!UICONTROL 追加]**.

![The [!UICONTROL パッケージに追加] ～との対話 [!UICONTROL 新しいパッケージを作成] 選択済みおよびハイライト [!UICONTROL 追加].](../images/ui/sandbox-tooling/create-new-package.png)

次の場所に戻ります。 **[!UICONTROL スキーマ]** 環境。 次に示す手順に従って、作成したパッケージにオブジェクトを追加できます。

### 既存のパッケージにオブジェクトを追加して公開する {#add-object-to-existing-package}

使用可能なスキーマのリストを表示するには、「 **[!UICONTROL スキーマ]** 左のナビゲーションから、 **[!UICONTROL 参照]** タブをクリックします。 次に、省略記号 (`...`) をクリックし、ドロップダウンメニューのコントロールオプションを表示します。 選択 **[!UICONTROL パッケージに追加]** をドロップダウンから選択します。

![スキーマのリスト。 [!UICONTROL パッケージに追加] コントロール。](../images/ui/sandbox-tooling/add-to-package.png)

The **[!UICONTROL パッケージに追加]** ダイアログが表示されます。 を選択します。 **[!UICONTROL 既存のパッケージ]** オプションを選択し、 **[!UICONTROL パッケージ名]** ドロップダウンし、必要なパッケージを選択します。 最後に、 **[!UICONTROL 追加]** をクリックして選択を確定します。

![[!UICONTROL パッケージに追加] ダイアログが開き、ドロップダウンから選択したパッケージが表示されます。](../images/ui/sandbox-tooling/add-to-existing-package.png)

パッケージに追加されたオブジェクトのリストが表示されます。 パッケージを公開し、サンドボックスに読み込めるようにするには、「 」を選択します。 **[!UICONTROL 公開]**.

![パッケージ内のオブジェクトのリスト。 [!UICONTROL 公開] オプション。](../images/ui/sandbox-tooling/publish-package.png)

選択 **[!UICONTROL 公開]** をクリックして、パッケージの公開を確認します。

![公開パッケージの確認ダイアログで、 [!UICONTROL 公開] オプション。](../images/ui/sandbox-tooling/publish-package-confirmation.png)

>[!NOTE]
>
>公開後は、パッケージの内容を変更できません。 互換性の問題を回避するには、必要なアセットがすべて選択されていることを確認します。 変更を加える必要がある場合は、新しいパッケージを作成する必要があります。

次の場所に戻ります。 **[!UICONTROL パッケージ]** 」タブをクリックします。 [!UICONTROL サンドボックス] 環境で、新しくパブリッシュされたパッケージを表示できます。

![新しく公開されたパッケージをハイライトするサンドボックスパッケージのリストです。](../images/ui/sandbox-tooling/published-packages.png)

## ターゲットサンドボックスにパッケージをインポートする {#import-package-to-target-sandbox}

>[!NOTE]
>
>すべてのインポートアクションは監査ログに記録されます。

パッケージをターゲットサンドボックスに読み込むには、サンドボックスに移動します。 **[!UICONTROL 参照]** 「 」タブをクリックし、サンドボックス名の横にあるプラス (+) オプションを選択します。

![サンドボックス **[!UICONTROL 参照]** 「 」タブで、インポートパッケージの選択をハイライト表示します。](../images/ui/sandbox-tooling/browse-sandboxes.png)

ドロップダウンメニューを使用して、 **[!UICONTROL パッケージ名]** ターゲットのサンドボックスに読み込む オプションの追加 **[!UICONTROL ジョブ名]**（将来の監視に使用）を選択し、 **[!UICONTROL 次へ]**.

![インポートの詳細ページに、 [!UICONTROL パッケージ名] ドロップダウン選択](../images/ui/sandbox-tooling/import-package-to-sandbox.png)

The [!UICONTROL パッケージオブジェクトと依存関係] ページには、このパッケージに含まれるすべてのアセットのリストが表示されます。 選択した親オブジェクトを正常にインポートするために必要な依存オブジェクトが自動的に検出されます。

![The [!UICONTROL パッケージオブジェクトと依存関係] ページには、パッケージに含まれているアセットのリストが表示されます。](../images/ui/sandbox-tooling/package-objects-and-dependencies.png)

>[!NOTE]
>
>依存オブジェクトは、ターゲットサンドボックス内の既存のオブジェクトに置き換えることができます。これにより、新しいバージョンを作成する代わりに、既存のオブジェクトを再利用できます。 例えば、スキーマを含むパッケージをインポートする場合、ターゲットサンドボックスで既存のカスタムフィールドグループと ID 名前空間を再利用できます。 または、ジャーニーを含むパッケージをインポートする際に、ターゲットサンドボックスで既存のセグメントを再利用することもできます。

既存のオブジェクトを使用するには、依存オブジェクトの横にある鉛筆アイコンを選択します。 新規作成または既存のを使用するオプションが表示されます。 選択 **[!UICONTROL 既存を使用]**.

![The [!UICONTROL パッケージオブジェクトと依存関係] 依存オブジェクトオプションを表示するページ [!UICONTROL 新規作成] および [!UICONTROL 既存を使用].](../images/ui/sandbox-tooling/use-existing-object.png)

The **[!UICONTROL フィールドグループ]** ダイアログには、オブジェクトで使用可能なフィールドグループのリストが表示されます。 必要なフィールドグループを選択し、「 」を選択します。 **[!UICONTROL 保存]**.

![次に示すフィールドのリスト： [!UICONTROL フィールドグループ] ダイアログ、ハイライト表示 [!UICONTROL 保存] 選択。 ](../images/ui/sandbox-tooling/field-group-list.png)

次の場所に戻ります。 [!UICONTROL パッケージオブジェクトと依存関係] ページに貼り付けます。 ここからを選択します。 **[!UICONTROL 完了]** をクリックして、パッケージのインポートを完了します。

![The [!UICONTROL パッケージオブジェクトと依存関係] ページには、パッケージに含まれているアセットのリストがハイライト表示されます [!UICONTROL 完了].](../images/ui/sandbox-tooling/finish-object-dependencies.png)

<!--
## Export and import an entire sandbox 

>[!NOTE]
>
>All export and import actions are recorded in the audit logs.

### Export an entire sandbox {#export-entire-sandbox}

To export an entire sandbox, navigate to the [!UICONTROL Sandboxes] **[!UICONTROL Packages]** tab and select **[!UICONTROL Create package]**.

![The [!UICONTROL Sandboxes] **[!UICONTROL Packages]** tab highlighting [!UICONTROL Create package].](../images/ui/sandbox-tooling/create-sandbox-package.png)

Select **[!UICONTROL Entire sandbox]** for the Type of package in the [!UICONTROL Create package] dialog. Provide a [!UICONTROL Package name] for your package and select the **[!UICONTROL Sandbox]** from the dropdown. Finally, select **[!UICONTROL Create]** to confirm your entries.

![The [!UICONTROL Create package] dialog showing completed fields and highlighting [!UICONTROL Create].](../images/ui/sandbox-tooling/create-package-dialog.png)

The package is created successfully, select **[!UICONTROL Publish]** to publish the package.

![List of sandbox packages highlighting the new published package.](../images/ui/sandbox-tooling/publish-entire-sandbox-packages.png)

You are returned to the **[!UICONTROL Packages]** tab in the [!UICONTROL Sandboxes] environment, where you can see the new published package.

### Import the entire sandbox package {#import-entire-sandbox-package}

To import the package into a target sandbox, navigate to the [!UICONTROL Sandboxes] **[!UICONTROL Browse]** tab and select the plus (+) option beside the sandbox name.

![The sandboxes **[!UICONTROL Browse]** tab highlighting the import package selection.](../images/ui/sandbox-tooling/browse-entire-package-sandboxes.png)

Using the dropdown menu, select the full sandbox using the **[!UICONTROL Package name]** dropdown. Add an optional **[!UICONTROL Job name]**, which will be used for future monitoring, then select **[!UICONTROL Next]**.

![The import details page showing the [!UICONTROL Package name] dropdown selection](../images/ui/sandbox-tooling/import-full-sandbox-package.png)

>[!NOTE]
>
>All objects are created as new from the package when importing an entire sandbox. The objects are not listed in the [!UICONTROL Package object and dependencies] page, as there can be multiples. An inline message is displayed, advising of object types that are not supported.

You are taken to the [!UICONTROL Package object and dependencies] page where you can see the number of objects and dependencies that are imported and excluded objects. From here, select **[!UICONTROL Import]** to complete the package import.

 ![The [!UICONTROL Package object and dependencies] page shows the inline message of object types not supported, highlighting [!UICONTROL Import].](../images/ui/sandbox-tooling/finish-dependencies-entire-sandbox.png)
-->

## インポートジョブの監視とインポートオブジェクトの詳細の表示

インポートしたオブジェクトとインポートした詳細を表示するには、 [!UICONTROL サンドボックス] **[!UICONTROL インポート]** 「 」タブに移動し、リストからパッケージを選択します。 または、検索バーを使用してパッケージを検索します。

![サンドボックス [!UICONTROL インポート] 「 」タブでは、インポートパッケージの選択が強調表示されます。](../images/ui/sandbox-tooling/imports-tab.png)

### インポートしたオブジェクトの表示 {#view-imported-objects}

次の日： **[!UICONTROL インポート]** 」タブをクリックします。 [!UICONTROL サンドボックス] 環境、選択 **[!UICONTROL インポートしたオブジェクトの表示]** を右側の詳細ペインから選択します。

選択 **[!UICONTROL インポートしたオブジェクトの表示]** の右側の詳細ペインから **[!UICONTROL インポート]** 」タブをクリックします。 [!UICONTROL サンドボックス] 環境。

![サンドボックス [!UICONTROL インポート] 「 」タブにハイライトが表示されます [!UICONTROL インポートしたオブジェクトの表示] を選択します。](../images/ui/sandbox-tooling/view-imported-objects.png)

矢印を使用してオブジェクトを展開し、パッケージにインポートされたフィールドの完全なリストを表示します。

![サンドボックス [!UICONTROL 読み込まれたオブジェクト] パッケージにインポートされたオブジェクトのリストを表示します。](../images/ui/sandbox-tooling/expand-imported-objects.png)

### インポートの詳細を表示 {#view-import-details}

選択 **[!UICONTROL インポートの詳細を表示]** 」をクリックします。 **[!UICONTROL インポート]** 」タブを使用して、サンドボックス環境で使用できます。

![サンドボックス [!UICONTROL インポート] 「 」タブにハイライトが表示されます [!UICONTROL インポートの詳細を表示] を選択します。](../images/ui/sandbox-tooling/view-import-details.png)

The **[!UICONTROL インポートの詳細]** ダイアログには、インポートの詳細な分類が表示されます。

![The [!UICONTROL インポートの詳細] インポートの詳細な分類を示すダイアログ。](../images/ui/sandbox-tooling/import-details.png)

>[!NOTE]
>
>インポートが完了すると、Platform UI で通知を受け取ります。 これらの通知には、アラートアイコンからアクセスできます。 ジョブが失敗した場合は、ここからトラブルシューティングに移動できます。

## 次の手順

このドキュメントでは、Experience PlatformUI 内でサンドボックスツール機能を使用する方法について説明しました。 サンドボックスについて詳しくは、 [サンドボックスユーザーガイド](../ui/user-guide.md).

サンドボックス API を使用して様々な操作を実行する手順については、[サンドボックス開発者ガイド](../api/getting-started.md)を参照してください。Experience Platformのサンドボックスの概要については、 [概要ドキュメント](../home.md).
