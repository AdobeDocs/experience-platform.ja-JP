---
title: 公開フロー
description: Adobe Experience Platform でライブラリを作成、ビルドをテストし、実稼動環境用に承認するプロセスについて説明します。
exl-id: 4885f60b-6401-4ec7-aa1a-29c135087847
source-git-commit: 2d71eafb00098d958c8cff9350caa27bd3f0260d
workflow-type: tm+mt
source-wordcount: '1407'
ht-degree: 84%

---

# 公開フロー {#publishing-flow}

>[!CONTEXTUALHELP]
>id="platform_tags_publishing_flow"
>title="公開フロー"
>abstract="開発、承認、公開の権限など、公開フローに必要なユーザー権限のレベルについて説明します。"

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

Adobe Experience Platform のタグ公開フローは、ライブラリを作成し、ビルドをテストして、実稼動用に承認するプロセスを指します。

ライブラリで実行できるアクションは、ライブラリの状態と権限のレベルによって異なります。 また、ライブラリの状態は、公開フローのアップストリームの内容に応じて、ライブラリに含まれるリソース（ルール、データ要素、拡張機能）にも影響します。

次の節では、公開フローに関する権限、ライブラリの状態、およびアップストリームに関する詳細について説明します。

## 権限 {#permissions}

公開フローにとって重要なユーザー権限には、様々なレベル（[!UICONTROL Develop]、[!UICONTROL Approve]、[!UICONTROL Publish] のプロパティ権限）があります。

* **[!UICONTROL Develop]**：ライブラリを作成、開発用にビルド、承認用に送信する機能が含まれます。
* **[!UICONTROL Approve]**：ステージング用にビルドを作成し、ステージされたビルドを承認する機能が含まれます。
* **[!UICONTROL Publish]**：承認されたライブラリを公開する機能が含まれます。

権限は包括的ではありません。1 人のユーザーが最初から最後までワークフローを実行するには、そのユーザーに特定のプロパティ内で 3 つの権限すべてを付与する必要があります。

タグの権限の管理について詳しくは、[ユーザー権限ガイド](../administration/user-permissions.md)を参照してください。

## ライブラリの状態 {#state}

公開フローでは、ライブラリの基本的な状態は次の 4 つになります。

* [[!UICONTROL Development]](#development)
* [[!UICONTROL Submitted]](#submitted)
* [[!UICONTROL Approved]](#approved)
* [[!UICONTROL Published]](#published)

これら4つの状態は **[!UICONTROL Publishing Flow]** タブ内の列として表されます。

![](./images/approval-workflow/flow-ui.png)

ライブラリをこれらの状態間で移動させるには、特定のアクションを実行する必要があります。次の図に、状態間でライブラリを移動する各アクションの概要を示します。

![](./images/approval-workflow/library-state.png)

### [!UICONTROL Development] {#development}

新しいライブラリを作成すると、初めは「[!UICONTROL Development]」状態になります。 ライブラリへの変更は、ライブラリが「[!UICONTROL Development]」となっている間に加える必要があります。開発とテストが完了したら、ライブラリを承認用に送信できます。

次の表に、「[!UICONTROL Development]」状態のライブラリで使用できるアクションの概要を示します。

| アクション | 説明 |
| --- | --- |
| [!UICONTROL Edit] | ライブラリの「[!UICONTROL Edit Library]」画面を使用して、ライブラリのコンポーネントを追加または削除します。 |
| [!UICONTROL Build to Development] | ライブラリのビルドを作成します。ビルドがコンパイルされ、ライブラリを割り当てた環境にデプロイされます。ライブラリが環境に割り当てられていない場合、またはアップストリームで定義された変更がライブラリに含まれている場合、この手順は失敗します。 |
| [!UICONTROL Submit for Approval] | 開発環境からライブラリの割り当てを解除し、承認権限を持つユーザーが作業できるように、ライブラリを「[!UICONTROL Submitted]」列に移動します。 このオプションを有効にするには、ライブラリの最新ビルドが成功している必要があります。 |
| [!UICONTROL Submit & Build to Staging] | この操作は、開発権限と承認権限の両方を持つユーザーのみが実行できます。 この操作により、開発環境からライブラリの割り当てが解除され、ライブラリが [!UICONTROL Submitted] 状態に移行し、ステージング 環境にライブラリがビルドされます。 このオプションを有効にするには、ライブラリの最新ビルドが成功している必要があります。 |
| [!UICONTROL Approve for Publishing] | この操作は、開発権限と承認権限の両方を持つユーザーのみが実行できます。 このアクションにより、開発環境からライブラリの割り当てが解除され、ステージング環境と[!UICONTROL Approved]状態が完全にスキップされ、[!UICONTROL Submitted]状態に移動します。このオプションを有効にするには、ライブラリの最新ビルドが成功している必要があります。 |
| [!UICONTROL Approve & Publish to Production] | この操作は、開発、承認および公開の権限を持つユーザーのみが実行できます。このアクションにより、開発環境からライブラリの割り当てが解除され、 [!UICONTROL Approved] 状態に移行して、運用環境に発行されます。 生産ビルドが完了すると、ライブラリは [!UICONTROL Published] 状態に移行します。 このオプションを有効にするには、ライブラリの最新ビルドが成功している必要があります。 |
| [!UICONTROL Delete] | システムからライブラリを削除します。環境からビルドが削除されるわけではありません。 |

### [!UICONTROL Submitted] {#submitted}

ライブラリが「[!UICONTROL Submitted]」状態の場合、承認権限を持つユーザーは、ステージング環境でライブラリをテストできます。 テストが完了すると、ライブラリは承認または却下されます。却下されたビルドは「[!UICONTROL Development]」に戻るので、公開フローを再開する前に追加の変更を加えることができます。

次の表に、「[!UICONTROL Submitted]」状態のライブラリで使用できるアクションの概要を示します。

| アクション | 説明 |
| --- | --- |
| [!UICONTROL Open] | ライブラリのコンテンツを表示します。「[!UICONTROL Development]」列以外のライブラリに対する変更は許可されません。 変更が必要な場合は、ライブラリを却下すると、「[!UICONTROL Development]」で変更できるようになります。 |
| [!UICONTROL Build for Staging] | デプロイメント用にステージング環境でライブラリを構築します。 |
| [!UICONTROL Approve for Publishing] | 公開権限を持つユーザーが作業できるように、ライブラリを「[!UICONTROL Approved]」列に移動します。 |
| [!UICONTROL Approve & Publish to Production] | この操作は、承認権限と公開権限の両方を持つユーザーのみが実行できます。 このアクションにより、ステージング 環境からライブラリの割り当てが解除され、 [!UICONTROL Approved] 状態に移行して、運用環境に発行されます。 生産ビルドが完了すると、ライブラリは [!UICONTROL Published] 状態に移行します。 このアクションは、ステージング環境でビルドが正常に完了しているかどうかに関わらず実行できます。 |
| [!UICONTROL Reject] | ステージング環境からライブラリの割り当てを解除し、さらに変更を加えるためにライブラリを「[!UICONTROL Development]」列に戻します。 |

### [!UICONTROL Approved] {#approved}

ライブラリが承認されると、公開権限を持つユーザーはライブラリを公開または却下できます。 却下されたビルドは「 [!UICONTROL Development]」に戻るので、公開フローが再び開始する前に変更を加えることができます。

次の表に、「[!UICONTROL Approved]」状態のライブラリで使用できるアクションの概要を示します。

| アクション | 説明 |
| --- | --- |
| [!UICONTROL Open] | ライブラリのコンテンツを表示します。「[!UICONTROL Development]」列以外のライブラリに対する変更は許可されません。 変更が必要な場合は、ライブラリを却下すると、「 [!UICONTROL Development]」で変更できるようになります。 |
| [!UICONTROL Build and Publish to Production] | ステージング環境からライブラリを割り当て解除し、そのライブラリを本番環境に割り当ててデプロイします。<br><br>**重要**：このオプションを選択すると、ライブラリは本番環境に移行します。このオプションを選択する前に、ライブラリに必要な変更が含まれていることを確認してください。 |
| [!UICONTROL Reject] | ステージング環境からライブラリを割り当て解除し、さらに変更を加えるため、ライブラリを「[!UICONTROL Development]」列へと移動させます。 |

### [!UICONTROL Published] {#published}

[!UICONTROL Published]列には、公開されたライブラリとその公開する日付が表示されます。現在公開中のライブラリの横には緑の点が表示されます。以前のライブラリを再公開しなければ、このライブラリが常に列の先頭になります。

| アクション | 説明 |
| --- | --- |
| [!UICONTROL Open] | ライブラリのコンテンツを表示します。「[!UICONTROL Development]」列以外のライブラリに対する変更は許可されません。 本番環境の内容を変更する場合は、新しいライブラリを作成し、すべての公開プロセスを進める必要があります。 |
| [!UICONTROL Republish] | このアクションは、最近公開された 5 つのライブラリでのみ使用でき、実稼働環境が (A) アーカイブオプションをオフに設定し、(b) ビルド時に [!UICONTROL Managed by Adobe] ホストを使用している場合に限ります。 |
| [!UICONTROL Download] | このアクションは、最近公開された 5 つのライブラリでのみ使用でき、実稼働環境が (A) アーカイブオプションをオンに設定され、(b) ビルド時に [!UICONTROL Managed by Adobe] ホストを使用している場合のみです。 |

## アップストリーム {#upstream}

最初のライブラリを公開した後、公開フローを通じて新しいライブラリを移動させるため、アップストリームの役割を理解することが重要になります。

現在ライブラリが「[!UICONTROL Development]」、「[!UICONTROL Submitted]」、または「[!UICONTROL Approved]」ステージにある場合、そのライブラリは、アップストリームのライブラリのルール、データ要素、拡張子を継承します。 継承されたこれらのリソースは、公開フロー内を移動するたびに、各ライブラリの「ベースライン」を構成します。 基本的に、新しいライブラリはそれぞれ、アップストリームで確立されたベースラインに対する一連の変更と考えることができます。 これにより、新しいイテレーションが公開された際に、前のライブラリから予期せず上書きされることがなくなります。

アップストリームに含まれる内容は、ライブラリの現在のステージによって異なります。 例えば、「[!UICONTROL Approved]」列のライブラリは「[!UICONTROL Published]」ライブラリからのリソースのみを継承し、「[!UICONTROL Development]」のライブラリは他のすべての列からのリソースを継承します。

![](./images/approval-workflow/upstream.png)

UIでライブラリを編集すると、アップストリームから継承されたすべてのリソースが **[!UICONTROL Resources Upstream]** セクションに表示されます。 これらのリソースを表示するには、セクションの見出しの下にある「拡張」タブを選択します。

![](./images/approval-workflow/upstream-collapse.png)

セクションが展開し、アップストリームから継承された個々のリソースが表示されます。 左側のパネルを使用して [!UICONTROL Rules]、[!UICONTROL Data Elements]、[!UICONTROL Extensions] でフィルタリングしたり、検索バーを使用して特定のリソースを名前で検索したりできます。

![](./images/approval-workflow/upstream-resources.png)

## 次の手順

このガイドでは、Adobe Experience Platform におけるライブラリの公開フローの概要を示しました。ライブラリの公開方法について詳しくは、「[公開の概要](./overview.md)」を参照してください。
