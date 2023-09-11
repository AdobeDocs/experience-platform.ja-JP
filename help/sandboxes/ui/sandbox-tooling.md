---
title: サンドボックスツール
description: サンドボックス間でのサンドボックス設定のシームレスな書き出しと読み込みをおこないます。
source-git-commit: 900cb35f6cb758f145904666c709c60dc760eff2
workflow-type: tm+mt
source-wordcount: '1619'
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

サンドボックスツール機能を使用すると、異なるオブジェクトを選択してパッケージにエクスポートできます。 パッケージは、1 つのオブジェクト、複数のオブジェクト、またはサンドボックス全体で構成できます。 パッケージに含まれるオブジェクトは、同じサンドボックスからのものである必要があります。

## サンドボックスツールでサポートされるオブジェクト {#supported-objects}

次の表に、サンドボックスツールで現在サポートされているオブジェクトを示します。

| Platform | オブジェクト |
| --- | --- |
| [!DNL Adobe Journey Optimizer] | ジャーニー |
| 顧客データプラットフォーム | ソース |
| 顧客データプラットフォーム | セグメント |
| 顧客データプラットフォーム | ID |
| 顧客データプラットフォーム | ポリシー |
| 顧客データプラットフォーム | スキーマ |
| 顧客データプラットフォーム | データセット |

次のオブジェクトはインポートされますが、ドラフトまたは無効のステータスです：

| 機能 | オブジェクト | ステータス |
| --- | --- | --- |
| インポートステータス | ソースのデータフロー | ドラフト |
| インポートステータス | ジャーニー | ドラフト |
| 統合プロファイル | スキーマ | 無効 |
| 統合プロファイル | データセット | 無効 |
| ポリシー | 同意ポリシー | 無効 |
| ポリシー | データガバナンスポリシー | 無効 |

以下に示すエッジケースは、パッケージには含まれていません。

* スキーマの関係

## パッケージへのオブジェクトのエクスポート {#export-objects}

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

## サンドボックス全体の書き出しと読み込み

### サンドボックス全体を書き出す {#export-entire-sandbox}

サンドボックス全体を書き出すには、 [!UICONTROL サンドボックス] **[!UICONTROL パッケージ]** 「 」タブで「 」を選択します。 **[!UICONTROL パッケージを作成]**.

![The [!UICONTROL サンドボックス] **[!UICONTROL パッケージ]** タブのハイライト [!UICONTROL パッケージを作成].](../images/ui/sandbox-tooling/create-sandbox-package.png)

選択 **[!UICONTROL サンドボックス全体]** ( [!UICONTROL パッケージを作成] ダイアログ。 次を提供： [!UICONTROL パッケージ名] を選択し、 **[!UICONTROL サンドボックス]** をドロップダウンから選択します。 最後に、 **[!UICONTROL 作成]** をクリックして、入力内容を確認します。

![The [!UICONTROL パッケージを作成] 入力済みフィールドとハイライト表示を示すダイアログ [!UICONTROL 作成].](../images/ui/sandbox-tooling/create-package-dialog.png)

パッケージが正常に作成されました。「 」を選択します。 **[!UICONTROL 公開]** をクリックして、パッケージを公開します。

![新しく公開されたパッケージをハイライトするサンドボックスパッケージのリストです。](../images/ui/sandbox-tooling/publish-entire-sandbox-packages.png)

次の場所に戻ります。 **[!UICONTROL パッケージ]** 」タブをクリックします。 [!UICONTROL サンドボックス] 環境で、新しくパブリッシュされたパッケージを表示できます。

### サンドボックスパッケージ全体をインポート {#import-entire-sandbox-package}

パッケージをターゲットサンドボックスに読み込むには、 [!UICONTROL サンドボックス] **[!UICONTROL 参照]** 「 」タブをクリックし、サンドボックス名の横にあるプラス (+) オプションを選択します。

![サンドボックス **[!UICONTROL 参照]** 「 」タブで、インポートパッケージの選択をハイライト表示します。](../images/ui/sandbox-tooling/browse-entire-package-sandboxes.png)

ドロップダウンメニューを使用し、 **[!UICONTROL パッケージ名]** ドロップダウン。 オプションの追加 **[!UICONTROL ジョブ名]**（将来の監視に使用）を選択し、 **[!UICONTROL 次へ]**.

![インポートの詳細ページに、 [!UICONTROL パッケージ名] ドロップダウン選択](../images/ui/sandbox-tooling/import-full-sandbox-package.png)

>[!NOTE]
>
>すべてのオブジェクトは、サンドボックス全体をインポートする際に、パッケージから新規として作成されます。 オブジェクトは [!UICONTROL パッケージオブジェクトと依存関係] ページに配置します。複数の値を指定できます。 インラインメッセージが表示され、サポートされていないオブジェクトタイプについてのアドバイスが示されます。

次の場所に移動します。 [!UICONTROL パッケージオブジェクトと依存関係] 読み込まれたオブジェクトと除外されたオブジェクトの数を確認できるページ。 ここからを選択します。 **[!UICONTROL インポート]** をクリックして、パッケージのインポートを完了します。

![The [!UICONTROL パッケージオブジェクトと依存関係] ページには、サポートされていないオブジェクトタイプのインラインメッセージが表示され、ハイライト表示されます [!UICONTROL インポート].](../images/ui/sandbox-tooling/finish-dependencies-entire-sandbox.png)

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
