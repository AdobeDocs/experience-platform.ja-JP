---
solution: Experience Platform
title: セグメント化サービス UI ガイド
description: Adobe Experience Platform UI でオーディエンスおよびセグメント定義を作成および管理する方法について説明します。
exl-id: 0a2e8d82-281a-4c67-b25b-08b7a1466300
source-git-commit: f6d700087241fb3a467934ae8e64d04f5c1d98fa
workflow-type: tm+mt
source-wordcount: '1046'
ht-degree: 70%

---

# セグメント化サービス UI ガイド

[!DNL Adobe Experience Platform Segmentation Service] は、オーディエンスおよびセグメント定義を作成および管理するためのユーザーインターフェイスを提供します。

## はじめに

オーディエンスおよびセグメント定義に取り組むには、セグメント化に関連する様々な [!DNL Experience Platform] サービスについて理解している必要があります。このユーザガイドを読む前に、次のサービスのドキュメントを確認してください。

- [[!DNL Segmentation Service]](../home.md)：[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータを細かいグループに分類できます。
- [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [[!DNL Adobe Experience Platform Identity Service]](../../identity-service/home.md)：[!DNL Experience Platform] に取り込まれる様々なデータソースの ID を結合することで、顧客プロファイルの作成を有効にします。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Experience Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。セグメント化を最大限に活用するには、[データモデリングのベストプラクティス](../../xdm/schema/best-practices.md)に従って、データがプロファイルとイベントとして取り込まれていることを確認してください。

また、このドキュメントで使用されている次の主な用語を理解し、それらの違いを理解する必要があります。

- **オーディエンス**：類似した行動や特性を共有する人の集まりです。このユーザーグループは、セグメント定義（Experience-Platform で生成されたオーディエンス）、オーディエンス構成を使用したAdobe Experience Platformによって生成することも、カスタムアップロードなどの外部ソース（外部で生成されたオーディエンス）から生成することもできます。
- **セグメント定義**：Adobe Experience Platform が、ターゲットオーディエンスの重要な特徴や行動の説明に使用するルールです。
- **セグメント**：プロファイルをオーディエンスに分割する行為です。

## 概要

Experience Platform UI で、左側のナビゲーションで「**[!UICONTROL オーディエンス]**」を選択して「**[!UICONTROL 概要]**」タブを開き、[!UICONTROL オーディエンス]ダッシュボードを表示します。

>[!NOTE]
>
>Experience Platformを初めて使用する組織で、アクティブなプロファイルデータセットや結合ポリシーが作成されていない場合は、[!UICONTROL &#x200B; オーディエンス &#x200B;] ダッシュボードは表示されません。 代わりに、「[!UICONTROL 概要]」タブには、オーディエンスを開始する際に役立つリンクとドキュメントが表示されます。

### [!UICONTROL オーディエンス]ダッシュボード {#segments-dashboard}

**[!UICONTROL ダッシュボード]**&#x200B;ダッシュボードは、組織のオーディエンスデータに関連する主要指標の概要を示します。

詳しくは、[オーディエンスダッシュボードガイド](../../dashboards/guides/audiences.md)を参照してください。

![オーディエンスダッシュボードが表示されます。オーディエンスサイズ、ID 別のプロファイル、ID オーバラップ、オーディエンスサイズの変化のトレンドなど、様々なウィジェットが表示されます。](../../dashboards/images/segments/dashboard-overview.png)

## 参照 {#browse}

**[!UICONTROL 参照]** タブを選択して、オーディエンスポータルを表示します。 Audience Portal には、組織およびサンドボックスに属するすべてのオーディエンスのリストが表示され、プロファイル数、接触チャネル、作成日、最終変更日、タグ、分類などの詳細が含まれます。

さらに、オーディエンスポータルを使用すると、セグメントビルダーまたはオーディエンスコンポジションを使用して新しいオーディエンスを作成したり、外部で生成されたオーディエンスをExperience Platformに読み込んだりできます。

Audience Portal について詳しくは、[Audience Portal の概要 ](./audience-portal.md) を参照してください。

## 構成 {#compositions}

「**[!UICONTROL 構成]**」タブを選択すると、組織のオーディエンス構成で生成されたすべてのオーディエンスのリストが表示されます。

![組織のオーディエンス構成で作成されたオーディエンスのリスト。](../images/ui/overview/compositions.png)

デフォルトで、このビューにはオーディエンスに関する情報（名前、ステータス、作成日、作成者、最終更新日、最終更新者など）がリスト表示されます。

各オーディエンスの横には省略記号アイコンが表示されます。これを選択すると、オーディエンスで使用可能なクイックアクションのリストが表示されます。

| アクション | 説明 |
| ------ | ----------- |
| 複製 | 選択したオーディエンスをコピーします。 |
| アクセスの管理 | オーディエンスに属するアクセスラベルを管理します。 アクセスラベルについて詳しくは、[ラベルの管理](../../access-control/abac/ui/labels.md)に関するドキュメントを参照してください。 |
| 削除 | 選択したオーディエンスを削除します。 ダウンストリームの宛先で使用されているオーディエンス、または他のオーディエンスに依存しているオーディエンス **削除できません**。 オーディエンスの削除について詳しくは、[ セグメント化に関する FAQ](../faq.md#lifecycle-states) を参照してください。 |

「![テーブルをカスタマイズ](/help/images/icons/column-settings.png)」アイコンを選択して、表示するフィールドを変更できます。

![「テーブルをカスタマイズ」ボタンがハイライト表示されます。このボタンを選択すると、オーディエンス構成ページに表示されるフィールドをカスタマイズできます。](../images/ui/overview/compositions-select-customize-table.png)

ポップオーバーが表示され、テーブル内に表示できるすべてのフィールドがリストされます。

![構成セクションに表示できる属性。](../images/ui/overview/compositions-customize-table.png)

| フィールド | 説明 |
| ----- | ----------- | 
| [!UICONTROL 名前] | オーディエンスの名前。 |
| [!UICONTROL ステータス] | オーディエンスのステータス。このフィールドの可能な値には、`Draft`、`Inactive` および `Published` が含まれます。 |
| [!UICONTROL 作成日] | オーディエンスが作成された日時。 |
| [!UICONTROL 作成者] | オーディエンスを作成した人物の名前。 |
| [!UICONTROL 更新済み] | オーディエンスが最後に更新された日時。 |
| [!UICONTROL 更新者] | オーディエンスを最後に更新したユーザーの名前。 |

オーディエンスの構成方法を確認するには、「[!UICONTROL オーディエンス]」タブでオーディエンス名を選択します。

オーディエンス構成 ページが表示され、オーディエンスを構成する構成要素が表示されます。オーディエンス構成の使用方法について詳しくは、[オーディエンス構成 UI ガイド](./audience-composition.md)を参照してください。

## 連合オーディエンス構成 {#fac}

オーディエンスの構成とセグメントの定義に加えて、Adobe Federated Audience Composition を使用すると、基になるデータをコピーせずにエンタープライズデータセットから新しいオーディエンスを作成し、それらのオーディエンスをAdobe Experience Platform Audience Portal に保存できます。 また、Enterprise Data Warehouse からフェデレーションされた作成済みオーディエンスデータを利用して、Adobe Experience Platformの既存のオーディエンスを強化することもできます。 [連合オーディエンス構成](https://experienceleague.adobe.com/ja/docs/federated-audience-composition/using/home)に関するガイドを参照してください。

![ 組織の Federated Audience Composition で作成されたオーディエンスのリスト。](../images/ui/overview/federated-audience-composition.png)

## ストリーミングセグメント化 {#streaming-segmentation}

ストリーミングセグメント化は、データの豊富さを重視しながら、ほぼリアルタイムで [!DNL Experience Platform] でセグメント化を実行する機能です。ストリーミングセグメント化を使用すると、データが [!DNL Experience Platform] に到達する際にセグメントの選定が行われるようになり、セグメント化ジョブをスケジュールして実行する必要性が軽減されます。

ストリーミングセグメント化について詳しくは、[ストリーミングセグメント化ユーザーガイド](../methods/streaming-segmentation.md)を参照してください。

>[!NOTE]
>
>ストリーミングセグメント化を機能させるには、スケジュールされたセグメント化を組織で有効にする必要があります。 スケジュールされたセグメント化を有効にする方法について詳しくは、[このユーザーガイドのストリーミングセグメント化の節](#scheduled-segmentation)を参照してください。

## エッジセグメント化 {#edge-segmentation}

Edgeのセグメント化は、Experience Platform内のオーディエンスをエッジ上で即座に評価する機能で、これにより、同じページや次のページのパーソナライゼーションのユースケースが可能になります。

エッジセグメント化について詳しくは、[エッジセグメント化 UI ガイド](../methods/edge-segmentation.md)を参照してください。

## ポリシー違反

>[!NOTE]
>
>ポリシー違反は、宛先に割り当てられたオーディエンスを作成する場合にのみ適用されます。

オーディエンスの作成が完了すると、そのオーディエンスが Adobe Experience Platform データガバナンスによって分析され、オーディエンス内にポリシー違反がないことが確認されます。詳しくは、[データガバナンスの概要](../../data-governance/home.md)を参照してください。

![オーディエンスのポリシー違反が表示されています。](../images/ui/overview/audience-dule-policy-violations.png)

## 次の手順とその他のリソース {#next-steps}

[!DNL Segmentation Service] UI には豊富なワークフローが用意されており、マーケティング可能なオーディエンスを [!DNL Real-Time Customer Profile] データから分離できます。

[!DNL Segmentation Service] について詳しくは、引き続きこのドキュメントを参照してください。[!DNL Segmentation Service] API の使用方法については、[[!DNL Segmentation Service] 開発者ガイド](../api/overview.md)を参照してください。
