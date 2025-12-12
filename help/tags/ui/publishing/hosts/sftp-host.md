---
title: SFTP ホスト
description: セキュリティで保護された自己ホスト SFTP サーバーにライブラリビルドを配信するように Adobe Experience Platform にタグを設定する方法について説明します。
exl-id: 3c1dc43b-291c-4df4-94f7-a03b25dbb44c
source-git-commit: 44e2b8241a8c348d155df3061d398c4fa43adcea
workflow-type: tm+mt
source-wordcount: '751'
ht-degree: 31%

---

# SFTP ホスト

Experience Platformを使用すると、タグライブラリビルドを、ホストする保護された SFTP サーバーに配信でき、ビルドの保存および管理方法をより詳細に制御できます。 このガイドでは、Experience Platform UI または Data Collection UI でタグプロパティ用に SFTP ホストを設定する方法について説明します。

>[!NOTE]
>
>代わりに、Adobeによって管理されるホストを使用することもできます。 詳しくは、[Adobeで管理されるホスト ](./managed-by-adobe-host.md) に関するガイドを参照してください。
>
>自己ホスト型ライブラリの利点と制限事項について詳しくは、[ 自己ホスト型ガイド ](./self-hosting-libraries.md) を参照してください。

## サーバーのアクセスキーの設定 {#access-key}

Experience Platformは、暗号化キーを使用して SFTP サイトに接続します。 これを正しく設定するには、いくつかの手順があります。

### 公開鍵/秘密鍵のペアの作成

SFTP サーバーに公開鍵と秘密鍵のペアがインストールされている必要があります。これらのキーは、サーバー上で生成することも、別の場所で生成してサーバーにインストールすることもできます。詳しくは、 [SSH キーを生成する方法](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#generating-a-new-ssh-key) に関する GitHub のドキュメントを参照してください。

### キーを暗号化

秘密鍵は、公開鍵の暗号化に使用されます。 SFTP ホストの作成プロセス中に、秘密キーを提供する必要があります。公開鍵の暗号化の手順については、Reactor API ガイドの [ 値の暗号化 ](../../../api/guides/encrypting-values.md) の節を参照してください。 特定の GPG キーが必要であることがわかっている場合を除き、本番環境の GPG キーを使用します。最後に、任意のマシンから秘密鍵を暗号化することができます。サーバーに GPG をインストールしてこの手順を完了する必要はありません。

### 許可リストExperience Platformの IP アドレス

Experience Platformが SFTP サーバーに到達して接続できるように、会社のファイアウォール内で使用する IP アドレスのセットを承認する必要が生じる場合があります。 これらの IP アドレスは次のとおりです。

* `34.227.138.75`
* `44.194.43.191`
* `3.215.163.18`

>[!NOTE]
>
>タグビルドの構造は時間の経過と共に変化しています。内部的にシンボリックリンク（symlinks）を使用して以前の埋め込みコードの下位互換性を維持し、以前の埋め込みコードが最新のビルド構造で引き続き機能するようにします。SFTP サーバーがタグビルドの有効な宛先として機能するには、シンボリックリンクの使用をサポートしている必要があります。

詳しくは、 [SFTP サーバーを設定してビルドを配信する方法](https://medium.com/launch-by-adobe/configuring-an-sftp-server-for-use-with-adobe-launch-bc626027e5a6) に関する次の Medium の記事を参照してください。

## SFTP ホストの作成 {#create}

左側のナビゲーションで「**[!UICONTROL Hosts]**」を選択し、次に「**[!UICONTROL Add Host]**」を選択します。

![UI で「ホストを追加」ボタンが選択されている様子を示す画像 ](../../../images/ui/publishing/sftp-hosts/add-host-button.png)

ホスト作成ダイアログが表示されます。 ホストの名前を指定し、「**[!UICONTROL Type]**」で「**[!UICONTROL SFTP]**」を選択します。

![SFTP ホスティングオプションが選択されていることを示す画像 ](../../../images/ui/publishing/sftp-hosts/select-sftp.png)

### SFTP ホストの設定 {#configure}

ダイアログが拡張され、SFTP ホスト用の追加の設定オプションが表示されます。 これらについては、以下で説明します。

![SFTP ホスト接続に必要な詳細を示す画像 ](../../../images/ui/publishing/sftp-hosts/host-details.png)

| 設定フィールド | 説明 |
| --- | --- |
| [!UICONTROL Don't Use Symlinks] | デフォルトでは、すべての SFTP ホストは、サーバーに保存されたライブラリ [ ビルド ](../builds.md) を参照するために、シンボリックリンク（symlinks）を使用します。 ただし、すべてのサーバーがシンボリックリンクの使用をサポートしているわけではありません。 このオプションを選択すると、ホストではシンボリックリンクを使用する代わりに、コピー操作を使用してビルドアセットを直接更新します。 |
| [!UICONTROL SFTP Server URL] | サーバーの URL ベースパス。 |
| [!UICONTROL Path] | このホストのベースサーバー URL に追加するパス。 |
| [!UICONTROL Port] | ポートは、次のいずれかである必要があります。<ul><li>`21`</li><li>`22`</li><li>`201`</li><li>`200`</li><li>`2002`</li><li>`2018`</li><li>`2022`</li><li>`2200`</li><li>`2222`</li><li>`2333`</li><li>`2939`</li><li>`443`</li><li>`4343`</li><li>`80`</li><li>`8080`</li><li>`8888`</li></ul>セキュリティ上のベストプラクティスとして、アドビでは、送信トラフィックに使用できるポートの数を制限しています。選択したポートは、一般的に企業のファイアウォールを通じて使用でき、柔軟性のためにいくつかの範囲を含んでいます。 |
| [!UICONTROL Username] | サーバーへのアクセス時に使用するユーザー名。 |
| [!UICONTROL Encrypted Private Key] | [ 前の手順 ](#access-key) で作成した暗号化された秘密鍵。 |

「**[!UICONTROL Save]**」を選択して、選択した設定でホストを作成します。

![SFTP ホストが保存されていることを示す画像 ](../../../images/ui/publishing/sftp-hosts/save-host.png)

**[!UICONTROL Save]** を選択すると、SFTP サーバーにファイルを配信するための接続と機能がテストされます。 Experience Platformは、フォルダーを作成してそのフォルダー内にファイルを書き込み、ファイルが書き込まれていることを確認した後、フォルダーをクリーンアップします。 SFTP サーバー上のユーザーアカウント（Experience Platformに提供したセキュア証明書に接続されているもの）が、このアクションを実行するために必要な権限を持っていない場合、ホストは「失敗」ステータスになります。

## 次の手順

このガイドでは、タグで使用する自己ホスト SFTP サーバーの設定方法について説明しました。 ホストが確立されたら、1 つ以上の [ 環境 ](../environments.md) に関連付けて、タグライブラリを公開できます。 Web プロパティまたはモバイルプロパティでタグ機能をアクティベートする高度なプロセスについて詳しくは、[ 公開の概要 ](../overview.md) を参照してください。
