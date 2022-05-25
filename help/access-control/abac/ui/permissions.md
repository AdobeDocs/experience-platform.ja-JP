---
keywords: Experience Platform；ホーム；人気のトピック；アクセス制御；属性ベースのアクセス制御；ABAC
title: 属性ベースのアクセス制御ロールの権限の管理
description: このドキュメントでは、Adobe Experience Platformの属性ベースのアクセス制御に関する情報を提供します
hide: true
hidefromtoc: true
exl-id: 8acd2bb6-eef8-4b23-8fd8-3566c7508fe7
source-git-commit: 19f1e8df8cd8b55ed6b03f80e42810aefd211474
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# ロールの権限の管理

>[!IMPORTANT]
>
>現在、米国ベースの医療関連のお客様向けに、属性ベースのアクセス制御が限定的なリリースで提供されています。 この機能は、完全にリリースされると、すべてのReal-time Customer Data Platformのお客様が利用できるようになります。

直後 [新しいロールの作成](#create-a-new-role)を含めない場合、 **[!UICONTROL 役割]** タブをクリックします。 既存のロールの権限を編集する場合は、 **[!UICONTROL 役割]** タブをクリックします。 または、「フィルター」オプションを使用して結果をフィルターし、役割を検索します。

## 役割のフィルタリング

ファネルアイコン (![フィルターアイコン](../../images/icon.png)) をクリックして、結果を絞り込むのに役立つフィルターコントロールのリストを表示します。

![flac-filters](../../images/flac-ui/flac-filters.png)

UI の役割では、次のフィルターを使用できます。

| Filter | 説明 |
| --- | --- |
| [!UICONTROL 作成期間] | 結果をフィルターする日付範囲を定義する開始日または終了日を選択します。 |
| [!UICONTROL 作成者] | ドロップダウンからユーザーを選択し、役割の作成者でフィルタリングします。 |
| [!UICONTROL 次の期間で変更] | 結果をフィルターする日付範囲を定義する開始日または終了日を選択します。 |
| [!UICONTROL 変更者] | 役割修飾子でフィルタリングします。そのためには、ドロップダウンからユーザーを選択します。 |

フィルターを削除するには、該当するフィルターのピルアイコンの「X」を選択するか、「 **[!UICONTROL すべてクリア]** をクリックして、すべてのフィルターを削除します。

![flac-clear-filters](../../images/flac-ui/flac-clear-filters.png)

## 役割の詳細

次の中からロールを選択します。 **[!UICONTROL 役割]** タブに移動し、役割の詳細ページが開きます。

![flac-details](../../images/flac-ui/flac-details.png)

「詳細」タブには、役割の概要が表示されます。 概要には、ロール名、ロールの説明、ロールを作成および変更したユーザーの名前、ロールが作成および変更された日時、ロールに関連付けられた権限が表示されます。 必要に応じて、ロール名とロールの説明を変更できます。

## ロールのラベルの管理

を選択します。 **[!UICONTROL ラベル]** 「 」タブをクリックして役割ラベルページを開き、「 」を選択します。 **[!UICONTROL ラベルを追加]** ロールにラベルを割り当てます。

![flac-labels](../../images/flac-ui/flac-labels.png)

このページにはラベルが表示されます。 このリストには、ラベル名、わかりやすい名前、カテゴリ、説明が表示されます。

ロールに追加するラベルをリストから選択し、「 」を選択します。 **[!UICONTROL 保存]**

![flac-add-labels](../../images/flac-ui/flac-add-labels.png)

追加されたラベルは、の下に表示されます。 **[!UICONTROL ラベル]** タブをクリックします。

![flac-added-labels](../../images/flac-ui/flac-added-labels.png)

ロールからラベルを削除するには、 **X** ラベル名の横にあるアイコン。

![flac-delete-labels](../../images/flac-ui/flac-delete-labels.png)

## 役割用サンドボックスの管理

を選択します。 **[!UICONTROL サンドボックス]** 「 」タブをクリックして、「役割サンドボックス」ページを開きます。 ここには、役割に追加されたサンドボックスのリストが表示されます。

![flac サンドボックス](../../images/flac-ui/flac-sandboxes.png)

ロールにサンドボックスを追加するには、以下を選択します。 **[!UICONTROL 編集]**.

![flac-add-sandboxes](../../images/flac-ui/flac-add-sandboxes.png)

次の画面では、ドロップダウンを使用して、役割に含めるサンドボックス内に存在するリソース権限を選択するよう求められます。 終了したら、「 」を選択します。 **[!UICONTROL 保存して終了]**.

![flac-add-role-permission](../../images/flac-ui/flac-add-role-permission.png)

## 役割のユーザーの管理

を選択します。 **[!UICONTROL ユーザー]** 「 」タブをクリックして役割ユーザーページを開き、「 」を選択します。 **[!UICONTROL ユーザーを追加]** ユーザーを役割に割り当てます。

![flac-users](../../images/flac-ui/flac-users.png)

ロールに追加するユーザーをリストから選択します。 または、検索バーを使用して名前または電子メールアドレスを入力してユーザーを検索し、「 」を選択します **[!UICONTROL 保存]**

![flac-add-users](../../images/flac-ui/flac-add-users.png)

追加されたユーザーは、の下に表示されます。 **[!UICONTROL ユーザー]** タブをクリックします。

![flac-added-users](../../images/flac-ui/flac-added-users.png)

ユーザーをロールから削除するには、 **X** ユーザー名の横のアイコン

![flac-remove-users](../../images/flac-ui/flac-remove-users.png)

## ロールの API 資格情報の管理

を選択します。 **[!UICONTROL API 資格情報]** 「 」タブをクリックしてロール API 資格情報ページを開き、「 」を選択します。 **[!UICONTROL API 資格情報の追加]** をクリックして、API 資格情報を役割に割り当てます。

![flac-api-credentials](../../images/flac-ui/flac-api-credentials.png)

ロールに追加するリストから API 資格情報を選択し、「 」を選択します。 **[!UICONTROL 保存]**

![flac-add-api-credentials](../../images/flac-ui/flac-add-api-credentials.png)

追加された API 資格情報は、の下に表示されます。 **[!UICONTROL API 資格情報]** タブをクリックします。

![flac-added-api-credentials](../../images/flac-ui/flac-added-api-credentials.png)

役割から API 資格情報を削除するには、 **X** API 秘密鍵証明書名の横にあるアイコン

![flac-remove-api-credentials](../../images/flac-ui/flac-remove-api-credentials.png)

この **[!UICONTROL API 資格情報を削除]** ダイアログが表示され、削除を確認するプロンプトが表示されます。

![flac-confirm-api-credentials-delete](../../images/flac-ui/flac-confirm-api-credentials-delete.png)

次の場所に戻ります： **[!UICONTROL API 資格情報]** タブをクリックします。

## 役割用のユーザーグループの管理

ユーザーグループは、1 つにグループ化され、同じ機能を実行するためのアクセス権を持つ複数のユーザーです。

を選択します。 **[!UICONTROL ユーザーグループ]** 「 」タブをクリックして役割のユーザーグループページを開き、「 」を選択します。 **[!UICONTROL グループを追加]** をクリックして、ユーザーグループを役割に割り当てます。

![flac-user-groups](../../images/flac-ui/flac-user-groups.png)

ロールに追加するユーザーグループをリストから選択します。 または、検索バーを使用してグループ名を入力してユーザーグループを検索し、「 」を選択します。 **[!UICONTROL 保存]**

![flac-add-user-groups](../../images/flac-ui/flac-add-user-groups.png)

追加されたユーザーグループは、の下に表示されます。 **[!UICONTROL ユーザーグループ]** タブをクリックします。

![flac-added-user-groups](../../images/flac-ui/flac-added-user-groups.png)

役割からユーザーグループを削除するには、 **X** ユーザーグループ名の横にあるアイコン

![flac-remove-user-groups](../../images/flac-ui/flac-remove-user-groups.png)

この **[!UICONTROL ユーザーグループを削除]** ダイアログが表示され、削除を確認するプロンプトが表示されます。

![flac-confirm-user-groups -delete](../../images/flac-ui/flac-confirm-user-groups-delete.png)

次の場所に戻ります： **[!UICONTROL ユーザーグループ]** タブをクリックします。

## 次の手順

権限が設定されている場合は、次の手順に進んで、 [ユーザーの管理](users.md).
