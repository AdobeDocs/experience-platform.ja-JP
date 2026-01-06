---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
solution: Experience Platform
title: GitHub Web インターフェイスを使用したソースドキュメントページの作成
description: このドキュメントでは、GitHub web インターフェイスを使用して、ドキュメントを作成し、プルリクエスト（PR）を送信する手順を説明します。
exl-id: 84b4219c-b3b2-4d0a-9a65-f2d5cd989f95
source-git-commit: acf1d4b9b2de6e0f674aca1f44b2504f3792327d
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 2%

---

# GitHub web インターフェイスを使用したソースドキュメントページの作成

このドキュメントでは、GitHub web インターフェイスを使用して、ドキュメントを作成し、プルリクエスト（PR）を送信する手順を説明します。

>[!TIP]
>
>ドキュメントプロセスをさらにサポートするには、Adobeのコントリビューション ガイドに記載されている次のドキュメントを使用できます。 <ul><li>[Git および Markdown オーサリングツールのインストール &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/install-tools.html?lang=ja)</li><li>[&#x200B; ドキュメントを参照するために Git リポジトリをローカルに設定する &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=ja)</li><li>[&#x200B; 大幅な変更点に対する GitHub 投稿ワークフロー &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/full-workflow.html?lang=ja)</li></ul>

## GitHub 環境の設定

GitHub 環境を設定する最初の手順は、[Adobe Experience Platform GitHub リポジトリ &#x200B;](https://github.com/AdobeDocs/experience-platform.ja-JP) に移動することです。

![platform-repo](../assets/platform-repo.png)

次に、「**分岐**」を選択します。

![&#x200B; 分岐 &#x200B;](../assets/fork.png)

分岐が完了したら、「**main**」を選択し、表示されるドロップダウンメニューに新しいブランチの名前を入力します。 ブランチの名前は作業を格納するために使用されるため、わかりやすい名前を指定し、「**ブランチを作成**」を選択します。

![create-branch](../assets/create-branch.png)

フォークしたリポジトリーの GitHub フォルダー構造で、[`experience-platform.en/help/sources/tutorials/api/create/`](https://github.com/AdobeDocs/experience-platform.ja-JP/tree/main/help/sources/tutorials/api/create) に移動し、リストからソースに適したカテゴリを選択します。 例えば、新しい CRM ソースのドキュメントを作成する場合は、「**crm**」を選択します。

>[!TIP]
>
>UI 用のドキュメントを作成する場合は、[`experience-platform.en/help/sources/tutorials/ui/create/`](https://github.com/AdobeDocs/experience-platform.ja-JP/tree/main/help/sources/tutorials/ui/create) に移動して、ソースに適したカテゴリを選択します。 画像を追加するには、に移動 [`experience-platform.en/help/sources/images/tutorials/create/sdk`](https://github.com/AdobeDocs/experience-platform.ja-JP/tree/main/help/sources/images/tutorials/create)、スクリーンショットを `sdk` フォルダーに追加します。

![crm](../assets/crm.png)

既存の CRM ソースのフォルダーが表示されます。 新しいソースのドキュメントを追加するには、「**ファイルを追加**」を選択し、表示されるドロップダウンメニューから「**新しいファイルを作成**」を選択します。

![create-new-file](../assets/create-new-file.png)

ソースファイルに `YOURSOURCE.md` という名前を付けます。YOURSOURCE は、Experience Platformでのソースの名前です。 例えば、会社が ACME CRM の場合、ファイル名は `acme-crm.md` にします。

![git インターフェイス &#x200B;](../assets/git-interface.png)

## ソースのドキュメントページを作成する

新しいソースのドキュメント化を開始するには、[sources documentation template](./template.md) のコンテンツを GitHub web エディターに貼り付けます。 テンプレートは [&#x200B; こちら &#x200B;](../assets/api-template.zip) からダウンロードすることもできます。

テンプレートを GitHub web エディターインターフェイスにコピーしたら、テンプレートに記載されている手順に従って、ソースに関連する情報を含んだ値を編集します。

![paste-template](../assets/paste-template.png)

完了したら、ブランチでファイルをコミットします。

![&#x200B; コミット &#x200B;](../assets/commit.png)

## レビュー用にドキュメントを送信

ファイルがコミットされたら、プルリクエスト（PR）を開いて、作業中のブランチをAdobe ドキュメントリポジトリのメインブランチに結合できます。 作業中のブランチが選択されていることを確認し、「**比較してプル要求**」を選択します。

![compare-pr](../assets/compare-pr.png)

ベースおよび比較ブランチが正しいことを確認します。 更新を説明するメモを PR に追加してから、「**プルリクエストを作成**」を選択します。 これにより、作業の作業ブランチをAdobe リポジトリの main ブランチに結合する PR が開きます。

>[!TIP]
>
>「**メンテナーによる編集を許可**」チェックボックスを選択したままにして、Adobe ドキュメントチームが PR を編集できるようにします。

![create-pr](../assets/create-pr.png)

この時点で、Adobe投稿者使用許諾契約（CLA）への署名を求める通知が表示されます。 これは必須の手順です。 CLA に署名したら、PR ページを更新し、プルリクエストを送信します。

https://github.com/AdobeDocs/experience-platform.ja-JPの「プルリクエスト」タブを調べると、プルリクエストが送信されたことを確認できます。

![confirm-pr](../assets/confirm-pr.png)
