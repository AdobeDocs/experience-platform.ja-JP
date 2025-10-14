---
title: 有効化アカウント管理の拡張
description: 拡張されたアクティベーションアカウントに対して、ライセンスの使用状況の監視や正しい権限の割り当てなどの管理タスクを実行する方法を説明します。
exl-id: ee0ec4b9-a083-447b-b7a7-e1307e90c646
source-git-commit: 2222e9fbf75f3082d331868f820247e0c0ce3ba2
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# アカウント管理

Audience Managerからオーディエンスを取り込み、ソーシャルおよび広告の宛先に対してアクティブ化するには、まず拡張アクティベーションユーザーアカウントを作成し、適切な権限のロールにアカウントを割り当てる必要があります。

ここでは、Admin Consoleでユーザーアカウントを作成し、拡張アクティベーションの正しい権限を付与する方法について説明します。

## ユーザーアカウントの作成 {#create-users}

[!DNL Audience Manager Expanded Activation] を使用するには、まずユーザーアカウントを作成する必要があります。

[!DNL Expanded Activation] のユーザーアカウントを作成するには、[Adobe Admin Console](https://helpx.adobe.com/jp/enterprise/using/manage-users-individually.html) ドキュメントのユーザーの管理の手順に従ってください。

## 権限ロールにユーザーを追加 {#permissions}

ユーザーアカウントを作成したら、[!DNL Expanded Activation] ユーザーインターフェイスで、そのユーザーアカウントを [!DNL Expanded Activation] 権限ロールに追加する必要があります。

**[!UICONTROL 管理]**/**[!UICONTROL 権限]**/**[!UICONTROL 役割]** に移動し、「**[!UICONTROL 拡張アクティベーションのデフォルトの役割]**」を選択します。

![&#x200B; 役割ページを示す、拡張されたアクティベーションユーザーインターフェイスの画像。](assets/expanded-activation-role.png)

「**[!UICONTROL ユーザー]**」タブに移動し、「**[!UICONTROL ユーザーを追加]**」を選択します。

![&#x200B; ユーザーページを示す、拡張されたアクティベーションユーザーインターフェイスの画像。](assets/add-users.png)

使用可能なリストから新しく作成したユーザーを選択し、「**[!UICONTROL 保存]**」を選択します。

![&#x200B; ユーザーを追加ページを示す、拡張されたアクティベーションユーザーインターフェイスの画像。](assets/add-user.png)

これでユーザーアカウントが作成され、正しい役割に割り当てられました。 これで、**[!UICONTROL 拡張アクティベーション]** ユーザーインターフェイスにアクセスする準備が整いました。

## ライセンス使用状況の監視 {#license-usage}

[!DNL Audience Manager Expanded Activation] 契約では、アカウントに取り込むことができるハッシュ化されたメールの最大数を指定します。

この情報は、**[!UICONTROL 管理]**/**[!UICONTROL ライセンスの使用状況]** ページに移動すると確認できます。

![&#x200B; ライセンス使用画面を示す拡張アクティベーションユーザーインターフェイス画像。](assets/license-usage.png)

このページには、次の情報が表示されます。

* **[!UICONTROL Product]**：ライセンスを取得したAdobe。 これは常に、**[!UICONTROL Audience Managerで展開されたライセンス認証]** になります。
* **[!UICONTROL プライマリ指標]**：使用状況について追跡されている指標の名前。 これは常に **[!UICONTROL アドレス可能なオーディエンス]** になります。
* **[!UICONTROL ライセンス量]**：取り込みライセンスがある、ハッシュ化されたメールの最大数です。

  >[!TIP]
  >
  >[Audience Managerソースコネクタ &#x200B;](../sources/connectors/adobe-applications/audience-manager.md) を介してハッシュ化されたメールを取り込みます。 詳しくは、[&#x200B; オーディエンスのアクティブ化方法 &#x200B;](activate-audiences.md) に関するドキュメントを参照してください。

* **[!UICONTROL 使用状況]**：取り込んだハッシュ化されたメールの数。
* **[!UICONTROL 使用状況 %]**：使用したライセンス量の割合。

Experience Platformでのライセンスの使用について詳しくは、[&#x200B; ライセンスの使用に関するドキュメント &#x200B;](../dashboards/guides/license-usage.md) を参照してください。

## 次の手順 {#next-steps}

展開されたアクティベーションへの適切なアクセス権を持つユーザーアカウントを少なくとも 1 つ設定したので、そのアカウントを使用して [&#x200B; オーディエンスをアクティブ化 &#x200B;](activate-audiences.md) できます。
