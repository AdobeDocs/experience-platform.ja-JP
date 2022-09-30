---
keywords: Experience Platform；ホーム；人気のトピック；アクセス制御；属性ベースのアクセス制御；ABAC
title: 属性ベースのアクセス制御ポリシーの作成
description: このドキュメントでは、Adobe Experience Cloudの権限インターフェイスを使用してポリシーを管理する方法について説明します
exl-id: 66820711-2db0-4621-908d-01187771de14
source-git-commit: 9e44e647e4647a323fa9d1af55266d6f32b5ccb9
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 0%

---

# ポリシーの管理

ポリシーとは、属性を統合して、許容される許容されるアクションと許容されないアクションを確立するステートメントです。 ポリシーは、ローカルまたはグローバルに設定でき、他のポリシーを上書きできます。

## 新しいポリシーを作成

新しいポリシーを作成するには、 **[!UICONTROL ポリシー]** サイドバーの「 」タブで「 」を選択し、 **[!UICONTROL ポリシーを作成]**.

![flac-new-policy](../../images/flac-ui/flac-new-policy.png)

この **[!UICONTROL 新しいポリシーを作成]** ダイアログが表示され、名前とオプションの説明を入力するよう求められます。 終了したら、「 」を選択します。 **[!UICONTROL 確認]**.

![flac-create-new-policy](../../images/flac-ui/flac-create-new-policy.png)

ドロップダウンの矢印を使用して、次を選択します。 **へのアクセスを許可** (![flac-permit-access-to](../../images/flac-ui/flac-permit-access-to.png)) リソースまたは **へのアクセスを拒否** (![flac-deny-access-to](../../images/flac-ui/flac-deny-access-to.png)) リソースを返します。

次に、ドロップダウンメニューを使用して、ポリシーに含めるリソースを選択し、アクセスタイプ（読み取り、書き込み）を検索します。

![flac-flac-policy-resource-dropdown](../../images/flac-ui/flac-policy-resource-dropdown.png)

次に、ドロップダウン矢印を使用して、このポリシーに適用する条件を選択します。 **次のことは真です** (![flac-policy-true](../../images/flac-ui/flac-policy-true.png)) または **次は false です** (![flac-policy-false](../../images/flac-ui/flac-policy-false.png)) をクリックします。

プラスアイコンを選択して、 **一致式を追加** または **式グループを追加** リソースの

![flac-policy-expression](../../images/flac-ui/flac-policy-expression.png)

ドロップダウンを使用して、 **リソース**.

![flac-policy-resource-dropdown](../../images/flac-ui/flac-policy-resource-dropdown-1.png)

次に、ドロップダウンを使用して、 **一致**.

![flac-policy-matches-dropdown](../../images/flac-ui/flac-policy-matches-dropdown.png)

次に、ドロップダウンを使用して、 **ユーザー**.

![flac-policy-user-dropdown](../../images/flac-ui/flac-policy-user-dropdown.png)

最後に、 **サンドボックス** ドロップダウンメニューを使用して、適用するポリシー条件を選択します。

![flac-policy-sandboxes-dropdown](../../images/flac-ui/flac-policy-sandboxes-dropdown.png)

選択 **リソースを追加** をクリックして、さらにリソースを追加します。 終了したら、「 」を選択します。 **[!UICONTROL 保存して終了]**.

![flac-policy-save-and-exit](../../images/flac-ui/flac-policy-save-and-exit.png)

新しいポリシーが正常に作成され、 **[!UICONTROL ポリシー]** タブに表示されます。新しく作成されたポリシーがリストに表示されます。

![flac-policy-saved](../../images/flac-ui/flac-policy-saved.png)

## ポリシーの編集

既存のポリシーを編集するには、 **[!UICONTROL ポリシー]** タブをクリックします。 または、「フィルター」オプションを使用して結果をフィルターし、編集するポリシーを見つけます。

![flac-policy-select](../../images/flac-ui/flac-policy-select.png)

次に、省略記号 (`…`) をクリックします。ドロップダウンに、役割を編集、非アクティブ化、削除または複製するためのコントロールが表示されます。 ドロップダウンから「編集」を選択します。

![flac-policy-edit](../../images/flac-ui/flac-policy-edit.png)

ポリシー権限画面が表示されます。 更新を行い、「 」を選択します。 **[!UICONTROL 保存して終了]**.

![flac-policy-save-and-exit](../../images/flac-ui/flac-policy-save-and-exit.png)

ポリシーが正常に更新され、 **[!UICONTROL ポリシー]** タブをクリックします。

## ポリシーの複製

既存のポリシーを複製するには、 **[!UICONTROL ポリシー]** タブをクリックします。 または、「フィルター」オプションを使用して結果をフィルターし、編集するポリシーを見つけます。

![flac-policy-select](../../images/flac-ui/flac-policy-select.png)

次に、省略記号 (`…`) をクリックします。ドロップダウンに、役割を編集、非アクティブ化、削除または複製するためのコントロールが表示されます。 ドロップダウンから「複製」を選択します。

![flac-policy-duplicate](../../images/flac-ui/flac-policy-duplicate.png)

この **[!UICONTROL ポリシーを複製]** ダイアログが表示され、複製を確認するよう求められます。

![flac-policy-duplicate-confirm](../../images/flac-ui/flac-duplicate-confirm.png)

新しいポリシーは、元のポリシーのコピーとしてリストに表示されます ( **[!UICONTROL ポリシー]** タブをクリックします。

![flac-role-duplicate-saved](../../images/flac-ui/flac-role-duplicate-saved.png)

## ポリシーを削除する

既存のポリシーを削除するには、 **[!UICONTROL ポリシー]** タブをクリックします。 または、「フィルター」オプションを使用して結果をフィルターし、削除するポリシーを見つけます。

![flac-policy-select](../../images/flac-ui/flac-policy-select.png)

次に、省略記号 (`…`) をクリックします。ドロップダウンに、役割を編集、非アクティブ化、削除または複製するためのコントロールが表示されます。 ドロップダウンから「削除」を選択します。

![flac-policy-delete](../../images/flac-ui/flac-policy-delete.png)

この **[!UICONTROL ユーザーポリシーを削除]** ダイアログが表示され、削除を確認するプロンプトが表示されます。

![flac-policy-delete-confirm](../../images/flac-ui/flac-policy-delete-confirm.png)

次の場所に戻ります： **[!UICONTROL ポリシー]** タブが開き、削除の確認ポップアップが表示されます。

![flac-policy-delete-confirmation](../../images/flac-ui/flac-policy-delete-confirmation.png)

## ポリシーのアクティブ化

既存のポリシーをアクティブにするには、 **[!UICONTROL ポリシー]** タブをクリックします。 または、「フィルター」オプションを使用して結果をフィルターし、削除するポリシーを見つけます。

![flac-policy-select](../../images/flac-ui/flac-policy-select.png)

次に、省略記号 (`…`) をクリックします。ドロップダウンに、役割を編集、アクティブ化、削除または複製するためのコントロールが表示されます。 ドロップダウンから「 activate 」を選択します。

![flac-policy-activate](../../images/flac-ui/flac-policy-delete.png)

この **[!UICONTROL ユーザーポリシーを有効化]** ダイアログが表示され、アクティベーションを確認するプロンプトが表示されます。

![flac-policy-activate-confirm](../../images/flac-ui/flac-policy-activate-confirm.png)

次の場所に戻ります： **[!UICONTROL ポリシー]** 」タブが開き、アクティベーションポップオーバーの確認が表示されます。 ポリシーのステータスはアクティブと表示されます。

![flac-policy-activated](../../images/flac-ui/flac-policy-activated.png)

## 次の手順

新しいポリシーを作成したら、次の手順に進んで、 [ロールの権限の管理](permissions.md).
