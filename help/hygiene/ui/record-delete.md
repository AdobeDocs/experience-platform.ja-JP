---
title: レコードを削除
description: Adobe Experience Platform UI でレコードを削除する方法を説明します。
hide: true
hidefromtoc: true
source-git-commit: ccb2236fa169c26ef2f75d26776eee9f0122e92a
workflow-type: tm+mt
source-wordcount: '1493'
ht-degree: 35%

---

# [!BADGE ベータ版]{type=Informative} レコードの削除 {#record-delete}

以下を使用します。 [[!UICONTROL データのライフサイクル] workspace](./overview.md) をクリックし、プライマリ id に基づいてAdobe Experience Platform内のレコードを削除します。 これらのレコードは、個々のコンシューマーや、ID グラフに含まれる他のエンティティに結び付けることができます。

>[!IMPORTANT]
> 
>レコードの削除機能は、現在ベータ版で、 **限定リリース**. すべてのお客様が利用できるわけではありません。 レコードの削除リクエストは、限定リリースの組織でのみ使用できます。
> 
> 
>レコードの削除は、データのクレンジング、匿名データの削除、またはデータの最小化に使用するためのものです。 これらは、EU 一般データ保護規則（GDPR）などのプライバシー規制に関するデータサブジェクト権利リクエスト（コンプライアンス）に対して使用するためのものでは&#x200B;**ありません**。コンプライアンスに関するユースケースについて詳しくは、[Adobe Experience Platform Privacy Service](../../privacy-service/home.md) を参照してください。

## 前提条件 {#prerequisites}

レコードを削除するには、ID フィールドがExperience Platformでどのように機能するかに関する十分な知識が必要です。 特に、レコードを削除するエンティティのプライマリ ID 値は、削除元のデータセット（複数可）に応じて、そのエンティティの主 ID 値を把握しておく必要があります。

Platform の ID について詳しくは、次のドキュメントを参照してください。

* [Adobe Experience Platform ID サービス](../../identity-service/home.md)：デバイスやシステム間で ID をブリッジし、準拠する XDM スキーマで定義された ID フィールドに基づいてデータセットをリンクします。
* [ID 名前空間](../../identity-service/namespaces.md)：ID 名前空間は、1 人の人物に関連している可能性のある様々なタイプの ID 情報を定義する、各 ID フィールドに必須のコンポーネントです。
* [リアルタイム顧客プロファイル](../../profile/home.md):ID グラフを使用して、ほぼリアルタイムで更新された、複数のソースからの集計データに基づいて統合された消費者プロファイルを提供します。
* [エクスペリエンスデータモデル（XDM）](../../xdm/home.md)：スキーマの使用により、Platform データの標準的な定義および構造を提供します。すべての Platform データセットは特定の XDM スキーマに準拠しており、スキーマはどのフィールドが ID であるかを定義しています。
* [ID フィールド](../../xdm/ui/fields/identity.md)：XDM スキーマで ID フィールドが定義される方法を説明します。

## リクエストの作成 {#create-request}

プロセスを開始するには、「 **[!UICONTROL データのライフサイクル]** （Platform UI の左側のナビゲーション）を使用します。 The [!UICONTROL データライフサイクルリクエスト] ワークスペースが表示されます。 次に、「 **[!UICONTROL リクエストを作成]** をワークスペースのメインページから削除します。

![The [!UICONTROL データライフサイクルリクエスト] ワークスペース [!UICONTROL リクエストを作成] 選択済み](../images/ui/record-delete/create-request-button.png)

リクエスト作成ワークフローが表示されます。 デフォルトでは、 **[!UICONTROL レコードを削除]** オプションが選択されている場合は、 **[!UICONTROL 要求されたアクション]** 」セクションに入力します。 このオプションを選択されたままにします。

>[!IMPORTANT]
> 
>効率を改善し、データセット操作のコストを抑えるための継続的な変更の一環として、デルタ形式に移動された組織は、ID サービス、リアルタイム顧客プロファイル、データレイクからデータを削除できます。 このタイプのユーザーは、差分移行済みと呼ばれます。 差分移行された組織のユーザーは、1 つのデータセットまたはすべてのデータセットからレコードを削除することができます。 差分移行されていない組織のユーザーは、以下の図に示すように、1 つのデータセットまたはすべてのデータセットからレコードを削除することは選択できません。 この場合は、 [ID を提供](#provide-identities) 」の節を参照してください。

![リクエスト作成ワークフローで、 [!UICONTROL レコードを削除] オプションを選択し、ハイライト表示します。](../images/ui/record-delete/delete-record.png)

## データセットの選択 {#select-dataset}

次の手順では、1 つのデータセットからレコードを削除するか、すべてのデータセットからレコードを削除するかを決定します。 このオプションが利用できない場合は、 [ID を提供](#provide-identities) 」の節を参照してください。

の下 **[!UICONTROL レコードの詳細]** 「 」セクションでは、ラジオボタンを使用して、特定のデータセットとすべてのデータセットの中から選択します。 次を選択した場合： **[!UICONTROL データセットを選択]**&#x200B;をクリックし、データベースアイコン (![データベースアイコン](../images/ui/record-delete/database-icon.png)) をクリックして、使用可能なデータセットのリストを提供するダイアログを開きます。 リストから目的のデータセットを選択し、その後に **[!UICONTROL 完了]**.

![The [!UICONTROL データセットを選択] データセットを選択したダイアログと [!UICONTROL 完了] ハイライト表示されました。](../images/ui/record-delete/select-dataset.png)

すべてのデータセットからレコードを削除する場合は、「 」を選択します。 **[!UICONTROL すべてのデータセット]**.

![The [!UICONTROL データセットを選択] ～との対話 [!UICONTROL すべてのデータセット] オプションが選択されています。](../images/ui/record-delete/all-datasets.png)

>[!NOTE]
>
>「**[!UICONTROL すべてのデータセット]**」オプションを選択すると、削除操作により時間がかかり、正確なレコード削除にならない可能性があります。

## ID の提供 {#provide-identities}

>[!CONTEXTUALHELP]
>id="platform_hygiene_primaryidentity"
>title="プライマリ ID"
>abstract="プライマリ ID は、Experience Platform でレコードを消費者のプロファイルに結び付ける属性です。データセットのプライマリ ID フィールドは、そのデータセットが基づいているスキーマによって定義されます。この列では、レコードのプライマリ ID のタイプ (または名前空間) を指定する必要があります (メールアドレスの場合は `email`、Experience Cloud ID の場合は `ecid` など)。詳しくは、データハイジーン UI ガイドを参照してください。"

>[!CONTEXTUALHELP]
>id="platform_hygiene_identityvalue"
>title="ID 値"
>abstract="この列では、レコードのプライマリ ID の値を指定する必要があります (左の列で指定した ID タイプに対応している必要があります)。プライマリ ID タイプが `email` の場合、値はレコードのメールアドレスである必要があります。詳しくは、データハイジーン UI ガイドを参照してください。"

レコードを削除する場合は、削除するレコードをシステムが決定できるように、ID 情報を指定する必要があります。 Platform のデータセットの場合、レコードは、データセットのスキーマによって定義された「**プライマリ ID**」フィールドに基づいて削除されます。

Platform のすべての ID フィールドと同様に、プライマリ ID は、**タイプ**（ID 名前空間とも呼ばれる）と&#x200B;**値**&#x200B;の 2 つで構成されます。ID タイプは、フィールドがレコード（E メールアドレスなど）を識別するコンテキストを提供し、値は、そのタイプのレコード固有の ID( 例： `jdoe@example.com` （の） `email` id タイプ )。 ID として使用される共通のフィールドには、アカウント情報、デバイス ID および Cookie ID が含まれます。

>[!TIP]
>
>特定のデータセットのプライマリ ID がわからない場合は、Platform UI で見つけることができます。**[!UICONTROL データセット]**&#x200B;ワークスペースで、リストから問題のデータセットを選択します。データセットの詳細ページの右側のパネルで、データセットのスキーマの名前の上にマウスポインターを置きます。プライマリ ID が、スキーマ名および説明と共に表示されます。
>
>![データセットが選択された「データセット」ダッシュボードと、データセットの詳細パネルから開いたスキーマダイアログ。 データセットのプライマリ ID がハイライト表示されています。](../images/ui/record-delete/dataset-primary-identity.png)

1 つのデータセットからレコードを削除する場合、データセットには 1 つのプライマリ ID しか割り当てられないので、指定する ID すべてに同じタイプが必要です。 すべてのデータセットから削除している場合、データセットが異なるとプライマリ ID が異なる可能性があるので、複数の ID タイプを含めることができます。

レコードを削除する際に ID を指定する方法は 2 つあります。

* [JSON ファイルのアップロード](#upload-json)
* [ID 値を手動で入力](#manual-identity)

### JSON ファイルのアップロード {#upload-json}

JSON ファイルをアップロードするには、指定された領域にファイルをドラッグ&amp;ドロップするか、「 **[!UICONTROL ファイルを選択]** をクリックし、ローカルディレクトリからを参照して選択します。

![ハイライト表示されている JSON ファイルをアップロードするためのファイルを選択してドラッグ&amp;ドロップインターフェイスを使用したリクエスト作成ワークフロー。](../images/ui/record-delete/upload-json.png)

JSON ファイルは、オブジェクトの配列（ID を表す各オブジェクト）として形式設定する必要があります。

```json
[
  {
    "namespaceCode": "email",
    "value": "jdoe@example.com"
  },
  {
    "namespaceCode": "email",
    "value": "san.gray@example.com"
  }
]
```

| プロパティ | 説明 |
| --- | --- |
| `namespaceCode` | ID タイプ。 |
| `value` | タイプで示される ID 値。 |

ファイルがアップロードされると、引き続き[リクエストを送信](#submit)できます。

### ID を手動で入力 {#manual-identity}

ID を手動で入力するには、「**[!UICONTROL ID を追加]**」を選択します。

![リクエスト作成ワークフローで、 [!UICONTROL ID を追加] オプションがハイライト表示されました。](../images/ui/record-delete/add-identity.png)

ID を 1 つずつ入力できるコントロールが表示されます。 「**[!UICONTROL プライマリ ID]**」で、ドロップダウンメニューを使用して ID タイプを選択します。の下 **[!UICONTROL ID 値]**&#x200B;に、レコードのプライマリ ID 値を指定します。

![ID フィールドを手動で追加したリクエスト作成ワークフロー。](../images/ui/record-delete/identity-added.png)

ID を追加するには、プラスアイコン (![プラスアイコン。](../images/ui/record-delete/plus-icon.png)) をクリックするか、「 **[!UICONTROL ID を追加]**.

![プラスアイコンと ID を追加アイコンがハイライトされたリクエスト作成ワークフロー。](../images/ui/record-delete/more-identities.png)

## リクエストの送信（#submit）

リクエストへの ID の追加が完了したら、「**[!UICONTROL 送信]**」を選択する前に&#x200B;**[!UICONTROL リクエスト設定]**&#x200B;でリクエストの名前とオプションの説明を入力します。

>[!IMPORTANT]
> 
>毎月送信できる個別 ID レコードの削除の合計数には、異なる制限があります。 これらの制限は、使用許諾契約に基づいています。 Adobe Real-time Customer Data PlatformとAdobe Journey Optimizerのすべてのエディションを購入した組織は、毎月最大 100,000 個の ID レコード削除を送信できます。 を購入した組織 **Adobeヘルスケアシールド** または **Adobeプライバシーとセキュリティシールド** は、毎月最大 600,000 個の id レコード削除を送信できます。

![リクエスト設定の [!UICONTROL 名前] および [!UICONTROL 説明] 次のフィールド [!UICONTROL 送信] ハイライト表示されました。](../images/ui/record-delete/submit.png)

A [!UICONTROL リクエストを確認] ダイアログが表示され、削除した id は復元できないことを示します。 選択 **[!UICONTROL 送信]** をクリックして、削除するデータの id のリストを確認します。

![The [!UICONTROL リクエストを確認] ダイアログ。](../images/ui/record-delete/confirm-request.png)

リクエストが送信されると、作業指示が作成され、 [!UICONTROL レコード] タブ [!UICONTROL データのライフサイクル] ワークスペース。 ここから、リクエストを処理する作業指示のステータスを監視できます。

>[!NOTE]
>
>概要に関する節 ( [タイムラインと透明性](../home.md#record-delete-transparency) を参照してください。

![The [!UICONTROL レコード] タブ [!UICONTROL データのライフサイクル] 新しいリクエストが強調表示されたワークスペース。](../images/ui/record-delete/request-log.png)

## 次の手順

このドキュメントでは、Experience PlatformUI でのレコードの削除方法を説明しました。 UI で他のデータライフサイクル管理タスクを実行する方法について詳しくは、 [データライフサイクル UI の概要](./overview.md).

データ衛生 API を使用してレコードを削除する方法については、 [作業順序エンドポイントガイド](../api/workorder.md).
