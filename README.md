---
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 98%

---
# コンテンツの投稿

アドビのコミュニティや、ドキュメントチーム以外のアドビ従業員からのコンテンツのコントリビューションを歓迎します。

## アドビオープンソース行動規範

このプロジェクトでは、[アドビのオープンソースの行動規範](code-of-conduct.md)または [.NET Foundation Code of Conduct](https://dotnetfoundation.org/code-of-conduct)（英語）を採用しています。詳しくは、[コントリビューション](contributing.md)の記事を参照してください。

## アドビコンテンツへの投稿方法

[アドビドキュメントのコントリビューターガイド](https://experienceleague.adobe.com/docs/contributor/contributor-guide/introduction.html?lang=ja)を参照してください。

コントリビューションの方法は、誰がどのような変更をコントリビューションするかに応じて異なります。

### 軽微な変更またはリクエスト

リクエストを送信するには、記事内の「**Log an issue**」リンクをクリックして、GitHub で問題を開きます。タイトルと説明を指定し、「**Submit new issue**」をクリックします。

マイナーアップデートをリクエストするには、記事の「**Edit this page**」リンクをクリックして、GitHub でソース記事を開きます。GitHub の UI を使用して、更新をおこないます。詳しくは、全般的な事項について説明した[アドビドキュメントのコントリビューターガイド](https://experienceleague.adobe.com/docs/contributor/contributor-guide/introduction.html?lang=ja)を参照してください。

このリポジトリー内のドキュメントやコード例に対して提案される軽微な変更や補足説明には、アドビの利用規約が適用されます。

### コミュニティメンバーによる大きな変更または新規記事

アドビのコミュニティメンバーが記事を作成したり、大きな変更を加える場合は、GitHub リポジトリの「**Issues**」タブをクリックして、イシューを提出します。イシューの提出によって、ドキュメントチームとのやり取りが始まります。新規コンテンツを公開するには、ライター（またはその他のアドビの従業員）との共同作業が必要になります。

<!--
If you submit a pull request with significant changes to documentation and code examples, you'll see a message in the pull request asking you to submit an online contribution license agreement (CLA). You must complete the online form before we can review your pull request.
-->

### アドビ社員による大きな変更または新規記事

Adobe Experience Cloud ソリューションの製品チームのテクニカルライター、プログラムマネージャーまたは開発者が、業務として技術記事の執筆やコントリビューションをする場合は、`https://git.corp.adobe.com/AdobeDocs` にあるプライベートリポジトリを使用する必要があります。詳しくは、『[社内コラボレーションガイド](https://experienceleague.adobe.com/docs/authoring-guide-exl/using/home.html)』を参照してください。

<!--Employees from other parts of the Adobe world should use the public repo for minor updates.-->

## Experience Platform ドキュメントチームへの問い合わせ

上述の通り、アドビコミュニティのメンバーは問題を提出でき、提出された問題は適切な作者に割り当てられます。 アドビの従業員は、問題を送信するか、Experience Platform のドキュメントチームに直接問い合わせることができます。 特定の Platform 領域のリードライターを見つけるには、[Adobe Experience Platform のドキュメント wiki](https://wiki.corp.adobe.com/display/DMSArchitecture/Adobe+Experience+Platform+Documentation) を参照してください。

## ツールとセットアップ

### GitHub UI

コミュニティのコントリビューターは、基本的な編集をするときには GitHub の UI を使用し、大きな変更を加えるときにはリポジトリーをフォークします。

詳しくは、[アドビドキュメントのコントリビューターガイド](https://experienceleague.adobe.com/docs/contributor/contributor-guide/introduction.html?lang=ja)を参照してください。

### Markdown

このリポジトリーの記事はいずれも GitHub Flavored Markdown（GFM）を使用して書かれています。Markdown について詳しくない場合は、以下を参照してください。

* [Markdown の基本](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/)（英語）
* [印刷可能な Markdown のクイックリファレンス](https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf)（英語）

### ラベル

公開リポジトリーでは、プルリクエストに以下のような自動ラベルが割り当てられ、プルリクエストワークフローの管理とプルリクエストの処理状況の把握に役立ちます。

* **Change sent to author**：保留中のプルリクエストの通知が作成者に送信されました。
* **ready- to- merge**：プルリクエストレビューチームによるレビューの準備ができました。
