---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;ABAC
title: 属性ベースのアクセス制御管理ラベル
description: ここでは、Adobe Experience Cloudの権限インターフェイスを使用したラベルの管理について説明します
exl-id: c790f09c-fda6-48bf-95db-3f5053cd882e
source-git-commit: 5810a7778d86db2720a0372ace33278348d1ffdf
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 37%

---

# ラベルの管理

>[!NOTE]
>
>特定のラベルを含むフィールドを使用して計算属性を作成または表示するには、そのラベルへのアクセス権が必要です。

ラベルを使用すると、データに適用される使用およびアクセスポリシーに従ってデータセットとフィールドを分類できます。 ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。ベストプラクティスでは、データが Platform に取得されるとすぐに、またはデータが Platform で使用できるようになるとすぐに、データのラベル付けが推奨されます。

## 新しいラベルを作成 {#create-new-label}

>[!CONTEXTUALHELP]
>id="platform_abac_labelusage"
>title="ラベルの使用"
>abstract="カスタムラベルを使用すると、データにデータガバナンスとアクセス制御の設定を適用できます。"

>[!NOTE]
>
>組織全体のラベルのリストは 1 つだけです。 カスタムラベルを作成するには、実稼動サンドボックスでの **[!UICONTROL 使用ラベルの管理]** 権限が必要になります。 ラベルの削除は現在サポートされていません。

新しいラベルを作成するには、サイドバーの「**[!UICONTROL ラベル]**」タブを選択し、「**[!UICONTROL ラベルを作成]**」を選択します。

![flac-new-label](../../images/flac-ui/create-label.png)

**[!UICONTROL 新しいラベルを作成]** ダイアログが表示され、名前、わかりやすい名前（オプション）、説明（オプション）を入力するように求められます。

![new-label-info](../../images/flac-ui/new-label-info.png)

終了したら、「**[!UICONTROL 確認]**」を選択します。
