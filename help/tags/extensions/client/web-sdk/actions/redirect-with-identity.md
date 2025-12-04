---
title: ID でリダイレクト
description: 組織のドメイン全体で訪問者識別子を共有できるようにします。
source-git-commit: db7e6df1b1a0eb19518d9c6ccd6e6bb9131d5a3e
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 1%

---

# ID でリダイレクト

**[!UICONTROL Redirect with identity]** アクションタイプを使用すると、現在のページの訪問者識別子を、組織が所有する別のドメインと共有できます。 クリックイベントおよび値比較条件で使用するように設計されています。 これは、JavaScript ライブラリの [`appendIdentityToUrl`](/help/collection/js/commands/appendidentitytourl.md) コマンドと機能的に似ています。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Rules]** に移動して、目的のルールを選択します。
1. 「[!UICONTROL Actions]」で、既存のアクションを選択するか、アクションを作成します。
1. [!UICONTROL Extension] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、[!UICONTROL Action type] を **[!UICONTROL Redirect with identity]** に設定します。

## ユースケース

* **ドメインをまたいで個人を識別**：訪問者があるドメインから組織が所有する別のドメインをクリックした場合、このアクションを使用して、訪問者が引き続き同じ個人と見なすことができます。 この識別方法は、複数のドメインのデータを組み合わせて、訪問者の水増しを防ぐレポートがある場合に特に便利です。
* **モバイルアプリから web アプリへの個人の識別**：訪問者がモバイルアプリ内にいて、web アプリへのリンクをクリックした場合、このアクションを使用して、web SDKが同じ人物であることを確認できます。 このワークフローにより、レポートとパーソナライゼーションで一貫性のあるエクスペリエンスを提供できます。

## 使用可能なフィールド

* **[!UICONTROL Instance]**：アクションが適用されるSDK インスタンス。 実装で 1 つのSDK インスタンスを使用している場合、このドロップダウンメニューは無効になります。
* **[!UICONTROL Datastream configuration overrides]**：このコマンドは、データストリーム設定の上書きをサポートし、このデータを受信するアプリやサービスを制御できます。 個々のコマンドとタグ拡張機能設定内の両方でデータストリーム設定の上書きを設定した場合、個々のコマンドが優先されます。 詳しくは、[ データストリーム設定の上書き ](../configure/configuration-overrides.md) を参照してください。

## ルールの例

このコマンドは、通常、クリックをリッスンして目的のドメインを確認する特定のルールと共に使用されます。

+++ルールイベントの条件

`href` プロパティを持つアンカータグがクリックされたときのトリガー。

* **[!UICONTROL Extension]**: コア
* **[!UICONTROL Event type]**: クリックします
* **[!UICONTROL When the user clicks on]**：特定の要素
* **[!UICONTROL Elements matching the CSS selector]**：`a[href]`

![ ルールイベント ](../assets/id-sharing-event-configuration.png)

+++

+++ルール条件

目的のドメインのみでトリガーします。

* **[!UICONTROL Logic type]**：標準
* **[!UICONTROL Extension]**: コア
* **[!UICONTROL Condition Type]**：値の比較
* **[!UICONTROL Left Operand]**：`%this.hostname%`
* **[!UICONTROL Operator]**：正規表現に一致
* **[!UICONTROL Right Operand]**：目的のドメインに一致する正規表現。 例：`adobe.com$|behance.com$`

![ ルールの条件 ](../assets/id-sharing-condition-configuration.png)

+++

+++ルールアクション

URL に ID を追加します。

* **[!UICONTROL Extension]**:Adobe Experience Platform Web SDK
* **[!UICONTROL Action Type]**: ID を使用したリダイレクト

![ ルールアクション ](../assets/id-sharing-action-configuration.png)

+++
