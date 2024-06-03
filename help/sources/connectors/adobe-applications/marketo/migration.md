---
title: 移行ソースを使用して、ECID マッピングをユーザーからアクティビティにMarketo Engageする
description: データソースを使用して、人物データセットからアクティビティデータセットに ECID マッピングをMarketo Engageする方法を説明します。
source-git-commit: 0c695e11e7d7c14ef7e047cd007668e1099bf127
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 1%

---

# からの ECID マッピングの移行 [!DNL Person] データセットを [!DNL Activity] データセット

から ECID マッピングを移行できます [!DNL Marketo Engage Person] データセットを [!DNL Activity] データ取り込みと id 管理の動作をより安定させるデータセット。 さらに、この移行では次の問題にも対処できます。

| 問題点 | ソリューション |
| --- | --- |
| いつ [!DNL Marketo Person] データセットに複数の ECID へのリンクがあり、 [エクスペリエンスデータモデル（XDM）レコードの ID の合計数が 20 を超えています](../../../../identity-service/guardrails.md). | ECID フィールドマッピングの移行 [!DNL Activity]を使用すると、からの ID の数を確実に [!DNL Marketo Person] データフローは制限内にとどまり、データの取り込みを成功させることができます。 |
| 毎回 [!DNL Marketo Person] データセットが ECID （から取得したすべての ECID のタイムスタンプ）で取り込まれる [!DNL Marketo Person] データセットは、人物レコードの最後に更新されたタイムスタンプで更新されます。 その結果、次のような結果となる可能性があります [id グラフからの最近の id の誤った削除](../../../../identity-service/guardrails.md#understanding-the-deletion-logic-when-an-identity-graph-at-capacity-is-updated). | ECID フィールドのマッピングをに移行する [!DNL Activity]を使用すると、ID サービスは ECID のタイムスタンプを正しく反映でき、ID サービスの「先入れ先出し」メカニズムによって、より安定した動作が提供されます。 |
| ECID がで取り込まれるタイミング [!DNL Marketo Person] データフローでは、新しく追加された ECID は、に対する更新がない限り、Experience Platformに取り込まれません。 [!DNL Person] レコードの場所 [!DNL Marketo]. | 新しい ECID がにリンクされている場合 [!DNL Person] レコードの場所 [!DNL Marketo]を使用すると、その ECID データを [!DNL Marketo Activity] データフローし、Experience Platform時に直ちに ID グラフの更新を求めます。 |

基本的に、次の操作を行う必要があります。

* を更新 [!DNL Marketo Activity] データフロー。
* を更新 [!DNL Marketo Person] データフロー。

## 更新 [!DNL Marketo Activity] データフロー {#update-activity-dataflow}

次の手順に従って、 [!DNL Marketo Activity] データフロー：

* Experience PlatformUI で、に移動します。 *ソース* ワークスペースで、の既存のデータフローを検索します [!DNL Marketo Activity] データ。
* データフローが有効な場合は、省略記号（`...`）を選択し、選択します。 **[!UICONTROL データフローの更新]**.
* 次に、を選択します **[!UICONTROL 次]** に到達するまで *マッピング* インターフェイス。
* が含まれる *マッピング* インタフェース、選択 **[!UICONTROL 新しいフィールド]** を選択してから、 **[!UICONTROL 計算フィールドを追加]**. ここから、次を追加する必要があります。

| ソースデータセット | XDM ターゲットフィールド |
| --- | --- |
| `iif(${web\.ecid} != null, to_object('ECID', arrays_to_objects('id', explode(last(split(${web\.ecid}, ":")), " "))), null)` | `identityMap` |

>[!NOTE]
>
>既存のを更新する場合 [!DNL Marketo] データフローは、ECID マッピングフィールドの追加または削除のみで構成され、履歴バックフィルジョブが自動的にスキップされます。 新しいデータ取り込みは、「web ページにアクセス」や「web ページをクリック」などのアクティビティタイプが発生した場合にのみ発生します。

## 更新 [!DNL Marketo Person] データフロー {#update-person-dataflow}

次の手順に従って、 [!DNL Marketo Person] データフロー：

* Experience PlatformUI で、に移動します。 *ソース* ワークスペースで、の既存のデータフローを検索します [!DNL Marketo Person] データ。
* データフローが有効な場合は、省略記号（`...`）を選択し、選択します。 **[!UICONTROL データフローの更新]**.
* 次に、を選択します **[!UICONTROL 次]** に到達するまで *マッピング* インターフェイス。
* が含まれる *マッピング* インターフェイスで、にマッピングする計算フィールドを削除します `identityMap` を選択してから、 **[!UICONTROL 次]** および **[!UICONTROL 保存して取り込み]**.

>[!NOTE]
>
>既存のを更新する場合 [!DNL Marketo] データフローは、ECID マッピングフィールドの追加または削除のみで構成され、履歴バックフィルジョブが自動的にスキップされます。 以前に取り込まれた ECID のタイムスタンプは同じままです。 これらは、既存の ECID に対応する新しいデータが取り込まれた場合にのみ更新されます。

## 次の手順

このドキュメントでは、から ECID マッピングを移行する方法を確認しました [!DNL Marketo Person] データセットを [!DNL Marketo Activity] データセット。 詳しくは、以下を参照してください [!DNL Marketo] ドキュメント：

* [取り込むデータフローの作成 [!DNL Marketo] データをExperience Platformに](../../../tutorials/ui/create/adobe-applications/marketo.md).
* [フィールドマッピングガイド](../mapping/marketo.md).