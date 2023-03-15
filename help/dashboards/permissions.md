---
solution: Experience Platform
title: Experience Platform ダッシュボードのアクセス権限の取得方法と付与方法
type: Documentation
description: Adobe Admin Console を使用して Experience Platform ダッシュボードの表示、編集、更新の機能をユーザーに付与します。
exl-id: 2e50790f-b3ab-4851-a9a5-7cb98bf98ce3
source-git-commit: fa4fc154f57243250dec9bdf9557db13ef7768e8
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 32%

---

# ダッシュボードのアクセス権限

ダッシュボードの表示、編集、更新の機能をユーザーに付与するには、まず権限を有効にする必要があります。Adobe Experience Platformでは、 [Adobe Admin Console](https://adminconsole.adobe.com/). ユーザーは、 [!DNL Admin Console].

このドキュメントでは、ダッシュボードで使用できる権限の概要を示します。これには、ダッシュボードがアクセスできる機能や、ダッシュボードで有効にするユーザー機能などが含まれます。 アクセス権限の取得と割り当てについて詳しくは[アクセス制御の概要](../access-control/home.md)を参照してください。

## 前提条件

[!DNL Experience Platform] 用にアクセス制御を設定するには、[!DNL Experience Platform] 製品統合を持つ組織の管理者権限が必要です。詳しくは、[管理者の役割](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html)に関する Adobe Help Center の記事を参照してください。

## 使用可能なダッシュボードの権限 {#available-permissions}

この [!DNL Dashboards] サービスは、組み合わせると、へのフルアクセスを提供する 3 つの権限を提供します。 [!UICONTROL プロファイル], [!UICONTROL セグメント], [!UICONTROL 宛先]、および [!UICONTROL ライセンスの使用状況] Adobe Experience Platform内のダッシュボード。 その権限は次の通りです。

| 権限 | 説明 |
|---|---|
| **標準ダッシュボードの管理** | この権限は、 **グローバルな読み取り/書き込み権限**. 次の操作が可能です。 [カスタムウィジェットを作成](./customize/custom-widgets.md) および [ウィジェットスキーマを編集](./customize/edit-schema.md) から [!UICONTROL Widget ライブラリ]. |
| **標準ダッシュボードの表示** | これにより、 **読み取り専用** 機能 [!UICONTROL プロファイル], [!UICONTROL 宛先]、および [!UICONTROL セグメント] ダッシュボードにアクセスし、Platform の左側のナビゲーションからダッシュボードにアクセスできます。 また、 [!UICONTROL ダッシュボード] をクリックし、 [!UICONTROL ダッシュボード] 「在庫と統合」タブ |
| **ライセンス使用状況ダッシュボードの表示** | この権限を持つユーザーは、 **読み取り専用** ～へのアクセス [ライセンス使用状況ダッシュボード](./guides/license-usage.md) Experience PlatformUI 内 |

には 5 つの権限が含まれていません [!DNL Dashboard] 必要に応じて必要になる可能性のあるカテゴリ。 次の表に、カテゴリ内のカテゴリの位置の概要を示します。

>[!IMPORTANT]
>
>両方の **[!DNL Manage Standard Dashboards]** そして **[!DNL View Standard Dashboards]** 権限 **必要** からの「表示」または「管理」権限 [!DNL Profile Management] または [!DNL Destinations] カテゴリを使用して、Platform UI 内の関連するセクションをアクティブ化します。

| 権限 | Admin Consoleカテゴリの場所 |
|---|---|
| [!DNL View Profiles] | [!DNL Profile Management] |
| [!DNL View Segments] | [!DNL Profile Management] |
| [!DNL View Destinations] | [!DNL Destinations] |
| [!DNL Manage Queries] | [!DNL Query Service] |
| [!DNL Manage Sandboxes] | [!DNL Sandbox Administration] |

## アクセス制御マトリックス

次のアクセス制御マトリックスは、必要な権限の分類と、様々なダッシュボード機能へのアクセスに関して提供される機能を示しています。 権限は上の水平行に表示され、Platform UI ワークスペースは左の列に表示されます。

|  | [!UICONTROL 標準ダッシュボードを表示] または [!UICONTROL 標準ダッシュボードを管理] | [!UICONTROL プロファイルの表示],<br/>[!UICONTROL セグメントを表示],<br/> [!UICONTROL 宛先の表示] | [!UICONTROL クエリの管理] &amp; [!UICONTROL サンドボックスの管理] | [!UICONTROL ライセンス使用状況ダッシュボードの表示] |
|---|---|---|---|---|
| [!DNL Profiles],<br/>[!DNL Segments],<br/>[!DNL Destinations] をクリックします。 | なし | **「表示」または「管理」権限が必要です** 各ダッシュボードに対して | なし | なし |
| [!DNL Dashboards] をクリックします。 | 有効 | **少なくとも 1 つの必須**. | なし | なし |
| [!DNL Dashboards] [!DNL Inventory] <br/>（「参照」タブ） | 有効 | なし | なし | なし |
| [!DNL Dashboards] [!DNL Integrations] タブ <br/>(Power BIのインストールに使用 ) | 有効 | **少なくとも 1 つの必須** | なし | なし |
| Power BIインストールボタンとワークフロー | 有効 | なし | **必須** | なし |
| [!DNL Profiles],<br/>[!DNL Segments],<br/>[!DNL Destinations] ダッシュボード。<br/>ウィジェットスキーマを編集し、ウィジェットのカスタマイズ用に新しい属性を追加する機能 | **管理 — 標準 — ダッシュボードが必要** | **必須（各ダッシュボード）** | なし | なし |
| [!DNL License Usage Dashboard] | なし | なし | なし | 有効 |

{style="table-layout:auto"}

## 製品プロファイルへの権限の追加

上記の情報を使用して、製品プロファイルに適切な権限を追加します。 詳しい手順については、ドキュメントを参照してください。 [アクセス制御 UI を使用して権限を追加する方法](../access-control/ui/permissions.md).

権限の説明については、このドキュメントの前の節[使用可能な権限](#available-permissions)を参照してください。

>[!NOTE]
>
>すべてのユーザーに対してすべての権限を有効にする必要はありません。組織の構造に応じて、特定のユーザーに対して個別の製品プロファイルを作成し、制限付きアクセス（読み取り専用など）を許可することができます。詳しくは、製品プロファイルのユーザー管理に関するドキュメントを参照してください [特定のユーザーに権限を割り当てる方法](../access-control/ui/users.md).

必要なアクセス権限を追加すると、組織内のユーザーはExperience PlatformUI 内でダッシュボードの表示を開始し、割り当てた権限に基づいて他のアクションを実行できます。
