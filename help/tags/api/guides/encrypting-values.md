---
title: 値の暗号化
description: Reactor API を使用する際に機密性の高い値を暗号化する方法を説明します。
exl-id: d89e7f43-3bdb-40a5-a302-bad6fd1f4596
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 100%

---

# 値の暗号化

Adobe Experience Platform でタグを使用する場合、一部のワークフローでは機密性の高い値を提供する必要があります（例えば、ホスト経由で環境にライブラリを配信する際に秘密キーを提供するなど）。これらの認証情報は機密性が高いため、
安全に転送し、保管する必要があります。

このドキュメントでは、[GnuPG 暗号化](https://www.gnupg.org/gph/en/manual/x110.html)（GPG とも呼ばれます）を使用して機密性の高い値を暗号化し、タグシステムだけが読み取れるようにする方法について説明します。

## 公開 GPG キーとチェックサムの取得

[ダウンロード](https://gnupg.org/download/)して最新バージョンの GPG をインストールした後、タグの実稼動環境用の公開 GPG キーを取得する必要があります。

* [GPG キー](https://github.com/adobe/reactor-developer-docs/blob/master/files/launch%40adobe.com_pub.gpg)
* [チェックサム](https://github.com/adobe/reactor-developer-docs/blob/master/files/launch%40adobe.com_pub.gpg.sum)

## キーチェーンにキーをインポート

マシンにキーを保存したら、次はそのキーを GPG キーチェーンに追加します。

**構文**

```shell
gpg --import {KEY_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{KEY_NAME}` | 公開キーファイルの名前。 |

{style="table-layout:auto"}

**例**

```shell
gpg --import launch@adobe.com_pub.gpg
```

## 値の暗号化

キーチェーンにキーを追加した後、`--encrypt` フラグを使用して値の暗号化を開始できます。 次のスクリプトは、このコマンドの動作方法を示します。

```shell
echo -n 'Example value' | gpg --armor --encrypt -r "Tags Data Encryption <launch@adobe.com>"
```

このコマンドは、次のように分類できます。

* 入力は `gpg` コマンドに渡されます。
* `--armor` はバイナリではなく ASCII アーマー形式の出力を作成します。これにより、JSON を使用した値の転送が簡単になります。
* `--encrypt` は、GPG にデータの暗号化を指示します。
* `-r` は、データの受信者を設定します。受信者（公開キーに対応する秘密キーの所有者）だけがデータを復号化できます。 目的のキーの受信者名は、`gpg --list-keys` の出力を調べれば見つかります。

上記のコマンドは、`Tags Data Encryption <launch@adobe.com>` の公開キーを使用して、ASCII アーマー形式の値 `Example value` を暗号化します。

コマンドの出力は次のようになります。

```shell
-----BEGIN PGP MESSAGE-----

hQIMAxJHCI6fydT/ARAAwQ0Y0k7eSAbd0T9seoaWX75G70O2gxAF20KY5FWiZ9/m
/RkgJwhJusZyEdazC/CmAdfXi9bsVxQT0i06ErUxXfQF0VtweRlcyRBsxzLz6Hr+
BpYGnq+cCCzGAT73Gg1CM4UWmaPKLLyWKGkXtDBAqVBRAIQT/8JhnkbyWIohHkWV
I/Uf7NrPXuaSmrqZ1SZQgwjIM3qNMR02qtqg59dncKoCQBji8Oeb8lqRLskRT0Jq
gVgbJYwSe2n6KpJkELJ6QtF9lCRl1+yU4mvM4jBHgkM1+vb1WmbFRIR40dDpg85N
0J9hVj4bg//eLRDfAdEC9kgq9Atph0WqJ5EpehdS7yVO9lO8mpbpqZ4BCGjTi/VS
isEPr6eZ2mxRbk8f9Z4csRZnkErY8ep5+cqC5CZVdmguWvC9PKzXqEsPFd0PSYk3
Qp3UIW2/JMf16E5CKmntm+gKdl6kggZOOvNQuyJYa9yNbzySPerHXsknTOxV+QP/
WXwrAL52g5+gpMib7Ve/KBz5/OViDhDqkmHzlGad73W74d+CYjf0AnuXuWRRlUMT
s8ORw1eplInldhXk2mgkGPZS/gWDs3zpKUu4GSO9AaeWldynLG/Bgh78XhumQ58h
ekGD+p3PyyvxjfS5G/wf9HQZ085+mnjpKFa7fuFBQPbg4WpBadhWrhobthC+hN3S
SAE9yWU11Y3xpoxqg4y7iYZ6rnX+qP2oUNYxC2/hdhsFbbZtUh4s51qaoLbe0iWB
OUoIPf4KxTaboHZOEy32ZBng5heVrn4i9w==
=jrfE
-----END PGP MESSAGE-----
```

この出力は、`Tags Data Encryption <launch@adobe.com>` 公開キーに対応する秘密キーを持つシステムによってのみ復号化できます。

この出力は、Reactor API へのデータ送信時に提供する必要がある値です。システムは、この暗号化された出力を保存し、必要に応じて一時的に復号化します。 例えば、システムはサーバーへの接続を開始するのに十分な時間、ホストの認証情報を復号化し、その後、復号化された値のすべてのトレースをすぐに削除します。

>[!NOTE]
>
>変換され、暗号化された値の形式は重要です。リクエストで指定された値で行の戻り値が適切にエスケープされていることを確認します。
