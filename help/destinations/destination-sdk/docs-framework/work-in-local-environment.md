---
title: ローカル環境でテキストエディターを使用して、宛先のドキュメントページを作成する
description: このページの手順では、テキストエディターを使用してローカル環境で作業し、Experience Platformの宛先のドキュメントページを作成して、レビュー用に送信する方法を示します。
exl-id: 125f2d10-0190-4255-909c-5bd5bb59fcba
source-git-commit: 83539a9aa2fddcae0c9a44302d8bfa9d9f56de0c
workflow-type: tm+mt
source-wordcount: '871'
ht-degree: 2%

---

# ローカル環境でテキストエディターを使用して、宛先のドキュメントページを作成する {#local-authoring}

このページの手順では、テキストエディターを使用してローカル環境で作業し、ドキュメントを作成してプル要求 (PR) を送信する方法を示します。 ここで示す手順を実行する前に、「[Adobe Experience Platformの宛先 ](./documentation-instructions.md) で宛先をドキュメント化する」をお読みください。

>[!TIP]
>
>Adobeのコントリビューターガイドのサポートドキュメントも参照してください。
>* [Git および Markdown オーサリングツールのインストール](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/install-tools.html?lang=en)
>* [ドキュメント用のローカル Git リポジトリの設定](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=en)
>* [大きな変更をする際の GitHub コントリビューションワークフロー](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/full-workflow.html?lang=en).


## GitHub に接続し、ローカルのオーサリング環境を設定する {#set-up-environment}

1. ブラウザーで、`https://github.com/AdobeDocs/experience-platform.en` に移動します。
2. リポジトリを [fork](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=en#fork-the-repository) するには、スクリーンショットに示すように **Fork** をクリックします。

   ![フォークAdobeドキュメントリポジトリ](./assets/ssd-fork-repository.gif)

3. ローカルマシンにリポジトリのクローンを作成. **Code/HTTPS/Open with GitHub Desktop** を選択します（下図を参照）。 [GitHub Desktop](https://desktop.github.com/) がインストールされていることを確認します。 詳しくは、Adobe貢献者ガイドの [ リポジトリのローカルクローンを作成する ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=en#create-a-local-clone-of-the-repository) を参照してください。

   ![Adobe・ドキュメント・リポジトリのローカル環境への複製](./assets/clone-local.png)

4. ローカルファイル構造で、 `experience-platform.en/help/destinations/catalog/[...]` に移動します。ここで `[...]` は宛先のカテゴリです。 例えば、パーソナライゼーションの宛先をExperience Platformに追加する場合は、`personalization` フォルダーを選択します。

## 宛先のドキュメントページの作成 {#author-documentation}

1. ドキュメントページは、[ セルフサービスの宛先テンプレート ](./self-service-template.md) に基づいています。 [ 宛先テンプレート ](assets/yourdestination-template.zip) をダウンロードします。 ファイルを解凍し、上記の手順 4 で示したディレクトリに `yourdestination-template.md` ファイルを展開します。  ファイルの名前を `YOURDESTINATION.md` に変更します。YOURDESTINATION はAdobe Experience Platformでの宛先の名前です。 例えば、会社の名前が Moviestar の場合、ファイルに `moviestar.md` という名前を付けます。
2. [ 任意のテキストエディター ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/install-tools.html?lang=en#understand-markdown-editors) で新しいファイルを開きます。 Adobeでは、[Visual Studio Code](https://code.visualstudio.com/) を使用し、Markdown AuthoringAdobe拡張機能をインストールすることをお勧めします。 拡張機能をインストールするには、Visual Studio Code を開き、画面の左側にある「**[!DNL Extensions]**」タブを選択して、「`adobe markdown authoring`」を検索します。 拡張機能を選択し、「**[!DNL Install]**」をクリックします。
   ![AdobeMarkdown Authoring 拡張機能のインストール](./assets/install-adobe-markdown-extension.gif)
3. 目的の宛先に関する情報を使用して、テンプレートを編集します。 テンプレートの指示に従います。
4. ドキュメントに追加する予定のスクリーンショットや画像については、`GitHub/experience-platform.en/help/destinations/assets/catalog/[...]` に移動します。`[...]` は目的のカテゴリです。 例えば、パーソナライゼーションの宛先をExperience Platformに追加する場合は、`personalization` フォルダーを選択します。 宛先の新しいフォルダーを作成し、ここに画像を保存します。 作成するページからリンクする必要があります。 [ 画像へのリンク方法 ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/linking.html?lang=en#link-to-images) を参照してください。
5. 準備が整ったら、作業中のファイルを保存します。

## ドキュメントのレビュー用送信 {#submit-review}

>[!TIP]
>
>ここでは何も壊せないことに注意してください。 この節の手順に従うことで、単にドキュメントの更新を提案するだけです。 推奨される更新は、Adobe Experience Platformドキュメントチームによって承認または編集されます。

1. GitHub Desktop で、更新の作業ブランチを作成し、「**ブランチを公開**」を選択して、ブランチを GitHub に公開します。

![ローカルの新しいブランチ](./assets/new-branch-local.gif)

1. GitHub Desktop で、次に示すように [ 作業をコミット ](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/github-glossary#commit) します。

   ![ローカルコミット](./assets/commit-local.png)

1. GitHub Desktop で、以下に示すように、[](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/github-glossary#push) 作業内容を [ リモート ](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/github-glossary#remote) ブランチにプッシュします。

   ![コミットをプッシュ](./assets/push-local-to-remote.png)

1. GitHub Web インターフェイスで、プルリクエスト (PR) を開き、作業ブランチをAdobeドキュメントリポジトリの master ブランチに結合します。 作業中のブランチが選択されていることを確認し、「**プルリクエスト**」を選択します。

   ![プル要求の作成](./assets/ssd-create-pull-request-1.gif)

1. ベースと比較ブランチが正しいことを確認します。 PR に、更新内容を説明するメモを追加し、「**プル要求を作成**」を選択します。 これにより、PR が開き、フォークの作業ブランチがAdobeリポジトリの master ブランチにマージされます。
   >[!TIP]
   >
   >Adobeドキュメントチームが PR を編集できるように、「**メンテナーによる編集を許可**」チェックボックスを選択したままにします。

   ![Adobeドキュメントリポジトリへのプル要求の作成](./assets/ssd-create-pull-request-2.png)

1. この時点で、Adobeコントリビューター使用許諾契約 (CLA) に署名するよう求める通知が表示されます。 これは必須の手順です。 CLA に署名した後、PR ページを更新し、プル要求を送信します。

1. プル要求が送信されたことを確認するには、`https://github.com/AdobeDocs/experience-platform.en` の「**プル要求**」タブを調べます。

![PR に成功](./assets/ssd-pr-successful.png)

1. ご協力ありがとうございます。Adobeドキュメントチームは、編集が必要な場合に備えて PR に連絡し、ドキュメントの公開日を知らせます。

>[!TIP]
>
>ドキュメントに画像やリンクを追加したり、Markdown に関するその他の質問については、Adobeの共同執筆ガイドの [Markdown](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=en) の使用を参照してください。
