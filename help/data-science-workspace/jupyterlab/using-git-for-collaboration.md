---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace；人気の高いトピック；Git;Github
solution: Experience Platform
title: Gitを使用したJupyterLabでの共同作業
topic-legacy: tutorial
type: Tutorial
description: Gitは、ソフトウェア開発中にソースコードの変更を追跡するための分散バージョン管理システムです。 Gitは、Data Science Workspace JupterLab環境内に事前にインストールされています。
exl-id: d7b766f7-b97d-4007-bc53-b83742425047
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 1%

---

# [!DNL Git]を使用した[!DNL JupyterLab]での共同作業

[!DNL Git] は、ソフトウェア開発時にソースコードの変更を追跡する分散バージョン管理システムです。Gitは[!DNL Data Science Workspace JupyterLab]環境内に事前にインストールされています。

## 前提条件

>[!NOTE]
>
> 使用するGitサーバーは、インターネット経由でアクセスできる必要があります。

[!DNL Data Science Workspace JupyterLab]環境はホスト環境で、会社のファイアウォール内に展開されていないため、接続するGitサーバーはパブリックインターネットからアクセスできる必要があります。 これは、[GitHub](https://github.com/)上のパブリックまたはプライベートのリポジトリ、または自分自身をホストすることに決定した[!DNL Git]サーバーの別のインスタンスである可能性があります。

## [!DNL Git]を[!DNL Data Science Workspace JupyterLab Notebooks]環境に接続

[!DNL Adobe Experience Platform]を起動し、[[!DNL JupyterLabs Notebooks]](https://platform.adobe.com/notebooks/jupyterLab)環境に移動して開始します。

[!DNL JupyterLab]内で、「**[!UICONTROL ファイル]**」を選択し、「**[!UICONTROL 新しい]**」の上にカーソルを置きます。 表示されるドロップダウンから[**[!UICONTROL ターミナル]**]を選択します。

![JupyterLab Nav](../images/jupyterlab/tutorials/open-terminal.png)

次に、*ターミナル*&#x200B;内で、次のコマンドを使用してワークスペースに移動します。`cd my-workspace`。

![cdワークスペース](../images/jupyterlab/tutorials/find-workspace.png)

>[!TIP]
>
> 使用可能なgitコマンドのリストを確認するには、次のコマンドを発行します。ターミナル内の`git -help`。

次に、`git clone`コマンドを使用して、使用するリポジトリをコピーします。 `ssh://`ではなく`https://` URLを使用してプロジェクトのコピーを作成します。

**例**:

`git clone https://github.com/adobe/experience-platform-dsw-reference.git`

![clone](../images/jupyterlab/tutorials/git-collaboration.png)

>[!NOTE]
>
> 書き込み操作（例えば`git push`）を実行するには、新しいセッションごとに次の設定コマンドを実行する必要があります。 また、プッシュコマンドによって、ユーザー名とパスワードの入力が求められる場合もあります。
>
>`git config --global user.email "you@example.com"`
>
>`git config --global user.name "Your Name"`

## 次の手順

リポジトリの複製が完了したら、通常はローカルコンピューター上で行うのと同じようにGitを使用して、ノートブック上の他のユーザーと共同作業できます。 [!DNL JupyterLab]内で何ができるかについて詳しくは、[[!DNL JupyterLab user guide]](./overview.md)を参照してください。
