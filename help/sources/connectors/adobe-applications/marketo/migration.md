---
title: 移行ソースを使用して、ECID マッピングをユーザーからアクティビティにMarketo Engageする
description: データソースを使用して、人物データセットからアクティビティデータセットに ECID マッピングをMarketo Engageする方法を説明します。
exl-id: bcc91c53-aeca-4d7c-89b5-cf025d0357a0
source-git-commit: d03fcf06fb63ad2484ded3b85fc11ae45661babc
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 1%

---

# データセットからデータセットへ [!DNL Person]ECID マッピング [!DNL Activity] 移行

ECID マッピングを [!DNL Marketo Engage Person] データセットから [!DNL Activity] データセットに移行して、データ取り込みと ID 管理のより安定した動作を提供できます。 さらに、この移行では次の問題にも対処できます。

| 問題点 | ソリューション |
| --- | --- |
| [!DNL Marketo Person] データセットに複数の ECID へのリンクがある場合、[&#x200B; エクスペリエンスデータモデル（XDM）レコードの ID の合計数が 20](../../../../identity-service/guardrails.md) を超えると、データ取り込みは失敗します。 | ECID フィールドマッピングを [!DNL Activity] に移行することで、[!DNL Marketo Person] データフローの ID の数が制限内に収まり、データ取り込みを成功させることができます。 |
| [!DNL Marketo Person] データセットが ECID で取り込まれるたびに、[!DNL Marketo Person] データセットからのすべての ECID のタイムスタンプが、人物レコードの最後に更新されたタイムスタンプで更新されます。 その結果、[ID グラフからの新しい ID の削除が正しく行われない &#x200B;](../../../../identity-service/guardrails.md#understanding-the-deletion-logic-when-an-identity-graph-at-capacity-is-updated) 可能性があります。 | ECID フィールドマッピングを [!DNL Activity] に移行することで、ID サービスは ECID のタイムスタンプを正しく反映でき、ID サービスの「先入れ先出し」メカニズムにより、より安定した動作が提供されます。 |
| ECID がデータフローを通じて取り込まれ [!DNL Marketo Person] 場合、新しく追加された ECID は、[!DNL Marketo] で [!DNL Person] レコードを更新しない限り、Experience Platformに取り込まれません。 | 新しい ECID が [!DNL Marketo] の [!DNL Person] レコードにリンクされている場合、[!DNL Marketo Activity] データフローを通じてその ECID データを取り込み、Experience Platform時に直ちに ID グラフの更新を求めることができます。 |

基本的に、次の操作を行う必要があります。

* [!DNL Marketo Activity] データフローを更新します。
* [!DNL Marketo Person] データフローを更新します。

## データフロー [!DNL Marketo Activity] 更新 {#update-activity-dataflow}

[!DNL Marketo Activity] データフローを更新するには、次の手順に従います。

* Experience PlatformUI で *ソース* ワークスペースに移動し、データの既存のデータフロー [!DNL Marketo Activity] 見つけます。
* データフローが有効な場合は、データフロー名の横にある省略記号（`...`）を選択し、「**[!UICONTROL データフローを更新]**」を選択します。
* 次に、**[!UICONTROL マッピング]** インターフェイスに到達するまで *次へ* を選択します。
* *マッピング* インターフェイスで、「**[!UICONTROL 新しいフィールド]**」を選択したあと、「**[!UICONTROL 計算フィールドを追加]**」を選択します。 ここから、次を追加する必要があります。

| ソースデータセット | XDM ターゲットフィールド |
| --- | --- |
| `iif(${web\.ecid} != null, to_object('ECID', arrays_to_objects('id', explode(last(split(${web\.ecid}, ":")), " "))), null)` | `identityMap` |

>[!NOTE]
>
>既存の [!DNL Marketo] データフローの更新が、ECID マッピングフィールドの追加または削除のみで構成されている場合、データフローは、履歴バックフィルジョブを自動的にスキップします。 新しいデータ取り込みは、「web ページにアクセス」や「web ページをクリック」などのアクティビティタイプが発生した場合にのみ発生します。

## データフロー [!DNL Marketo Person] 更新 {#update-person-dataflow}

[!DNL Marketo Person] データフローを更新するには、次の手順に従います。

* Experience PlatformUI で *ソース* ワークスペースに移動し、データの既存のデータフロー [!DNL Marketo Person] 見つけます。
* データフローが有効な場合は、データフロー名の横にある省略記号（`...`）を選択し、「**[!UICONTROL データフローを更新]**」を選択します。
* 次に、**[!UICONTROL マッピング]** インターフェイスに到達するまで *次へ* を選択します。
* *マッピング* インターフェイスで、`identityMap` にマッピングする計算フィールドを削除し、「**[!UICONTROL 次へ]**」および「**[!UICONTROL 保存して取り込み]** を選択します。

>[!NOTE]
>
>既存の [!DNL Marketo] データフローの更新が、ECID マッピングフィールドの追加または削除のみで構成されている場合、データフローは、履歴バックフィルジョブを自動的にスキップします。 以前に取り込まれた ECID のタイムスタンプは同じままです。 これらは、既存の ECID に対応する新しいデータが取り込まれた場合にのみ更新されます。

## 次の手順

このドキュメントでは、[!DNL Marketo Person] データセットからデータセットへの ECID マッピングの移行方法を確認 [!DNL Marketo Activity] ました。 詳しくは、次の [!DNL Marketo] ドキュメントを参照してください。

* [Experience Platformにデータを取り込むデ  [!DNL Marketo]  タフローを作成 &#x200B;](../../../tutorials/ui/create/adobe-applications/marketo.md)。
* [&#x200B; フィールドマッピングガイド &#x200B;](../mapping/marketo.md)。
