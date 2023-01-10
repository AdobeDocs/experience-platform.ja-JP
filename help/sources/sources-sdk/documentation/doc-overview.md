---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
solution: Experience Platform
title: ソースのドキュメント化
description: Adobe Experience Platformで新しいソースをライブにする前の最後の手順は、新しいソースをドキュメント化することです。
exl-id: 80daadb1-127f-4f42-8bc9-fb89a7898462
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 3%

---

# ソースをドキュメント化

Adobe Experience Platformで新しいソースをライブに設定する前の最後の手順は、新しいソースをドキュメント化することです。

このドキュメントガイドには、以下が含まれます。

* 新しいソースのドキュメントページを作成するために従うことのできるチュートリアル。
* 新しいソースに入力するためのドキュメントテンプレート。
* [Markdown を使用して技術ドキュメントを記述する方法に関する説明](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=en);
* [Markdown のフレーバーの理解Adobe](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=en#custom-markdown-extensions).

## 前提条件

新しいソースのドキュメント化を開始する前に、次の項目が必要です。

* **有効な GitHub ユーザーアカウント**:既存の GitHub アカウントを持っていない場合、 [GitHub の新規登録ページ](https://github.com/);
* **GitHub Desktop へのアクセス**:次を使用する必要があります。 [GitHub Desktop アプリ](https://desktop.github.com/) ローカル環境でソースドキュメントを作成する場合。
* Adobeとの統合は、Platform のステージング環境にデプロイされたソースとのテストフェーズにある必要があります。

## Platform でソースに関するドキュメントを作成するための概要手順

ソースのドキュメントを作成するには、Platform ドキュメントリポジトリのフォークを作成し、提供されたドキュメントテンプレートを新しいブランチで編集する必要があります。 Adobeが提供するテンプレートを使用して、新しいソースページを作成し、準備が整ったらプルリクエスト (PR) を開きます。 これをおこなう手順は、新しいソースページを作成する手順で詳しく説明します。

## ドキュメントテンプレート

事前入力された [ドキュメントテンプレート](./template.md) を使用して、ソースのドキュメントを作成できます。 以下では、テンプレートの編集方法とプル要求の開き方について説明します。 新しいソース用に送信されたドキュメントは、Adobeドキュメントチームがレビューおよび公開します。

また、以下のドキュメントテンプレートをダウンロードすることもできます。

* [API ドキュメントのテンプレート](../assets/api-template.zip)
* [UI ドキュメントテンプレート](../assets/ui-template.zip)

## 新しいソースページを作成します

GitHub Web インターフェイスまたはローカル環境を使用して、Platform での新しいソースに関するドキュメントを作成できます。 両方のオプションの手順については、次のリンクを参照してください。

* [GitHub Web インターフェイスを使用してソースドキュメントページを作成する](./github.md)
* [ローカル環境でテキストエディターを使用して、ソースドキュメントページを作成します](./text-editor.md)
