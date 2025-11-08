---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
solution: Experience Platform
title: ローカル環境でのテキストエディターを使用したソースドキュメントページの作成
description: このドキュメントでは、ローカル環境を使用して、ソースのドキュメントを作成し、プルリクエスト（PR）を送信する手順を説明します。
exl-id: 4cc89d1d-bc42-473d-ba54-ab3d1a2cd0d6
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '601'
ht-degree: 5%

---

# ローカル環境でのテキストエディターを使用したソースドキュメントページの作成

このドキュメントでは、ローカル環境を使用して、ソースのドキュメントを作成し、プルリクエスト（PR）を送信する手順を説明します。

>[!TIP]
>
>ドキュメントプロセスをさらにサポートするには、Adobeのコントリビューション ガイドに記載されている次のドキュメントを使用できます。 <ul><li>[Git および Markdown オーサリングツールのインストール &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/install-tools.html?lang=ja)</li><li>[&#x200B; ドキュメントを参照するために Git リポジトリをローカルに設定する &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=ja)</li><li>[&#x200B; 大幅な変更点に対する GitHub 投稿ワークフロー &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/full-workflow.html?lang=ja)</li></ul>

## 前提条件

次のチュートリアルでは、ローカルマシンに GitHub Desktop をインストールする必要があります。 GitHub デスクトップがない場合は、アプリケーションを [&#x200B; こちら &#x200B;](https://desktop.github.com/) からダウンロードできます。

## GitHub に接続し、ローカルオーサリング環境を設定します

ローカルオーサリング環境を設定する最初の手順は、[Adobe Experience Platform GitHub リポジトリ &#x200B;](https://github.com/AdobeDocs/experience-platform.ja-JP) に移動することです。

![platform-repo](../assets/platform-repo.png)

Experience Platform GitHub リポジトリのメインページで、「**分岐**」を選択します。

![&#x200B; 分岐 &#x200B;](../assets/fork.png)

リポジトリをローカルマシンにクローンするには、「**コード**」を選択します。 表示されるドロップダウンメニューから「**HTTPS**」を選択し、「**GitHub デスクトップで開く**」を選択します。

>[!TIP]
>
>詳しくは、[&#x200B; ドキュメント用の Git リポジトリのローカルでのセットアップ &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=ja#create-a-local-clone-of-the-repository) に関するチュートリアルを参照してください。

![open-git-desktop](../assets/open-git-desktop.png)

次に、GitHub デスクトップで `experience-platform.en` リポジトリのクローンが作成されるまでしばらく待ちます。

![&#x200B; 複製 &#x200B;](../assets/cloning.png)

クローン作成が完了したら、GitHub デスクトップに移動して新しいブランチを作成します。 上部ナビゲーションから「**マスター**」を選択し、「**新しいブランチ**」を選択します

![new-branch](../assets/new-branch.png)

表示されるポップオーバーパネルで、ブランチのわかりやすい名前を入力し、「**ブランチを作成**」を選択します。

![create-branch-vs](../assets/create-branch-vs.png)

次に、「**ブランチを公開**」を選択します。

![publish-branch](../assets/publish-branch.png)

## ソースのドキュメントページを作成する

ローカルマシンにリポジトリのクローンを作成し、新しいブランチを作成したら、選択した [&#x200B; テキストエディター &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/install-tools.html?lang=ja#understand-markdown-editors) を使用して、新しいソースのドキュメントページのオーサリングを開始できます。

Adobeでは、[Visual Studio Code](https://code.visualstudio.com/) を使用し、Adobe Markdown オーサリング拡張機能をインストールすることをお勧めします。 拡張機能をインストールするには、Visual Studio Code を起動し、左側のナビゲーションから「**拡張機能**」タブを選択します。

![拡張機能](../assets/extension.png)

次に、検索バーに `Adobe Markdown Authoring` と入力し、表示されるページから **インストール** を選択します。

![&#x200B; インストール &#x200B;](../assets/install.png)

ローカルマシンの準備が整ったら、[sources ドキュメントテンプレート &#x200B;](../assets/api-template.zip) をダウンロードし、選択したカテゴリを表す [`...`] を使用してファイルを抽出し `experience-platform.en/help/sources/tutorials/api/create/...` す。 例えば、データベースソースを作成する場合、データベースフォルダーを選択します。

最後に、テンプレートに記載されている手順に従って、ソースに関連する関連情報を使用してテンプレートを編集します。

![edit-template](../assets/edit-template.png)

## レビュー用にドキュメントを送信

プルリクエスト（PR）を作成し、ドキュメントをレビュー用に送信するには、まず [!DNL Visual Studio Code] （または選択したテキストエディター）で作業内容を保存します。 次に、GitHub デスクトップを使用して、コミットメッセージを入力し、「**Commit to create-source-documentation**」を選択します。

![commit-vs](../assets/commit-vs.png)

次に、「**接触チャネルをプッシュ**」を選択して、作業をリモートブランチにアップロードします。

![&#x200B; プッシュオリジン &#x200B;](../assets/push-origin.png)

プルリクエストを作成するには、「**プルリクエストを作成**」を選択します。

![create-pr-vs](../assets/create-pr-vs.png)

ベースおよび比較ブランチが正しいことを確認します。 更新を説明するメモを PR に追加してから、「**プルリクエストを作成**」を選択します。 これにより、作業の作業ブランチをAdobe リポジトリのマスターブランチに結合する PR が開きます。

>[!TIP]
>
>「**メンテナーによる編集を許可**」チェックボックスを選択したままにして、Adobe ドキュメントチームが PR を編集できるようにします。

![create-pr](../assets/create-pr.png)

https://github.com/AdobeDocs/experience-platform.ja-JPの「プルリクエスト」タブを調べると、プルリクエストが送信されたことを確認できます。

![confirm-pr](../assets/confirm-pr.png)
