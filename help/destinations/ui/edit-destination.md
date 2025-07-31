---
title: 宛先の編集
type: Tutorial
description: Adobe Experience Platform UI で既存の宛先アカウントを編集および更新する方法について説明します
badgeBeta: label="ベータ版" type="Informative"
exl-id: f3298836-668b-43fb-b4f3-85a650766f05
source-git-commit: 990fe9162c5b2970f269a5b0668916b7b6e61f44
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 宛先の編集

Experience Platform UI を使用して、認証資格情報や書き出し場所などを更新する方法など、既存の宛先接続の様々なコンポーネントを編集する方法について説明します。

>[!IMPORTANT]
>
>この機能はベータ版で、一部のお客様のみご利用いただけます。 アクセス権をリクエストするには、Adobe担当者にお問い合わせください。

>[!NOTE]
>
> このチュートリアルで説明する編集操作は、API 操作を介してもサポートされます。 詳しくは、[API で宛先を編集 ](/help/destinations/api/edit-destination.md) する方法に関するチュートリアルを参照してください。

既存の宛先接続の様々なコンポーネントを編集するには：

1. **[!UICONTROL 宛先]**/**[!UICONTROL 参照]** に移動します。
2. 編集する宛先を選択します。
3. `...` 名前 [!UICONTROL  列の省略記号（]）を選択し、![ 宛先を編集コントロール ](/help/images/icons/edit.png)**[!UICONTROL  宛先を編集 ]**コントロールを使用して、既存の宛先接続を編集します。
4. モーダルウィンドウで、必要な設定を編集します。 終了したら「**[!UICONTROL 保存]**」を選択します。

宛先を編集ウィンドウでは、最初に宛先に接続した際に設定した設定を更新できます。 これらの設定は、更新している宛先プラットフォームによって異なります。

宛先の設定方法によっては、一部のフィールドが読み取り専用となっており、編集できない場合があります。 読み取り専用フィールドの値を変更するには、新しいフィールド値で [ 新しい宛先接続を作成 ](../ui/connect-destination.md) する必要があります。

![ 読み取り専用フィールドを示すスクリーンショット。](../assets/ui/edit-destinations/read-only.png)

[Amazon S3](../catalog/cloud-storage/amazon-s3.md)、{Azure Event Hubs[ および ](../catalog/cloud-storage/azure-event-hubs.md)4}Google Ads[ の宛先に対して更新できる設定の例を以下に示します。](../catalog/advertising/google-ads-destination.md)

<div style="display: flex; gap: 12px; justify-content: flex-start; align-items: flex-start;">
  <img class="modal-image" src="../assets/ui/edit-destinations/edit-amazon-s3-connection.png" alt="Amazon S3 の宛先の宛先画面を編集します。" style="max-width: 200px; height: auto; border: 1px solid #ccc;">
  <img class="modal-image" src="../assets/ui/edit-destinations/edit-eventhubs-connection.png" alt="Azure EventHubs 宛先の宛先画面を編集します。" style="max-width: 200px; height: auto; border: 1px solid #ccc;">
  <img class="modal-image" src="../assets/ui/edit-destinations/edit-google-ads-connection.png" alt="Google Ads の宛先の宛先画面を編集します。" style="max-width: 200px; height: auto; border: 1px solid #ccc;">
</div>

>[!SUCCESS]
>
>これで、宛先接続設定が更新されました。

## その他の編集オプション

Experience Platform UI または Flow Service API を使用すると、以下のリンクで説明されているように、様々な宛先設定を編集できます。

| Experience Platform UI の使用 | Flow Service API の使用 |
|---------|----------|
| 宛先接続の編集（このページ） | [ ターゲット接続コンポーネント（ストレージの場所およびその他のコンポーネント）の編集 ](/help/destinations/api/edit-destination.md#patch-target-connection) |
| [ アカウントの編集 ](/help/destinations/ui/update-accounts.md) | [ ベース接続コンポーネント（認証パラメーターおよびその他のコンポーネント）の編集 ](/help/destinations/api/edit-destination.md#patch-base-connection) |
| [ アクティベーションデータフローの編集 ](/help/destinations/ui/edit-activation.md) | [ 宛先データフローの更新 ](/help/destinations/api/update-destination-dataflows.md) |

## 次の手順

このチュートリアルでは、**[!UICONTROL destinations]** ワークスペースを使用して既存の宛先接続を正常に更新しました。

宛先について詳しくは、[ 宛先の概要 ](../catalog/overview.md) を参照してください。
