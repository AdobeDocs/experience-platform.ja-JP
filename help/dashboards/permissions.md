---
solution: Experience Platform
title: Experience Platform ダッシュボードのアクセス権限の取得方法と付与方法
type: Documentation
description: Adobe Admin Console を使用して Experience Platform ダッシュボードの表示、編集、更新の機能をユーザーに付与します。
exl-id: 2e50790f-b3ab-4851-a9a5-7cb98bf98ce3
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '640'
ht-degree: 28%

---

# ダッシュボードのアクセス権限

ダッシュボードの表示、編集、更新の機能をユーザーに付与するには、まず権限を有効にする必要があります。Adobe Experience Platformでは、アクセス制御は [Adobe Admin Console](https://adminconsole.adobe.com/) を通じて提供されます。 ユーザーは、[!DNL Admin Console] の製品プロファイルを通じて権限およびサンドボックスとリンクされます。

このドキュメントでは、アクセス権を付与する機能や有効にするユーザー機能など、ダッシュボードで使用可能な権限の概要を説明します。 アクセス権限の取得と割り当てについて詳しくは[アクセス制御の概要](../access-control/home.md)を参照してください。

## 前提条件

[!DNL Experience Platform] 用にアクセス制御を設定するには、[!DNL Experience Platform] 製品統合を持つ組織の管理者権限が必要です。詳しくは、[管理者の役割](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html)に関する Adobe Help Center の記事を参照してください。

## 使用可能なダッシュボード権限 {#available-permissions}

[!DNL Dashboards] サービスには 3 つの権限が用意されており、これらの権限を組み合わせることで、Adobe Experience Platform内の [!UICONTROL &#x200B; プロファイル &#x200B;]、[!UICONTROL &#x200B; セグメント &#x200B;]、[!UICONTROL &#x200B; 宛先 &#x200B;]、[!UICONTROL &#x200B; ライセンスの使用状況 &#x200B;] の各ダッシュボードへのフルアクセスが可能になります。 その権限は次の通りです。

| 権限 | 説明 |
|---|---|
| **標準ダッシュボードの管理** | この権限は **グローバル読み取りおよび書き込み権限** です。 [&#x200B; ウィジェットライブラリ &#x200B;](./customize/custom-widgets.md) を介して [&#x200B; カスタムウィジェットの作成 &#x200B;](./customize/edit-schema.md) および [!UICONTROL &#x200B; ウィジェットスキーマの編集 &#x200B;] を行うことができます。 |
| **標準ダッシュボードの表示** | これにより、[!UICONTROL &#x200B; プロファイル **、[!UICONTROL &#x200B; 宛先 &#x200B;] および [!UICONTROL &#x200B; セグメント &#x200B;] ダッシュボードに** 読み取り専用 &#x200B;] 機能が提供され、Experience Platformの左側のナビゲーションからアクセスできるようになります。 また、左側のナビゲーションに [!UICONTROL &#x200B; ダッシュボード &#x200B;] が追加され、[!UICONTROL &#x200B; ダッシュボード &#x200B;] インベントリと統合」タブにアクセスできます。 |
| **ライセンス使用状況ダッシュボードの表示** | この権限を持つユーザーはExperience Platform UI 内の [&#x200B; ライセンス使用状況ダッシュボード &#x200B;](./guides/license-usage.md) に **読み取り専用** アクセスできます。 |

[!DNL Dashboard] カテゴリには含まれていない 5 つの権限があり、必要に応じて必要になる可能性があります。 次の表に、Admin Consoleでのカテゴリの場所の概要を示します。

>[!IMPORTANT]
>
>**[!DNL Manage Standard Dashboards]** 権限と **[!DNL View Standard Dashboards]** 権限の両方 **必要** Admin Consoleの [!DNL Profile Management] カテゴリまたは [!DNL Destinations] カテゴリからの「表示」または「管理」権限を使用して、Experience Platform UI 内の関連セクションをアクティブ化します。

| 権限 | Admin Console カテゴリの場所 |
|---|---|
| [!DNL View Profiles] | [!DNL Profile Management] |
| [!DNL View Segments] | [!DNL Profile Management] |
| [!DNL View Destinations] | [!DNL Destinations] |
| [!DNL Manage Queries] | [!DNL Query Service] |
| [!DNL Manage Sandboxes] | [!DNL Sandbox Administration] |

## アクセス制御マトリックス

次のアクセス制御マトリックスは、必要な権限の分類と、様々なダッシュボード機能へのアクセスに関して権限が提供する機能を示しています。 権限は上部の水平行にリストされ、Experience Platform UI ワークスペースは左側の列にリストされます。

|   | [!UICONTROL &#x200B; 標準ダッシュボードの表示 &#x200B;] または [!UICONTROL &#x200B; 標準ダッシュボードの管理 &#x200B;] | [!UICONTROL &#x200B; プロファイルの表示 &#x200B;],<br/>[!UICONTROL &#x200B; セグメントの表示 &#x200B;],<br/> [!UICONTROL 宛先の表示] | [!UICONTROL &#x200B; クエリの管理 &#x200B;] および [!UICONTROL &#x200B; サンドボックスの管理 &#x200B;] | [!UICONTROL ライセンス使用状況ダッシュボードの表示] |
|---|---|---|---|---|
| [!DNL Profiles],<br/>[!DNL Segments],<br/>[!DNL Destinations] が左側のナビゲーションに表示されます。 | なし | **各ダッシュボードに「表示」または「管理」権限が必要です**。 | なし | なし |
| 左側のナビゲーショ [!DNL Dashboards] の「」をクリックします。 | ENABLED | **1 つ以上が必要**。 | なし | なし |
| [!DNL Dashboards] [!DNL Inventory] <br/> （「参照」タブ） | ENABLED | なし | なし | なし |
| [!DNL Dashboards] [!DNL Integrations] tab <br/> （Power BIのインストールに使用） | ENABLED | **1 つ以上が必要** | なし | なし |
| Power BIのインストールボタンとワークフロー | ENABLED | なし | **必須** | なし |
| [!DNL Profiles],<br/>[!DNL Segments],<br/>[!DNL Destinations] ダッシュボード。<br/> ウィジェットスキーマを編集し、ウィジェットをカスタマイズするための新しい属性を追加する機能 | **Manage-standard-dashboard REQUIRED** | **必須（各ダッシュボードに対して）** | なし | なし |
| [!DNL License Usage Dashboard] | なし | なし | なし | ENABLED |

{style="table-layout:auto"}

## 製品プロファイルへの権限の追加

上記の情報を使用して、製品プロファイルに適切な権限を追加します。 [&#x200B; アクセス制御 UI を使用して権限を追加する方法 &#x200B;](../access-control/ui/permissions.md) の手順について詳しくは、ドキュメントを参照してください。

権限の説明については、このドキュメントの前の節[使用可能な権限](#available-permissions)を参照してください。

>[!NOTE]
>
>すべてのユーザーに対してすべての権限を有効にする必要はありません。組織の構造に応じて、特定のユーザーに対して個別の製品プロファイルを作成し、制限付きアクセス（読み取り専用など）を許可することができます。 [&#x200B; 特定のユーザーに権限を割り当てる方法 &#x200B;](../access-control/ui/users.md) については、製品プロファイルのユーザー管理のドキュメントを参照してください。

必要なアクセス権限を追加すると、組織内のユーザーはExperience Platform UI 内でダッシュボードの表示を開始し、割り当てた権限に基づいて他の操作を実行できます。
