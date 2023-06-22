---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;ABAC
title: 属性ベースのアクセス制御管理役割の権限
description: このドキュメントでは、Adobe Experience Cloud の権限インターフェイスを使用して、役割の権限を設定する方法について説明します
exl-id: 8acd2bb6-eef8-4b23-8fd8-3566c7508fe7
source-git-commit: 7c44683c8110d78654baba4bc53f2c3c2daf2831
workflow-type: tm+mt
source-wordcount: '966'
ht-degree: 97%

---

# 役割の権限の管理

>[!IMPORTANT]
>
>アクセス制御は、権限の付与にユーザー ID（ユーザーに割り当てられた内部の一意の ID）を使用します。組織を Adobe ID から Business ID に移行すると、ユーザー ID が変更され、新しく生成されたユーザー ID がアクセス制御で使用されるので、そのユーザーに設定されたすべての権限は失われます。組織が Business ID に移行されている場合、アドビ担当者に連絡して、Adobe IDから Business ID にユーザー ID を移行してください。

権限は、管理者がユーザーの役割およびアクセスポリシーを定義し、製品アプリケーション内の機能およびオブジェクトのアクセス権限を管理できる、Experience Cloud の領域です。

権限を通じて、役割を作成および管理し、それらの役割に対して必要なリソース権限を割り当てることができます。また、権限では、特定の役割に関連付けられたラベル、サンドボックス、ユーザーを管理することもできます。

[新しい役割の作成](#create-a-new-role)が完了するとすぐに、「**[!UICONTROL 役割]**」タブに戻ります。既存の役割の権限を編集する場合は、「**[!UICONTROL 役割]**」タブをクリックします。または、「フィルター」オプションを使用して結果をフィルターし、役割を見つけます。

## 役割のフィルタリング

ファネルアイコン（![フィルターアイコン](../../images/icon.png)）を選択し、フィルターコントロールのリストを表示して、結果を絞り込みます。

![flac-filters](../../images/flac-ui/flac-filters.png)

役割には次のフィルターを UI で使用できます。

| フィルター | 説明 |
| --- | --- |
| [!UICONTROL 作成期間] | 結果をフィルターする日付範囲を定義する開始日および／または終了日を選択します。 |
| [!UICONTROL 作成者] | ドロップダウンからユーザーを選択し、役割の作成者でフィルタリングします。 |
| [!UICONTROL 次の期間で変更された] | 結果をフィルターする日付範囲を定義する開始日および／または終了日を選択します。 |
| [!UICONTROL 変更者] | 役割の変更者でフィルタリングします。そのためには、ドロップダウンからユーザーを選択します。 |

フィルターを削除するには、該当するフィルターのピルアイコンの「X」を選択するか、「**[!UICONTROL すべてクリア]**」をクリックして、すべてのフィルターを削除します。

![flac-clear-filters](../../images/flac-ui/flac-clear-filters.png)

## 役割の詳細

「**[!UICONTROL 役割]**」タブから役割を選択すると、役割の詳細ページが開きます。

![flac-details](../../images/flac-ui/flac-details.png)

「詳細」タブには、役割の概要が表示されます。概要には、役割名、役割の説明、役割の作成者および変更者、役割の作成日時および変更日時、役割に関連付けられた権限が表示されます。 必要に応じて、役割名と役割の説明を変更できます。

## 役割のラベルの管理

「**[!UICONTROL ラベル]** 」タブを選択して役割ラベルページを開き、「**[!UICONTROL ラベルを追加]** 」を選択して役割にラベルを割り当てます。

![flac-labels](../../images/flac-ui/flac-labels.png)

このページにラベルが表示されます。 リストには、ラベル名、わかりやすい名前、カテゴリ、説明が表示されます。

役割に追加するラベルをリストから選択し、「**[!UICONTROL 保存]**」を選択します。

![flac-add-labels](../../images/flac-ui/flac-add-labels.png)

追加されたラベルは、「**[!UICONTROL ラベル]**」タブに表示されます。

![flac-added-labels](../../images/flac-ui/flac-added-labels.png)

役割からラベルを削除するには、ラベル名の横にある「**X**」アイコンを選択します。

![flac-delete-labels](../../images/flac-ui/flac-delete-labels.png)

## 役割用サンドボックスの管理

「**[!UICONTROL サンドボックス]**」タブを選択して、役割サンドボックスページを開きます。ここには、役割に追加されたサンドボックスのリストが表示されます。

![flac-sandboxes](../../images/flac-ui/flac-sandboxes.png)

役割にさらにサンドボックスを追加するには、「**[!UICONTROL 編集]**」を選択します。

![flac-add-sandboxes](../../images/flac-ui/flac-add-sandboxes.png)

次の画面では、サンドボックス内に存在するどのリソース権限を役割に含めるのか、ドロップダウンを使用して選択するよう求められます。終了したら、「**[!UICONTROL 保存して終了]**」を選択します。

![flac-add-role-permission](../../images/flac-ui/flac-add-role-permission.png)

## 役割でのユーザーの管理

「**[!UICONTROL ユーザー]**」タブをクリックして役割ユーザーページを開き、「**[!UICONTROL ユーザーを追加]**」を選択してユーザーを役割に割り当てます。

![flac-users](../../images/flac-ui/flac-users.png)

役割に追加するユーザーをリストから選択します。 または、検索バーを使用して名前またはメールアドレスを入力してユーザーを検索し、「**[!UICONTROL 保存]**」を選択します。

![flac-add-users](../../images/flac-ui/flac-add-users.png)

追加されたユーザーは「**[!UICONTROL ユーザー]**」タブの下に表示されます。

![flac-added-users](../../images/flac-ui/flac-added-users.png)

役割からユーザーを削除するには、ユーザー名の横の「**X**」アイコンを選択します。

![flac-remove-users](../../images/flac-ui/flac-remove-users.png)

## 役割での API 資格情報の管理 {#manage-api-credentials-for-role}

「**[!UICONTROL API 資格情報]**」タブをクリックして役割 API 資格情報ページを開き、「**[!UICONTROL API 資格情報を追加]**」を選択して、API 資格情報を役割に割り当てます。

![flac-api-credentials](../../images/flac-ui/flac-api-credentials.png)

役割に追加するリストから API 資格情報を選択し、「**[!UICONTROL 保存]**」を選択します。 

![flac-add-api-credentials](../../images/flac-ui/flac-add-api-credentials.png)

追加された API 資格情報は、「**[!UICONTROL API 資格情報]**」タブの下に表示されます。 

![flac-added-api-credentials](../../images/flac-ui/flac-added-api-credentials.png)

API 資格情報を役割から削除するには、API 資格情報名の横にある「**X**」アイコンを選択します。

![flac-remove-api-credentials](../../images/flac-ui/flac-remove-api-credentials.png)

**[!UICONTROL API 資格情報を削除]**&#x200B;ダイアログが表示され、削除を確認するよう求められます。 

![flac-confirm-api-credentials-delete](../../images/flac-ui/flac-confirm-api-credentials-delete.png)

「**[!UICONTROL API 資格情報]**」タブに戻されます。

## 役割でのユーザーグループの管理

ユーザーグループとは、同じ機能を実行するためのアクセス権を持つ、グループ化された複数のユーザーのことです。

「**[!UICONTROL ユーザーグループ]**」タブを選択して役割のユーザーグループページを開きます。次に、「**[!UICONTROL グループを追加]**」を選択して、ユーザーグループを役割に割り当てます。

![flac-user-groups](../../images/flac-ui/flac-user-groups.png)

役割に追加するユーザーグループをリストから選択します。 または、検索バーを使用してグループ名を入力してユーザーグループを検索し、「**[!UICONTROL 保存]**」を選択します。

![flac-add-user-groups](../../images/flac-ui/flac-add-user-groups.png)

追加されたユーザーグループは「**[!UICONTROL ユーザーグループ]**」タブの下に表示されます。

![flac-added-user-groups](../../images/flac-ui/flac-added-user-groups.png)

役割からユーザーグループを削除するには、ユーザーグループ名の横にある「**X**」アイコンを選択します。

![flac-remove-user-groups](../../images/flac-ui/flac-remove-user-groups.png)

**[!UICONTROL ユーザーグループを削除]**&#x200B;ダイアログが表示され、削除を確認するよう求められます。 

![flac-confirm-user-groups-delete](../../images/flac-ui/flac-confirm-user-groups-delete.png)

「**[!UICONTROL ユーザーグループ]**」タブに戻されます。

## 製品プロファイルを使用した Experience Platform へのユーザーの追加

製品プロファイルにユーザーを追加するには、Admin Console にログインし、「**[!UICONTROL ユーザーを追加]**」を選択します。

![product-profile-add-users](../../images/flac-ui/product-profile-add-users.png)

**[!UICONTROL チームにユーザーを追加]**&#x200B;ダイアログが表示されます。 ユーザーのメールアドレス、名（オプション）および姓（オプション）を入力します。

鉛筆アイコンを選択して、製品およびユーザーグループを選択し、「 」を選択します。 **[!UICONTROL Adobe Experience Platform]**&#x200B;を選択し、「 **[!UICONTROL AEP-Default-All-Users]**&#x200B;を選択し、「  **[!UICONTROL 保存]**.

![product-profile](../../images/flac-ui/product-profile.png)

## 次の手順

権限が設定されたら、次の手順に進んで、[ユーザーを管理](users.md)することができます。
