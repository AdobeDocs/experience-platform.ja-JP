---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
solution: Experience Platform
title: ローカル環境でのテキストエディターを使用したソースドキュメントページの作成
description: このドキュメントでは、ローカル環境を使用して、ソースのドキュメントを作成し、プルリクエスト（PR）を送信する手順を説明します。
exl-id: 4cc89d1d-bc42-473d-ba54-ab3d1a2cd0d6
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 5%

---

# ローカル環境でのテキストエディターを使用したソースドキュメントページの作成

このドキュメントでは、ローカル環境を使用して、ソースのドキュメントを作成し、プルリクエスト（PR）を送信する手順を説明します。

>[!TIP]
>
>ドキュメントプロセスをさらにサポートするには、Adobeコントリビューション ガイドの次のドキュメントを使用できます。 <ul><li>[Git および Markdown オーサリングツールのインストール ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/install-tools.html)</li><li>[ ドキュメントを参照するために Git リポジトリをローカルに設定する ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html)</li><li>[ 大幅な変更点に対する GitHub 投稿ワークフロー ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/full-workflow.html)</li></ul>

## 前提条件

次のチュートリアルでは、ローカルマシンに GitHub Desktop をインストールする必要があります。 GitHub デスクトップがない場合は、アプリケーションを [ こちら ](https://desktop.github.com/) からダウンロードできます。

## GitHub に接続し、ローカルオーサリング環境を設定します

ローカルオーサリング環境を設定する最初の手順は、[Adobe Experience Platform GitHub リポジトリ ](https://github.com/AdobeDocs/experience-platform.en) に移動することです。

![platform-repo](../assets/platform-repo.png)

Platform GitHub リポジトリのメインページで、「**分岐**」を選択します。

![ 分岐 ](../assets/fork.png)

リポジトリをローカルマシンにクローンするには、「**コード**」を選択します。 表示されるドロップダウンメニューから「**HTTPS**」を選択し、「**GitHub デスクトップで開く**」を選択します。

>[!TIP]
>
>詳しくは、[ ドキュメント用の Git リポジトリのローカルでのセットアップ ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html#create-a-local-clone-of-the-repository) に関するチュートリアルを参照してください。

![open-git-desktop](../assets/open-git-desktop.png)

次に、GitHub デスクトップで `experience-platform.en` リポジトリのクローンが作成されるまでしばらく待ちます。

![ 複製 ](../assets/cloning.png)

クローン作成が完了したら、GitHub デスクトップに移動して新しいブランチを作成します。 上部ナビゲーションから「**マスター**」を選択し、「**新しいブランチ**」を選択します

![new-branch](../assets/new-branch.png)

表示されるポップオーバーパネルで、ブランチのわかりやすい名前を入力し、「**ブランチを作成**」を選択します。

![create-branch-vs](../assets/create-branch-vs.png)

次に、「**Publishブランチ**」を選択します。

![publish-branch](../assets/publish-branch.png)

## ソースのドキュメントページを作成する

ローカルマシンにリポジトリのクローンを作成し、新しいブランチを作成したら、選択した [ テキストエディター ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/install-tools.html#understand-markdown-editors) を使用して、新しいソースのドキュメントページのオーサリングを開始できます。

Adobeでは、[Visual Studio Code](https://code.visualstudio.com/) を使用し、Adobeの Markdown オーサリング拡張機能をインストールすることをお勧めします。 拡張機能をインストールするには、Visual Studio Code を起動し、左側のナビゲーションから「**拡張機能**」タブを選択します。

![拡張機能](../assets/extension.png)

次に、検索バーに `Adobe Markdown Authoring` と入力し、表示されるページから **インストール** を選択します。

![ インストール ](../assets/install.png)

ローカルマシンの準備が整ったら、[sources ドキュメントテンプレート ](../assets/api-template.zip) をダウンロードし、選択したカテゴリを表す [`...`] を使用してファイルを抽出し `experience-platform.en/help/sources/tutorials/api/create/...` す。 例えば、データベースソースを作成する場合、データベースフォルダーを選択します。

最後に、テンプレートに記載されている手順に従って、ソースに関連する関連情報を使用してテンプレートを編集します。

![edit-template](../assets/edit-template.png)

## レビュー用にドキュメントを送信

プルリクエスト（PR）を作成し、ドキュメントをレビュー用に送信するには、まず [!DNL Visual Studio Code] （または選択したテキストエディター）で作業内容を保存します。 次に、GitHub デスクトップを使用して、コミットメッセージを入力し、「**Commit to create-source-documentation**」を選択します。

![commit-vs](../assets/commit-vs.png)

次に、「**接触チャネルをプッシュ**」を選択して、作業をリモートブランチにアップロードします。

![ プッシュオリジン ](../assets/push-origin.png)

プルリクエストを作成するには、「**プルリクエストを作成**」を選択します。

![create-pr-vs](../assets/create-pr-vs.png)

ベースおよび比較ブランチが正しいことを確認します。 更新を説明するメモを PR に追加してから、「**プルリクエストを作成**」を選択します。 これにより、作業の作業ブランチをAdobeリポジトリのマスターブランチに結合する PR が開きます。

>[!TIP]
>
>**メンテナーによる編集を許可** チェックボックスを選択したままにして、Adobeドキュメントチームが PR を編集できるようにします。

![create-pr](../assets/create-pr.png)

https://github.com/AdobeDocs/experience-platform.enの「プルリクエスト」タブを調べると、プルリクエストが送信されたことを確認できます。

![confirm-pr](../assets/confirm-pr.png)
