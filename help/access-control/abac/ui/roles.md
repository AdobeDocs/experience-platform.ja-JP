---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;ABAC
title: 属性ベースのアクセス制御ロールの作成
description: このドキュメントでは、Adobe Experience Cloudの権限インターフェイスを使用した役割の管理について説明します
exl-id: 85699716-339d-4992-8390-95563c7ea7fe
source-git-commit: d8f72bb5ae56daf5a41c763f821ca6306514bc48
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 16%

---

# 役割の管理

役割は、管理者、スペシャリスト、またはエンドユーザーが組織内のリソースに対して持つアクセス権を定義します。 役割ベースのアクセス制御環境では、ユーザーアクセスプロビジョニングは、共通の責任とニーズによってグループ化されます。役割には特定の権限セットがあり、必要な表示または書き込みアクセスの範囲に応じて、組織のメンバーを 1 つ以上の役割に割り当てることができます。

## 新しい役割の作成

新しいロールを作成するには、 **[!UICONTROL 役割]** サイドバーの「 」タブで「 」を選択し、 **[!UICONTROL ロールを作成]**.

![flac-new-role](../../images/flac-ui/flac-new-role.png)

The **[!UICONTROL 新しいロールを作成]** ダイアログが表示され、名前とオプションの説明を入力するよう求められます。

終了したら、「**[!UICONTROL 確認]**」を選択します。

![flac-create-new-role](../../images/flac-ui/flac-create-new-role.png)

次に、役割に含めるリソース権限をドロップダウンメニューで選択します。

![flac-add-role-permission](../../images/flac-ui/flac-add-role-permission.png)

リソースを追加するには、「 **[!UICONTROL Adobe Experience Platform]** 左側のナビゲーションパネルから、リソースのリストを表示します。 または、左側のナビゲーションパネルの検索バーにリソース名を入力します。

![flac-add-additional-resources](../../images/flac-ui/flac-add-additional-resources.png)

関連するリソースをクリックしてドラッグし、メインパネルにドロップします。

![flac-additional-resources-added](../../images/flac-ui/flac-additional-resources-added.png)

役割に含めるリソース権限をドロップダウンメニューで選択します。 ロールに含めるすべてのリソースに対して、この手順を繰り返します。 終了したら、「**[!UICONTROL 保存して終了]**」を選択します。

![flac-save-resources](../../images/flac-ui/flac-save-resources.png)

新しい役割が正常に作成され、にリダイレクトされます。 **[!UICONTROL 役割]** 新しく作成された役割がリストに表示されるページです。

![flac-role-saved](../../images/flac-ui/flac-role-saved.png)

次の節を参照してください： [ロールの権限の管理](#manage-permissions-for-a-role) ロールの権限を作成後に管理する方法の詳細については、を参照してください。

次のビデオは、新しい役割の作成と、その役割のユーザーの管理に関する理解を深めることを目的としています。

>[!VIDEO](https://video.tv.adobe.com/v/336081/?learn=on)

## ロールを複製

既存の役割を複製するには、 **[!UICONTROL 役割]** タブをクリックします。 または、「フィルター」オプションを使用して結果をフィルタリングし、複製する役割を見つけます。

![flac-duplicate-role](../../images/flac-ui/flac-duplicate-role.png)

次に、「 **[!UICONTROL 複製]** 画面の右上から。

![flac-duplicate](../../images/flac-ui/flac-duplicate.png)

The **[!UICONTROL 重複した役割]** ダイアログが表示され、複製を確認するよう求められます。

![flac-duplicate-confirm](../../images/flac-ui/flac-duplicate-confirm.png)

次に、役割の詳細ページが表示され、役割の名前と権限を変更できます。 詳細、ラベル、サンドボックスは、以前の役割から複製されます。 ユーザーは、「ユーザー」タブから追加する必要があります。 次の項目を表示すると、 [ロールの権限の管理](permissions.md) 詳細、ラベル、サンドボックス、ユーザーをロールに追加する方法について詳しくは、ドキュメントを参照してください。

左向き矢印をクリックして、 **[!UICONTROL 役割]** タブをクリックします。

![flac-return-to-roles](../../images/flac-ui/flac-return-to-roles.png)

新しい役割が **[!UICONTROL 役割]** ページに貼り付けます。

![flac-role-duplicate-saved](../../images/flac-ui/flac-role-duplicate-saved.png)

## 役割を削除

省略記号 (`…`) をクリックし、ロール名の横に表示されます。また、ドロップダウンに、ロールを編集、削除、複製するためのコントロールが表示されます。 ドロップダウンから「プロジェクト」を選択します。

![flac-role-delete](../../images/flac-ui/flac-role-delete.png)

The **[!UICONTROL ユーザーロールを削除]** ダイアログが表示され、削除を確認するプロンプトが表示されます。

![flac-confirm-role-delete](../../images/flac-ui/flac-confirm-role-delete.png)

次の場所に戻ります： **[!UICONTROL 役割]** タブをクリックします。

## 次の手順

新しいロールを作成したら、次の手順に進んで、 [ロールの権限の管理](permissions.md).
