---
keywords: Experience Platform、ホーム、人気のあるトピック、Marketo Engage、marketo engage、marketo
solution: Experience Platform
title: Marketoソースコネクタの認証
topic-legacy: overview
description: このドキュメントでは、Marketo認証資格情報の生成方法に関する情報を提供します。
exl-id: 594dc8b6-cd6e-49ec-9084-b88b1fe8167a
source-git-commit: 50e92ac8c1eccc9ccfb6b078ad8b996817a6d693
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---

# [!DNL Marketo Engage] ソースコネクタの認証

[!DNL Marketo Engage]（以下「[!DNL Marketo]」と呼ばれる）ソースコネクタを作成する前に、まず [!DNL Marketo] インターフェイスを使用してカスタムサービスを設定し、Munchkin ID、クライアント ID、クライアントシークレットの値を取得する必要があります。

以下のドキュメントでは、[!DNL Marketo] ソースコネクタを作成するために認証資格情報を取得する手順を説明します。

## 新しい役割の設定

認証資格情報を取得する最初の手順は、[[!DNL Marketo]](https://app-sjint.marketo.com/#MM0A1) インターフェイスを使用して新しい役割を設定することです。

[!DNL Marketo] にログインし、上部のナビゲーションバーから **[!DNL Admin]** を選択します。

![新しい役割の管理者](../images/marketo/home.png)

*[!DNL Users & Role]s* ページには、ユーザー、役割、ログイン履歴に関する情報が含まれます。 新しい役割を作成するには、上部のヘッダーから「**[!DNL Roles]**」を選択し、「**[!DNL New Role]**」を選択します。

![新しい役割](../images/marketo/new-role.png)

**[!DNL Create New Role]**&#x200B;ダイアログボックスが開きます。名前と説明を入力し、この役割に付与する権限を選択します。 権限は特定のワークスペースに制限され、ユーザーは、権限を持つワークスペースでのみアクションを実行できます。

付与する権限を選択したら、**[!DNL Create]** を選択します。

![create-new-role](../images/marketo/create-new-role.png)

[!DNL Marketo] でロールを作成する際に、API で制限付き権限を管理できます。 「Access API」を選択する代わりに、次の権限を選択して、ロールに最低限のアクセスレベルを付与できます。

* [!DNL Read-Only Activity]
* [!DNL Read-Only Assets]
* [!DNL Read-Only Campaign]
* [!DNL Read-Only Company]
* [!DNL Read-Only Custom Object]
* [!DNL Read-Only Custom Object Type]
* [!DNL Read-Only Named Account]
* [!DNL Read-Only Named Account List]
* [!DNL Read-Only Opportunity]
* [!DNL Read-Only Person]
* [!DNL Read-Only Sales Person]

## 新しいユーザーの設定

役割と同様に、**[!DNL Users & Roles]** ページから新しいユーザーを設定できます。 **[!DNL Users]** ページには、現在Marketoでプロビジョニングされているアクティブなユーザーのリストが表示されます。 **[!DNL Invite New User]** を選択して新しいユーザーをプロビジョニングします。

![invite-new-user](../images/marketo/invite-new-user.png)

ポップオーバーダイアログメニューが表示されます。 E メールに適切な情報、名、姓、理由を入力します。 この手順の間に、招待する新しいユーザーアカウントへのアクセスの有効期限を設定することもできます。 終了したら **[!DNL Next]** を選択します。

>[!IMPORTANT]
>
>新しいユーザーを設定する場合は、作成するカスタムサービス専用のユーザーにアクセス権を割り当てる必要があります。

![user-info](../images/marketo/new-user-info.png)

**[!DNL Permissions]** 手順で適切なフィールドを選択し、**[!DNL API Only]** チェックボックスを選択して新しいユーザーに API の役割を指定します。 **[!DNL Next]** を選択して続行します。

![permissions](../images/marketo/permissions.png)

プロセスを完了するには、**[!DNL Send]** を選択します。

![メッセージ](../images/marketo/message.png)

## カスタムサービスの設定

新しいユーザーを確立したら、カスタムサービスを設定して新しい資格情報を取得できます。 管理ページで、「**[!DNL LaunchPoint]**」を選択します。

![admin-launchpoint](../images/marketo/admin-launchpoint.png)

**[!DNL Installed services]** ページには既存のサービスのリストが含まれています。新しいカスタムサービスを作成するには、**[!DNL New]** を選択し、**[!DNL New Service]** を選択します。

![新規サービス](../images/marketo/new-service.png)

新しいサービスにわかりやすい表示名を付け、**[!DNL Service]** ドロップダウンメニューから **[!DNL Custom]** を選択します。 適切な説明を入力し、**[!DNL API Only User]** ドロップダウンメニューからプロビジョニングするユーザーを選択します。 必要な情報を入力したら、**[!DNL Create]** を選択して新しいカスタムサービスを作成します。

![作成](../images/marketo/create.png)

## クライアント ID とクライアント秘密鍵の取得

新しいカスタムサービスを作成すると、クライアント ID とクライアント秘密鍵の値を取得できるようになります。 **[!DNL Installed Services]** メニューから、アクセスするカスタムサービスを探し、**[!DNL View Details]** を選択します。

![view-details](../images/marketo/view-details.png)

ダイアログボックスが開き、クライアント ID とクライアントの秘密鍵が表示されます。

![資格情報](../images/marketo/credentials.png)

## マンチキン ID の取得

[!DNL Marketo] ソースコネクタを認証するために、最後の手順は Munchkin ID を取得することです。 管理ページの **[!DNL Integration]** パネルで **[!DNL Munchkin]** を選択します。

![admin-munchkin](../images/marketo/admin-munchkin.png)

*[!DNL Munchkin]* ページが開き、固有の Munchkin ID がパネルの上部に表示されます。

![munchkin-id](../images/marketo/munchkin-id.png)

クライアント ID とクライアントの秘密鍵を組み合わせると、Munchkin ID を使用して新しいアカウントを設定し、[ 新しい  [!DNL Marketo]  ソース接続 ](../../../tutorials/ui/create/adobe-applications/marketo.md) をExperience Platformに作成できます。
