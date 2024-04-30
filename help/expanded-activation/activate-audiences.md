---
title: 拡張されたアクティベーションによるAudience Managerオーディエンスのアクティブ化
description: Audience Managerの拡張されたアクティベーションを通じて、Audience Managerオーディエンスをソーシャルおよび広告の宛先に対してアクティブ化する方法を説明します。
source-git-commit: 19fb369a7faa0c5ac27a34db7b848b0332cb8695
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---


# Audience Managerでのアクティベーション拡張によるオーディエンスのアクティブ化

ここでは、拡張アクティベーションでサポートされる宛先プラットフォームに対してAudience Managerからオーディエンスをアクティブ化するために従う必要があるエンドツーエンドのワークフローについて説明します。

## 始める前に {#before-you-begin}

このガイドで説明する手順は、を読んだことを前提としています [拡張されたアクティベーションの概要ページ](overview.md) また、オーディエンスアクティベーションの前提条件を満たしていることを確認しました。

>[!IMPORTANT]
>
>を使用してオーディエンスをアクティブ化するには [!DNL Expanded Activation]を参照します。Audience Managerオーディエンスがに基づいていることを確認します **ハッシュ化されたメールアドレス**. を参照してください。 [前提条件](overview.md#prerequisites) を参照してください。

## 手順 1:Audience Managerソース接続の設定 {#configure-source}

この [Audience Managerソースコネクタ](../sources/connectors/adobe-applications/audience-manager.md) は、Adobe Audience Managerで収集されたオーディエンスデータを送信して、拡張されたアクティベーションでサポートされる出力先プラットフォームでアクティベーションを行います。

方法に関するガイドに従う [Audience Managerソース接続の作成](../sources/tutorials/ui/create/adobe-applications/audience-manager.md) ：ソースコネクタを設定します。

![Audience Managerソース接続を含む「ソース」タブを示す Platform UI 画像。](assets/sources-tab.png)

>[!TIP]
>
>Adobe Audience Manager ソースコネクタは、拡張されたアクティベーションで使用できる唯一のソースコネクタです。
>
>追加の識別子に基づいてオーディエンスを取り込む場合は、のエディションを購入する必要があります [Real-Time CDP](../rtcdp/overview.md). 詳しくは、Adobe担当者にお問い合わせください。

### 取り込んだオーディエンスの表示と監視 {#view-audiences}

Audience Managerから拡張アクティベーションに取り込んだオーディエンスは、で表示できます。 **[!UICONTROL オーディエンス]** ダッシュボード。

オーディエンスを表示するには、に移動します。 **[!UICONTROL 顧客]** -> **[!UICONTROL オーディエンス]** -> **[!UICONTROL 参照]**.

![オーディエンス ページを示す Platform UI 画像。](assets/audiences-browse.png)

>[!IMPORTANT]
>
>* 拡張されたアクティベーションにオーディエンスが完全に入力されるまでに、最大 48 時間かかる場合があります。 これは、既存のAudience Managerオーディエンスの更新にも当てはまります。
>* 新しく作成されたAudience Managerオーディエンスは、拡張されたアクティベーションでは自動的には表示されません。 拡張されたアクティベーションで新しいセグメントを取り込むには、Audience Managerソースコネクタを使用してセグメントを追加する必要があります。

Audience Managerソースコネクタを設定したら、に移動します。 [手順 2](#create-destination-connection).

## 手順 2：新しい宛先接続の作成 {#create-destination-connection}

Audience Managerオーディエンスを任意の宛先プラットフォームに送信するには、まず宛先プラットフォームへの接続を作成する必要があります。

左側のサイドバーで、に移動します。 **[!UICONTROL 接続]** -> **[!UICONTROL 宛先]** -> **[!UICONTROL カタログ]**.

使用可能な宛先カテゴリ [!DNL Expanded Activation] は [広告](../destinations/catalog/advertising/overview.md) および [ソーシャル](../destinations/catalog/social/overview.md).

![拡張されたアクティベーションの宛先カタログを示す Platform UI 画像。](assets/destination-catalog.png)

宛先プラットフォームへの新しい接続を作成するには、のガイドに従ってください。 [新しい宛先接続を作成する方法](../destinations/ui/connect-destination.md). 次に、に移動します [手順 3](#activate-audiences).

## 手順 3：宛先に対するオーディエンスのアクティブ化 {#activate-audiences}

正常に完了したら [取り込まれたAudience Managerオーディエンス](#configure-source) および [新しい宛先接続を作成しました](#create-destination-connection)を選択した宛先プラットフォームに対してオーディエンスをアクティブ化できるようになりました。

![拡張されたアクティベーションの宛先カタログを示す Platform UI 画像。](assets/activate-audiences.png)

宛先に対してオーディエンスをアクティブ化するには、に関するガイドに従ってください。 [ストリーミング宛先に対してオーディエンスをアクティブ化する方法](../destinations/ui/activate-segment-streaming-destinations.md).

## Audience Activation の検証 {#verify}

を確認します [宛先の監視ドキュメント](../dataflows/ui/monitor-destinations.md) 宛先へのデータのフローを監視する方法について詳しくは、こちらを参照してください。