---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年10月）
doc-type: release notes
last-update: October, 2020
author: crhoades, ens28527
translation-type: tm+mt
source-git-commit: 1b398e479137a12bcfc3208d37472aae3d6721e1
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 50%

---


# Adobe Experience Platform リリースノート

**リリース日：2020年10月**

Adobe Experience Platform の新機能：

- [[!DNLアクセス制御]](#access-control)
- [[!DNLサンドボックス]](#sandboxes)

## [!DNL Access control] {#access-control}

[!DNL Experience Platform] は、[Adobe Admin Console](https://adminconsole.adobe.com) 製品プロファイルを活用し、ユーザーを権限とサンドボックスにリンクします。権限は、データモデリング、プロファイル管理、サンドボックス管理など、さまざまな Platform 機能へのアクセスを制御します。

**主な特長**

| 機能 | 説明 |
|--- | ---|
| 権限 | In the [!DNL Admin Console], the  tab within a [!DNL Platform] product profile allows you customize which [!DNL Platform] capabilities are available for the users attached to that profile. Available permission categories include: [!UICONTROL Data Modeling], [!UICONTROL Data Management], [!UICONTROL Profile Management], [!UICONTROL Identities], [!UICONTROL Data Monitoring], [!UICONTROL Sandbox Administration], [!UICONTROL Destinations], [!UICONTROL Sources]. |
| サンドボックスへのアクセス |  製品プロファイル内の「[!UICONTROL _権限_]」タブでは、特定のサンドボックスへのアクセス権をユーザーに付与できます。[!DNL Platform]詳しくは、以下の「[サンドボックス](#sandboxes)」の節を参照してください。 |

詳しくは、「[アクセス制御の概要](../../access-control/home.md)」を参照してください。

## [!DNL Sandboxes] {#sandboxes}

[!DNL Experience Platform] は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。In order to address this need, [!DNL Experience Platform] provides sandboxes which partition a single [!DNL Platform] instance into separate virtual environments to help develop and evolve digital experience applications.

**主な特長**

| 機能 | 説明 |
|--- | ---|
| 実稼働用サンドボックス | [!DNL Experience Platform] は、1 つの実稼動用サンドボックスを提供します。このサンドボックスは、削除またはリセットすることはできません。 |
| 非実稼働用サンドボックス | Multiple non-production sandboxes can be created for a single [!DNL Platform] instance, allowing you to test features, run experiments, and make custom configurations without impacting your production sandbox. |
| サンドボックス切り替えボタン | In the [!DNL Experience Platform] user interface, the sandbox switcher in the top-left corner of the screen allows you to switch between available sandboxes through a dropdown menu. |
| `x-sandbox-name` ヘッダー | All calls to [!DNL Experience Platform] APIs must now include the new `x-sandbox-name` header, whose value references the `name` attribute of the sandbox the operation will take place in. |

詳しくは、[サンドボックスの概要](../../sandboxes/home.md)を参照してください。