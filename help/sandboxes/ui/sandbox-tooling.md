---
title: サンドボックスツール
description: サンドボックス間でサンドボックス設定をシームレスに書き出し、読み込みます。
exl-id: f1199ab7-11bf-43d9-ab86-15974687d182
source-git-commit: 1a474fa0947cb930bad95f94c1901fffb7e23e7b
workflow-type: tm+mt
source-wordcount: '2241'
ht-degree: 8%

---

# サンドボックスツール

>[!NOTE]
>
>サンドボックスツールは、の両方をサポートする基本機能です [!DNL Real-Time Customer Data Platform] および [!DNL Journey Optimizer] 開発サイクルの効率化及び構成精度の向上を図る。<br><br>サンドボックスツール機能を使用するには、次の 2 つの役割ベースのアクセス制御権限が必要です。<br>- `manage-sandbox` または `view-sandbox`<br>- `manage-package`

サンドボックス間の設定精度を向上させ、サンドボックスツール機能を使用して、サンドボックス間でサンドボックス設定をシームレスに書き出し、読み込みます。 サンドボックスツールを使用すると、実装プロセスの価値創出までの時間を短縮し、複数のサンドボックスをまたいで正常な設定を移行できます。

サンドボックスツール機能を使用して、様々なオブジェクトを選択し、それらをパッケージに書き出すことができます。 パッケージは、1 つのオブジェクトまたは複数のオブジェクトで構成できます。<!--or an entire sandbox.-->パッケージに含まれるオブジェクトは、同じサンドボックスからのオブジェクトである必要があります。

## サンドボックスツールでサポートされるオブジェクト {#supported-objects}

サンドボックスツール機能を使用すると、をエクスポートできます [!DNL Adobe Real-Time Customer Data Platform] および [!DNL Adobe Journey Optimizer] オブジェクトをパッケージに含めます。

### Real-time Customer Data Platform オブジェクト {#real-time-cdp-objects}

次の表に示します [!DNL Adobe Real-Time Customer Data Platform] サンドボックスツールで現在サポートされているオブジェクトは次のとおりです。

| Platform | オブジェクト | 詳細 |
| --- | --- | --- |
| 顧客データプラットフォーム | ソース | ソースアカウント資格情報は、セキュリティ上の理由から、ターゲットサンドボックスでレプリケートされず、手動で更新する必要があります。 デフォルトでは、ソースデータフローはドラフトステータスでコピーされます。 |
| 顧客データプラットフォーム | オーディエンス | のみ **[!UICONTROL 顧客オーディエンス]** タイプ **[!UICONTROL セグメント化サービス]** はサポートされています。 同意およびガバナンス用の既存のラベルは、同じインポートジョブでコピーされます。 システムは、結合ポリシーの依存関係を確認する際に、同じ XDM クラスを持つターゲットサンドボックスのデフォルトの結合ポリシーを自動的に選択します。 |
| 顧客データプラットフォーム | ID | ターゲットサンドボックスでを作成する場合、Adobeの標準 ID 名前空間の重複が自動的に排除されます。 オーディエンスは、オーディエンスルールのすべての属性が結合スキーマで有効になっている場合にのみコピーできます。 最初に、必要なスキーマを移動し、統合プロファイルを有効にする必要があります。 |
| 顧客データプラットフォーム | スキーマ | 同意およびガバナンス用の既存のラベルは、同じインポートジョブでコピーされます。 「統合プロファイル」オプションが有効になっていないスキーマを柔軟にインポートできます。 スキーマ関係のエッジケースはパッケージには含まれていません。 |
| 顧客データプラットフォーム | データセット | データセットは、統合プロファイル設定がデフォルトで無効の状態でコピーされます。 |
| 顧客データプラットフォーム | 同意およびガバナンスポリシー | ユーザーが作成したカスタムポリシーをパッケージに追加し、サンドボックス間で移動します。 |

次のオブジェクトが読み込まれますが、ステータスがドラフトまたは無効です。

| 機能 | オブジェクト | ステータス |
| --- | --- | --- |
| インポートステータス | ソースデータフロー | ドラフト |
| インポートステータス | ジャーニー | ドラフト |
| 統合プロファイル | データセット | 統合プロファイルが無効 |
| ポリシー | データガバナンスポリシー | 無効 |

### Adobe Journey Optimizer オブジェクト {#abobe-journey-optimizer-objects}

次の表に示します [!DNL Adobe Journey Optimizer] サンドボックスツールおよび制限事項で現在サポートされているオブジェクトは次のとおりです。

| Platform | オブジェクト | 詳細 |
| --- | --- | --- |
| [!DNL Adobe Journey Optimizer] | オーディエンス | オーディエンスは、ジャーニーオブジェクトの依存オブジェクトとしてコピーできます。 ターゲットサンドボックスで、「新しいオーディエンスを作成」を選択するか、既存のオーディエンスを再利用できます。 |
| [!DNL Adobe Journey Optimizer] | スキーマ | ジャーニーで使用されるスキーマは、依存オブジェクトとしてコピーできます。 ターゲットサンドボックスで「新しいスキーマを作成」を選択するか、既存のスキーマを再利用できます。 |
| [!DNL Adobe Journey Optimizer] | 結合ポリシー | ジャーニーで使用される結合ポリシーは、依存オブジェクトとしてコピーできます。 ターゲットサンドボックスでは、次の操作を行います **できません** 新しい結合ポリシーを作成します。既に存在する結合ポリシーのみを利用できます。 |
| [!DNL Adobe Journey Optimizer] | ジャーニー - キャンバスの詳細 | キャンバス上のジャーニーの表示域には、条件、アクション、イベント、オーディエンスを読み取りなど、コピーされたジャーニー内のオブジェクトが含まれます。 ジャンプアクティビティは、コピーから除外されます。 |
| [!DNL Adobe Journey Optimizer] | イベント | ジャーニーで使用されるイベントとイベントの詳細がコピーされます。 ターゲットサンドボックスに新しいバージョンが常に作成されます。 |
| [!DNL Adobe Journey Optimizer] | アクション | ジャーニーで使用されるメールおよびプッシュメッセージは、依存オブジェクトとしてコピーできます。 ジャーニーフィールドで使用されるチャネルアクションアクティビティ（メッセージ内のパーソナライゼーションに使用される）が完全かどうかはチェックされません。 コンテンツブロックはコピーされません。<br><br>ジャーニーで使用される「プロファイルを更新」アクションをコピーできます。 ジャーニーで使用されるカスタムアクションとアクションの詳細もコピーされます。 ターゲットサンドボックスに新しいバージョンが常に作成されます。 |

サーフェス（プリセットなど）はコピーされません。 メッセージタイプとサーフェス名に基づいて、最も近いものが宛先サンドボックスで自動的に選択されます。 ターゲットのサンドボックスでサーフェスが見つからない場合、サーフェスのコピーが失敗します。その結果、メッセージを設定するためにサーフェスを使用する必要があるので、メッセージのコピーが失敗します。 この場合、コピーを機能させるには、メッセージの適切なチャネル用に少なくとも 1 つのサーフェスを作成する必要があります。

カスタム ID タイプは、ジャーニーを書き出す際に依存オブジェクトとしてサポートされません。

## パッケージへのオブジェクトの書き出し {#export-objects}

>[!NOTE]
>
>すべてのエクスポートアクションが監査ログに記録されます。

>[!CONTEXTUALHELP]
>id="platform_sandbox_tooling_remove_object"
>title="オブジェクトを削除"
>abstract="パッケージからオブジェクトを削除するには、削除する行を選択し、選択時に使用可能になる削除オプションを使用します。公開済みパッケージからオブジェクトを削除することはできません。"

>[!CONTEXTUALHELP]
>id="platform_sandbox_package_expiry"
>title="パッケージの有効期限設定"
>abstract="パッケージは、ドラフトステータスで非アクティブな状態が続くと、有効期限が切れるように設定されます。デフォルトの日付は今日から 90 日に設定されています。この日付は、パッケージが公開されるまで変更され続けます。翌日ドラフトステータスのパッケージにアクセスすると、手動で設定しない限り、日付は 1 日後に移動します。"

>[!CONTEXTUALHELP]
>id="platform_sandbox_tooling_package_status"
>title="パッケージステータス"
>abstract="デフォルトでは、ステータスはドラフトに設定されています。パッケージが公開されると、ステータスが「公開済み」に変わります。パッケージが公開された後は、変更することはできません。"

>[!NOTE]
>
>オブジェクトにアクセスする権限がある場合にのみ、パッケージを読み込むことができます。

この例では、スキーマを書き出してパッケージに追加するプロセスを説明します。 データセットやジャーニーなど、他のオブジェクトも同じ手順で書き出すことができます。

### 新しいパッケージにオブジェクトを追加 {#add-object-to-new-package}

を選択 **[!UICONTROL スキーマ]** 左側のナビゲーションからを選択し、 **[!UICONTROL 参照]** タブに使用可能なスキーマが表示されます。 次に、省略記号（`...`）が選択され、ドロップダウンにコントロールが表示されます。 を選択 **[!UICONTROL パッケージに追加]** ドロップダウンから。

![をハイライト表示したドロップダウンメニューを示すスキーマのリスト [!UICONTROL パッケージに追加] 制御。](../images/ui/sandbox-tooling/add-to-package.png)

から **[!UICONTROL パッケージに追加]** ダイアログで、 **[!UICONTROL 新規パッケージを作成]** オプション。 を指定 [!UICONTROL 名前] （パッケージ用）とオプション [!UICONTROL 説明]を選択してから、 **[!UICONTROL 追加]**.

![この [!UICONTROL パッケージに追加] ～との対話 [!UICONTROL 新規パッケージを作成] 選択およびハイライト表示 [!UICONTROL 追加].](../images/ui/sandbox-tooling/create-new-package.png)

「」に戻ります **[!UICONTROL スキーマ]** 環境。 次の手順に従って、作成したパッケージにオブジェクトを追加できるようになりました。

### 既存のパッケージにオブジェクトを追加して公開 {#add-object-to-existing-package}

使用可能なスキーマのリストを表示するには、以下を選択します **[!UICONTROL スキーマ]** 左側のナビゲーションからを選択し、 **[!UICONTROL 参照]** タブ。 次に、省略記号（`...`）を選択し、ドロップダウンメニューにコントロールオプションを表示します。 を選択 **[!UICONTROL パッケージに追加]** ドロップダウンから。

![をハイライト表示したドロップダウンメニューを示すスキーマのリスト [!UICONTROL パッケージに追加] 制御。](../images/ui/sandbox-tooling/add-to-package.png)

この **[!UICONTROL パッケージに追加]** ダイアログが表示されます。 「」を選択します **[!UICONTROL 既存のパッケージ]** オプションを選択してから、 **[!UICONTROL パッケージ名]** 必要なパッケージをドロップダウンから選択します。 最後に、を選択します **[!UICONTROL 追加]** をクリックして選択を確定します。

![[!UICONTROL パッケージに追加] ダイアログにドロップダウンから選択したパッケージが表示されます。](../images/ui/sandbox-tooling/add-to-existing-package.png)

パッケージに追加されたオブジェクトのリストが表示されます。 パッケージを公開し、サンドボックスに読み込めるようにするには、次を選択します。 **[!UICONTROL 公開]**.

![パッケージ内のオブジェクトのリスト。ハイライト表示は [!UICONTROL 公開] オプション。](../images/ui/sandbox-tooling/publish-package.png)

を選択 **[!UICONTROL 公開]** パッケージの公開を確認します。

![パッケージを公開する確認ダイアログ、ハイライト表示 [!UICONTROL 公開] オプション。](../images/ui/sandbox-tooling/publish-package-confirmation.png)

>[!NOTE]
>
>公開すると、パッケージのコンテンツを変更できなくなります。 互換性の問題を回避するには、必要なアセットがすべて選択されていることを確認します。 変更が必要な場合は、新規パッケージを作成する必要があります。

「」に戻ります **[!UICONTROL パッケージ]** タブ [!UICONTROL サンドボックス] 環境（新しく公開されたパッケージを確認できます）。

![新しく公開されたパッケージを強調表示したサンドボックスパッケージのリスト。](../images/ui/sandbox-tooling/published-packages.png)

## ターゲットサンドボックスへのパッケージの読み込み {#import-package-to-target-sandbox}

>[!NOTE]
>
>すべてのインポートアクションは監査ログに記録されます。

パッケージをターゲットサンドボックスに読み込むには、サンドボックスに移動します **[!UICONTROL 参照]** tab キーを押して、サンドボックス名の横にあるプラス（+） オプションを選択します。

![サンドボックス **[!UICONTROL 参照]** 「パッケージをインポート」の選択を強調表示するタブ。](../images/ui/sandbox-tooling/browse-sandboxes.png)

ドロップダウンメニューを使用して、 **[!UICONTROL パッケージ名]** ターゲットサンドボックスに読み込む。 オプションを追加 **[!UICONTROL ジョブ名]**（今後の監視に使用されます）。 デフォルトでは、パッケージのスキーマが読み込まれると、統合プロファイルは無効になります。 切り替え **プロファイルでスキーマを有効にする** これを有効にするには、を選択します **[!UICONTROL 次]**.

![を表示する読み込みの詳細ページ [!UICONTROL パッケージ名] ドロップダウン選択](../images/ui/sandbox-tooling/import-package-to-sandbox.png)

この [!UICONTROL パッケージオブジェクトと依存関係] このページには、このパッケージに含まれるすべてのアセットのリストが表示されます。 選択した親オブジェクトのインポートに必要な依存オブジェクトが自動的に検出されます。 見つからない属性は、ページの上部に表示されます。 を選択 **[!UICONTROL 詳細を表示]** を参照してください。

![この [!UICONTROL パッケージオブジェクトと依存関係] ページに、属性が見つからない問題が表示される。](../images/ui/sandbox-tooling/missing-attributes.png)

>[!NOTE]
>
>依存オブジェクトは、ターゲットサンドボックス内の既存のオブジェクトに置き換えることができます。これにより、新しいバージョンを作成するのではなく、既存のオブジェクトを再利用できます。 例えば、スキーマを含むパッケージを読み込む場合、既存のカスタムフィールドグループと ID 名前空間をターゲットサンドボックスで再利用できます。 または、ジャーニーを含むパッケージを読み込む際に、ターゲットサンドボックス内の既存のセグメントを再利用できます。

既存のオブジェクトを使用するには、依存オブジェクトの横にある鉛筆アイコンを選択します。

![この [!UICONTROL パッケージオブジェクトと依存関係] このページには、パッケージに含まれるアセットのリストが表示されます。](../images/ui/sandbox-tooling/package-objects-and-dependencies.png)

新規作成または既存のものを使用するためのオプションが表示されます。 を選択 **[!UICONTROL 既存のものを使用]**.

![この [!UICONTROL パッケージオブジェクトと依存関係] 依存オブジェクトのオプションを示すページ [!UICONTROL 新規作成] および [!UICONTROL 既存のものを使用].](../images/ui/sandbox-tooling/use-existing-object.png)

この **[!UICONTROL フィールドグループ]** ダイアログには、オブジェクトで使用可能なフィールドグループのリストが表示されます。 必要なフィールドグループを選択し、次に選択します **[!UICONTROL 保存]**.

![に表示されるフィールドのリスト [!UICONTROL フィールドグループ] ダイアログ、ハイライト表示 [!UICONTROL 保存] 選択。 ](../images/ui/sandbox-tooling/field-group-list.png)

「」に戻ります [!UICONTROL パッケージオブジェクトと依存関係] ページ。 ここから選択 **[!UICONTROL 終了]** パッケージの読み込みを完了します。

![この [!UICONTROL パッケージオブジェクトと依存関係] このページには、パッケージに含まれるアセットのリストが強調表示されます [!UICONTROL 終了].](../images/ui/sandbox-tooling/finish-object-dependencies.png)

## サンドボックス全体の書き出しと読み込み

>[!NOTE]
>
>完全なサンドボックスの書き出し/読み込みでは、リアルタイム顧客データプラットフォームオブジェクトのみがサポートされます。 ジャーニーオブジェクトは含まれません。

### サンドボックス全体の書き出し {#export-entire-sandbox}

サンドボックス全体を書き出すには、に移動します [!UICONTROL サンドボックス] **[!UICONTROL パッケージ]** tab キーを押して選択 **[!UICONTROL パッケージを作成]**.

![この [!UICONTROL サンドボックス] **[!UICONTROL パッケージ]** タブのハイライト [!UICONTROL パッケージを作成].](../images/ui/sandbox-tooling/create-sandbox-package.png)

を選択 **[!UICONTROL サンドボックス全体]** の場合 [!UICONTROL パッケージのタイプ] が含まれる [!UICONTROL パッケージを作成] ダイアログ。 を指定 [!UICONTROL パッケージ名] 新しいパッケージのを選択し、 **[!UICONTROL Sandbox]** ドロップダウンから。 最後に、を選択します **[!UICONTROL 作成]** をクリックして入力を確認します。

![この [!UICONTROL パッケージを作成] 完了したフィールドとハイライト表示を示すダイアログ [!UICONTROL 作成].](../images/ui/sandbox-tooling/create-package-dialog.png)

パッケージが正常に作成されたら、を選択します。 **[!UICONTROL 公開]** をクリックしてパッケージを公開します。

![新しく公開されたパッケージを強調表示したサンドボックスパッケージのリスト。](../images/ui/sandbox-tooling/publish-entire-sandbox-packages.png)

「」に戻ります **[!UICONTROL パッケージ]** タブ [!UICONTROL サンドボックス] 環境（新しく公開されたパッケージを確認できます）。

### サンドボックスパッケージ全体を読み込みます {#import-entire-sandbox-package}

>[!NOTE]
>
>すべてのオブジェクトが、新しいオブジェクトとしてターゲットサンドボックスに読み込まれます。 完全なサンドボックスパッケージを空のサンドボックスに読み込むことをお勧めします。

パッケージをターゲットサンドボックスに読み込むには、に移動します [!UICONTROL サンドボックス] **[!UICONTROL 参照]** tab キーを押して、サンドボックス名の横にあるプラス（+） オプションを選択します。

![サンドボックス **[!UICONTROL 参照]** 「パッケージをインポート」の選択を強調表示するタブ。](../images/ui/sandbox-tooling/browse-entire-package-sandboxes.png)

ドロップダウンメニューを使用し、を使用して完全なサンドボックスを選択します **[!UICONTROL パッケージ名]** ドロップダウン。 を追加 **[!UICONTROL ジョブ名]**（将来の監視に使用される）。オプション **[!UICONTROL ジョブ説明]**&#x200B;を選択してから、 **[!UICONTROL 次]**.

![を表示する読み込みの詳細ページ [!UICONTROL パッケージ名] ドロップダウン選択](../images/ui/sandbox-tooling/import-full-sandbox-package.png)

>[!NOTE]
>
>パッケージに含まれるすべてのオブジェクトに対する完全な権限が必要です。 権限がない場合、読み込み操作は失敗し、エラーメッセージが表示されます。

に移動しました [!UICONTROL パッケージオブジェクトと依存関係] インポートおよび除外されたオブジェクトの数と依存関係を確認できるページ。 ここから選択 **[!UICONTROL インポート]** パッケージの読み込みを完了します。

![この [!UICONTROL パッケージオブジェクトと依存関係] ページに、サポートされていないオブジェクトタイプのインラインメッセージが表示され、ハイライト表示される [!UICONTROL インポート].](../images/ui/sandbox-tooling/finish-dependencies-entire-sandbox.png)

読み込みが完了するまでしばらく待ちます。 完了までの時間は、パッケージ内のオブジェクトの数によって異なる場合があります。 読み込みジョブは、 [!UICONTROL サンドボックス] **[!UICONTROL ジョブ]** タブ。

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

## 読み込みの詳細を監視 {#view-import-details}

読み込んだ詳細を表示するには、に移動します [!UICONTROL サンドボックス] **[!UICONTROL ジョブ]** tab キーを押して、リストからパッケージを選択します。 または、検索バーを使用してパッケージを検索します。

![サンドボックス [!UICONTROL ジョブ] タブで「パッケージをインポート」選択をハイライト表示します。](../images/ui/sandbox-tooling/imports-tab.png)

<!--### View imported objects {#view-imported-objects}

On the **[!UICONTROL Jobs]** tab in the [!UICONTROL Sandboxes] environment, select **[!UICONTROL View imported objects]** from the right details pane.

Select **[!UICONTROL View imported objects]** from the right details pane on the **[!UICONTROL Jobs]** tab in the [!UICONTROL Sandboxes] environment.

![The sandboxes [!UICONTROL Imports] tab highlights the [!UICONTROL View imported objects] selection in the right pane.](../images/ui/sandbox-tooling/view-imported-objects.png)

Use the arrows to expand objects to view the full list of fields that have been imported into the package.

![The sandboxes [!UICONTROL Imported objects] showing a list of objects imported into the package.](../images/ui/sandbox-tooling/expand-imported-objects.png)-->

を選択 **[!UICONTROL 読み込みの概要を表示]** の右側の詳細パネルから **[!UICONTROL ジョブ]** サンドボックス環境で「」タブをクリックします。

![サンドボックス [!UICONTROL インポート] タブは、 [!UICONTROL 読み込みの詳細を表示] 右側のペインで選択します。](../images/ui/sandbox-tooling/view-import-details.png)

この **[!UICONTROL 読み込みの概要]** ダイアログには、読み込みの分類が表示され、進行状況がパーセントで示されます。

>[!NOTE]
>
>特定のインベントリページに移動すると、オブジェクトのリストを表示できます。

![この [!UICONTROL 詳細をインポート] インポートの詳細な分類を表示するダイアログ。](../images/ui/sandbox-tooling/import-details.png)

読み込みが完了すると、Platform UI に通知が届きます。 これらの通知には、アラート アイコンからアクセスできます。 ジョブが失敗した場合は、ここからトラブルシューティングに移動できます。

## ビデオチュートリアル

次のビデオは、サンドボックスツールに関する理解を深めるために、新しいパッケージの作成、パッケージの公開、パッケージの読み込み方法の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/3424763/?learn=on)

## 次の手順

このドキュメントでは、Experience PlatformUI 内でサンドボックスツール機能を使用する方法について説明しました。 サンドボックスについて詳しくは、 [サンドボックスユーザーガイド](../ui/user-guide.md).

サンドボックス API を使用して様々な操作を実行する手順については、[サンドボックス開発者ガイド](../api/getting-started.md)を参照してください。Experience Platformのサンドボックスの概要については、を参照してください。 [概要ドキュメント](../home.md).
