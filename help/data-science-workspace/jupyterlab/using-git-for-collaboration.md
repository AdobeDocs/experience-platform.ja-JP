---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace;popular topics;Git;Github
solution: Experience Platform
title: Gitを使用したJupyterLabでのコラボレーション
topic: Tutorial
translation-type: tm+mt
source-git-commit: 0134c21bc35c0cb1bde7f0201a33517a81addae3
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 1%

---


# Gitを使用したJupyterLabでのコラボレーション

Gitは、ソフトウェア開発中にソースコードの変更を追跡するための分散バージョン管理システムです。 Gitは、Data Science Workspace JupterLab環境内に事前にインストールされています。

## 前提条件

>[!NOTE]
> 使用するGitサーバーは、インターネット経由でアクセスできる必要があります。

Data Science Workspace JupyterLab環境はホスト環境で、会社のファイアウォール内に配置されていません。したがって、接続するGitサーバはパブリックインターネットからアクセスできる必要があります。 これは、GitHub上のパブリックまたはプライベートのリポジトリ、 [](https://github.com/) または自分自身をホストすることに決定したGitサーバーの別のインスタンスである可能性があります。

## GitをData Science Workspace JupyterLabノートブック環境に接続

Adobe Experience Platformを起動し、 [JupyterLabs Notebooks](https://platform.adobe.com/notebooks/jupyterLab) 環境に移動して開始します。

「JupyterLab」内で、「 **[!UICONTROL File]** 」を選択し、「 **[!UICONTROL New]**」の上にカーソルを置きます。 表示されるドロップダウンで[ **[!UICONTROL ターミナル]**]を選択します。

![JupyterLab Nav](../images/jupyterlab/tutorials/open-terminal.png)

次に、 *ターミナル* (Terminal)内で、次のコマンドを使用してワークスペースに移動します。 `cd my-workspace`.

![cdワークスペース](../images/jupyterlab/tutorials/find-workspace.png)

>[!TIP]
> 使用可能なgitコマンドのリストを確認するには、次のコマンドを発行します。 `git -help` 」と入力します。

次に、 `git clone` コマンドを使用して、使用するリポジトリをコピーします。 ではなく、 `https://` URLを使用してプロジェクトのコピーを作成し `ssh://`ます。

**例**:

`git clone https://github.com/adobe/experience-platform-dsw-reference.git`

![clone](../images/jupyterlab/tutorials/git-collaboration.png)

>[!NOTE]
> 書き込み操作(例えば`git push` )を実行するには、新しいセッションごとに次の設定コマンドを実行する必要があります。 また、プッシュコマンドによって、ユーザー名とパスワードの入力が求められる場合もあります。
>
>`git config --global user.email "you@example.com"`
>
>`git config --global user.name "Your Name"`

## 次の手順

リポジトリの複製が完了したら、通常はローカルコンピューター上で行うのと同じようにGitを使用して、ノートブック上の他のユーザーと共同作業できます。 JupyterLabで実行できる操作の詳細については、『JupyterLabユーザガイド [』を参照してください](./overview.md)。
