---
keywords: Experience Platform；ホーム；人気の高いトピック；ソース；コネクタ；ソースコネクタ；ソース sdk;SDK;SDK
solution: Experience Platform
title: GitHub Web インターフェイスを使用したソースドキュメントページの作成
topic-legacy: tutorial
description: このドキュメントでは、GitHub Web インターフェイスを使用してドキュメントを作成し、プル要求 (PR) を送信する手順を説明します。
hide: true
hidefromtoc: true
source-git-commit: d4b5b54be9fa2b430a3b45eded94a523b6bd4ef8
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 3%

---

# GitHub Web インターフェイスを使用してソースドキュメントページを作成する

このドキュメントでは、GitHub Web インターフェイスを使用してドキュメントを作成し、プル要求 (PR) を送信する手順を説明します。

>[!TIP]
>
>ドキュメントプロセスをさらにサポートするには、Adobeのコントリビューションガイドに記載されている次のドキュメントを使用できます。 <ul><li>[Git および Markdown オーサリングツールのインストール](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/install-tools.html?lang=en)</li><li>[ドキュメント用のローカル Git リポジトリの設定](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=en)</li><li>[大きな変更をする際の GitHub コントリビューションワークフロー](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/full-workflow.html?lang=en)</li></ul>

## GitHub 環境の設定

GitHub 環境を設定する最初の手順は、 [Adobe Experience Platform GitHub リポジトリ](https://github.com/AdobeDocs/experience-platform.en).

![platform-repo](../assets/platform-repo.png)

次に、 **分岐**.

![フォーク](../assets/fork.png)

分岐が完了したら、「 」を選択します。 **プライマリ** をクリックし、表示されるドロップダウンメニューに新しいブランチの名前を入力します。 ブランチは作業内容を含むために使用されるので、分岐にわかりやすい名前を付けてから、「 」を選択します。 **ブランチを作成**.

![create-branch](../assets/create-branch.png)

フォークされたリポジトリの GitHub フォルダー構造で、 `experience-platform.en/help/sources/tutorials/api/create/` 次に、ソースに適したカテゴリをリストから選択します。 例えば、新しいクラウドストレージソースのドキュメントを作成する場合は、 **クラウドストレージ**.

![クラウドストレージ](../assets/cloud-storage.png)

既存のクラウドストレージソースのフォルダが表示されます。 新しいソースのドキュメントを追加するには、 **ファイルを追加** 次に、 **新しいファイルを作成** を選択します。

![create-new-file](../assets/create-new-file.png)

ソースファイルに名前を付ける `YOURSOURCE.md` ここで、YOURSOURCE は、Platform でのソースの名前です。 例えば、会社が [!DNL Mailchimp]の場合、ファイル名は `mailchimp.md`.

![git-interface](../assets/git-interface.png)

## ソースに関するドキュメントページの作成

新しいソースのドキュメント化を開始するには、 [ソースドキュメントテンプレート](./template.md) を GitHub Web エディターに追加します。 また、テンプレートをダウンロードすることもできます [ここ](../assets/template.zip).

テンプレートを GitHub Web エディターインターフェイスにコピーした状態で、テンプレートに記載されている手順に従い、ソースに関連する情報を含む値を編集します。

![paste-template](../assets/paste-template.png)

完了したら、ブランチのファイルをコミットします。

![コミット](../assets/commit.png)

## ドキュメントを送信してレビュー

ファイルがコミットされたら、プルリクエスト (PR) を開き、作業ブランチをAdobeドキュメントリポジトリの master ブランチにマージできます。 作業中のブランチが選択されていることを確認し、「 」を選択します。 **リクエストの比較とプル**.

![compare-pr](../assets/compare-pr.png)

ベースと比較ブランチが正しいことを確認します。 PR にメモを追加し、更新内容を説明してから、「 」を選択します。 **プル要求の作成**. PR を開き、作業の作業ブランチをAdobeリポジトリの master ブランチにマージします。

>[!TIP]
>
>を **メンテナーによる編集を許可** チェックボックスをオンにして、Adobeドキュメントチームが PR を編集できるようにします。

![create-pr](../assets/create-pr.png)

この時点で、Adobeコントリビューター使用許諾契約 (CLA) に署名するよう求める通知が表示されます。 これは必須の手順です。 CLA に署名した後、PR ページを更新し、プル要求を送信します。

https://github.com/AdobeDocs/experience-platform.enの「プルリクエスト」タブを調べて、プルリクエストが送信されたことを確認できます。

![confirm-pr](../assets/confirm-pr.png)
