---
title: Sourceのドキュメント化（ストリーミング SDK）
description: 新しいソースをAdobe Experience Platformでライブにするには、最後に新しいソースをドキュメント化する必要があります。
exl-id: 65ca7a4d-3e02-4f54-bf07-ea2c92b8dbf1
badge: ベータ版
source-git-commit: 256857103b4037b2cd7b5b52d6c5385121af5a9f
workflow-type: tm+mt
source-wordcount: '409'
ht-degree: 2%

---

# ソースのドキュメント化（ストリーミング SDK）

>[!NOTE]
>
>セルフサービスソース Streaming SDK はベータ版です。 ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

新しいソースをAdobe Experience Platformでライブ設定する前に行う最後の手順は、新しいソースをドキュメント化することです。

このドキュメントガイドには、次のものが含まれます。

* 新しいソースのドキュメントページを作成する際に従うことができるチュートリアル。
* 新しいソースに入力するためのドキュメントテンプレート。
* [ 技術ドキュメントの執筆に Markdown を使用する方法に関する手順 ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html)
* [Adobeマークダウンフレーバーを理解するための手順 ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html#custom-markdown-extensions)。

## 前提条件

新しいソースのドキュメント化を開始する前に、次の項目が必要です。

* **有効な GitHub ユーザーアカウント**：既存の GitHub アカウントがない場合は、[GitHub サインアップ ページ ](https://github.com/) から作成する必要があります。
* **GitHub デスクトップへのアクセス**：ローカル環境でソースドキュメントを作成するには、[GitHub デスクトップアプリ ](https://desktop.github.com/) を使用する必要があります。
* Adobeとの統合は、Platform のステージング環境にソースをデプロイして、テスト段階にある必要があります。

## Platform でソースのドキュメントを作成するための高度な手順

大まかに言えば、ソースのドキュメントを作成するには、Platform ドキュメントリポジトリのフォークを作成し、提供されたドキュメントテンプレートを新しいブランチで編集する必要があります。 Adobe提供のテンプレートを使用して新しいソースページを作成し、準備が整ったらプルリクエスト（PR）を開きます。 これを行う手順は、新しいソースページを作成する手順で後述します。

## ドキュメントテンプレート

ソースのドキュメントの作成に役立つように ](streaming-template-api.md) 事前入力された [API ドキュメントテンプレート [ または ](streaming-template-ui.md)UI ドキュメントテンプレート）を使用することができます。 さらに下では、テンプレートを編集してプルリクエストを開く方法を説明します。 新しいソース用に送信されたドキュメントは、Adobeドキュメントチームによってレビューおよび公開されます。

以下のドキュメントテンプレートもダウンロードできます。

* [API ドキュメントのテンプレート](../assets/streaming/streaming-template-api.zip)
* [UI ドキュメントテンプレート](../assets/streaming/streaming-template-ui.zip)

## 新しいソースページを作成

GitHub web インターフェイスまたはローカル環境を使用して、Platform の新しいソース用のドキュメントを作成できます。 以下のリンクに、両方のオプションの手順が記載されています。

* [GitHub web インターフェイスを使用したソースドキュメントページの作成](../documentation/github.md)
* [ローカル環境でのテキストエディターを使用したソースドキュメントページの作成](../documentation/text-editor.md)
