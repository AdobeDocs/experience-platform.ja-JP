---
title: 有効化アカウント管理の拡張
description: 拡張されたアクティベーションアカウントに対して、ライセンスの使用状況の監視や正しい権限の割り当てなどの管理タスクを実行する方法を説明します。
source-git-commit: 5bc8d6c7173f221c2830a9b15c8ec6241e8bc59d
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---


# アカウント管理

Audience Managerからオーディエンスを取り込み、ソーシャルおよび広告の宛先に対してアクティブ化するには、まず拡張アクティベーションユーザーアカウントを作成し、適切な権限のロールにアカウントを割り当てる必要があります。

ここでは、Admin Consoleでユーザーアカウントを作成し、拡張アクティベーションの正しい権限を付与する方法について説明します。

## ユーザーアカウントの作成 {#create-users}

使用する前に [!DNL Audience Manager Expanded Activation]の場合、ユーザーアカウントを作成する必要があります。

のユーザーアカウントを作成するには [!DNL Expanded Activation]のユーザー管理の手順に従います。 [Adobe Admin Console](https://helpx.adobe.com/jp/enterprise/using/manage-users-individually.html) ドキュメント。

## 権限ロールにユーザーを追加 {#permissions}

ユーザーアカウントの作成後、そのユーザーアカウントをに追加する必要があります [!DNL Expanded Activation] 権限の役割（内） [!DNL Expanded Activation] ユーザーインターフェイス。

に移動 **[!UICONTROL 管理]** -> **[!UICONTROL 権限]** -> **[!UICONTROL 役割]**&#x200B;を選択し、 **[!UICONTROL 展開されたアクティブ化の既定の役割]**.

![役割ページを示す、アクティベーションユーザーインターフェイスの拡張画像。](assets/expanded-activation-role.png)

に移動します **[!UICONTROL ユーザー]** tab キーを押して選択 **[!UICONTROL ユーザーを追加]**.

![「ユーザー」ページを示すアクティベーションのユーザーインターフェイス画像を拡張しました。](assets/add-users.png)

使用可能なリストから新しく作成したユーザーを選択し、次のオプションを選択します **[!UICONTROL 保存]**.

![ユーザーを追加ページを示す、アクティベーションユーザーインターフェイスの拡張画像。](assets/add-user.png)

これでユーザーアカウントが作成され、正しい役割に割り当てられました。 これで、にアクセスする準備ができました **[!UICONTROL アクティベーションの拡張]** ユーザーインターフェイス。

## ライセンス使用状況の監視 {#license-usage}

あなたの [!DNL Audience Manager Expanded Activation] 契約では、アカウントに取り込むことができるハッシュ化されたメールの最大数を指定します。

この情報を見つけるには、 **[!UICONTROL 管理]** -> **[!UICONTROL ライセンス使用状況]** ページ。

![ライセンス使用画面を示すアクティベーションユーザーインターフェイスの画像を拡張しました。](assets/license-usage.png)

このページには、次の情報が表示されます。

* **[!UICONTROL 製品]**：ライセンスを取得しているAdobe。 これは常に **[!UICONTROL Audience Managerの拡張有効化]**.
* **[!UICONTROL プライマリ指標]**：使用について追跡される指標の名前。 これは常に **[!UICONTROL アドレス可能オーディエンス]**.
* **[!UICONTROL ライセンス量]**：取り込みのライセンスを持つ、ハッシュ化されたメールの最大数。

  >[!TIP]
  >
  >ハッシュ化されたメールは、 [Audience Managerソースコネクタ](../sources/connectors/adobe-applications/audience-manager.md). のドキュメントを参照してください。 [オーディエンスのアクティブ化方法](activate-audiences.md) を参照してください。

* **[!UICONTROL 使用方法]**：取り込んだハッシュ化されたメールの数。
* **[!UICONTROL 使用状況 %]**：使用したライセンス量の割合。

Experience Platformでのライセンス使用状況について詳しくは、 [ライセンス使用状況ドキュメント](../dashboards/guides/license-usage.md).

## 次の手順 {#next-steps}

展開されたアクティベーションへの適切なアクセス権を持つユーザーアカウントを少なくとも 1 つ設定したので、そのアカウントを使用して、にアクセスできます。 [オーディエンスをアクティブ化](activate-audiences.md).
