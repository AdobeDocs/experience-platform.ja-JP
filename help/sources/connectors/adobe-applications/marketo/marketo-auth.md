---
keywords: Experience Platform；ホーム；人気の高いトピック；Marketo Engage;marketo engage;marketo
solution: Experience Platform
title: Marketoソースコネクタの認証
description: このドキュメントでは、Marketo認証資格情報の生成方法に関する情報を提供します。
exl-id: 594dc8b6-cd6e-49ec-9084-b88b1fe8167a
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 1%

---

# の認証 [!DNL Marketo Engage] ソースコネクタ

事前に [!DNL Marketo Engage] （以下「」という。）[!DNL Marketo]&quot;) ソースコネクタの場合、最初に [!DNL Marketo] インターフェイスに加えて、Munchkin ID、クライアント ID、クライアントシークレットの値を取得します。

以下のドキュメントでは、 [!DNL Marketo] ソースコネクタ。

## 新しい役割の設定

認証資格情報を取得する最初の手順は、 [[!DNL Marketo]](https://app-sjint.marketo.com/#MM0A1) インターフェイス。

にログインします。 [!DNL Marketo] を選択し、 **[!DNL Admin]** 上部ナビゲーションバーから。

![新しいロールの管理者](../images/marketo/home.png)

この *[!DNL Users & Role]s* ページには、ユーザー、役割、ログイン履歴に関する情報が含まれています。 新しいロールを作成するには、「 」を選択します。 **[!DNL Roles]** 上部のヘッダーから、「 」を選択します。 **[!DNL New Role]**.

![新規ロール](../images/marketo/new-role.png)

**[!DNL Create New Role]**&#x200B;ダイアログボックスが開きます。名前と説明を入力し、この役割に付与する権限を選択します。 権限は特定のワークスペースに制限され、ユーザーは、権限を持つワークスペースでのみアクションを実行できます。

付与する権限を選択したら、 **[!DNL Create]**.

![create-new-role](../images/marketo/create-new-role.png)

でロールを作成する際に、API で制限付き権限を管理できます。 [!DNL Marketo]. 「Access API」を選択する代わりに、次の権限を選択して、ロールに最小限のアクセスレベルを付与できます。

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

役割と同様に、新しいユーザーを **[!DNL Users & Roles]** ページ。 この **[!DNL Users]** ページには、現在Marketoでプロビジョニングされているアクティブなユーザーのリストが表示されます。 選択 **[!DNL Invite New User]** 新しいユーザーをプロビジョニングする。

![invite-new-user](../images/marketo/invite-new-user.png)

ポップオーバーダイアログメニューが表示されます。 電子メールに適した情報、名、姓、理由を入力します。 この手順の間に、招待する新しいユーザーアカウントのアクセスの有効期限を設定することもできます。 終了したら「**[!DNL Next]**」を選択します。

>[!IMPORTANT]
>
>新しいユーザーを設定する場合は、作成するカスタムサービス専用のユーザーにアクセス権を割り当てる必要があります。

![user-info](../images/marketo/new-user-info.png)

適切なフィールドを **[!DNL Permissions]** ステップと選択 **[!DNL API Only]** 」チェックボックスをオンにして、新しいユーザーに API の役割を割り当てます。 選択 **[!DNL Next]** をクリックして続行します。

![permissions](../images/marketo/permissions.png)

プロセスを完了するには、「 **[!DNL Send]**.

![メッセージ](../images/marketo/message.png)

## カスタムサービスの設定

新しいユーザーを確立したら、カスタムサービスを設定して新しい資格情報を取得できます。 管理ページで、「 」を選択します。 **[!DNL LaunchPoint]**.

![admin-launchpoint](../images/marketo/admin-launchpoint.png)

この **[!DNL Installed services]** ページに既存のサービスの一覧が含まれる場合、新しいカスタムサービスを作成するには、「 」を選択します。 **[!DNL New]** 次に、 **[!DNL New Service]**.

![新規サービス](../images/marketo/new-service.png)

新しいサービスにわかりやすい表示名を付け、「 」を選択します。 **[!DNL Custom]** から **[!DNL Service]** ドロップダウンメニュー。 適切な説明を入力し、プロビジョニングするユーザーを **[!DNL API Only User]** ドロップダウンメニュー。 必要な情報を入力したら、「 」を選択します。 **[!DNL Create]** をクリックして、新しいカスタムサービスを作成します。

![作成](../images/marketo/create.png)

## クライアント ID とクライアント秘密鍵を取得する

新しいカスタムサービスを作成したら、クライアント ID とクライアント秘密鍵の値を取得できるようになりました。 次の **[!DNL Installed Services]** メニューで、アクセスするカスタムサービスを探し、 **[!DNL View Details]**.

![view-details](../images/marketo/view-details.png)

ダイアログボックスが開き、クライアント ID とクライアントの秘密鍵が表示されます。

![資格情報](../images/marketo/credentials.png)

## Munchkin ID の取得

最後の手順を完了して、 [!DNL Marketo] ソースコネクタは、Munchkin ID を取得します。 管理ページで、「 」を選択します。 **[!DNL Munchkin]** の下に **[!DNL Integration]** パネル。

![admin-munchkin](../images/marketo/admin-munchkin.png)

この *[!DNL Munchkin]* 」ページが表示され、一意の Munchkin ID がパネルの上部に表示されます。

![munchkin-Id](../images/marketo/munchkin-id.png)

お使いのクライアント ID とクライアントの秘密鍵を組み合わせて、Munchkin ID を使用して新しいアカウントを設定し、 [新しい [!DNL Marketo] ソース接続](../../../tutorials/ui/create/adobe-applications/marketo.md) Experience Platform
