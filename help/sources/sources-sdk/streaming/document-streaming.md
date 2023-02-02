---
title: ソースのドキュメント化（ストリーミング SDK）
description: Adobe Experience Platformで新しいソースをライブにする前の最後の手順は、新しいソースをドキュメント化することです。
hide: true
hidefromtoc: true
source-git-commit: a9879530a8911d77dfc4cde3824834e03fa5e0a9
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 0%

---

# ソースのドキュメント化（ストリーミング SDK）

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

事前入力された [API ドキュメントのテンプレート](streaming-template-api.md) または [UI ドキュメントテンプレート](streaming-template-ui.md) を使用して、ソースのドキュメントを作成できます。 以下では、テンプレートの編集方法とプル要求の開き方について説明します。 新しいソース用に送信されたドキュメントは、Adobeドキュメントチームがレビューおよび公開します。

また、以下のドキュメントテンプレートをダウンロードすることもできます。

* [API ドキュメントのテンプレート](../assets/streaming/streaming-template-api.zip)
* [UI ドキュメントテンプレート](../assets/streaming/streaming-template-ui.zip)

## 新しいソースページを作成します

GitHub Web インターフェイスまたはローカル環境を使用して、Platform での新しいソースに関するドキュメントを作成できます。 両方のオプションの手順については、次のリンクを参照してください。

* [GitHub Web インターフェイスを使用してソースドキュメントページを作成する](../documentation/github.md)
* [ローカル環境でテキストエディターを使用して、ソースドキュメントページを作成します](../documentation/text-editor.md)
