---
title: 提案の適用
description: 単一ページアプリケーションで、指標を増分せずに提案をレンダリングします。
source-git-commit: 217282135bcd750740f4d3f8c6e17a0b8f9578bd
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 1%

---

# 提案の適用

**[!UICONTROL Apply propositions]** アクションタイプを使用すると、単一ページアプリケーションで、指標を増分せずに提案をレンダリングできます。 このアクションタイプは、単一ページアプリケーションを操作している際に、ページの一部が再レンダリングされ、ページに適用済みのパーソナライゼーションが上書きされる可能性が生じる場合に役立ちます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Rules]** に移動して、目的のルールを選択します。
1. 「[!UICONTROL Actions]」で、既存のアクションを選択するか、アクションを作成します。
1. [!UICONTROL Extension] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、[!UICONTROL Action type] を **[!UICONTROL Apply propositions]** に設定します。

![ 「提案を適用」アクションタイプを示すExperience Platform タグ UI。](../assets/apply-propositions.png)

## ユースケース

このアクションタイプは、次のような様々なユースケースに使用できます。

1. **mbox HTML オファーをレンダリング**。 **[!UICONTROL Send event]** アクションから範囲またはサーフェスを介して明示的にリクエストされた提案は、自動的にはレンダリングされません。 **[!UICONTROL Apply propositions]** アクションタイプを使用すると、提案メタデータを指定してレンダリングする場所を Web SDKに指示できます。
2. **単一ページアプリケーションでビューのオファーをレンダリング** ビュー変更イベントのレンダリング時に、分析データの準備がまだ整っていない場合は、**[!UICONTROL Apply propositions]** アクションを使用して、ページ上部にビューの提案をレンダリングできます。 詳しくは [ ページイベントの上部と下部（2 番目のページビュー – オプション 2） ](/help/collection/use-cases/personalization/top-bottom-page-events.md) を参照してください。 これを使用するには、フォームに **[!UICONTROL View name]** を入力します。
3. **提案を再レンダリング**。 サイトで React などのフレームワークを使用してコンテンツを再レンダリングする場合、パーソナライゼーションの再適用が必要になる場合があります。 そのような場合は、**[!UICONTROL Apply propositions]** のアクションタイプを使用してこれを行うことができます。

このアクションタイプでは、レンダリングされた提案の表示イベントは送信されません。 レンダリングされた提案を追跡し、後続の **[!UICONTROL Send event]** 呼び出しに含められるようにします。

## 使用可能なフィールド

このアクションタイプでは、次のフィールドがサポートされます。

* **[!UICONTROL Instance]**：アクションが適用されるSDK インスタンス。 実装で 1 つのSDK インスタンスを使用している場合、このドロップダウンメニューは無効になります。
* **[!UICONTROL Propositions]**：再レンダリングする提案オブジェクトの配列。
* **[!UICONTROL View name]**: レンダリングするビューの名前。
* **[!UICONTROL Proposition metadata]**:HTML オファーの適用方法を指定するオブジェクト。 この情報は、フォームまたはデータ要素を通じて提供できます。 これには、次のプロパティが含まれます。
   * **[!UICONTROL Scope]**
   * **[!UICONTROL Selector]**
   * **[!UICONTROL Action type]**
