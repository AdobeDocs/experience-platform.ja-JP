---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
solution: Experience Platform
title: Sourceの文書化
description: 新しいソースをAdobe Experience Platformでライブにするには、最後に新しいソースをドキュメント化する必要があります。
exl-id: 80daadb1-127f-4f42-8bc9-fb89a7898462
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 4%

---

# ソースのドキュメント化

新しいソースをAdobe Experience Platformでライブ設定する前に行う最後の手順は、新しいソースをドキュメント化することです。

このドキュメントガイドには、次のものが含まれます。

* 新しいソースのドキュメントページを作成する際に従うことができるチュートリアル。
* 新しいソースに入力するためのドキュメントテンプレート。
* [ 技術ドキュメントの執筆に Markdown を使用する方法に関する手順 ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html)
* [Adobe Markdown フレーバーを理解するための手順 ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html#custom-markdown-extensions)。

## 前提条件

新しいソースのドキュメント化を開始する前に、次の項目が必要です。

* **有効な GitHub ユーザーアカウント**：既存の GitHub アカウントがない場合は、[GitHub サインアップ ページ ](https://github.com/) から作成する必要があります。
* **GitHub デスクトップへのアクセス**：ローカル環境でソースドキュメントを作成するには、[GitHub デスクトップアプリ ](https://desktop.github.com/) を使用する必要があります。
* Adobeとの統合は、Experience Platformのステージング環境にソースをデプロイして、テスト段階にある必要があります。

## Experience Platformでソースのドキュメントを作成するための高度な手順

大まかなレベルでは、ソースのドキュメントを作成するには、Experience Platform ドキュメントリポジトリのフォークを作成し、提供されたドキュメントテンプレートを新しいブランチで編集する必要があります。 Adobe提供のテンプレートを使用して新しいソースページを作成し、準備が整ったらプルリクエスト（PR）を開きます。 これを行う手順は、新しいソースページを作成する手順で後述します。

## ドキュメントテンプレート

事前入力された [ ドキュメントテンプレート ](./template.md) を使用して、ソースのドキュメントの作成に役立てることができます。 さらに下では、テンプレートを編集してプルリクエストを開く方法を説明します。 新しいソース用に送信されたドキュメントは、Adobe ドキュメントチームによってレビューおよび公開されます。

以下のドキュメントテンプレートもダウンロードできます。

* [API ドキュメントのテンプレート](../assets/api-template.zip)
* [UI ドキュメントテンプレート](../assets/ui-template.zip)

## 新しいソースページを作成

GitHub の web インターフェイスまたはローカル環境を使用して、Experience Platformで新しいソースのドキュメントを作成することができます。 以下のリンクに、両方のオプションの手順が記載されています。

* [GitHub web インターフェイスを使用したソースドキュメントページの作成](./github.md)
* [ローカル環境でのテキストエディターを使用したソースドキュメントページの作成](./text-editor.md)
