---
keywords: Experience Platform;home;popular topics;access control;adobe admin console
solution: Experience Platform
topic: overview
title: アクセス制御の概要
description: Adobe Experience Platformのアクセス制御はAdobe Admin Consoleを通じて提供される。 この機能は、Admin Console の製品プロファイルを利用して、ユーザーを権限およびサンドボックスにリンクします。
translation-type: tm+mt
source-git-commit: 205bfb5f3b3fa083f64fc0160ea6bdf7bba74c9b
workflow-type: tm+mt
source-wordcount: '1298'
ht-degree: 54%

---


# アクセス制御の概要

Access control for [!DNL Experience Platform] is provided through the [Adobe Admin Console](https://adminconsole.adobe.com). This functionality leverages product profiles in [!DNL Admin Console], which link users with permissions and sandboxes.

## アクセス制御階層とワークフロー

In order to configure access control for [!DNL Experience Platform], you must have administrator privileges for an organization that has an [!DNL Experience Platform] product integration. 権限を付与または取り消す最小の役割は、製品プロファイル管理者です。権限を管理できる他の管理者の役割は、 製品管理者（製品内のすべてのプロファイルを管理）とシステム管理者（制限なし）です。詳しくは、[管理者の役割](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html)に関する Adobe Help Center の記事を参照してください。

>[!NOTE]
>
> これ以降、このドキュメントでの「管理者」は、（前述の説明に従って）製品プロファイル管理者以上を指します。

アクセス権限を取得し、割り当てるための高度なワークフローを、次のように要約できます。

- Adobe Experience Platformのライセンス認証、またはExperience Platformを使用するアプリケーション/アプリケーションサービスのライセンス認証が完了すると、ライセンス認証時に指定した管理者に電子メールが送信されます。
- 管理者は [Adobe Admin Console](#adobe-admin-console) にログインし、概要ページの製品のリストから **Adobe Experience Platform** を選択します。
- 管理者は、必要に応じて、デフォルトの[製品プロファイル](#product-profiles)を表示、または新しい顧客製品プロファイルを作成できます。
- 管理者は、既存の製品プロファイルの権限とユーザーを編集できます。
- When creating or editing a product profile, the administrator adds users to the profile using the **[!UICONTROL users]** tab, and grants permissions to these users (such as &quot;[!UICONTROL Read Datasets]&quot; or &quot;[!UICONTROL Manage Schemas]&quot;) by accessing the **[!UICONTROL permissions]** tab. 同様に、管理者は同じ「権限」タブを使用してサンドボックスにアクセス権を割り当てることができます。
- When users log in to the [!DNL Experience Platform] user interface, their access to [!DNL Platform] capabilities is driven by the permissions that have been granted to them from Step 2. For example, if a user does not have the &quot;[!UICONTROL View Datasets]&quot; permission, the **[!UICONTROL Datasets]** tab in the side menu will not be visible to that user.

For more detailed steps on how to manage access control in [!DNL Experience Platform], see the [access control user guide](./ui/overview.md).

All calls to [!DNL Experience Platform] APIs are validated for permissions, and will return errors if the appropriate permission(s) are not found in the current user context. UI 内では、現在のユーザーに付与されている権限に応じて、要素が非表示になるか変更されます。

## Adobe Admin Console

Adobe Admin Console は、アドビ製品の権限を管理し、組織のアクセス権を一元的に管理するためのツールです。Through the console, you can grant groups of users access permissions for various [!DNL Platform] capabilities, such as &quot;[!UICONTROL Manage Datasets]&quot;, &quot;[!UICONTROL View Datasets]&quot;, or &quot;[!UICONTROL Manage Profiles]&quot;.

### 製品プロファイル

In the [!DNL Admin Console], permissions are assigned to users through the use of product profiles. 製品プロファイルを使用すると、1 人または複数のユーザーに権限を付与でき、また、製品プロファイルを通じて割り当てられるサンドボックスの範囲に対するアクセス権を含めることができます。ユーザーは、組織に属する 1 つ以上の製品プロファイルに割り当てることができます。

### デフォルトの製品プロファイル

[!DNL Experience Platform] には、事前設定済みのデフォルト製品プロファイルが2つ用意されています。 次の表に、各デフォルトプロファイルで提供される内容を示します。これには、ユーザーがアクセスを許可するサンドボックスと、そのサンドボックスの範囲内で許可する権限が含まれます。

| 製品プロファイル | サンドボックスアクセス | 権限 |
| --- | --- | --- |
| 既定の実稼働環境のフルアクセス | 実稼動 | All permissions applicable to [!DNL Experience Platform], except for Sandbox Administration permissions. |
| Sandbox管理者 | なし | サンドボックス管理権限へのアクセスのみを提供します。 |

## サンドボックスと権限

非実稼働サンドボックスは、他のサンドボックスからデータを分離できるデータ仮想化の一種で、通常は開発実験、テストまたは試用に使用されます。A product profile&#39;s permissions give the profile&#39;s users access to [!DNL Platform] features within the sandbox environments to which they&#39;ve been granted access to. デフォルトのExperience Platformライセンスでは、5つのサンドボックス（1つの実稼働環境と4つの非実稼働環境）が許可されます。 10個の非実稼動用サンドボックス（合計75個まで）のパックを追加できます。 詳しくは、IMS組織管理者またはAdobeのセールス担当者にお問い合わせください。

For more information about sandboxes in [!DNL Experience Platform], please refer to the [sandboxes overview](../sandboxes/home.md).

### サンドボックスへのアクセス

サンドボックスへのアクセスは、製品のプロファイルを通じて管理されます。製品プロファイルのサンドボックスへのアクセスを有効にする手順について詳しくは、『[アクセス制御ユーザーガイド](./ui/overview.md)』を参照してください。

ユーザーには、製品プロファイル内の 1 つ以上のサンドボックスへのアクセス権を付与できます。1 人のユーザーが複数の製品プロファイルに含まれている場合、そのユーザーはそれらのプロファイルに含まれるすべてのサンドボックスにアクセスできます。

「サンドボックスの管理」権限を使用すると、ユーザーはサンドボックスの管理、表示またはリセットをおこなえます。

### 権限

製品プロファイル内の「権限」タブには、そのプロファイル:に対してアクティブなサンドボックスと権限が表示されます。

![権限の概要](./images/permissions-overview.png)

Permissions that are granted through the [!DNL Admin Console] are sorted by category, with some permissions granting access to several low-level functionalities.

The following table outlines the available permissions for [!DNL Experience Platform] in the [!DNL Admin Console], with descriptions of the specific [!DNL Platform] capabilities they grant access to. 製品プロファイルに権限を追加する手順について詳しくは、『[アクセス制御ユーザーガイド](./ui/overview.md)』を参照してください。

| カテゴリ | 権限 | 説明 |
| --- | --- | --- |
| [!DNL Data Modeling] | [!UICONTROL スキーマの管理] | 各スキーマと関連リソースへの読み取り、作成、編集および削除アクセス |
| [!DNL Data Modeling] | [!UICONTROL スキーマの表示] | スキーマおよび関連リソースへの読み取り専用アクセス |
| [!DNL Data Modeling] | [!UICONTROL 関係の管理] | スキーマ関係の読み取り、作成、編集、削除を行うことができます。 |
| [!DNL Data Modeling] | [!UICONTROL IDメタデータの管理] | スキーマのIDメタデータの読み取り、作成、編集および削除を行うことができます。 |
| [!DNL Data Management] | [!UICONTROL データセットの管理] | データセットへの読み取り、作成、編集、削除アクセススキーマへの読み取り専用アクセス |
| [!DNL Data Management] | [!UICONTROL データセットの表示] | データセットおよびスキーマへの読み取り専用アクセス |
| [!DNL Data Management] | [!UICONTROL データ監視] | 監視データセットおよびストリームへの読み取り専用アクセス |
| [!DNL Profile Management] | [!UICONTROL プロファイルの管理] | 顧客プロファイルに使用するデータセットへの読み取り、作成、編集、削除アクセス使用可能なプロファイルへの読み取り専用アクセス |
| [!DNL Profile Management] | [!UICONTROL プロファイルの表示] | 使用可能なプロファイルへの読み取り専用アクセス |
| [!DNL Profile Management] | [!UICONTROL セグメントの管理] | セグメントの読み取り、作成、編集および削除を行うことができます。 |
| [!DNL Profile Management] | [!UICONTROL 表示セグメント] | 使用可能なセグメントへの読み取り専用アクセス権。 |
| [!DNL Profile Management] | [!UICONTROL 結合ポリシーの管理] | 読み取り、作成、編集および削除のマージポリシーへのアクセス権。 |
| [!DNL Profile Management] | [!UICONTROL 表示結合ポリシー] | 使用可能なマージポリシーへの読み取り専用アクセス権。 |
| [!DNL Profile Management] | [!UICONTROL セグメントのオーディエンスの書き出し] | 評価済みのデータセットセグメントをオーディエンスセットに書き出す機能 |
| [!DNL Profile Management] | [!UICONTROL オーディエンスへのセグメントの評価] | セグメント定義を評価して、オーディエンスのプロファイルを生成する機能。 |
| [!DNL Identities] | [!UICONTROL ID 名前空間の管理] | ID 名前空間への読み取り、作成、編集および削除アクセス |
| [!DNL Identities] | [!UICONTROL ID 名前空間の表示] | ID 名前空間への読み取り専用アクセス |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスの管理] | サンドボックスへの読み取り、作成、編集、削除アクセス |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスの表示] | 組織に属するサンドボックスへの読み取り専用アクセス |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスのリセット] | サンドボックスをリセットする機能 |
| [!DNL Destinations] | [!UICONTROL 宛先の管理] | 宛先への読み取り、作成、編集、無効化アクセス* |
| [!DNL Destinations] | [!UICONTROL 宛先の表示] | 「**[!UICONTROL カタログ]**」タブでの使用可能な宛先および「**[!UICONTROL 参照]**」タブでの認証済みの宛先への読み取り専用アクセス* |
| [!DNL Destinations] | [!UICONTROL 宛先のアクティブ化] | 作成済みのアクティブな宛先に対するデータのアクティブ化機能This permission requires either “View Destinations” or “Manage [!UICONTROL Destinations”] to be granted to the user who will activate destinations.* |
| [!DNL Data Ingestion] | [!UICONTROL ソースの管理] | ソースへの読み取り、作成、編集、無効化アクセス |
| [!DNL Data Ingestion] | [!UICONTROL ソースの表示] | 「**[!UICONTROL カタログ]**」タブでの使用可能なソースおよび「**[!UICONTROL 参照]**」タブでの認証済みのソースへの読み取り専用アクセス |
| [!DNL Data Science Workspace] | [!UICONTROL Data Science Workspace の管理] | Access to read, create, edit, and delete in [!DNL Data Science Workspace]. |
| [!DNL Data Governance] | [!UICONTROL データ使用量ラベルの適用] | 使用状況ラベルの読み取り、作成、削除を行う。 |
| [!DNL Data Governance] | [!UICONTROL データ使用ポリシーの管理] | データ使用ポリシーの読み取り、作成、編集および削除にアクセスできます。 |
| [!DNL Data Governance] | [!UICONTROL 表示データ使用ポリシー] | 組織に属するデータ使用ポリシーに対する読み取り専用アクセス権。 |
| [!DNL Query Service] | [!UICONTROL クエリの管理] | プラットフォームデータの構造化SQLクエリの読み取り、作成、編集および削除を行う。 |

_(*)この許可には，に対する規定が必要 [!DNL Real-time Customer Data Platform]である。 リアルタイム CDP の詳細については、まず「[リアルタイム CDP の概要](https://docs.adobe.com/content/help/ja-JP/experience-platform/rtcdp/overview.html)」を参照してください。_

## 次の手順

By reading this guide, you have been introduced to the main principles of access control in [!DNL Experience Platform]. You can now continue to the [access control user guide](./ui/overview.md) for detailed steps on how use the [!DNL Admin Console] to create product profiles and assign permissions for [!DNL Platform].