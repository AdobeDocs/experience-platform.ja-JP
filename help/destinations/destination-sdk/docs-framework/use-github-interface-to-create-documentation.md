---
title: 'GitHub Web インターフェイスを使用して、宛先のドキュメントページを作成します '
description: このページの手順では、GitHub Web インターフェイスを使用して、Experience Platformの宛先に関するドキュメントページを作成し、レビュー用に送信する方法を示します。
exl-id: 4780e05e-3d1d-4f1b-8441-df28d09c1a88
source-git-commit: 83539a9aa2fddcae0c9a44302d8bfa9d9f56de0c
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 1%

---

# GitHub Web インターフェイスを使用して、宛先のドキュメントページを作成します {#github-interface}

以下の手順は、GitHub Web インターフェイスを使用してドキュメントを作成し、プル要求 (PR) を送信する方法を示しています。 ここで示す手順を実行する前に、「[Adobe Experience Platformの宛先 ](./documentation-instructions.md) で宛先をドキュメント化する」をお読みください。

>[!TIP]
>
>Adobeのコントリビューターガイドのサポートドキュメントも参照してください。
>* [Git および Markdown オーサリングツールのインストール](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/install-tools.html?lang=en)
>* [ドキュメント用のローカル Git リポジトリの設定](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=en)
>* [大きな変更をする際の GitHub コントリビューションワークフロー](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/full-workflow.html?lang=en).


## GitHub オーサリング環境の設定 {#set-up-environment}

1. ブラウザーで、`https://github.com/AdobeDocs/experience-platform.en` に移動します。
2. リポジトリを [fork](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=en#fork-the-repository) するには、下の図に示すように、「**Fork**」をクリックします。

   ![フォークAdobeドキュメントリポジトリ](./assets/ssd-fork-repository.gif)

3. リポジトリのフォークで、次に示すように、プロジェクトの新しいブランチを作成します。 この新しいブランチを作業に使用します。

   ![新しい GitHub ブランチの作成](./assets/new-branch-github.gif)

4. フォークされたリポジトリの GitHub フォルダー構造で、 `experience-platform.en/help/destinations/catalog/[...]` に移動します。ここで `[...]` は、宛先の目的のカテゴリです。 例えば、パーソナライゼーションの宛先をExperience Platformに追加する場合は、カテゴリ `personalization` を選択します。 **ファイルを追加/新しいファイルを作成** を選択します。

   ![新しいファイルの追加](./assets/github-navigate-and-create-file.gif)

5. 宛先に `YOURDESTINATION.md` と名前を付けます。YOURDESTINATION はAdobe Experience Platformでの宛先の名前です。 例えば、会社の名前が Moviestar の場合、ファイルに `moviestar.md` という名前を付けます。

## 宛先のドキュメントページの作成 {#author-documentation}

1. [ ドキュメントセルフサービステンプレート ](./self-service-template.md) に基づいて、宛先ページのコンテンツを作成します。 **[](assets/yourdestination-template.zip)** テンプレートをダウンロードし、解凍してファイルテンプレ `.md` ートを抽出します。
2. [dillinger.io](https://dillinger.io/) など、目的の宛先に関する情報を使用して、テンプレートのコンテンツを貼り付け、編集します。 入力内容と削除可能な段落の詳細については、テンプレートの手順に従ってください。

   >[!TIP]
   >
   >ブラウザーウィンドウはいつでも閉じて、後で再度開くことができます。 作業内容は自動的に保存され、ブラウザーを再度開く際に待ちます。
3. Markdown エディターから GitHub の新しいファイルにコンテンツをコピーします。
4. 使用するスクリーンショットや画像については、GitHub インターフェイスを使用して `experience-platform.en/help/destinations/assets/catalog/[...]` にファイルをアップロードします。ここで、`[...]` は目的のカテゴリです。 例えば、パーソナライゼーションの宛先をExperience Platformに追加する場合は、カテゴリ `personalization` を選択します。 オーサリング中のページの画像にリンクする必要があります。 [ 画像へのリンク方法 ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/linking.html?lang=en#link-to-images) を参照してください。

   ![GitHub への画像のアップロード](./assets/upload-image.gif)

5. 準備が整ったら、ブランチにファイルを保存します。

![ファイル作成の確認](./assets/ssd-confirm-file-creation.png)

## ドキュメントのレビュー用送信 {#submit-review}

>[!TIP]
>
>ここでは何も壊せないことに注意してください。 この節の手順に従うことで、単にドキュメントの更新を提案するだけです。 推奨される更新は、Adobe Experience Platformドキュメントチームによって承認または編集されます。

1. ファイルを保存し、必要な画像をアップロードした後、プルリクエスト (PR) を開いて、作業ブランチをAdobeドキュメントリポジトリの master ブランチにマージできます。 作業中のブランチが選択されていることを確認し、**Contribute/Pull request** を選択します。

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
