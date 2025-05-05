---
title: 金融口座データ タイプ
description: 金融口座 XDM データタイプについて説明します。
exl-id: badf9b20-d397-4b46-b045-19c69806fe8e
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '90'
ht-degree: 8%

---

# [!UICONTROL &#x200B; 金融口座 &#x200B;] データタイプ

[!UICONTROL &#x200B; 金融口座 &#x200B;] は、金融口座のタイプ、所有者、現在の残高など、金融口座の詳細を記述する標準 XDM データタイプです。

![](../images/data-types/financial-account.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `currentAccountBalance` | [[!UICONTROL 通貨]](./currency.md) | 口座の現在の残高。 |
| `financialAccountId` | [!UICONTROL 文字列] | アカウントの一意の ID。 |
| `financialAccountName` | [!UICONTROL 文字列] | アカウントに割り当てられた名前。 |
| `financialAccountType` | [!UICONTROL 文字列] | 金融口座のタイプ （当座預金、普通預金、退職金など）。 |

{style="table-layout:auto"}

データタイプについて詳しくは、[ 公開 XDM リポジトリ ](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/financial-account.schema.json) を参照してください。
