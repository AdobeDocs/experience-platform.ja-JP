---
title: 値の暗号化
description: Reactor API を使用する際に機密性の高い値を暗号化する方法を説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: ht
source-wordcount: '395'
ht-degree: 100%

---

# 値の暗号化

Adobe Experience Platform でタグを使用する場合、一部のワークフローでは機密性の高い値を提供する必要があります（例えば、ホスト経由で環境にライブラリを配信する際に秘密キーを提供するなど）。これらの認証情報は機密性が高いため、
安全に転送し、保管する必要があります。

このドキュメントでは、[GnuPG 暗号化](https://www.gnupg.org/gph/en/manual/x110.html)（GPG とも呼ばれます）を使用して機密性の高い値を暗号化し、タグシステムだけが読み取れるようにする方法について説明します。

## GPG 公開キーとチェックサムの取得

最新バージョンの GPG を[ダウンロード](https://gnupg.org/download/)してインストールした後、タグ実稼動環境用の以下の GPG 公開キーを取得する必要があります。

* [GPG キー](https://github.com/adobe/reactor-developer-docs/blob/master/files/launch%40adobe.com_pub.gpg)
* [チェックサム](https://github.com/adobe/reactor-developer-docs/blob/master/files/launch%40adobe.com_pub.gpg.sum)

## キーチェーンに鍵を読み込む

マシンにキーを保存したら、そのキーを GPG キーチェーンに追加します。

**構文**

```shell
gpg --import {KEY_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{KEY_NAME}` | 公開キーのファイルの名前。 |

{style=&quot;table-layout:auto&quot;}

**例**

```shell
gpg --import launch@adobe.com_pub.gpg
```

## 値の暗号化

キーチェーンにキーを追加したら、`--encrypt` フラグを使用して値の暗号化を開始できます。次のスクリプトは、このコマンドがどのように動作するかを示しています。

```shell
echo -n 'Example value' | gpg --armor --encrypt -r "Tags Data Encryption <launch@adobe.com>"
```

このコマンドは、次のように分解できます。

* 入力は `gpg` コマンドに渡されます。
* `--armor` はバイナリではなく ASCII アーマー形式の出力を作成します。これで、JSON を使用して値を簡単に転送できるようになります。
* `--encrypt` によって、GPG にデータの暗号化が指示されます。
* `-r` によって、データの受信者が設定されます。受信者（公開キーに対応する秘密キーの保有者）のみがデータを復号化できます。`gpg --list-keys` の出力を調べることで、目的のキーの受信者名が分かります。

上記のコマンドは、`Tags Data Encryption <launch@adobe.com>` の公開キーを使用して、値 `Example value` を ASCII アーマー形式で暗号化します。

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

この出力を復号化できるのは、`Tags Data Encryption <launch@adobe.com>` 公開キーに対応する秘密キーを持つシステムのみです。

この出力の値は、Reactor API にデータを送信する際に指定する必要があります。システムにはこの暗号化された出力が保存され、必要に応じて一時的に復号化されます。例えば、システムでは、サーバーへの接続を開始するのに十分な長さのホスト認証情報が復号化され、復号化された値のトレースすべてがすぐに削除されます。

>[!NOTE]
>
>アーマー形式の暗号化された値は重要です。リクエストで指定された値で、行の戻り値が適切にエスケープされていることを確認します。
