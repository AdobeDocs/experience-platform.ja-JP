---
keywords: Experience Platform；ホーム；人気の高いトピック；Marketo Engage；マーケティング担当；マーケティング担当
solution: Experience Platform
title: Marketoソースコネクタを認証する
topic-legacy: overview
description: このドキュメントでは、Marketo認証資格情報を生成する方法について説明します。
translation-type: tm+mt
source-git-commit: f12baaa9d4b37f1101792a4ae479b5a62893eb68
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 0%

---


# （ベータ版）[!DNL Marketo Engage]ソースコネクタを認証する

>[!IMPORTANT]
>
>[!DNL Marketo Engage]ソースは現在ベータ版です。 機能とドキュメントは変更される可能性があります。

[!DNL Marketo Engage] （以下「[!DNL Marketo]」と呼びます）ソースコネクタを作成する前に、まず[!DNL Marketo]インターフェイスを介してカスタムサービスを設定し、Munchkin ID、クライアントID、クライアントシークレットの値を取得する必要があります。

次のドキュメントは、[!DNL Marketo]ソースコネクタを作成するための認証資格情報の取得方法に関する手順を示しています。

## 新しいロールの設定

認証資格情報を取得する最初の手順は、[[!DNL Marketo]](https://app-sjint.marketo.com/#MM0A1)インターフェイスを介して新しい役割を設定することです。

[!DNL Marketo]にログインし、上部ナビゲーションバーで&#x200B;**[!DNL Admin]**&#x200B;を選択します。

![新しいロールの管理者](../images/marketo/home.png)

*[!DNL Users & Role]s*&#x200B;ページには、ユーザー、役割、ログイン履歴に関する情報が含まれています。 新しいロールを作成するには、上部のヘッダーから「**[!DNL Roles]**」を選択し、「**[!DNL New Role]**」を選択します。

![新役](../images/marketo/new-role.png)

**[!DNL Create New Role]**&#x200B;ダイアログボックスが開きます。名前と説明を入力し、このロールに付与する権限を選択します。 権限は特定のワークスペースに制限され、ユーザーは権限を持つワークスペースでのみ操作を実行できます。

付与する権限を選択したら、**[!DNL Create]**&#x200B;を選択します。

![create-new-role](../images/marketo/create-new-role.png)

[!DNL Marketo]でロールを作成する場合、APIの制限付き権限を管理できます。 「アクセスAPI」を選択する代わりに、次の権限を選択して、ロールに最小限のアクセスレベルを与えることができます。

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

ロールと同様に、**[!DNL Users & Roles]**&#x200B;ページから新しいユーザーを設定できます。 **[!DNL Users]**&#x200B;ページには、現在Marketoでプロビジョニングされているアクティブなユーザーのリストが表示されます。 **[!DNL Invite New User]**&#x200B;を選択して、新しいユーザーをプロビジョニングします。

![invite-new-user](../images/marketo/invite-new-user.png)

ポーバーダイアログメニューが表示されます。 電子メール、名、姓、理由に適した情報を入力します。 この手順では、招待する新しいユーザーアカウントへのアクセスの有効期限を設定することもできます。 終了したら、**[!DNL Next]**&#x200B;を選択します。

>[!IMPORTANT]
>
>新しいユーザーを設定する場合、作成するカスタムサービス専用のユーザーにアクセス権を割り当てる必要があります。

![user-info](../images/marketo/new-user-info.png)

**[!DNL Permissions]**&#x200B;手順で適切なフィールドを選択し、**[!DNL API Only]**&#x200B;チェックボックスを選択して、新しいユーザーにAPIロールを提供します。 **[!DNL Next]**&#x200B;を選択して続行します。

![permissions](../images/marketo/permissions.png)

処理を完了するには、**[!DNL Send]**&#x200B;を選択します。

![message](../images/marketo/message.png)

## カスタムサービスの設定

新しいユーザーを確立すると、カスタムサービスを設定して、新しい資格情報を取得できます。 管理ページで&#x200B;**[!DNL LaunchPoint]**&#x200B;を選択します。

![admin-launchpoint](../images/marketo/admin-launchpoint.png)

**[!DNL Installed services]**&#x200B;ページには既存のサービスのリストが含まれています。新しいカスタムサービスを作成するには、**[!DNL New]**&#x200B;を選択し、**[!DNL New Service]**&#x200B;を選択します。

![新サービス](../images/marketo/new-service.png)

新しいサービスにわかりやすい表示名を付け、[**[!DNL Service]**]ドロップダウンメニューから[**[!DNL Custom]**]を選択します。 適切な説明を入力し、プロビジョニングするユーザーを[**[!DNL API Only User]**]ドロップダウンメニューから選択します。 必要な情報を入力したら、**[!DNL Create]**&#x200B;を選択して新しいカスタムサービスを作成します。

![create](../images/marketo/create.png)

## クライアントIDとクライアントシークレットの取得

新しいカスタムサービスを作成すると、クライアントIDとクライアントシークレットの値を取得できるようになります。 **[!DNL Installed Services]**&#x200B;メニューからアクセスするカスタムサービスを探し、**[!DNL View Details]**&#x200B;を選択します。

![表示の詳細](../images/marketo/view-details.png)

ダイアログボックスが表示され、クライアントIDとクライアントシークレットが表示されます。

![資格情報](../images/marketo/credentials.png)

## マンチキンIDの取得

[!DNL Marketo]ソースコネクタを認証するためには、最後の手順を完了する必要があります。 Munchkin IDを取得します。 管理ページで、**[!DNL Integration]**&#x200B;パネルの下の&#x200B;**[!DNL Munchkin]**&#x200B;を選択します。

![管理人](../images/marketo/admin-munchkin.png)

*[!DNL Munchkin]*&#x200B;ページが表示され、一意のMunchkin IDがパネルの上部に表示されます。

![マンチキンID](../images/marketo/munchkin-id.png)

クライアントIDとクライアントシークレットを組み合わせると、Munchkin IDを使って新しいアカウントを設定し、[Experience Platform上に新しい [!DNL Marketo] ソース接続](../../../tutorials/ui/create/adobe-applications/marketo.md)を作成できます。
