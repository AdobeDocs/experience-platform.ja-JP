---
title: Experience Platform内のデータ収集の権限管理
description: Adobe Experience Platformの権限を管理し、データ収集機能へのアクセスを制御する方法の概要です。
exl-id: 8426d54b-ec1d-475a-a769-f45a8c924fe7
source-git-commit: 60590a77859320891717244eec58b556935354b5
workflow-type: tm+mt
source-wordcount: '1338'
ht-degree: 28%

---

# Experience Platform内のデータ収集の権限管理

[Adobe Experience Platformのデータ収集 ](./home.md) は、データを収集して転送するために連携するいくつかの異なるテクノロジーで構成されています。 これらのテクノロジーへのアクセスは、Adobe Admin Consoleの詳細な役割ベースの権限によって制御されます。

このガイドでは、データ収集機能の権限を管理する方法について説明します。

## はじめに

データ収集のアクセス制御を設定するには、Adobe Experience Platform Data Collection と連携する製品を所有する組織の管理者権限が必要です。 権限を付与または取り消す最小の役割は、**製品プロファイル管理者**&#x200B;です。権限を管理できる他の管理者の役割は、**製品管理者**（製品内のすべてのプロファイルを管理）と&#x200B;**システム管理者**（制限なし）です。詳しくは、『Adobe エンタープライズ管理ガイド』の[管理者の役割](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html)に関する記事を参照してください。

このガイドは、製品プロファイルなどの基本的な Admin Console の概念と、製品の権限を個々のユーザーやグループに付与する方法について理解していることを前提としています。詳しくは、[Admin Console ユーザーガイド](https://helpx.adobe.com/jp/enterprise/using/admin-console.html)を参照してください。

## 使用可能な権限

データ収集に関連する権限は、Admin Consoleで **Adobe Experience Platformと** 2}Adobe Experience Platform Data Collection} の 2 つの製品指定を通じて提供され **す。**&#x200B;以下の節では、各製品で提供される権限の概要と、権限によってアクセス権が付与される特定の機能の説明を説明します。

### Adobe Experience Platformの権限

Adobe Experience Platformでの権限には、データストリーム、ID、スキーマおよびサンドボックスへのアクセスが含まれます。 Adobe Experience Platformの権限を設定する手順については、『 [ アクセス制御ユーザーガイド ](../access-control/ui/overview.md) 』を参照してください。

| カテゴリ | 権限 | 説明 |
| --- | --- | --- |
| サンドボックス | （N/A） | 組織の下に作成した [ サンドボックス ](../sandboxes/home.md) に応じて、Admin Consoleのこの権限カテゴリを使用して、各サンドボックスへのアクセスを制御できます。 |
| データモデリング | スキーマの管理 | [ エクスペリエンスデータモデル（XDM）スキーマ ](../xdm/home.md) を表示、作成および編集する機能を付与します。 |
| データモデリング | スキーマの表示 | スキーマへの読み取り専用アクセスを許可します。 |
| Identity Management | ID 名前空間の管理 | [ID 名前空間 ](../identity-service/features/namespaces.md) の表示、作成および編集機能を付与します。 |
| Identity Management | ID 名前空間の表示 | ID 名前空間への読み取り専用アクセスを許可します。 |
| データ収集 | データストリームの管理 | [ データストリーム ](../datastreams/overview.md) の表示、作成、編集の機能を付与します。 |
| データ収集 | データストリームを表示 | データストリームへの読み取り専用アクセスを許可します。 |

{style="table-layout:auto"}

### Adobe Experience Platform Data Collection の権限

Adobe Experience Platform Data Collection の下にある権限は、プロパティ、拡張機能、環境を含む、タグおよびイベント転送機能へのアクセスを制御します。 Adobe Experience Platform データ収集の権限を設定する手順については、[ 以下の節 ](#manage) を参照してください。

| カテゴリ | 権限 | 説明 |
| --- | --- | --- |
| Platform | Web | 他のプロパティ権限と組み合わせると [web プロパティ ](../tags/ui/administration/companies-and-properties.md) へのアクセス権を付与します。 |
| Platform | Mobile | 他のプロパティ権限と組み合わせると、[ モバイルプロパティ ](../tags/ui/administration/companies-and-properties.md) へのアクセス権を付与します。 |
| Platform | Edge | 他のプロパティ権限と組み合わせると、[ イベント転送Edge プロパティ ](../tags/ui/event-forwarding/getting-started.md) へのアクセス権を付与します。 |
| プロパティ | （N/A） | 組織内で作成したプロパティに応じて、Admin Consoleのこの権限カテゴリを使用して各プロパティへのアクセスを制御できます。<br><br> ユーザーに割り当てられたプロパティ権限は、この権限カテゴリを通じてアクセス権を付与されたプロパティにのみ適用されます。 |
| プロパティ権限 | 承認 | [ 公開フロー ](../tags/ui/publishing/publishing-flow.md) の一部としてライブラリビルドを承認する機能を付与します。 |
| プロパティ権限 | 開発 | [ 公開フロー ](../tags/ui/publishing/publishing-flow.md) の一部としてライブラリビルドを開発する機能を付与します。 |
| プロパティ権限 | プロパティを編集 | ユーザーがアクセスできるプロパティの基本設定を編集する機能を付与します。 |
| プロパティ権限 | 環境の管理 | ユーザーがアクセスできるプロパティの [ 環境 ](../tags/ui/publishing/environments.md) を管理する機能を付与します。 |
| プロパティ権限 | 拡張機能の管理 | ユーザーがアクセスできるプロパティの [ 拡張機能 ](../tags/ui/managing-resources/extensions/overview.md) を管理する機能を付与します。 |
| プロパティ権限 | 公開 | [ 公開フロー ](../tags/ui/publishing/publishing-flow.md) の一部としてライブラリビルドを公開する機能を付与します。 |
| 会社権限 | 拡張機能の開発 | 組織が所有する拡張機能パッケージを作成および変更する機能を付与します。これには、非公開リリースと、公開リリースのリクエストが含まれます。 |
| 会社権限 | アプリ設定の管理 | この権限は、モバイルのアプリ内メッセージとプッシュメッセージへのアクセス権を付与するAdobe Journey Optimizerまたは他のソリューションのライセンスをお持ちの場合にのみ適用されます。 これにより、Adobe Experience Cloudが認識しているアプリを、Firebase Cloud Messaging サービスおよびApple Push Notification サービスとの通信に必要なプッシュ認証情報と共に管理できます。 |
| 会社権限 | プロパティの管理 | タグ（Web プロパティ）、イベント転送（Edge プロパティ）、モバイルプロパティを作成および管理する機能を付与されます。 |

{style="table-layout:auto"}

>[!NOTE]
>
>一般的なシナリオの管理戦略など、これらの権限がタグの機能に与える影響について詳しくは、[ ユーザー権限 ](../tags/ui/administration/user-permissions.md) に関するタグドキュメントを参照してください。

## 権限の管理 {#manage}

データ収集のアクセス許可は、次の 2 つの製品指定を通じて管理されます。**Adobe Experience Platform** および **Adobe Experience Platform データ収集**。

Admin Consoleの各製品で関連する権限を管理する手順については、次のサブセクションを参照してください。

* [Adobe Experience Platformの権限](#manage-platform)
* [Adobe Experience Platform Data Collection の権限](#manage-collection)

### Adobe Experience Platformでの権限の管理 {#manage-platform}

>[!NOTE]
>
>役割の権限を管理するには、管理者権限が必要です。 管理者権限がない場合は、システム管理者にお問い合わせください。

Experience Cloudの **[!UICONTROL 権限]** セクションでは、製品アプリケーション内の機能とオブジェクトへのアクセスを管理するユーザーの役割とポリシーを定義できます。

[!UICONTROL  権限 ] を通じて、役割を作成および管理し、それらの役割に対して必要なリソース権限を割り当てることができます。

![ 権限製品をハイライト表示したAdobe Experience Cloud。](./images/permissions/permissions-product.png)

データ収集機能にアクセスするには、**[!UICONTROL サンドボックス]**、**[!UICONTROL データモデリング]**、**[!UICONTROL Identity Management]**、{ データ収集 **[!UICONTROL の各カテゴリですべての権限を有効にする必要が]** ります。

![Admin Console内のデータ収集プロダクトカードを示す画像 ](./images/permissions/platform-permission-card.png)

Platform 権限の管理手順について詳しくは、[ アクセス制御 UI ガイド ](../access-control/ui/overview.md) を参照してください。

>[!NOTE]
>
>組織がアクセスできる製品 SKU によっては、使用できるすべての Platform 権限がない場合があります。

### Adobe Experience Platform Data Collection での権限の管理 {#manage-collection}

これらの権限を管理するには、Admin Consoleにログインし、上部ナビゲーションから **[!UICONTROL 製品]** を選択してから、「**[!UICONTROL Adobe Experience Platform Data Collection]**」を選択します。

![Admin Console内のデータ収集プロダクトカードを示す画像 ](./images/permissions/data-collection-card.png)

#### 製品プロファイルの選択または作成

次の画面には、組織で使用可能なデータ収集用の製品プロファイルのリストが表示されます。デフォルトのプロファイルは **[!DNL Default Data Collection All Access]** です。 必要に応じて、デフォルトの製品プロファイルを編集することも、「新しいプロファイル **[!UICONTROL を選択して作成するこ]** もできます。 組織内に異なるレベルのアクセスを必要とする複数の役割またはユーザーグループがある場合は、それぞれに個別の製品プロファイルを作成する必要があります。

![Admin Consoleのデータ収集用の製品プロファイルを示す画像 ](./images/permissions/new-profile.png)

製品プロファイルを選択または作成したら、**[!UICONTROL 編集]** アイコンを使用してプロファイルの [ 権限の編集 ](#edit-permissions) を開始するか、「**[!UICONTROL ユーザー]** タブを選択してプロファイルへの [ ユーザーの割り当て ](#assign-users) を開始できます。

![製品プロファイル Admin Console の「権限」タブを示す画像](./images/permissions/edit-permission-categories.png)

#### 製品プロファイルの権限の編集 {#edit-permissions}

プロファイルの権限を編集する場合、使用可能な権限が左側の列にリストされ、プロファイルに含まれている権限が右側の列にリストされます。リストされた権限を選択して、いずれかの列間で移動します。

![ 「含まれる」列に追加された権限を示す画像 ](./images/permissions/added-permissions.png)

権限はカテゴリに分類されています。カテゴリを切り替えるには、左のナビゲーションから目的のカテゴリを選択します。

![ 権限の下の「会社権限」セクションを示す画像 ](./images/permissions/switch-category.png)

権限の設定が完了したら、「**[!UICONTROL 保存]**」を選択します。

![製品プロファイル用に権限設定が保存されていることを示す画像](./images/permissions/save-permissions.png)

追加された権限が反映された製品プロファイルビューが再表示されます。

![製品プロファイル用に追加された権限を示す画像](./images/permissions/permissions-added.png)

#### 製品プロファイルへのユーザーの割り当て {#assign-users}

ユーザーを製品プロファイルに割り当て（およびプロファイルで設定された権限を付与）するには、「**[!UICONTROL ユーザー]**」タブに続いて「**[!UICONTROL ユーザーを追加]**」を選択します。

![Admin Console の製品プロファイル用の「ユーザー」タブを示す画像](./images/permissions/manage-users.png)

製品プロファイル用のユーザー管理について詳しくは、[Admin Console のドキュメント](https://helpx.adobe.com/jp/enterprise/using/manage-product-profiles.html)を参照してください。

## 次の手順

このガイドでは、データ収集に使用できる権限と、Admin Consoleを通じた管理方法について説明しました。 その他の Adobe Experience Platform 機能に関する権限の管理について詳しくは、[アクセス制御のドキュメント](../access-control/home.md)を参照してください。
