---
title: Marketo Munchkin拡張機能の概要
description: Adobe Experience Platform の Marketo Munchkin 拡張機能について説明します。
exl-id: 8efc5203-91fc-4e89-be8f-74bf1aeeee5f
source-git-commit: 44e2b8241a8c348d155df3061d398c4fa43adcea
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 97%

---

# Marketo Munchkin 拡張機能の概要

この拡張機能を使用して、[!DNL Marketo Munchkin] JavaScript トラッキングコードをプロパティと統合します。[!DNL Marketo Munchkin] JavaScript を使用すると、Marketo ランディングページと外部 web ページに対するエンドユーザーのページ訪問回数およびナビゲーション回数を追跡できます。

## Marketo Munchkin 拡張機能のインストール

[!DNL Marketo Munchkin] 拡張機能がまだインストールされていない場合は、プロパティを開いてから&#x200B;**[!UICONTROL Extensions > Catalog]**&#x200B;を選択し、「[!DNL Marketo Munchkin]」拡張機能にカーソルを置いて「**[!UICONTROL Install]**」を選択します。

この拡張には必要な設定はありません。

## Marketo Munchkin 拡張機能のアクションタイプ

ここでは、 [!DNL Marketo Munchkin] 拡張機能で使用できるアクションタイプについて説明します。

### 初期設定

![](../../../images/munchkin-Init.png)

**Munchkin ID:（必須）**&#x200B;管理者／統合／Munchkin メニューにある Munchkin アカウント ID。

**Configurations：**&#x200B;設定可能なパラメーターはすべて[ここ](https://developers.marketo.com/javascript-api/lead-tracking/configuration/)に記載されています。

### Web ページにアクセス

![](../../../images/munchkin-visit-page.png)

**url：（必須）**&#x200B;ページ訪問の記録に使用する URL ファイルパス。

**params：**&#x200B;記録する必要のあるパラメーターのクエリ文字列。

**name：** Web ページアセットのカスタム名。

### リンクをクリック

![](../../../images/munchkin-click-link.png)

**href：（必須）**&#x200B;リンク選択の記録に使用する URL ファイルパス。
