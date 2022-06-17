---
title: 消費者レコードの削除
description: Adobe Experience Platform で消費者レコードを削除する方法を説明します。
exl-id: 5303905a-9005-483e-9980-f23b3b11b1d9
hide: true
hidefromtoc: true
source-git-commit: d17c53066d77652e46471ba4c696fde682eb3bab
workflow-type: tm+mt
source-wordcount: '1091'
ht-degree: 96%

---

# 消費者レコードの削除

>[!IMPORTANT]
>
>Adobe Experience Platform のデータハイジーン機能は、現在、Adobe Shield for Healthcare を購入した組織でのみ利用できます。

Adobe Experience Platform UI の[[!UICONTROL データハイジーン]ワークスペース](./overview.md)を使用すると、ID サービスおよびリアルタイム顧客プロファイルに参加している消費者レコードを削除できます。

## 前提条件

消費者レコードを削除するには、Experience Platform の ID フィールドがどのように機能するかについての実践的な理解が必要です。特に、削除元の（1 つまたは複数の）データセットに応じて、レコードを削除する消費者のプライマリ ID 値を把握する必要があります。

Platform の ID について詳しくは、次のドキュメントを参照してください。

* [Adobe Experience Platform ID サービス](../../identity-service/home.md)：デバイスやシステム間で ID をブリッジし、準拠する XDM スキーマで定義された ID フィールドに基づいてデータセットをリンクします。
   * [ID 名前空間](../../identity-service/namespaces.md)：ID 名前空間は、1 人の人物に関連している可能性のある様々なタイプの ID 情報を定義する、各 ID フィールドに必須のコンポーネントです。
* [リアルタイム顧客プロファイル](../../profile/home.md)：消費者 ID グラフを活用して、ほぼリアルタイムで更新される、複数のソースからの収集に基づいて統合された消費者プロファイルを提供します。
* [エクスペリエンスデータモデル（XDM）](../../xdm/home.md)：スキーマの使用により、Platform データの標準的な定義および構造を提供します。すべての Platform データセットは特定の XDM スキーマに準拠しており、スキーマはどのフィールドが ID であるかを定義しています。
   * [ID フィールド](../../xdm/ui/fields/identity.md)：XDM スキーマで ID フィールドが定義される方法を説明します。

## 新しいリクエストの作成

プロセスを開始するには、ワークスペースのメインページから「**[!UICONTROL リクエストを作成]**」を選択します。

![「[!UICONTROL リクエストを作成]」ボタンが選択されていることを示す画像](../images/ui/delete-consumer/create-request-button.png)

リクエスト作成ダイアログが表示されます。デフォルトでは、「**[!UICONTROL アクション]**」セクションで「**[!UICONTROL 消費者]**」オプションが選択されています。このオプションを選択されたままにします。

![作成ダイアログで「消費者」オプションが選択されていることを示す画像](../images/ui/delete-consumer/consumer-action.png)

## データセットの選択

次の手順は、「**[!UICONTROL 消費者の詳細]**」セクションで、単一のデータセットとすべてのデータセットのどちらから消費者データを削除するかを決定することです。

「**[!UICONTROL データセットを選択]**」を選択する場合、データベースアイコン（![データベースアイコンの画像](../images/ui/delete-consumer/database-icon.png)）を選択すると、リストから目的のデータセットを選択できるダイアログが表示されます。

![データセット選択ダイアログを示す画像](../images/ui/delete-consumer/select-dataset.png)

すべてのデータセットから消費者データを削除したい場合、「**[!UICONTROL すべてのデータセット]**」を選択します。

![「[!UICONTROL すべてのデータセット]」オプションが選択されていることを示す画像](../images/ui/delete-consumer/all-datasets.png)

>[!NOTE]
>
>「**[!UICONTROL すべてのデータセット]**」オプションを選択すると、削除操作により時間がかかり、正確なレコード削除にならない可能性があります。

## 消費者 ID の提供 {#provide-consumer-identities}

>[!CONTEXTUALHELP]
>id="platform_hygiene_primaryidentity"
>title="プライマリ ID"
>abstract="プライマリ ID は、Experience Platform でレコードを消費者のプロファイルに結び付ける属性です。データセットのプライマリ ID フィールドは、そのデータセットが基づいているスキーマによって定義されます。この列では、消費者のプライマリ ID のタイプ（または名前空間）を指定する必要があります ( 例： `email` （電子メールアドレスの場合） `ecid` (Experience CloudID)。 詳しくは、データハイジーン UI ガイドを参照してください。"

>[!CONTEXTUALHELP]
>id="platform_hygiene_identityvalue"
>title="ID 値"
>abstract="この列では、消費者のプライマリ ID の値を指定する必要があります（これは、左の列で指定した ID タイプに対応している必要があります）。プライマリ ID タイプが `email`の場合、値は消費者の電子メールアドレスにする必要があります。 詳しくは、データハイジーン UI ガイドを参照してください。"

消費者データを削除する場合、システムがどのレコードを削除する必要があるかを判断できるように、ID 情報を指定する必要があります。Platform のデータセットの場合、レコードは、データセットのスキーマによって定義された「**プライマリ ID**」フィールドに基づいて削除されます。

Platform のすべての ID フィールドと同様に、プライマリ ID は、**タイプ**（ID 名前空間とも呼ばれる）と&#x200B;**値**&#x200B;の 2 つで構成されます。ID タイプは、フィールドがどのように消費者を識別するかについてのコンテキストを提供し（電子メールアドレスなど）、値は、そのタイプに対する消費者の特定の ID を表します（例えば、`email` ID タイプでは `jdoe@example.com` など）。ID として使用される共通のフィールドには、アカウント情報、デバイス ID および Cookie ID が含まれます。

>[!TIP]
>
>特定のデータセットのプライマリ ID がわからない場合は、Platform UI で見つけることができます。**[!UICONTROL データセット]**&#x200B;ワークスペースで、リストから問題のデータセットを選択します。データセットの詳細ページの右側のパネルで、データセットのスキーマの名前の上にマウスポインターを置きます。プライマリ ID が、スキーマ名および説明と共に表示されます。
>
>![UI でデータセットのプライマリ ID がハイライトされていることを示す画像](../images/ui/delete-consumer/dataset-primary-identity.png)

単一のデータセットから消費者レコードを削除している場合、データセットが持つことができるのは 1 つのプライマリ ID のみなので、指定したすべての ID が同じタイプである必要があります。すべてのデータセットから削除している場合、データセットが異なるとプライマリ ID が異なる可能性があるので、複数の ID タイプを含めることができます。

消費者レコードを削除する場合、消費者 ID を提供するには 2 つのオプションがあります。

* [JSON ファイルのアップロード](#upload-json)
* [ID 値を手動で入力](#manual-identity)

### JSON ファイルのアップロード {#upload-json}

JSON ファイルをアップロードするには、ファイルを提供領域にドラッグ＆ドロップするか、「**[!UICONTROL ファイルを選択]**」を選択して、ローカルディレクトリから参照して選択できます。

![UI で JSON のアップロードする方法を示す画像](../images/ui/delete-consumer/upload-json.png)

JSON ファイルは、各オブジェクトが消費者 ID を表す、オブジェクトの配列としてフォーマットされている必要があります。

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
| `value` | タイプで示される消費者の ID。 |

ファイルがアップロードされると、引き続き[リクエストを送信](#submit)できます。

### ID を手動で入力 {#manual-identity}

ID を手動で入力するには、「**[!UICONTROL ID を追加]**」を選択します。

![「[!UICONTROL ID を追加]」ボタンが選択されていることを示す画像](../images/ui/delete-consumer/add-identity.png)

一度に 1 つずつ消費者 ID を入力できるコントロールが表示されます。「**[!UICONTROL プライマリ ID]**」で、ドロップダウンメニューを使用して ID タイプを選択します。「**[!UICONTROL ID 値]**」で、消費者のプライマリ ID 値を指定します。

![手動で追加された ID フィールドを示す画像](../images/ui/delete-consumer/identity-added.png)

さらに ID を追加するには、行の横にあるプラスアイコン（![プラスアイコンの画像](../images/ui/delete-consumer/plus-icon.png)）を選択するか、「**[!UICONTROL ID を追加]**」を選択します。

![リクエストにさらに ID を追加する方法を示す画像](../images/ui/delete-consumer/more-identities.png)

## リクエストの送信（#submit）

リクエストへの ID の追加が完了したら、「**[!UICONTROL 送信]**」を選択します。

![「[!UICONTROL 送信]」ボタンが選択されていることを示す画像](../images/ui/delete-consumer/submit.png)

データを削除する ID のリストを確認することを求められます。「**[!UICONTROL 送信]**」を選択して、選択を確定します。

![確認ダイアログを示す画像](../images/ui/delete-consumer/confirm-request.png)

リクエストが送信されると、作業指示が作成され、[!UICONTROL データハイジーン]ワークスペースの「[!UICONTROL 消費者]」タブに表示されます。ここから、リクエストを処理する作業指示のステータスを監視できます。ほとんどの消費者削除の作業指示は、完了するまで数日かかります。

## 次の手順

このドキュメントでは、Experience Platform UI で消費者レコードを削除する方法について説明しました。UI で他のデータハイジーンタスクを実行する方法について詳しくは、[データハイジーン UI の概要](./overview.md)を参照してください。

<!--

Paragraph below should be commented out until workorder.md will be added to the TOC.

To learn how to delete consumer records using the Data Hygiene API, refer to the [work order endpoint guide](../api/workorder.md).

-->
