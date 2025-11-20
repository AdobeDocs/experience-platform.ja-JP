---
title: 属性ベースのアクセス制御権限マネージャー
description: Adobe Experience Platformの権限マネージャーを使用して、レポートを生成し、アクセス権限を検証する方法を説明します。
exl-id: 4c2b8b8e-ac4f-4c6e-a23f-66f658bb6e24
source-git-commit: 7e65e88bc49ea28d567e8204db877d22ddb8d9a6
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 10%

---

# 権限マネージャー

>[!NOTE]
>
>[!UICONTROL Permission Manager] にアクセスするには、製品管理者である必要があります。 管理者権限がない場合は、システム管理者に問い合わせてアクセス権を取得してください。

[!UICONTROL Permission Manager]で簡単なクエリを使用して、アクセス管理を理解し、多くのワークフローおよび精度レベルでアクセス許可を検証する時間を節約するのに役立つ簡潔なレポートを作成します。[!UICONTROL Permission Manager]を使用して、ユーザーグループに属し、指定されたアクセス権限を持つユーザー、および特定のラベルを持つロールを検索できます。

## 指定したユーザーグループ内のユーザーの検索を実行 {#search-users}

>[!CONTEXTUALHELP]
>id="platform_permission_manager"
>title="権限マネージャー"
>abstract="ページのドロップダウンセレクターを使用して、ユーザーと役割に対する様々な精度レベルのアクセスレベルレポートを取得します。"
<!-- >additional-url="https://experienceleague.adobe.com/docs/experience-platform/access-control/abac/permissions-manager/permissions.html" text="Permission manager" -->

ドロップダウンを使用して、属性 **[!UICONTROL Users]**&#x200B;を選択します。

![属性ドロップダウンで「ユーザー」が強調表示されます。](../../images/permission-manager/users-select.png)

次に、ドロップダウンを使用して、検索する **[!UICONTROL User Group]** を選択します。

>[!INFO]
>
>[!UICONTROL User Group] は必須フィールドではありません。 各レポートで選択できるユーザーグループは 1 つだけです。

![ 「ユーザーグループ」ドロップダウンがハイライト表示されている様子 ](../../images/permission-manager/user-group-select.png)

レポートをよりきめ細かく制御するには、特定のサンドボックス内のアクションを使用してリソースを指定します。 ドロップダウンを使用して **[!UICONTROL Resource]**、**[!UICONTROL Actions]**、**[!UICONTROL Sandboxes]** を選択し、「**[!UICONTROL Show Results]**」を選択します。

>[!INFO]
>
>[!UICONTROL Resource]、 [!UICONTROL Actions] および [!UICONTROL Sandboxes] は必須フィールドではありません。 アクションまたはサンドボックスは、削除したい選択範囲の横にある **&#39;x&#39;を選択することで** 追加後に削除できます。

![リソース、アクション、サンドボックスドロップダウン、および表示結果ハイライト表示](../../images/permission-manager/users-additional-attributes-select.png)

選択した条件に基づいて、リストユーザーとその電子メールアドレスが報告されます。 左側のフィルターメニューを使用して、属性と結果を更新します。 特定のユーザーについて詳しくは、リストからユーザー名を選択してください。

![選択した属性に基づいて生成されたレポートがハイライト表示されます。](../../images/permission-manager/users-report.png)

## 特定のラベルを持つロールのSearch {#search-roles}

ドロップダウンを使用して、属性 **[!UICONTROL Roles]**&#x200B;を選択します。

>[!INFO]
>
>[!UICONTROL Labels] は必須フィールドではありません。 複数のラベルを選択できます。選択すると、このドロップダウンの下にラベルが表示されます。 ラベルを追加したら、アクションの横にある「**」を選択** て削除できます。

![ 属性ドロップダウンで役割がハイライト表示されます。](../../images/permission-manager/roles-select.png)

次へ、ドロップダウンを使用するために検索する **[!UICONTROL Labels]** を選択します。

![[ラベル] ドロップダウンが強調表示されます。](../../images/permission-manager/roles-labels-select.png)

より詳細なレポートを作成するには、特定のサンドボックス内のアクションでリソースを指定できます。 ドロップダウンを使用して **[!UICONTROL Resource]**、 **[!UICONTROL Actions]**、 **[!UICONTROL Sandboxes]** を選択し、[ **[!UICONTROL Show Results]**] を選択します。

>[!INFO]
>
>[!UICONTROL Resource]、 [!UICONTROL Actions] および [!UICONTROL Sandboxes] は必須フィールドではありません。 1 つのレポートで選択できる [!UICONTROL Resource] は 1 つだけです。 アクションまたはサンドボックスは、削除したい選択範囲の横にある **&#39;x&#39;を選択することで** 追加後に削除できます。

![リソース、アクション、サンドボックスドロップダウン、および表示結果ハイライト表示](../../images/permission-manager/roles-additional-attributes-select.png)

選択した条件に基づいて、役割のリストがレポートされます。 左側のフィルターメニューを使用して、属性と結果を更新します。 特定の役割の詳細については、リストから役割を選択してください。

条件に一致する各役割に対して、次の情報が表示されます。

| 属性 | 説明 |
| --- | --- |
| 説明 | 役割の簡単な説明。 |
| ラベル | 役割に関連付けられているラベルのリスト。 |
| サンドボックス | この役割を含む Sanbox のリスト。 |
| 変更日時 | 役割が最後に更新された日付とタイムスタンプ。 |
| 作成日時 | 役割が作成された日時とタイムスタンプ。 |
| 作成者 | 役割作成者の詳細。 |

![ 選択した属性に基づいて生成されたレポートがハイライト表示されている様子 ](../../images/permission-manager/roles-report.png)

## 次の手順

これで、ユーザーと役割に関するレポートの生成方法を学習しました。 属性ベースのアクセス制御の詳細については、[ 属性ベースのアクセス制御の概要 ](../overview.md) を参照してください。
