---
title: 拡張されたアクティベーションによるAudience Managerオーディエンスのアクティブ化
description: Audience Managerの拡張されたアクティベーションを通じて、Audience Managerオーディエンスをソーシャルおよび広告の宛先に対してアクティブ化する方法を説明します。
exl-id: 4105f5c5-db69-414f-9ee4-8630b0a86da7
source-git-commit: 2222e9fbf75f3082d331868f820247e0c0ce3ba2
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---

# Audience Managerでのアクティベーション拡張によるオーディエンスのアクティブ化

ここでは、拡張アクティベーションでサポートされる宛先プラットフォームに対してAudience Managerからオーディエンスをアクティブ化するために従う必要があるエンドツーエンドのワークフローについて説明します。

## 始める前に {#before-you-begin}

このガイドで説明されている手順は、[ 拡張されたアクティベーションの概要ページ ](overview.md) を読み、オーディエンスのアクティベーションの前提条件を満たしていることを確認したことを前提としています。

>[!IMPORTANT]
>
>[!DNL Expanded Activation] を通じてオーディエンスをアクティブ化するには、Audience Managerオーディエンスが **ハッシュ化されたメールアドレス** に基づいていることを確認します。 詳しくは、[ 前提条件 ](overview.md#prerequisites) を参照してください。

## 手順 1:Audience Managerソース接続の設定 {#configure-source}

[Audience Managerソースコネクタ ](../sources/connectors/adobe-applications/audience-manager.md) は、Adobe Audience Managerで収集されたオーディエンスデータを送信して、拡張されたアクティベーションでサポートされる出力先プラットフォームでアクティベーションを行います。

ソースコネクタを設定するには、[Audience Managerソース接続を作成する ](../sources/tutorials/ui/create/adobe-applications/audience-manager.md) 方法に関するガイドに従います。

![Audience Managerソース接続を含む「ソース」タブを示す Platform UI 画像。](assets/sources-tab.png)

>[!TIP]
>
>Adobe Audience Manager ソースコネクタは、拡張されたアクティベーションで使用できる唯一のソースコネクタです。
>
>追加の識別子に基づいてオーディエンスを取り込む場合は、[Real-Time CDP](../rtcdp/overview.md) のエディションを購入する必要があります。 詳しくは、Adobe担当者にお問い合わせください。

### 取り込んだオーディエンスの表示と監視 {#view-audiences}

Audience Managerから拡張アクティベーションに取り込んだオーディエンスは、**[!UICONTROL オーディエンス]** ダッシュボードで表示できます。

オーディエンスを表示するには、**[!UICONTROL 顧客]**/**[!UICONTROL オーディエンス]**/**[!UICONTROL 参照]** に移動します。

![ オーディエンスページを示す Platform UI 画像。](assets/audiences-browse.png)

>[!IMPORTANT]
>
>* 拡張されたアクティベーションにオーディエンスが完全に入力されるまでに、最大 48 時間かかる場合があります。 これは、既存のAudience Managerオーディエンスの更新にも当てはまります。
>* 新しく作成されたAudience Managerオーディエンスは、拡張されたアクティベーションでは自動的には表示されません。 拡張されたアクティベーションで新しいセグメントを取り込むには、Audience Managerソースコネクタを使用してセグメントを追加する必要があります。

Audience Managerソースコネクタを設定したら、[ 手順 2](#create-destination-connection) に進みます。

## 手順 2：新しい宛先接続の作成 {#create-destination-connection}

Audience Managerオーディエンスを任意の宛先プラットフォームに送信するには、まず宛先プラットフォームへの接続を作成する必要があります。

左サイドバーで、**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]** に移動します。

[!DNL Expanded Activation] で使用可能な宛先カテゴリは [ 広告 ](../destinations/catalog/advertising/overview.md) および [ ソーシャル ](../destinations/catalog/social/overview.md) です。

![ 拡張されたアクティベーションの宛先カタログを示す Platform UI 画像。](assets/destination-catalog.png)

宛先プラットフォームへの新しい接続を作成するには、[ 新しい宛先接続の作成方法 ](../destinations/ui/connect-destination.md) に関するガイドに従ってください。 次に、[ 手順 3](#activate-audiences) に進みます。

## 手順 3：宛先に対するオーディエンスのアクティブ化 {#activate-audiences}

Audience Managerオーディエンスを正常に [ 取り込み ](#configure-source)、[ 新しい宛先接続を作成 ](#create-destination-connection) したら、選択した宛先プラットフォームに対してオーディエンスをアクティブ化できます。

![ 拡張されたアクティベーションの宛先カタログを示す Platform UI 画像。](assets/activate-audiences.png)

宛先に対してオーディエンスをアクティブ化するには、[ ストリーミング宛先に対してオーディエンスをアクティブ化する方法 ](../destinations/ui/activate-segment-streaming-destinations.md) のガイドに従ってください。

## Audience Activation の検証 {#verify}

宛先へのデータのフローを監視する方法について詳しくは、[ 宛先の監視に関するドキュメント ](../dataflows/ui/monitor-destinations.md) を参照してください。
