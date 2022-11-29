---
title: Privacy Serviceの権限の管理
description: Adobe Admin Consoleを使用してAdobe Experience Platform Privacy Serviceのユーザー権限を管理する方法について説明します。
source-git-commit: 59dc28a84971dc8c21d633741cfe2dc1b44ea1a6
workflow-type: tm+mt
source-wordcount: '964'
ht-degree: 6%

---

# Privacy Serviceの権限の管理

アクセス先 [Adobe Experience Platform Privacy Service](./home.md) は、Adobe Admin Consoleの役割に基づく詳細な権限によって制御されます。 ユーザーのグループに権限を割り当てる製品プロファイルを作成することで、Privacy Serviceのどの機能にアクセスできるユーザーを決定できます [UI](./ui/overview.md) および [API](./api/overview.md).

>[!NOTE]
>
>Privacy ServiceAPI の統合を作成する場合、既存の製品プロファイルを選択して、統合に権限を持つ機能やアクションを決定する必要があります。 詳しくは、 [Privacy ServiceAPI の概要](./api/getting-started.md) を参照してください。

このガイドでは、Privacy Serviceの権限を管理する方法について説明します。

## はじめに

アクセス制御をPrivacy Service用に設定するには、Adobe Experience Platform Privacy Serviceとの製品統合を持つ組織の管理者権限が必要です。 権限を付与または取り消すことができる最小の役割は、 **製品プロファイル管理者**. 権限を管理できる他の管理者の役割は、 **製品管理者**（製品内のすべてのプロファイルを管理）と&#x200B;**システム管理者**（制限なし）です。次の記事を参照してください： [管理者ロール](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html) (『Adobeエンタープライズ管理ガイド』内 ) を参照してください。

このガイドは、ユーザーが製品プロファイルなどの基本的なAdmin Console概念と、製品が個々のユーザーおよびグループに製品権限を付与する方法に詳しいことを前提としています。 詳しくは、 [Admin Consoleユーザーガイド](https://helpx.adobe.com/jp/enterprise/using/admin-console.html).

## 使用可能な権限

次の表に、Privacy Serviceに使用できる権限と、アクセスを許可する特定の機能の説明を示します。

| カテゴリ | 権限 | 説明 |
| --- | --- | --- |
| [!UICONTROL Privacy Service権限] | [!UICONTROL プライバシー読み取り権限] | ユーザーが、既存のアクセス要求と削除要求を詳細と共に表示できるかどうかを指定します。 |
| [!UICONTROL Privacy Service権限] | [!UICONTROL プライバシー書き込み権限] | ユーザーが新しいアクセス要求および削除要求を作成できるかどうかを指定します。 |
| [!UICONTROL Privacy Service権限] | [!UICONTROL コンテンツ配信権限の読み取り（アクセス）] | アクセス要求がPrivacy Serviceによって処理されると、顧客のデータを含む ZIP ファイルがその顧客に送信されます。 アクセス要求の詳細を検索する場合、この権限によって、ユーザーが要求の ZIP ファイルのダウンロードリンクにアクセスできるかどうかが決まります。 |
| [!UICONTROL 販売のオプトアウト権限] | [!UICONTROL 読み取り権限 — 販売のオプトアウト] | ユーザーが、既存の販売オプトアウトリクエストとその詳細を表示できるかどうかを指定します。 |
| [!UICONTROL 販売のオプトアウト権限] | [!UICONTROL 書き込み権限 — 販売のオプトアウト] | ユーザーが新しい販売オプトアウトリクエストを作成できるかどうかを指定します。 |

{style=&quot;table-layout:auto&quot;}

## 権限の管理 {#manage}

Privacy Service権限を管理するには、にログインします。 [Admin Console](https://adminconsole.adobe.com/) を選択し、 **[!UICONTROL 製品]** 上部ナビゲーションから。 ここからを選択します。 **[!UICONTROL Adobe Experience Platform Privacy Service]**.

![Admin ConsoleのPrivacy Service製品カードを示す画像](./images/permissions/privacy-service-card.png)

### 製品プロファイルを選択または作成

次の画面には、組織内で使用可能なPrivacy Serviceプロファイルのリストが表示されます。 製品プロファイルが存在しない場合は、「 」を選択します。 **[!UICONTROL 新しいプロファイル]** をクリックして、1 つを作成します。 組織内に異なるレベルのアクセスを必要とする複数の役割またはユーザーグループがある場合、各ユーザーに対して個別の製品プロファイルを作成する必要があります。

![Admin Console内のPrivacy Serviceの製品プロファイルを示す画像](./images/permissions/select-or-create-profile.png)

製品プロファイルを選択した後、 **[!UICONTROL 権限]** タブを開始 [権限の編集](#edit-permissions) プロファイルの場合は、 **[!UICONTROL ユーザー]** タブを開始 [ユーザーの割り当て](#assign-users) をプロファイルに追加します。

![製品プロファイルAdmin Consoleの「権限」タブを示す画像](./images/permissions/users-permissions-tabs.png)

### プロファイルの権限の編集 {#edit-permissions}

の **[!UICONTROL 権限]** タブで、表示された権限カテゴリを選択して、権限編集ビューにアクセスします。

プロファイルに対する権限を編集する場合、使用可能な権限は左の列に、プロファイルに含まれる権限は右の列に表示されます。 リストに表示された権限を選択して、どちらかの列に移動します。

![使用可能な権限列と含まれる権限列を示す画像](./images/permissions/edit-permissions.png)

権限はカテゴリに分類されます。 カテゴリを切り替えるには、左のナビゲーションから目的のカテゴリを選択します。

![を示す画像 [!UICONTROL 販売のオプトアウト] 権限のあるセクション](./images/permissions/switch-category.png)

選択 **[!UICONTROL 保存]** 権限の設定が完了したら、次の手順に従います。

![製品プロファイルに保存される権限設定を示す画像](./images/permissions/save-permissions.png)

製品プロファイルビューが再び表示され、追加された権限が反映されます。

![製品プロファイルに追加された権限を示す画像](./images/permissions/permissions-added.png)

### プロファイルへのユーザーの割り当て {#assign-users}

製品プロファイルにユーザーを割り当て ( およびユーザーにプロファイルで設定された権限を付与するには、 **[!UICONTROL ユーザー]** タブ、続いて **[!UICONTROL ユーザーを追加]**.

![Admin Consoleの製品プロファイルの「ユーザー」タブを示す画像](./images/permissions/manage-users.png)

製品プロファイルのユーザー管理について詳しくは、 [Admin Console文書](https://helpx.adobe.com/jp/enterprise/using/manage-product-profiles.html).

### 従来の API 資格情報をプロファイルに移行 {#migrate-tech-accounts}

>[!NOTE]
>
>この節の説明は、Adobe Admin Consoleに統合される前に作成された既存の APIPrivacy Service情報にのみ適用されます。 新しい資格情報の場合、製品プロファイル（およびその権限）は、 [Adobe Developer Console プロジェクト](https://developer.adobe.com/developer-console/docs/guides/projects/) 代わりに、<br><br>詳しくは、 [プロジェクトへの製品プロファイルの割り当て](./api/getting-started.md#product-profiles) (Privacy ServiceAPI 入門ガイド ) を参照してください。

従来の API 資格情報を製品プロファイルに移行するには、「 」を選択します。 **[!UICONTROL API 資格情報]**&#x200B;に続いて **[!UICONTROL API 資格情報を追加]**.

![[!UICONTROL API 資格情報を追加] Admin Consoleで選択され、 [!UICONTROL API 資格情報] 製品プロファイルのタブ](./images/permissions/api-credentials.png)

リストから目的の開発者コンソールプロジェクトを選択し、「 」を選択します。 **[!UICONTROL 保存]** をクリックして、製品プロファイルに追加します。 これらのプロジェクトの資格情報を使用するすべての API 呼び出しは、製品プロファイルから付与された詳細な権限を継承します。

## 次の手順

このガイドでは、Privacy Serviceに使用できる権限と、Admin Consoleを通じた管理方法について説明しました。

製品プロファイルを設定した後に新しい API 統合を作成する手順については、 [Privacy ServiceAPI の概要ガイド](./api/getting-started.md). その他のAdobe Experience Platform機能に関する権限の管理について詳しくは、 [アクセス制御ドキュメント](../access-control/home.md).
