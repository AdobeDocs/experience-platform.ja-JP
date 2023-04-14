---
title: Privacy Service の権限の管理
description: Adobe Admin Console を使用して Adobe Experience Platform Privacy Service のユーザー権限を管理する方法について説明します。
exl-id: 6aa81850-48d7-4fff-95d1-53b769090649
source-git-commit: 1e164166f58540cbaaa4ad789b10cdfc40fa8a70
workflow-type: tm+mt
source-wordcount: '1634'
ht-degree: 60%

---

# Privacy Service の権限の管理

>[!IMPORTANT]
>
>Adobe Experience Platform Privacy Service の権限が改善されて、精度のレベルが向上しました。これらの変更により、組織管理者は、目的とする役割と権限レベルのアクセス権を、より多くのユーザーに付与できます。この近々の更新は重大な変更となるので、技術アカウントのユーザーは、Privacy Service の権限を更新する必要があります。 この権限の変更の適用は、 **2023 年 4 月 14 日**. 詳しくは、 [従来の API 資格情報の移行](#migrate-tech-accounts) を参照してください。
>
>技術アカウントは、大規模法人のお客様が利用でき、Adobe Developer Console を通じて作成されます。技術アカウント所有者の Adobe ID は、末尾が `@techacct.adobe.com` になっています。自分が技術アカウントの所有者かどうか不明な場合は、組織の管理者にお問い合わせください。

[Adobe Experience Platform Privacy Service](./home.md) へのアクセスは、Adobe Admin Console の詳細な役割ベースの権限によって制御されます。 ユーザーのグループに権限を割り当てる製品プロファイルを作成することにより、Privacy Service の [UI](./ui/overview.md) および [API](./api/overview.md) の機能にアクセスできるユーザーを決定できます。

>[!NOTE]
>
>Privacy Service API の統合を作成する場合、既存の製品プロファイルを選択して、その統合に権限を持つ機能やアクションを決定する必要があります。詳しくは、[Privacy Service API の基本を学ぶ](./api/getting-started.md)に関するガイドを参照してください。

このガイドでは、Privacy Service の権限を管理する方法について説明します。

## はじめに

Privacy Service のアクセス制御を設定するには、Adobe Experience Platform Privacy Service と製品が統合されている組織の管理者権限が必要です。権限を付与または取り消す最小の役割は、**製品プロファイル管理者**&#x200B;です。権限を管理できる他の管理者の役割は、**製品管理者**（製品内のすべてのプロファイルを管理）と&#x200B;**システム管理者**（制限なし）です。詳しくは、『Adobe エンタープライズ管理ガイド』の[管理者の役割](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html)に関する記事を参照してください。

このガイドは、製品プロファイルなどの基本的な Admin Console の概念と、製品の権限を個々のユーザーやグループに付与する方法について理解していることを前提としています。詳しくは、[Admin Console ユーザーガイド](https://helpx.adobe.com/jp/enterprise/using/admin-console.html)を参照してください。

## 使用可能な権限

次の表に、アクセス権を付与する特定の機能の説明と共に、Privacy Service で使用可能な権限の概要を示します。

>[!NOTE]
>
>すべてのPrivacy Serviceと [!UICONTROL 販売のオプトアウト] 権限は異なり、機能の重複がない間、互いに切り離されます。 これは、Privacy ServiceAPI がべき等と見なされるので可能です。

| カテゴリ | 権限 | 説明 |
| --- | --- | --- |
| [!UICONTROL Privacy Service の権限] | [!UICONTROL プライバシー読み取り権限] | ユーザーが既存のアクセスリクエストおよび削除リクエストとその詳細を表示できるかどうかを決定します。 |
| [!UICONTROL Privacy Service の権限] | [!UICONTROL プライバシー書き込み権限] | ユーザーが新しいアクセスリクエストおよび削除リクエストを作成できるかどうかを決定します。 |
| [!UICONTROL Privacy Service の権限] | [!UICONTROL コンテンツ配信の読み取り（アクセス）権限] | アクセスリクエストが Privacy Service によって処理されると、顧客のデータを含む ZIP ファイルがその顧客に送信されます。アクセスリクエストの詳細を検索する場合、この権限によって、ユーザーがリクエストの ZIP ファイルのダウンロードリンクにアクセスできるかどうかが決まります。 |
| [!UICONTROL 販売のオプトアウト権限] | [!UICONTROL 読み取り権限 - 販売のオプトアウト] | ユーザーが既存の販売のオプトアウトリクエストとその詳細を表示できるかどうかを決定します。 |
| [!UICONTROL 販売のオプトアウト権限] | [!UICONTROL 書き込み権限 - 販売のオプトアウト] | ユーザーが新しい販売のオプトアウトリクエストを作成できるかどうかを決定します。 |

{style="table-layout:auto"}

## 権限の管理 {#manage}

Privacy Service の権限を管理するには、[Admin Console](https://adminconsole.adobe.com/) にログインし、上部ナビゲーションから「**[!UICONTROL 製品]**」を選択します。こちらから、「**[!UICONTROL Adobe Experience Platform Privacy Service]**」を選択します。

![Admin Console の Privacy Service 製品カードを示す画像](./images/permissions/privacy-service-card.png)

### 製品プロファイルの選択または作成

次の画面には、組織で使用可能な Privacy Service の製品プロファイルのリストが表示されます。製品プロファイルが存在しない場合は、「**[!UICONTROL 新規プロファイル]**」を選択して作成します。組織内に異なるレベルのアクセスを必要とする複数の役割またはユーザーグループがある場合は、それぞれに個別の製品プロファイルを作成する必要があります。

![Admin Console の Privacy Service の製品プロファイルを示す画像](./images/permissions/select-or-create-profile.png)

製品プロファイルを選択したら、「**[!UICONTROL 権限]**」タブを使用してプロファイルの[権限の編集](#edit-permissions)を開始するか、「**[!UICONTROL ユーザー]**」タブを選択してプロファイルへの[ユーザーの割り当て](#assign-users)を開始できます。

![製品プロファイル Admin Console の「権限」タブを示す画像](./images/permissions/users-permissions-tabs.png)

### プロファイルの権限の編集 {#edit-permissions}

「**[!UICONTROL 権限]**」タブで、表示されている権限カテゴリのいずれかを選択して、権限編集表示にアクセスします。

プロファイルの権限を編集する場合、使用可能な権限が左側の列にリストされ、プロファイルに含まれている権限が右側の列にリストされます。リストされた権限を選択して、いずれかの列間で移動します。

![使用可能な権限列と含まれている権限列を示す画像](./images/permissions/edit-permissions.png)

権限はカテゴリに分類されています。カテゴリを切り替えるには、左のナビゲーションから目的のカテゴリを選択します。

![権限の下の「[!UICONTROL 販売のオプトアウト]」セクションを示す画像](./images/permissions/switch-category.png)

権限の設定が完了したら、「**[!UICONTROL 保存]**」を選択します。

![製品プロファイル用に権限設定が保存されていることを示す画像](./images/permissions/save-permissions.png)

追加された権限が反映された製品プロファイルビューが再表示されます。

![製品プロファイル用に追加された権限を示す画像](./images/permissions/permissions-added.png)

### プロファイルへのユーザーの割り当て {#assign-users}

ユーザーを製品プロファイルに割り当て（およびプロファイルで設定された権限を付与）するには、「**[!UICONTROL ユーザー]**」タブに続いて「**[!UICONTROL ユーザーを追加]**」を選択します。

![Admin Console の製品プロファイル用の「ユーザー」タブを示す画像](./images/permissions/manage-users.png)

製品プロファイル用のユーザー管理について詳しくは、[Admin Console のドキュメント](https://helpx.adobe.com/jp/enterprise/using/manage-product-profiles.html)を参照してください。

### 従来の API 資格情報のプロファイルへの移行 {#migrate-tech-accounts}

>[!NOTE]
>
>この節の説明は、Privacy Service 権限が Adobe Admin Console に統合される前に作成された既存の API 資格情報にのみ適用されます。新しい資格情報の場合、代わりに、[Adobe Developer Console プロジェクト](https://developer.adobe.com/developer-console/docs/guides/projects/)を通じて製品プロファイル（およびその権限）が割り当てられます。<br><br>詳しくは、[プロジェクトへの製品プロファイルの割り当て](./api/getting-started.md#product-profiles)（Privacy Service API 入門ガイド）に関する節を参照してください。

以前は、技術アカウントでは、統合と権限に対して製品プロファイルは必要ありませんでした。 ただし、Privacy Service権限が最近改善されたので、従来の API 資格情報を製品プロファイルに移行する必要が生じました。 この更新により、テクニカルアカウント所有者に対して詳細な権限を付与できます。 以下の手順に従って、Privacy Serviceのテクニカルアカウント権限を更新します。

#### テクニカルアカウント権限の更新 {#update-tech-account-permissions}

テクニカルアカウントに権限セットを割り当てる最初の手順は、 [Adobe Admin Console](https://adminconsole.adobe.com/) をクリックし、Privacy Service用の新しい製品プロファイルを作成します。

Admin ConsoleUI から、 **製品** ナビゲーションバーから、 **[!UICONTROL Experience Cloud]** および **[!UICONTROL Adobe Experience Platform Privacy Service]** 左側のサイドバーに表示されます。 この [!UICONTROL 製品プロファイル] 」タブが表示されます。 選択 **新しいプロファイル** をクリックして、Privacy Service用の新しい製品プロファイルを作成します。

![Adobe Admin Consoleの「Experience PlatformPrivacy Service製品プロファイル」タブで、「新しいプロファイル」がハイライトされています。](./images/permissions/create-product-profile.png)

この [!UICONTROL 新しい製品プロファイルの作成] ダイアログが表示されます。 製品プロファイルの作成方法に関する完全な手順については、 [プロファイル作成用 UI ガイド](../access-control/ui/create-profile.md).

新しい製品プロファイルを保存したら、 [Adobe Developer Console](https://developer.adobe.com/console/home) その製品またはそのプロジェクトにログインします。 選択 **[!UICONTROL プロジェクト]** 上部のナビゲーションから、プロジェクトのカードの順に選択します。

>[!NOTE]
>
>キャッシュをクリアするか、新しいプロジェクトが開発者コンソールプロジェクトのリストに表示されるまでしばらく待つ必要がある場合があります。

プロジェクトにログインした後、 **[!UICONTROL Privacy ServiceAPI]** 左側のサイドバーからの統合。

![プロジェクト API とPrivacy ServiceAPI がハイライトされたAdobe Developerコンソールの「プロジェクト」タブ。](./images/permissions/login-to-dev-console-project.png)

「Privacy ServiceAPI 統合」ダッシュボードが表示されます。 このダッシュボードから、そのプロジェクトに関連付けられている製品プロファイルを編集できます。 選択 **[!UICONTROL 製品プロファイルの編集]** をクリックしてプロセスを開始します。 この [!UICONTROL API の設定] ダイアログが表示されます。

![Adobe Developerコンソールの「Privacy ServiceAPI 統合」ダッシュボード（「製品プロファイルを編集」がハイライトされています）](./images/permissions/edit-product-profiles.png)

この [!UICONTROL API の設定] ダイアログには、サービスに現在存在する使用可能な製品プロファイルが表示されます。 管理コンソールで作成した製品プロファイルと関連付けられます。 使用可能な製品プロファイルのリストから、Admin Console でテクニカルアカウント用に作成した新しい製品プロファイルのチェックボックスを選択します。 これにより、このテクニカルアカウントが、選択した製品プロファイルの権限に自動的に関連付けられます。 選択 **[!UICONTROL 設定済み API を保存]** をクリックして設定を確定します。

>[!NOTE]
>
>テクニカルアカウントが既に製品プロファイルに関連付けられている場合、使用可能な製品プロファイルのリストにあるチェックボックスの 1 つが既に選択されています。

![Adobe Developer Console の API を設定ダイアログに、製品プロファイルのチェックボックスと「設定済み API を保存」がハイライト表示されています。](./images/permissions/select-profile-for-tech-account.png)

#### 設定が適用されていることを確認します {#confirm-applied-settings}

設定がアカウントに適用されたことを確認するには、以下を実行します。 に戻る [Admin Console](https://adminconsole.adobe.com/) 新しく作成した製品プロファイルに移動します。 を選択します。 **[!UICONTROL API 資格情報]** タブをクリックし、関連するプロジェクトのリストを表示します。 製品プロファイルをテクニカルアカウントに割り当てた開発者コンソールで使用されるプロジェクトが、資格情報のリストに表示されます。 各 API 秘密鍵証明書の名前は、プロジェクト名と、ランダムに生成された番号が末尾に付加されたもので構成されます。 秘密鍵証明書を選択してを開きます。 [!UICONTROL 詳細] パネル。

![「 API 資格情報」タブが表示され、Admin Console内の製品プロファイルと、一連のプロジェクト資格情報がハイライト表示されています。](./images/permissions/confirm-credentials-in-admin-console.png)

この [!UICONTROL 詳細] パネルには、関連する技術 ID、API キー、作成日、最終変更日、および関連するAdobe製品など、API 資格情報に関する情報が含まれています。

![Admin Console内の API 秘密鍵証明書で強調表示されている詳細パネル。](./images/permissions/admin-console-details-panel.png)

## 次の手順

このガイドでは、Privacy Service に使用できる権限と、Admin Console を通じた管理方法について説明しました。

製品プロファイルを設定した後に新しい API 統合を作成する手順については、[Privacy Service API の入門ガイド](./api/getting-started.md)を参照してください。 その他の Adobe Experience Platform 機能に関する権限の管理について詳しくは、[アクセス制御のドキュメント](../access-control/home.md)を参照してください。
