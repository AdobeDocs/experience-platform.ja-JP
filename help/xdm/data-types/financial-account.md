---
title: 金融口座データタイプ
description: 金融口座 XDM データ型について説明します。
exl-id: badf9b20-d397-4b46-b045-19c69806fe8e
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '90'
ht-degree: 8%

---

# [!UICONTROL 金融口座] データタイプ

[!UICONTROL 金融口座] は、金融口座のタイプ、所有者、現在の残高などの詳細を記述する標準の XDM データ型です。

![](../images/data-types/financial-account.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `currentAccountBalance` | [[!UICONTROL 通貨]](./currency.md) | 勘定の現在の残高。 |
| `financialAccountId` | [!UICONTROL 文字列] | アカウントの一意の ID。 |
| `financialAccountName` | [!UICONTROL 文字列] | アカウントに割り当てられた名前。 |
| `financialAccountType` | [!UICONTROL 文字列] | 金融口座のタイプ（当座預金、貯蓄、退職金など）。 |

{style="table-layout:auto"}

データタイプについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/financial-account.schema.json).
