---
keywords: Experience Platform;JupyterLab；ノートブック；Data Science Workspace；人気の高いトピック；Git;Github
solution: Experience Platform
title: Git を使用した JupyterLab での共同作業
topic-legacy: tutorial
type: Tutorial
description: Git は、ソフトウェア開発時にソースコードの変更を追跡するための分散バージョン管理システムです。 Git は、Data Science Workspace JupyterLab 環境内に事前にインストールされています。
exl-id: d7b766f7-b97d-4007-bc53-b83742425047
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 1%

---

# での共同作業 [!DNL JupyterLab] using [!DNL Git]

[!DNL Git] は、ソフトウェア開発時にソースコードの変更を追跡するための分散バージョン管理システムです。 Git は [!DNL Data Science Workspace JupyterLab] 環境。

## 前提条件

>[!NOTE]
>
> 使用する Git サーバーは、インターネット経由でアクセスできる必要があります。

この [!DNL Data Science Workspace JupyterLab] 環境はホストされた環境で、会社のファイアウォール内にデプロイされていないので、接続先の Git サーバーはパブリックインターネットからアクセスできる必要があります。 これは、 [GitHub](https://github.com/) または別の [!DNL Git] 自分をホストすることを決定したサーバー。

## 接続 [!DNL Git] から [!DNL Data Science Workspace JupyterLab Notebooks] 環境

開始 [!DNL Adobe Experience Platform] そして、 [[!DNL JupyterLabs Notebooks]](https://platform.adobe.com/notebooks/jupyterLab) 環境。

内 [!DNL JupyterLab]を選択します。 **[!UICONTROL ファイル]** 次に、 **[!UICONTROL 新規]**. 表示されるドロップダウンから、「 」を選択します。 **[!UICONTROL ターミナル]**.

![JupyterLab Nav](../images/jupyterlab/tutorials/open-terminal.png)

次へ、内 *ターミナル* 次のコマンドを使用して、ワークスペースに移動します。 `cd my-workspace`.

![cd workspace](../images/jupyterlab/tutorials/find-workspace.png)

>[!TIP]
>
> 使用可能な Git コマンドのリストを表示するには、次のコマンドを発行します。 `git -help` を使用します。

次に、 `git clone` コマンドを使用します。 を使用してプロジェクトを複製 `https://` ではなく URL `ssh://`.

**例**：

`git clone https://github.com/adobe/experience-platform-dsw-reference.git`

![複製](../images/jupyterlab/tutorials/git-collaboration.png)

>[!NOTE]
>
> 書き込み操作を実行する場合 (`git push` 例えば、次の設定コマンドは、新しいセッションのたびに実行する必要があります。 また、プッシュコマンドは、ユーザ名とパスワードの入力を求めます。
>
>`git config --global user.email "you@example.com"`
>
>`git config --global user.name "Your Name"`

## 次の手順

リポジトリーのクローンを作成した後は、通常ローカルマシン上で行うのと同じように Git を使用して、ノートブック上の他のユーザーと共同作業できます。 内で実行できる操作の詳細 [!DNL JupyterLab]を参照し、 [[!DNL JupyterLab user guide]](./overview.md).
