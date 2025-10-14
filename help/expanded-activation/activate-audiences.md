---
title: 拡張されたアクティベーションによるAudience Manager オーディエンスのアクティブ化
description: Audience Manager Expanded Activation を通じて、Audience Manager オーディエンスをソーシャルおよび広告の宛先に対してアクティブ化する方法について説明します。
exl-id: 4105f5c5-db69-414f-9ee4-8630b0a86da7
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---

# Audience Manager拡張アクティベーションによるオーディエンスのアクティブ化

ここでは、Audience Managerから、Expanded Activation でサポートされている宛先プラットフォームにオーディエンスをアクティベートするために従う必要があるエンドツーエンドのワークフローについて説明します。

## 始める前に {#before-you-begin}

このガイドで説明されている手順は、[&#x200B; 拡張されたアクティベーションの概要ページ &#x200B;](overview.md) を読み、オーディエンスのアクティベーションの前提条件を満たしていることを確認したことを前提としています。

>[!IMPORTANT]
>
>[!DNL Expanded Activation] を通じてオーディエンスをアクティブ化するには、Audience Manager オーディエンスが **ハッシュ化されたメールアドレス** に基づいていることを確認します。 詳しくは、[&#x200B; 前提条件 &#x200B;](overview.md#prerequisites) を参照してください。

## 手順 1:Audience Manager ソース接続の設定 {#configure-source}

[Audience Manager ソースコネクタ &#x200B;](../sources/connectors/adobe-applications/audience-manager.md) は、Adobe Audience Managerで収集されたオーディエンスデータを送信して、拡張されたアクティベーションでサポートされる出力先プラットフォームでアクティベーションを行います。

ソースコネクタを設定するには、[Audience Manager ソース接続を作成する &#x200B;](../sources/tutorials/ui/create/adobe-applications/audience-manager.md) 方法に関するガイドに従います。

![Audience Manager ソース接続を含む「ソース」タブを示すExperience Platform UI 画像 &#x200B;](assets/sources-tab.png)

>[!TIP]
>
>Adobe Audience Manager ソースコネクタは、拡張されたアクティベーションで使用できる唯一のソースコネクタです。
>
>追加の識別子に基づいてオーディエンスを取り込む場合は、[Real-Time CDP](../rtcdp/overview.md) のエディションを購入する必要があります。 詳しくは、Adobe担当者にお問い合わせください。

### 取り込んだオーディエンスの表示と監視 {#view-audiences}

Audience Managerから拡張アクティベーションに取り込んだオーディエンスは、**[!UICONTROL オーディエンス]** ダッシュボードで表示できます。

オーディエンスを表示するには、**[!UICONTROL 顧客]**/**[!UICONTROL オーディエンス]**/**[!UICONTROL 参照]** に移動します。

![&#x200B; オーディエンスページを示すExperience Platform UI 画像。](assets/audiences-browse.png)

>[!IMPORTANT]
>
>* 拡張されたアクティベーションにオーディエンスが完全に入力されるまでに、最大 48 時間かかる場合があります。 これは、既存のAudience Manager オーディエンスの更新にも当てはまります。
>* 新しく作成されたAudience Manager オーディエンスは、拡張されたアクティベーションでは自動的には表示されません。 拡張されたアクティベーションで新しいセグメントを取り込むには、Audience Manager ソースコネクタを使用してセグメントを追加する必要があります。

Audience Manager ソースコネクタを設定したら、[&#x200B; 手順 2](#create-destination-connection) に進みます。

## 手順 2：新しい宛先接続の作成 {#create-destination-connection}

Audience Manager オーディエンスを任意の宛先プラットフォームに送信するには、まず宛先プラットフォームへの接続を作成する必要があります。

左サイドバーで、**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]** に移動します。

[!DNL Expanded Activation] で使用可能な宛先カテゴリは [&#x200B; 広告 &#x200B;](../destinations/catalog/advertising/overview.md) および [&#x200B; ソーシャル &#x200B;](../destinations/catalog/social/overview.md) です。

![&#x200B; 拡張されたアクティベーションの宛先カタログを示すExperience Platform UI 画像。](assets/destination-catalog.png)

宛先プラットフォームへの新しい接続を作成するには、[&#x200B; 新しい宛先接続の作成方法 &#x200B;](../destinations/ui/connect-destination.md) に関するガイドに従ってください。 次に、[&#x200B; 手順 3](#activate-audiences) に進みます。

## 手順 3：宛先に対するオーディエンスのアクティブ化 {#activate-audiences}

Audience Manager オーディエンスを正常に [&#x200B; 取り込み &#x200B;](#configure-source)、[&#x200B; 新しい宛先接続を作成 &#x200B;](#create-destination-connection) したら、選択した宛先プラットフォームに対してオーディエンスをアクティブ化できます。

![&#x200B; 拡張されたアクティベーションの宛先カタログを示すExperience Platform UI 画像。](assets/activate-audiences.png)

宛先に対してオーディエンスをアクティブ化するには、[&#x200B; ストリーミング宛先に対してオーディエンスをアクティブ化する方法 &#x200B;](../destinations/ui/activate-segment-streaming-destinations.md) のガイドに従ってください。

## Audience Activation の検証 {#verify}

宛先へのデータのフローを監視する方法について詳しくは、[&#x200B; 宛先の監視に関するドキュメント &#x200B;](../dataflows/ui/monitor-destinations.md) を参照してください。
