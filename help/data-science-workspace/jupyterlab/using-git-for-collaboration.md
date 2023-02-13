---
keywords: Experience Platform;JupyterLab;ノートブック;データサイエンスワークスペース;人気のトピック;Git;Github
solution: Experience Platform
title: Git を使用した JupyterLab での共同作業
type: Tutorial
description: Git は、ソフトウェア開発時にソースコードの変更を追跡するための分散バージョン管理システムです。 Git は、データサイエンスワークスペース JupyterLab 環境内に事前にインストールされています。
exl-id: d7b766f7-b97d-4007-bc53-b83742425047
source-git-commit: 86e6924078c115fb032ce39cd678f1d9c622e297
workflow-type: ht
source-wordcount: '284'
ht-degree: 100%

---

# [!DNL Git] を使用した [!DNL JupyterLab] での共同作業

[!DNL Git] は、ソフトウェア開発時にソースコードの変更を追跡するための分散バージョン管理システムです。Git は [!DNL Data Science Workspace JupyterLab] 環境内に事前にインストールされています。

## 前提条件

>[!NOTE]
>
> 使用する Git サーバーは、インターネット経由でアクセスできる必要があります。

[!DNL Data Science Workspace JupyterLab] 環境はホストされた環境であり、会社のファイアウォール内にデプロイされていないので、接続先の Git サーバーはパブリックインターネットからアクセスできる必要があります。これは、[GitHub](https://github.com/) のパブリックまたはプライベートリポジトリ、または自分でホストすることを決定した [!DNL Git] サーバーの別のインスタンスである可能性があります。

## [!DNL Git] を [!DNL Data Science Workspace JupyterLab Notebooks] 環境に接続する

まず、[!DNL Adobe Experience Platform] を起動し、[[!DNL JupyterLabs Notebooks]](https://platform.adobe.com/notebooks/jupyterLab) 環境に移動します。

[!DNL JupyterLab] 内で、「**[!UICONTROL ファイル]**」を選択し、「**[!UICONTROL 新規]**」にポインタを合わせます。表示されるドロップダウンから、「**[!UICONTROL ターミナル]**」を選択します。

![JupyterLab での移動](../images/jupyterlab/tutorials/open-terminal.png)

次に、*ターミナル*&#x200B;内で次のコマンドを使用してワークスペースに移動します。`cd my-workspace`。

![cd workspace](../images/jupyterlab/tutorials/find-workspace.png)

>[!TIP]
>
> 使用可能な git コマンドのリストを表示するには、ターミナル内でコマンド `git -help` を発行します。

次に、`git clone` コマンドを使用して、使用するリポジトリのクローンを作成します。`ssh://` ではなく `https://` URL を使用してプロジェクトのクローンを作成します。

**例**：

`git clone https://github.com/adobe/experience-platform-dsw-reference.git`

![クローン](../images/jupyterlab/tutorials/git-collaboration.png)

>[!NOTE]
>
> 書き込み操作（`git push` など）を実行するには、新しいセッションごとに次の設定コマンドを実行する必要があります。また、プッシュコマンドでは、ユーザー名とパスワードの入力が求められます。
>
>`git config --global user.email "you@example.com"`
>
>`git config --global user.name "Your Name"`

## 次の手順

リポジトリのクローン作成が完了したら、ローカルマシンで通常行うように Git を使用して、ノートブックで他のユーザーと共同作業を行うことができます。[!DNL JupyterLab] 内でできることについて詳しくは、[[!DNL JupyterLab user guide]](./overview.md) を参照してください。
