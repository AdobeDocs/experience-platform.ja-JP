---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;ABAC
title: アクセス制御ポリシーの管理
description: このドキュメントでは、Adobe Experience Cloudの権限インターフェイスを使用してアクセス制御ポリシーを管理する方法について説明します。
exl-id: 66820711-2db0-4621-908d-01187771de14
source-git-commit: 38447348bc96b2f3f330ca363369eb423efea1c8
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 86%

---

# アクセス制御ポリシーの管理

アクセス制御ポリシーとは、属性を統合して、許容されるアクションと許容されないアクションを確立するステートメントです。 アクセスポリシーは、ローカルまたはグローバルに設定でき、他のポリシーを上書きできます。

>[!IMPORTANT]
>
>アクセスポリシーは、組織内のどのユーザーがアクセス権を持つかではなく、Adobe Experience Platformでのデータの使用方法を制御するデータ使用ポリシーと混同しないでください。 作成に関するガイドを参照してください。 [データ使用ポリシー](../../../data-governance/policies/create.md) を参照してください。

## 新しいポリシーの作成

新しいポリシーを作成するには、サイドバーの「**[!UICONTROL ポリシー]**」タブを選択し、「**[!UICONTROL ポリシーを作成]**」を選択します。

![flac-new-policy](../../images/flac-ui/flac-new-policy.png)

**[!UICONTROL 新しいポリシーを作成]**&#x200B;ダイアログが表示され、名前とオプションの説明を入力するよう求められます。 終了したら、「**[!UICONTROL 確認]**」を選択します。

![flac-create-new-policy](../../images/flac-ui/flac-create-new-policy.png)

ドロップダウン矢印を使用して、リソースへの「**アクセスを許可**」（![flac-permit-access-to](../../images/flac-ui/flac-permit-access-to.png)）するか、リソースへの「**アクセスを拒否**」（![flac-deny-access-to](../../images/flac-ui/flac-deny-access-to.png)）するかを選択します。

次に、ドロップダウンメニューを使用して、ポリシーに含めるリソースを選択し、アクセスタイプ（読み取りまたは書き込み）を検索します。

![flac-flac-policy-resource-dropdown](../../images/flac-ui/flac-policy-resource-dropdown.png)

次に、ドロップダウン矢印を使用して、このポリシーに適用する条件、**以下が true**（![flac-policy-true](../../images/flac-ui/flac-policy-true.png)）または&#x200B;**以下が false**（![flac-policy-false](../../images/flac-ui/flac-policy-false.png)）を選択します。

「プラス」アイコンを選択して、リソースの 「**一致式を追加**」または「**式グループを追加**」を実行します。

![flac-policy-expression](../../images/flac-ui/flac-policy-expression.png)

ドロップダウンを使用して、「**リソース**」を選択します。

![flac-policy-resource-dropdown](../../images/flac-ui/flac-policy-resource-dropdown-1.png)

次に、ドロップダウンを使用して、「**一致**」を選択します。

![flac-policy-matches-dropdown](../../images/flac-ui/flac-policy-matches-dropdown.png)

次に、ドロップダウンを使用して、ラベルのタイプ（「**[!UICONTROL コアラベル]**」または「**[!UICONTROL カスタムラベル]**」）を選択して、役割のユーザーに割り当てられたラベルに一致させます。

![flac-policy-user-dropdown](../../images/flac-ui/flac-policy-user-dropdown.png)

最後に、ドロップダウンメニューを使用して、ポリシー条件を適用する「**サンドボックス**」を選択します。

![flac-policy-sandboxes-dropdown](../../images/flac-ui/flac-policy-sandboxes-dropdown.png)

「**リソースを追加**」を選択して、さらにリソースを追加します。 終了したら、「**[!UICONTROL 保存して終了]**」を選択します。

![flac-policy-save-and-exit](../../images/flac-ui/flac-policy-save-and-exit.png)

新しいポリシーが正常に作成されると、「**[!UICONTROL ポリシー]**」タブにリダイレクトされ、新しく作成されたポリシーがリストに表示されます。

![flac-policy-saved](../../images/flac-ui/flac-policy-saved.png)

## ポリシーの編集

既存のポリシーを編集するには、「**[!UICONTROL ポリシー]**」タブからポリシーを選択します。 または、「フィルター」オプションを使用して結果をフィルタリングし、編集するポリシーを見つけます。

![flac-policy-select](../../images/flac-ui/flac-policy-select.png)

次に、ポリシー名の横にある省略記号（`…`）を選択すると、役割を編集、非アクティブ化、削除または複製するためのコントロールがドロップダウンに表示されます。 ドロップダウンから「編集」を選択します。

![flac-policy-edit](../../images/flac-ui/flac-policy-edit.png)

ポリシー権限画面が表示されます。 更新を行ってから、「**[!UICONTROL 保存して終了]**」を選択します。

![flac-policy-save-and-exit](../../images/flac-ui/flac-policy-save-and-exit.png)

ポリシーが正常に更新されると、「**[!UICONTROL ポリシー]**」タブにリダイレクトされます。

## ポリシーの複製

既存のポリシーを複製するには、「**[!UICONTROL ポリシー]**」タブからポリシーを選択します。 または、「フィルター」オプションを使用して結果をフィルタリングし、編集するポリシーを見つけます。

![flac-policy-select](../../images/flac-ui/flac-policy-select.png)

次に、ポリシー名の横にある省略記号（`…`）を選択すると、役割を編集、非アクティブ化、削除または複製するためのコントロールがドロップダウンに表示されます。 ドロップダウンから「複製」を選択します。

![flac-policy-duplicate](../../images/flac-ui/flac-policy-duplicate.png)

**[!UICONTROL ポリシーを複製]**&#x200B;ダイアログが表示され、複製操作の確認を求められます。

![flac-policy-duplicate-confirm](../../images/flac-ui/flac-duplicate-confirm.png)

新しいポリシーは、「**[!UICONTROL ポリシー]**」タブで、元のポリシーのコピーとしてリストに表示されます。

![flac-role-duplicate-saved](../../images/flac-ui/flac-role-duplicate-saved.png)

## ポリシーの削除

既存のポリシーを削除するには、「**[!UICONTROL ポリシー]**」タブをクリックします。 または、「フィルター」オプションを使用して結果をフィルターし、削除するポリシーを見つけます。

![flac-policy-select](../../images/flac-ui/flac-policy-select.png)

次に、省略記号（`…`）をクリックします。ドロップダウンに、役割を編集、非アクティブ化、削除または複製するためのコントロールが表示されます。 ドロップダウンから「プロジェクト」を選択します。

![flac-policy-delete](../../images/flac-ui/flac-policy-delete.png)

**[!UICONTROL ユーザーポリシーを削除]**&#x200B;ダイアログが表示され、削除を確認するよう求められます。

![flac-policy-delete-confirm](../../images/flac-ui/flac-policy-delete-confirm.png)

「**[!UICONTROL ポリシー]**」タブが開き、削除を確認するポップアップが表示されます。

![flac-policy-delete-confirmation](../../images/flac-ui/flac-policy-delete-confirmation.png)

## ポリシーのアクティベート

既存のポリシーをアクティブにするには、「**[!UICONTROL ポリシー]**」タブをクリックします。または、「フィルター」オプションを使用して結果をフィルターし、削除するポリシーを見つけます。

![flac-policy-select](../../images/flac-ui/flac-policy-select.png)

次に、省略記号（`…`）をクリックします。ドロップダウンに、役割を編集、アクティブ化、削除または複製するためのコントロールが表示されます。 ドロップダウンから「アクティブ化」を選択します。

![flac-policy-activate](../../images/flac-ui/flac-policy-delete.png)

**[!UICONTROL ユーザーポリシーをアクティブ化]**&#x200B;ダイアログが表示され、アクティベーションを確認するプロンプトが表示されます。

![flac-policy-activate-confirm](../../images/flac-ui/flac-policy-activate-confirm.png)

「 **[!UICONTROL ポリシー]** 」タブが開き、アクティベーションを確認するポップアップが表示されます。ポリシーのステータスはアクティブと表示されます。

![flac-policy-activated](../../images/flac-ui/flac-policy-activated.png)

## 次の手順

新しいポリシープロファイルを作成したら、次の手順、[役割の権限を管理](permissions.md)に進むことができます。
