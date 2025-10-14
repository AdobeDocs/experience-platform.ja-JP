---
title: 拡張機能バージョンビュー
description: このガイドでは、Adobe Experience Platform Assurance の拡張機能バージョンビューについて詳しく説明します。
exl-id: a3a649da-1ef1-45a3-a1ed-6a7bc16c2987
source-git-commit: 05a7b73da610a30119b4719ae6b6d85f93cdc2ae
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# 拡張機能バージョンビュー

拡張機能のバージョン ビューを使用すると、インストールしたAdobe Experience Platform モバイル拡張機能の順序を簡単に並べ替えたり、Assurance セッションに接続されたクライアントで拡張機能が最新かどうかを確認したりできます。

## 拡張機能バージョンビューの基本を学ぶ

[Assurance の設定 &#x200B;](../tutorials/implement-assurance.md) 後に、**ホーム** ビューで「**[!UICONTROL 拡張機能バージョン]**」を選択します

![&#x200B; 拡張機能のバージョン &#x200B;](./images/versions/versions-extension.png)

## バージョンが最新かどうかを確認します

このビュー内に、各 Mobile SDK の最新バージョンと、インストールした現在のバージョン（該当する場合）の両方がテーブルに表示されます。 特定のバージョンが最新バージョンと同期している場合、そのインストール済みバージョンには緑色のバッジが表示されます。 それ以外の場合は、バッジが赤で表示されます。

![&#x200B; 拡張機能のバージョン比較 &#x200B;](./images/versions/versions-extension-version.png)

## バージョンを書き出し

ビューの右上にある「**[!UICONTROL バージョンを書き出し]**」を選択すると、すべての拡張機能情報と、クライアントが使用するプラットフォームを含む JSON ペイロードが表示されます。 このデータを JSON ファイルに書き出すか、クリップボードにコピーするかを選択できます。

![&#x200B; 拡張機能バージョンのエクスポート &#x200B;](./images/versions/versions-extension-export.png)
