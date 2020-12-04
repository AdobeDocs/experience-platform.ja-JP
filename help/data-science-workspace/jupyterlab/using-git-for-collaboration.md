---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace;popular topics;Git;Github
solution: Experience Platform
title: Gitを使用したJupyterLabでのコラボレーション
topic: tutorial
type: Tutorial
description: Gitは、ソフトウェア開発中にソースコードの変更を追跡するための分散バージョン管理システムです。 Gitは、Data Science Workspace JupterLab環境内に事前にインストールされています。
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 1%

---


# 使用に関する共同作業 [!DNL JupyterLab] [!DNL Git]

[!DNL Git] は、ソフトウェア開発時にソースコードの変更を追跡する分散バージョン管理システムです。 Gitが [!DNL Data Science Workspace JupyterLab] 環境ー内にプリインストールされています。

## 前提条件

>[!NOTE]
>
> 使用するGitサーバーは、インターネット経由でアクセスできる必要があります。

この [!DNL Data Science Workspace JupyterLab] 環境はホスト環境で、会社のファイアウォール内に展開されていないため、接続先のGitサーバーはパブリックインターネットからアクセスできる必要があります。 これは、GitHub上のパブリックまたはプライベートのリポジトリ [、または自分自身をホストすることに決定した](https://github.com/)[!DNL Git] サーバーの別のインスタンスである可能性があります。

## 環境 [!DNL Git] に接続し [!DNL Data Science Workspace JupyterLab Notebooks] ます

環境を起動 [!DNL Adobe Experience Platform] し、 [[!DNL JupyterLabs Notebooks]](https://platform.adobe.com/notebooks/jupyterLab) 開始に移動する。

内 [!DNL JupyterLab]で、「 **[!UICONTROL ファイル]** 」を選択し、「 **[!UICONTROL 新規」の上にマウスポインターを置きます]**。 表示されるドロップダウンで[ **[!UICONTROL ターミナル]**]を選択します。

![JupyterLab Nav](../images/jupyterlab/tutorials/open-terminal.png)

次に、 *ターミナル* (Terminal)内で、次のコマンドを使用してワークスペースに移動します。 `cd my-workspace`.

![cdワークスペース](../images/jupyterlab/tutorials/find-workspace.png)

>[!TIP]
>
> 使用可能なgitコマンドのリストを確認するには、次のコマンドを発行します。 `git -help` 」と入力します。

次に、 `git clone` コマンドを使用して、使用するリポジトリをコピーします。 ではなく、 `https://` URLを使用してプロジェクトのコピーを作成し `ssh://`ます。

**例**:

`git clone https://github.com/adobe/experience-platform-dsw-reference.git`

![clone](../images/jupyterlab/tutorials/git-collaboration.png)

>[!NOTE]
>
> 書き込み操作(例えば`git push` )を実行するには、新しいセッションごとに次の設定コマンドを実行する必要があります。 また、プッシュコマンドによって、ユーザー名とパスワードの入力が求められる場合もあります。
>
>`git config --global user.email "you@example.com"`
>
>`git config --global user.name "Your Name"`

## 次の手順

リポジトリの複製が完了したら、通常はローカルコンピューター上で行うのと同じようにGitを使用して、ノートブック上の他のユーザーと共同作業できます。 内で実行できる操作の詳細については、を参照し [!DNL JupyterLab]てくだ [[!DNL JupyterLab user guide]](./overview.md)さい。
