---
title: 'GitHub Webインターフェイスを使用して、宛先のドキュメントページを作成します '
seo-title: Use the GitHub web interface to create a destination documentation page
description: このページの手順では、GitHub Webインターフェイスを使用してドキュメントを作成し、プル要求を送信する方法を示します。
seo-description: The instructions on this page show you how to use the GitHub web interface to author documentation and submit a pull request.
exl-id: 4780e05e-3d1d-4f1b-8441-df28d09c1a88
source-git-commit: d2452bf0e59866d3deca57090001c4c5a0935525
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 2%

---

# GitHub Webインターフェイスを使用して、宛先のドキュメントページを作成します {#github-interface}

以下の手順は、GitHub Webインターフェイスを使用してドキュメントを作成し、プル要求(PR)を送信する方法を示しています。 ここで示す手順を実行する前に、「[Adobe Experience Platformの宛先](./documentation-instructions.md)で宛先をドキュメント化する」を読んでください。

>[!TIP]
>
>Adobeのコントリビューターガイドのサポートドキュメントも参照してください。
>* [GitおよびMarkdownオーサリングツールのインストール](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/install-tools.html?lang=en)
>* [ドキュメント用のローカル Git リポジトリの設定](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=en)
>* [大きな変更をする際の GitHub コントリビューションワークフロー](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/full-workflow.html?lang=en).


## GitHubオーサリング環境の設定 {#set-up-environment}

1. ブラウザーで、`https://github.com/AdobeDocs/experience-platform.en`に移動します。
2. リポジトリを[fork](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=en#fork-the-repository)するには、下の図に示すように、「**Fork**」をクリックします。

   ![ForkAdobeドキュメントリポジトリ](./assets/ssd-fork-repo.png)

3. リポジトリのフォークで、次に示すように、プロジェクトの新しいブランチを作成します。 この新しい分岐を作業に使用します。

   ![新しいGitHubブランチの作成](./assets/new-branch-github.gif)

4. フォークされたリポジトリのGitHubフォルダー構造で、`experience-platform.en/help/destinations/catalog/[...]`に移動します。`[...]`は宛先の目的のカテゴリです。 例えば、パーソナライゼーションの宛先をExperience Platformに追加する場合は、カテゴリ`personalization`を選択します。 **ファイルを追加/新しいファイルを作成**&#x200B;を選択します。

   ![新しいファイルの追加](./assets/github-navigate-and-create-file.gif)

5. 宛先に`YOURDESTINATION.md`と名前を付けます。YOURDESTINATIONは、Adobe Experience Platformでの宛先の名前です。 例えば、会社の名前がMoviestarの場合、ファイルに`moviestar.md`という名前を付けます。

## 宛先のドキュメントページの作成 {#author-documentation}

1. [ドキュメントセルフサービステンプレート](./self-service-template.md)に基づいて、宛先ページのコンテンツを作成します。 **[](assets/yourdestination-template.zip)** テンプレートをダウンロードし、解凍してファイルテンプレ `.md` ートを抽出します。
2. [dillinger.io](https://dillinger.io/)などのオンラインマークダウンエディターで、目的の宛先に関する情報を使用して、テンプレートのコンテンツを貼り付けて編集します。 入力内容と削除可能な段落の詳細については、テンプレートの手順に従ってください。
3. Markdownエディターの内容をGitHubの新しいファイルにコピーします。
4. 使用するスクリーンショットや画像については、GitHubインターフェイスを使用して`experience-platform.en/help/destinations/assets/catalog/[...]`にファイルをアップロードします。ここで、`[...]`は、目的のカテゴリです。 例えば、パーソナライゼーションの宛先をExperience Platformに追加する場合は、カテゴリ`personalization`を選択します。 オーサリング中のページの画像にリンクする必要があります。 [画像へのリンク方法](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/linking.html?lang=en#link-to-images)を参照してください。

   ![GitHubへの画像のアップロード](./assets/upload-image.gif)

5. 準備が整ったら、ブランチにファイルを保存します。

![ファイル作成の確認](./assets/ssd-confirm-file-creation.png)

## ドキュメントを送信してレビュー {#submit-review}

1. ファイルを保存し、必要な画像をアップロードしたら、プルリクエスト(PR)を開いて、作業ブランチをAdobeドキュメントリポジトリのmasterブランチにマージできます。 作業中のブランチが選択されていることを確認し、「**プルリクエスト**」を選択します。

![プル要求の作成](./assets/ssd-create-pull-request-1.png)

1. ベースと比較ブランチが正しいことを確認します。 PRに、更新を説明するメモを追加し、「**プル要求を作成**」を選択します。 これにより、フォークの作業ブランチをAdobeリポジトリのmasterブランチにマージするPRが開きます。

   >[!TIP]
   >
   >AdobeドキュメントチームがPRを編集できるように、「**メンテナーによる編集を許可**」チェックボックスは選択したままにします。

   ![ドキュメントリポジトリへのプルリクエストのAdobe](./assets/ssd-create-pull-request-2.png)

1. この時点で、Adobe寄稿者使用許諾契約(CLA)に署名するよう求める通知が表示されます。 これは必須の手順です。 CLAに署名した後、PRページを更新し、プル要求を送信します。

1. プルリクエストが送信されたことを確認するには、`https://github.com/AdobeDocs/experience-platform.en`の「**プルリクエスト**」タブを調べます。

   ![PR成功](./assets/ssd-pr-successful.png)

1. ご協力ありがとうございます。編集が必要な場合はAdobeドキュメントチームがPRに連絡し、ドキュメントの公開日を知らせます。

>[!TIP]
>
>ドキュメントに画像やリンクを追加したり、Markdownに関するその他の質問については、Adobeの共同執筆ガイドの「Markdown](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=en)の使用」を参照してください。[
