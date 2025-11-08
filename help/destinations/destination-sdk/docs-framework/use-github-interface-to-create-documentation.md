---
title: GitHub web インターフェイスを使用した宛先ドキュメントページの作成
description: このページの手順では、GitHub web インターフェイスを使用して、Experience Platformの宛先のドキュメントページを作成し、レビュー用に送信する方法について説明します。
exl-id: 4780e05e-3d1d-4f1b-8441-df28d09c1a88
source-git-commit: ff094c0c2c75e097140626d77478b8da9a7edf04
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 3%

---

# GitHub web インターフェイスを使用した宛先ドキュメントページの作成 {#github-interface}

以下の手順は、GitHub web インターフェイスを使用して、ドキュメントを作成し、プルリクエスト（PR）を送信する方法を示しています。 ここに示す手順を実行する前に、[Adobe Experience Platformの宛先での宛先のドキュメント化 &#x200B;](./documentation-instructions.md) を読んでください。

>[!TIP]
>
>Adobe投稿者ガイドのサポートドキュメントも参照してください。
>
>* [Git および Markdown オーサリングツールのインストール &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/install-tools.html?lang=ja)
>* [&#x200B; ドキュメントを参照するために Git リポジトリをローカルに設定する &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=ja)
>* [&#x200B; 大規模な変更点に対する GitHub 投稿ワークフロー &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/full-workflow.html?lang=ja)。

## GitHub オーサリング環境の設定 {#set-up-environment}

1. ブラウザーで `https://github.com/AdobeDocs/experience-platform.ja-JP` に移動します。
1. リポジトリを [&#x200B; 分岐 &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=ja#fork-the-repository) するには、次に示すように、「**分岐**」をクリックします。 これにより、Experience Platform リポジトリのコピーが独自の GitHub アカウントに作成されます。

   ![Adobeの分岐ドキュメントリポジトリ &#x200B;](../assets/docs-framework/ssd-fork-repository.gif)

1. 次に示すように、リポジトリのフォークで、プロジェクトに新しいブランチを作成します。 この新しいブランチを作業に使用します。

   ![&#x200B; 新しい GitHub ブランチの作成 &#x200B;](../assets/docs-framework/new-branch-github.gif)

1. フォークされたリポジトリーの GitHub フォルダー構造で、`experience-platform.en/help/destinations/catalog/[...]` に移動します。ここで、`[...]` は宛先に必要なカテゴリです。 例えば、パーソナライゼーションの宛先をExperience Platformに追加する場合は、`personalization` カテゴリを選択します。 **ファイルを追加/新しいファイルを作成** を選択します。

   ![&#x200B; 新しいファイルを追加 &#x200B;](../assets/docs-framework/github-navigate-and-create-file.gif)

1. 宛先に `YOURDESTINATION.md` という名前を付けます。ここで、YOURDESTINATION はAdobe Experience Platformの宛先の名前です。 例えば、会社の名前が Moviestar の場合は、ファイルに `moviestar.md` という名前を付けます。

## 宛先に関するドキュメントページの作成 {#author-documentation}

1. [&#x200B; ドキュメントのセルフサービステンプレート &#x200B;](./self-service-template.md) に基づいて、宛先ページのコンテンツを作成します。 テンプレートを **[ダウンロード](../assets/docs-framework/yourdestination-template.zip)** し、展開して、`.md` ファイルテンプレートを抽出します。
1. [dillinger.io](https://dillinger.io/) などのオンラインマークダウンエディターに、宛先に関連する情報とテンプレートのコンテンツを貼り付けて編集します。 入力すべき内容と削除可能な段落の詳細については、テンプレートの指示に従います。

   >[!TIP]
   >
   >ブラウザーウィンドウはいつでも閉じ、後で再度開くことができます。 作業内容は自動的に保存され、ブラウザーを再び開くと待機されます。
1. マークダウンエディターから GitHub の新しいファイルにコンテンツをコピーします。
1. 使用する予定のスクリーンショットや画像について、GitHub インターフェイスを使用してファイルを `experience-platform.en/help/destinations/assets/catalog/[...]` にアップロードします。ここで、`[...]` は宛先に必要なカテゴリです。 例えば、パーソナライゼーションの宛先をExperience Platformに追加する場合は、`personalization` カテゴリを選択します。 オーサリングしているページから画像にリンクする必要があります。 [&#x200B; 画像へのリンク方法 &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/linking.html?lang=ja#link-to-images) を参照してください。

   ![GitHub への画像のアップロード &#x200B;](../assets/docs-framework/upload-image.gif)

1. 準備ができたら、ブランチにファイルを保存します。

   ![&#x200B; ファイルの作成を確認 &#x200B;](../assets/docs-framework/ssd-confirm-file-creation.png)

## レビュー用にドキュメントを送信 {#submit-review}

>[!TIP]
>
>ここで壊れるものはないことに注意してください。 この節の手順に従うことで、ドキュメントのアップデートを提案するだけです。 提案した更新は、Adobe Experience Platform ドキュメントチームによって承認または編集されます。

1. ファイルを保存し、目的の画像をアップロードしたら、プルリクエスト（PR）を開いて、作業ブランチをAdobe ドキュメントリポジトリのマスターブランチに結合できます。 作業したブランチが選択されていることを確認し、**投稿/プルリクエストを開く** を選択します。

   ![&#x200B; プルリクエストの作成 &#x200B;](../assets/docs-framework/ssd-create-pull-request-1.gif)

1. ベースおよび比較ブランチが正しいことを確認します。 更新を説明するメモを PR に追加し、「**プルリクエストを作成**」を選択します。 これにより、フォークの作業ブランチをAdobe リポジトリのマスターブランチに結合する PR が開きます。

   >[!TIP]
   >
   >Adobe ドキュメントチームが PR の編集を行えるように、「**メンテナーによる編集を許可**」チェックボックスを選択したままにします。

   ![Adobe ドキュメントリポジトリへのプルリクエストを作成する &#x200B;](../assets/docs-framework/ssd-create-pull-request-2.png)

1. この時点で、Adobe投稿者使用許諾契約（CLA）への署名を求める通知が表示されます。 これは必須の手順です。 CLA に署名したら、PR ページを更新し、プルリクエストを送信します。

1. **の「** プルリクエスト `https://github.com/AdobeDocs/experience-platform.ja-JP`」タブを調べると、プルリクエストが送信されたことを確認できます。

   ![PR 成功 &#x200B;](../assets/docs-framework/ssd-pr-successful.png)

1. ご協力ありがとうございます。Adobe ドキュメントチームは、編集が必要な場合は PR に連絡し、ドキュメントがいつ公開されるかを知らせます。

>[!TIP]
>
>画像やドキュメントへのリンクを追加する方法、および Markdown に関するその他の質問については、Adobe共同作業ライティングガイドの [Markdown の使用 &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=ja) を参照してください。
