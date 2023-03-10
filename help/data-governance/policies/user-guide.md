---
keywords: Experience Platform;ホーム;人気のトピック;データガバナンス;データ使用ポリシーユーザーガイド
solution: Experience Platform
title: UI でのデータ使用ポリシーの管理
description: Adobe Experience Platform データガバナンスは、データ使用ポリシーを作成および管理できるユーザーインターフェイスを提供します。このドキュメントでは、Experience Platform ユーザーインターフェイスのポリシーワークスペースで実行できるアクションの概要について説明します。
exl-id: 29434dc1-02c2-4267-a1f1-9f73833e76a0
source-git-commit: e539b1e165227d9a888bfe12d8205e285b3ce259
workflow-type: tm+mt
source-wordcount: '1413'
ht-degree: 100%

---

# UI でのデータ使用ポリシーの管理 {#user-guide}

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_dataUsagePolicies_description"
>title="説明"
>abstract=""

このドキュメントでは、Adobe Experience Platform UI の&#x200B;**[!UICONTROL ポリシー]**&#x200B;ワークスペースを使用して、データ使用ポリシーを作成および管理する方法について説明します。

>[!NOTE]
>
>UI でアクセス制御ポリシーを管理する方法については、代わりに[属性ベースのアクセス制御 UI ガイド](../../access-control/abac/ui/policies.md)を参照してください。

>[!IMPORTANT]
>
>すべてのデータ使用ポリシー（アドビが提供するコアポリシーを含む）は、デフォルトで無効になっています。個々のポリシーを適用対象と見なすには、そのポリシーを手動で有効にする必要があります。UI でこれをおこなう手順については、[ポリシーの有効化](#enable)に関する節を参照してください。

## 前提条件

このガイドでは、次の [!DNL Experience Platform] の概念に関する十分な知識が必要です。

* [データガバナンス](../home.md)
* [データ使用ポリシー](./overview.md)

## 既存のポリシーの表示 {#view-policies}

[!DNL Experience Platform] UI で、「**[!UICONTROL ポリシー]**」を選択して、「**[!UICONTROL ポリシー]**」ワークスペースを開きます。「**[!UICONTROL 参照]**」タブには、使用可能なポリシー（関連するラベル、マーケティングアクション、ステータスなど）が一覧表示されます。

![](../images/policies/browse-policies.png)

同意ポリシーへのアクセス権がある場合は、**[!UICONTROL 同意ポリシー]**&#x200B;切り替えスイッチを選択して、「[!UICONTROL 参照]」タブで表示します。

![](../images/policies/consent-policy-toggle.png)

リストされているポリシーを選択すると、説明と種類が表示されます。カスタムポリシーを選択すると、ポリシーを編集、削除、[有効／無効にする](#enable)ための追加のコントロールが表示されます。

![](../images/policies/policy-details.png)

## カスタムポリシーの作成 {#create-policy}

新しいカスタムデータ使用ポリシーを作成するには、「**[!UICONTROL ポリシー]**」ワークスペースの「 **[!UICONTROL 参照]**」タブの右上にある「**[!UICONTROL ポリシーの作成]**」をクリックします。

![](../images/policies/create-policy-button.png)

同意ポリシーに関するベータ版に含まれているかどうかに応じて、次のいずれかが発生します。

* ベータ版に属していない場合は、すぐに[データガバナンスポリシーの作成](#create-governance-policy)のワークフローが表示されます。
* ベータ版に属している場合は、[同意ポリシーを作成する](#consent-policy)追加のオプションが表示されます。
   ![](../images/policies/choose-policy-type.png)

### データガバナンスポリシーの作成 {#create-governance-policy}

**[!UICONTROL ポリシーの作成]**&#x200B;ワークフローが表示されます。まず、新しいポリシーの名前と説明を指定します。

![](../images/policies/create-policy-description.png)

次に、ポリシーの基にするデータ使用ラベルを選択します。複数のラベルを選択する場合は、ポリシーが適用するために、データにすべてのラベルを含めるか、1 つのラベルのみを含めるかを選択できます。完了したら、「**[!UICONTROL 次へ]**」をクリックします。

![](../images/policies/add-labels.png)

「**[!UICONTROL マーケティングアクションの選択]**」手順が表示されます。表示されたリストから適切なマーケティングアクションを選択してから、「**[!UICONTROL 次へ]**」を選択して続行します。

>[!NOTE]
>
> 複数のマーケティングアクションを選択する場合、ポリシーはそれらを「OR」ルールとして解釈します。つまり、選択したマーケティングアクションの&#x200B;**いずれか**&#x200B;が実行された場合に、ポリシーが適用されます。

![](../images/policies/add-marketing-actions.png)

「**[!UICONTROL 確認]**」手順が表示され、新しいポリシーを作成する前にその詳細を確認できます。確認したら、「**[!UICONTROL 完了]**」を選択して、ポリシーを作成します。

![](../images/policies/policy-review.png)

「**[!UICONTROL 参照]**」タブが再び表示され、新しく作成したポリシーが「草案」ステータスでリストに表示されるようになります。ポリシーを有効にするには、次の節を参照してください。

![](../images/policies/created-policy.png)

### 同意ポリシーの作成 {#consent-policy}

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_dataUsagePolicies_instructions"
>title="説明"
>abstract=""

>[!IMPORTANT]
>
>同意ポリシーは、**Adobe Healthcare Shield** または **Adobe Privacy &amp; Security Shield** を購入した組織でのみ使用できます。

同意ポリシーの作成を選択すると、新しい画面が表示され、新しいポリシーを設定できます。

![](../images/policies/consent-policy-dialog.png)

同意ポリシーを利用するには、プロファイルデータに同意属性が存在する必要があります。結合スキーマに必要な属性を含める手順について詳しくは、[Experience Platform での同意処理](../../landing/governance-privacy-security/consent/adobe/overview.md)を参照してください。

同意ポリシーは、次の 2 つの論理コンポーネントで構成されます。

* **[!UICONTROL If]**：ポリシーチェックをトリガーにする条件。これは、実行される特定のマーケティングアクション、特定のデータ使用ラベルの有無またはこれら 2 つの組み合わせに基づくことができます。
* **[!UICONTROL Then]**：プロファイルをポリシーをトリガーしたアクションに含めるために存在する必要がある同意属性。

#### 条件の設定 {#consent-conditions}

>[!CONTEXTUALHELP]
>id="platform_governance_policies_consentif"
>title="If 条件"
>abstract="まず、ポリシーチェックをトリガーする条件を定義します。条件には、特定のマーケティングアクションが実行されていること、特定のデータガバナンスラベルが存在すること、またはその両方の組み合わせが含まれます。"

「**[!UICONTROL If]**」セクションで、このポリシーをトリガーするマーケティングアクションやデータ使用ラベルを選択します。**[!UICONTROL すべて表示]**&#x200B;および&#x200B;**[!UICONTROL ラベルを選択]**&#x200B;を選択すると、使用可能なマーケティングアクションとラベルの完全なリストがそれぞれ表示されます。

少なくとも 1 つの条件を追加したら、「**[!UICONTROL 条件を追加]**」 を選択して、必要に応じてさらに条件を追加し、ドロップダウンから適切な条件タイプを選択できます。

![](../images/policies/add-condition.png)

複数の条件を選択する場合は、条件間に表示されるアイコンを使用して、「AND」と「OR」の条件関係を切り替えることができます。

![](../images/policies/and-or-selection.png)

#### 同意属性を選択 {#consent-attributes}

>[!CONTEXTUALHELP]
>id="platform_governance_policies_consentthen"
>title="Then 条件"
>abstract="「If」条件を定義したら、「Then」セクションを使用して、結合スキーマから少なくとも 1 つの同意属性を選択します。これは、このポリシーが管理するアクションにプロファイルを含めるために存在する必要がある属性です。"

「**[!UICONTROL Then]**」セクションで、結合スキーマから少なくとも 1 つの同意属性を選択します。これは、このポリシーが管理するアクションにプロファイルを含めるために存在する必要がある属性です。 リストから提供されたオプションの 1 つを選択するか、**[!UICONTROL すべて表示]**&#x200B;を選択して結合スキーマから直接属性を選択できます。

同意属性を選択する場合、このポリシーで確認する属性の値を選択します。

![](../images/policies/select-schema-field.png)

1 つ以上の同意属性を選択した後、**[!UICONTROL ポリシーのプロパティ]**&#x200B;パネルが更新され、このポリシーで許可されるプロファイルの推定数（プロファイルストア全体に対する割合を含む）が表示されます。 この推定値は、ポリシーの設定を調整すると自動的に更新されます。

![](../images/policies/audience-preview.png)

ポリシーにさらに同意属性を追加するには、**[!UICONTROL 結果を追加]**&#x200B;を選択します。

![](../images/policies/add-result.png)

必要に応じて、ポリシーに条件と同意属性を引き続き追加および調整できます。 設定に問題がなければ、ポリシーの名前と説明（オプション）を入力した後、「**[!UICONTROL 保存]**」を選択します。

![](../images/policies/name-and-save.png)

これで同意ポリシーが作成され、そのステータスはデフォルトで[!UICONTROL 無効]に設定されます。すぐにポリシーを有効にするには、右側のパネルにある&#x200B;**[!UICONTROL ステータス]**&#x200B;切り替えスイッチを選択します。

![](../images/policies/enable-consent-policy.png)

#### ポリシーの適用を検証

同意ポリシーを作成して有効にしたら、宛先に対するセグメントをアクティブ化する際に、同意されたオーディエンスに与える影響をプレビューできます。詳しくは、[同意ポリシーの評価](../enforcement/auto-enforcement.md#consent-policy-evaluation)に関する節を参照してください。

## ポリシーの有効化または無効化 {#enable}

すべてのデータ使用ポリシー（アドビが提供するコアポリシーを含む）は、デフォルトで無効になっています。個々のポリシーの適用が考慮されるようにするには、API または UI を使用して手動でそのポリシーを有効にする必要があります。

「**[!UICONTROL ポリシー]**」ワークスペースの「**[!UICONTROL 参照]**」タブで、ポリシーを有効または無効にできます。リストからカスタムポリシーを選択して、右側に詳細を表示します。「**[!UICONTROL ステータス]**」で、ポリシーを有効または無効にする切り替えボタンを選択します。

![](../images/policies/enable-policy.png)

## マーケティングアクションの表示 {#view-marketing-actions}

「**[!UICONTROL ポリシー]**」ワークスペースで、「**[!UICONTROL マーケティングアクション]**」タブを選択し、アドビおよび自分の組織で定義されている使用可能なマーケティングアクションのリストを表示します。

![](../images/policies/marketing-actions.png)

## マーケティングアクションの作成 {#create-marketing-action}

新しいカスタムマーケティングアクションを作成するには、「**[!UICONTROL ポリシー]**」ワークスペースの「**[!UICONTROL マーケティングアクション]**」タブの右上隅にある「**[!UICONTROL マーケティングアクションを作成]**」を選択します。

![](../images/policies/create-marketing-action.png)

「**[!UICONTROL マーケティングアクションを作成]**」ダイアログが表示されます。マーケティングアクションの名前と説明を入力し、「**[!UICONTROL 作成]**」を選択します。

![](../images/policies/create-marketing-action-details.png)

新しく作成したアクションは、「**[!UICONTROL マーケティングアクション]**」タブに表示されます。これで、[新しいデータ使用ポリシーを作成](#create-policy)するときに、マーケティングアクションを使用できるようになりました。

![](../images/policies/created-marketing-action.png)

## マーケティングアクションの編集または削除 {#edit-delete-marketing-action}

>[!NOTE]
>
>編集できるのは、組織で定義されたカスタムマーケティングアクションのみです。アドビによって定義されたマーケティングアクションは、変更または削除できません。

「**[!UICONTROL ポリシー]**」ワークスペースで、「**[!UICONTROL マーケティングアクション]**」タブを選択し、アドビおよび自分の組織で定義されている使用可能なマーケティングアクションのリストを表示します。リストからカスタムマーケティングアクションを選択し、右側のセクションに表示されるフィールドを使用して、マーケティングアクションの詳細を編集します。

![](../images/policies/edit-marketing-action.png)

既存の使用ポリシーでマーケティングアクションが使用されていない場合は、「**[!UICONTROL マーケティングアクションを削除]**」を選択して削除できます。

>[!NOTE]
>
>既存のポリシーで使用されているマーケティングアクションを削除しようとすると、削除の試行に失敗したことを示すエラーメッセージが表示されます。

![](../images/policies/delete-marketing-action.png)

## 次の手順

このドキュメントでは、[!DNL Experience Platform] UI でデータ使用ポリシーを管理する方法の概要を説明しました。[!DNL Policy Service API] を使用してポリシーを管理する手順については、[デベロッパーガイド](../api/getting-started.md)を参照してください。データ使用ポリシーの適用方法について詳しくは、「[ポリシー実施の概要](../enforcement/overview.md)」を参照してください。

次のビデオでは、[!DNL Experience Platform] UI で使用ポリシーを操作する方法のデモを示します。

>[!VIDEO](https://video.tv.adobe.com/v/32977?quality=12&learn=on)
