---
keywords: Experience Platform；ホーム；人気のトピック；アクセス制御；属性ベースのアクセス制御
title: 属性ベースのアクセス制御管理ラベル
description: Adobe Experience Cloudの権限インターフェイスを使用したラベルの管理。
exl-id: c790f09c-fda6-48bf-95db-3f5053cd882e
source-git-commit: 855f0a1384f658d39aa9d4fbb6bcb032933e59db
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 13%

---

# ラベルの管理

ラベルを使用すると、データ使用状況と属性ベースのアクセス制御ポリシーに従ってデータセットとフィールドを分類できます。 ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。ベストプラクティスでは、データがAdobe Experience Platformに取得されるとすぐに、またはデータが使用できるようになるとすぐに、データのラベル付けが推奨されます。 権限 UI でラベルを管理する方法については、このドキュメントを参照してください。

ラベルと対応するガバナンスポリシーの包括的なリストについては、[&#x200B; コアデータ使用ラベル &#x200B;](../../../data-governance/labels/reference.md){target="_blank"} に関するガイドを参照してください。

>[!NOTE]
>
>特定のラベルを含むフィールドで [&#x200B; 計算済み属性 &#x200B;](../../../profile/computed-attributes/overview.md){target="_blank"} を作成または表示するには、そのラベルへのアクセス権が必要です。

## ラベルを探索 {#explore-labels}

使用可能なすべてのラベルを表示するには、**[!UICONTROL Permissions]** Adobe Experience Cloud[&#x200B; の &#x200B;](https://experience.adobe.com/){target="_blank"} に移動します。 左パネルから「**[!UICONTROL Labels]**」を選択します。

![&#x200B; 左側のパネルでラベルがハイライト表示された権限内のラベルワークスペース。](../../images/ui/labels/labels-home.png){zoomable="yes"}

ラベルはタイプ別に分類され、次のいずれかのカテゴリに属します。

| タイプ | 説明 |
| --- | --- |
| [&#x200B; 契約 &#x200B;](../../../data-governance/labels/reference.md#contract){target="_blank"} | このカテゴリは、契約上の義務があるデータ、または組織のデータガバナンスポリシーに関連するデータの分類に使用されます。 |
| [ID](../../../data-governance/labels/reference.md#identity){target="_blank"} | このカテゴリは、個人を直接または間接的に識別できるデータの分類に使用されます。 |
| [&#x200B; 機密 &#x200B;](../../../data-governance/labels/reference.md#sensitive){target="_blank"} | このカテゴリは、組織が機密と見なすデータの分類に使用されます。 |
| [&#x200B; パートナーエコシステム &#x200B;](../../../data-governance/labels/reference.md#partner){target="_blank"} | このカテゴリは、組織外のソースから取得したデータを分類するために使用されます。 |
| 責任ある取り組み | このカテゴリには、バイアスを引き起こす可能性のあるデータを反映する単一のラベル **[!UICONTROL Potential for Bias]** が含まれています。 |
| カスタム | このカテゴリには、組織で作成されたラベルが含まれます。 |

ラベルをフィルターするには、フィルターアイコン（![&#x200B; フィルターアイコン &#x200B;](/help/images/icons/filter.png)）を選択して、**[!UICONTROL Type]** ドロップダウンから目的のラベルタイプを選択します。

![&#x200B; フィルターオプションが展開され、「タイプ」ドロップダウンリストがハイライト表示されたラベルワークスペース。](../../images/ui/labels/label-filter.png){zoomable="yes"}

個々のラベルを表示するには、リストからラベルの名前を選択します。 ラベルの詳細ページが表示されます。 Adobeのコアラベルは **編集でき** せん。

![&#x200B; 個々のラベルの詳細ページ。](../../images/ui/labels/label-details.png){zoomable="yes"}

## カスタムラベルの作成 {#create-custom-label}

>[!CONTEXTUALHELP]
>id="platform_abac_labelusage"
>title="ラベルの使用"
>abstract="カスタムラベルを使用すると、データにデータガバナンスとアクセス制御の設定を適用できます。"

>[!CONTEXTUALHELP]
>id="platform_permissions_labels_about_create"
>title="新しいラベルの作成"
>abstract="組織のニーズに合わせて独自のカスタムラベルを作成できます。カスタムラベルを使用して、データガバナンスとアクセス制御の両方の設定をデータに適用できます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-governance/labels/overview.html?lang=ja#manage-labels" text="カスタムラベルの管理"

>[!NOTE]
>
>カスタムラベルを作成するには、**[!UICONTROL Manage Usage Labels]** 権限と `Prod` サンドボックスを含む役割が必要です。

新しいラベルを作成するには、**[!UICONTROL Labels]** ワークスペースの左パネルで **[!UICONTROL Permissions]** を選択し、「**[!UICONTROL Create label]**」を選択します。

![&#x200B; ラベルを作成オプションがハイライト表示されたラベルワークスペース。](../../images/ui/labels/create-label.png){zoomable="yes"}

**[!UICONTROL Create new label]** ダイアログが表示され、**[!UICONTROL Name]**、**[!UICONTROL Friendly name]** および **[!UICONTROL Description]** を入力するように求められます。

>[!IMPORTANT]
>
> ラベルを作成し、ラベルの削除が現在サポートされていない場合、ラベルの [!UICONTROL Name] は変更できません。

「**[!UICONTROL Confirm]**」を選択して、ラベルの作成を完了します。

![&#x200B; 名前、わかりやすい名前、説明が入力され、「確認」オプションがハイライト表示された新しいラベルを作成ダイアログ &#x200B;](../../images/ui/labels/create-new-label.png){zoomable="yes"}

## カスタムラベルの編集 {#edit-custom-label}

カスタムラベルの **[!UICONTROL Name]** は編集できませんが、**[!UICONTROL Friendly name]** と **[!UICONTROL Description]** は編集できます。 カスタムラベルを編集するには、**[!UICONTROL Labels]** ワークスペース内のリストからラベルを選択します。

![&#x200B; ラベルがハイライト表示されたラベルワークスペース。](../../images/ui/labels/select-label.png){zoomable="yes"}

いずれかのフィールドを編集し、テキストボックスの外側をクリックして変更を保存します。 画面に確認メッセージが表示され、**[!UICONTROL Modified by]** 名と **[!UICONTROL Modified]** 日が変更されます。

![&#x200B; ラベルの詳細ページに「updated successfully」というメッセージが表示されます。](../../images/ui/labels/edit-label.png){zoomable="yes"}

## 次の手順

これでラベルをより深く理解できたので、次は [&#x200B; ラベルをスキーマに適用する &#x200B;](../../../xdm/tutorials/labels.md) ことができます。
