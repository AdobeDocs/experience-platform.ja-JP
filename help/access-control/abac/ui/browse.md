---
keywords: Experience Platform；ホーム；人気のトピック；アクセス制御；属性ベースのアクセス制御；ABAC
title: 属性ベースのアクセス制御の参照
description: このドキュメントでは、Adobe Experience Platformの属性ベースのアクセス制御に関する情報を提供します
hide: true
hidefromtoc: true
exl-id: 39634bde-8858-44a6-b39a-776846654fc1
source-git-commit: 143db2c19ec5ee7628b5cb9b30e71f24b4b3dcc8
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 20%

---

# 権限ガイド

>[!IMPORTANT]
>
>現在、米国ベースの医療関連のお客様向けに、属性ベースのアクセス制御が限定的なリリースで提供されています。 この機能は、完全にリリースされると、すべてのReal-time Customer Data Platformのお客様が利用できるようになります。

権限はAdobe Experience Cloudの領域で、管理者がユーザーの役割とアクセスポリシーを定義して、製品アプリケーション内の機能およびオブジェクトのアクセス権限を管理できます。

権限を使用して、次の項目を設定できます。

* [ラベル](./labels.md)
* [権限](./permissions.md)
* [ポリシー](./permissions.md)
* [ロール](./roles.md)
* [サンドボックス](./sandboxes.md)
* [ユーザー](./users.md)

の属性ベースのアクセス制御権限にアクセスするには [!DNL Experience Cloud]のサブスクリプションを購入している組織の管理者である必要があります。 [!DNL Experience Cloud]. Adobeは組織に対して柔軟な管理者階層を提供していますが、権限を設定するにはAdobe Experience Platformの製品管理者である必要があります。 詳しくは、[管理者の役割](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html)に関する Adobe Help Center の記事を参照してください。

管理者権限がない場合は、システム管理者に問い合わせてアクセス権を取得してください。

管理者権限を取得したら、に移動します。 [Adobe Experience Cloud](https://experience.adobe.com/) にログインし、Adobeの資格情報を使用してログインします。 ログイン後、 **[!UICONTROL 概要]** ページが表示されます。 このページには、組織が購読している製品と、組織全体にユーザーおよび管理者を追加するためのその他のコントロールが表示されます。 選択 **[!UICONTROL 権限]** をクリックして、Platform 統合用の属性ベースのアクセス制御ワークスペースを開きます。

![flac-select-product](../../images/flac-ui/flac-select-product.png)

Adobe Experience Cloudの属性ベースのアクセス制御ワークスペースが表示され、 **[!UICONTROL 役割]** ページ。 このページでは、このドキュメントで説明されているすべての役割を表示し、様々な設定を管理できます。

>[!IMPORTANT]
>
>組織で属性ベースのアクセス制御を有効にしたら、Adobe Admin Consoleの製品プロファイルではなくAdobe Experience Cloudで権限を使用して、組織のユーザー、機能、ラベル、その他のリソースに対する権限を管理できます。

![flac-select-roles](../../images/flac-ui/flac-select-roles.png)

このユーザーガイドは、[!DNL Experience Cloud] を使用して Platform の権限を割り当てる方法を説明します。[!DNL Admin Console] のナビゲーション方法に関する一般情報については、「[Admin Console ユーザーガイド](https://helpx.adobe.com/jp/enterprise/using/admin-console.html)」を参照してください。

## 次の手順

権限ワークスペースに移動したら、次の手順に進み、 [新しい役割を作成する](roles.md).
