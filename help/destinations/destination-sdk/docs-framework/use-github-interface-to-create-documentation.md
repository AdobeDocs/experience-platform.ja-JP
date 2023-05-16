---
title: GitHub web インターフェイスを使用した宛先ドキュメントページの作成
description: このページの手順では、GitHub Web インターフェイスを使用して、Experience Platform先のドキュメントページを作成し、レビュー用に送信する方法を示します。
exl-id: 4780e05e-3d1d-4f1b-8441-df28d09c1a88
source-git-commit: e239de97a26ea2ff36bb74390e249851a13d2e13
workflow-type: tm+mt
source-wordcount: '780'
ht-degree: 5%

---

# GitHub web インターフェイスを使用した宛先ドキュメントページの作成 {#github-interface}

以下の手順は、GitHub Web インターフェイスを使用してドキュメントを作成し、プル要求 (PR) を送信する方法を示しています。 ここで示す手順を実行する前に、必ず [Adobe Experience Platform Destinations での宛先のドキュメント化](./documentation-instructions.md).

>[!TIP]
>
>Adobeのコントリビューターガイドのサポートドキュメントも参照してください。
>* [Git および Markdown オーサリングツールのインストール](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/install-tools.html?lang=en)
>* [ドキュメント用のローカル Git リポジトリの設定](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=en)
>* [大きな変更をする際の GitHub コントリビューションワークフロー](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/full-workflow.html?lang=en).


## GitHub オーサリング環境の設定 {#set-up-environment}

1. ブラウザーで、`https://github.com/AdobeDocs/experience-platform.en` に移動します。
2. 宛先 [フォーク](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=en#fork-the-repository) リポジトリで、 **分岐** 以下に示すように。 これにより、自分の GitHub アカウントにExperience Platformリポジトリのコピーが作成されます。

   ![ForkAdobeドキュメントリポジトリ](../assets/docs-framework/ssd-fork-repository.gif)

3. リポジトリのフォークに、以下に示すように、プロジェクト用の新しいブランチを作成します。 この新しい分岐を作業に使用します。

   ![新しい GitHub ブランチを作成](../assets/docs-framework/new-branch-github.gif)

4. フォークされたリポジトリの GitHub フォルダー構造で、に移動します。 `experience-platform.en/help/destinations/catalog/[...]`で、 `[...]` は、宛先の目的のカテゴリです。 例えば、パーソナライゼーションの宛先をExperience Platformに追加する場合は、 `personalization` カテゴリ。 選択 **ファイルを追加/新しいファイルを作成**.

   ![新しいファイルを追加](../assets/docs-framework/github-navigate-and-create-file.gif)

5. 宛先の名前を設定 `YOURDESTINATION.md`:YOURDESTINATION は、Adobe Experience Platformでの宛先の名前です。 例えば、会社名が Moviestar の場合、ファイルにはという名前を付けます `moviestar.md`.

## 宛先のドキュメントページのオーサリング {#author-documentation}

1. 次の条件に基づいて、リンク先のページのコンテンツを作成します。 [ドキュメントセルフサービステンプレート](./self-service-template.md). **[ダウンロード](../assets/docs-framework/yourdestination-template.zip)** テンプレートを展開し、抽出します。 `.md` ファイルテンプレート。
2. テンプレートのコンテンツを、目的の宛先に関する情報と共に、オンライン Markdown エディター（例： ）に貼り付けて編集します。 [dilinger.io](https://dillinger.io/). 入力内容と削除可能な段落の詳細については、テンプレートの手順に従ってください。

   >[!TIP]
   >
   >ブラウザーウィンドウはいつでも閉じて、後で再度開くことができます。 作業内容は自動的に保存され、ブラウザーを再度開いたときに待ちます。
3. Markdown エディターから GitHub の新しいファイルに、コンテンツをコピーします。
4. 使用する予定のスクリーンショットや画像については、GitHub インターフェイスを使用して、ファイルをにアップロードします。 `experience-platform.en/help/destinations/assets/catalog/[...]`で、 `[...]` は、宛先の目的のカテゴリです。 例えば、パーソナライゼーションの宛先をExperience Platformに追加する場合は、 `personalization` カテゴリ。 オーサリング中のページから画像にリンクする必要があります。 詳しくは、 [画像へのリンク方法の説明](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/linking.html?lang=en#link-to-images).

   ![GitHub に画像をアップロード](../assets/docs-framework/upload-image.gif)

5. 準備が整ったら、ブランチにファイルを保存します。

![ファイルの作成を確認](../assets/docs-framework/ssd-confirm-file-creation.png)

## ドキュメントを送信してレビュー {#submit-review}

>[!TIP]
>
>ここで壊れるものは何もありません。 この節の手順に従うことで、単にドキュメントの更新を提案するだけです。 推奨される更新は、Adobe Experience Platformドキュメントチームが承認または編集します。

1. ファイルを保存し、必要な画像をアップロードした後、プルリクエスト (PR) を開き、作業ブランチをAdobeドキュメントリポジトリの master ブランチに結合できます。 作業したブランチが選択されていることを確認し、「 」を選択します。 **貢献/プルリクエストを開く**.

![プル要求の作成](../assets/docs-framework/ssd-create-pull-request-1.gif)

1. ベースと比較ブランチが正しいことを確認します。 PR に、更新内容を説明するメモを追加し、「 」を選択します。 **プル要求の作成**. これにより、PR が開き、フォークの作業ブランチがAdobeリポジトリの master ブランチにマージされます。

   >[!TIP]
   >
   >を **メンテナーによる編集を許可** チェックボックスがオンになっているので、Adobeドキュメントチームが PR を編集できます。

   ![Adobeドキュメントリポジトリへのプル要求の作成](../assets/docs-framework/ssd-create-pull-request-2.png)

1. この時点で、Adobeコントリビューター使用許諾契約 (CLA) に署名するよう求める通知が表示されます。 これは必須の手順です。 CLA に署名した後、PR ページを更新し、プル要求を送信します。

1. プルリクエストが送信されたことを確認するには、 **プル要求** タブ `https://github.com/AdobeDocs/experience-platform.en`.

   ![PR 成功](../assets/docs-framework/ssd-pr-successful.png)

1. ご協力ありがとうございます。Adobeドキュメントチームは、編集が必要な場合に備えて PR に問い合わせ、ドキュメントをいつ公開するかを知らせます。

>[!TIP]
>
>ドキュメントに画像やリンクを追加したり、Markdown に関するその他の質問については、以下を参照してください。 [Markdown の使用](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=en) (Adobeのコラボレーション執筆ガイド )
