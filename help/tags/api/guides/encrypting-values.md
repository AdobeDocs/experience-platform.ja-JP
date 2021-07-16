---
title: 値の暗号化
description: Reactor APIを使用する際に機密値を暗号化する方法を説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 1%

---

# 値の暗号化

Adobe Experience Platformでタグを使用する場合、一部のワークフローでは機密値を指定する必要があります（例えば、ホストを介して環境にライブラリを配信する際に秘密鍵を提供するなど）。 これらの資格情報の機密性は、
安全な転送と保存。

このドキュメントでは、[GnuPG暗号化](https://www.gnupg.org/gph/en/manual/x110.html)（GPGとも呼ばれます）を使用して機密値を暗号化し、タグシステムだけが読めるようにする方法について説明します。

## 公開GPGキーとチェックサムの取得

[ダウンロード](https://gnupg.org/download/)して最新バージョンのGPGをインストールした後、タグ実稼動環境用の公開GPGキーを入手する必要があります。

* [GPGキー](https://github.com/adobe/reactor-developer-docs/blob/master/files/launch%40adobe.com_pub.gpg)
* [チェックサム](https://github.com/adobe/reactor-developer-docs/blob/master/files/launch%40adobe.com_pub.gpg.sum)

## キーチェーンにキーを読み込む

マシンに鍵を保存したら、次の手順は、鍵をGPGキーチェーンに追加することです。

**構文**

```shell
gpg --import {KEY_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{KEY_NAME}` | 公開鍵ファイルの名前。 |

{style=&quot;table-layout:auto&quot;}

**例**

```shell
gpg --import launch@adobe.com_pub.gpg
```

## 値の暗号化

キーチェーンにキーを追加した後、`--encrypt`フラグを使用して値の暗号化を開始できます。 次のスクリプトは、このコマンドの動作を示します。

```shell
echo -n 'Example value' | gpg --armor --encrypt -r "Tags Data Encryption <launch@adobe.com>"
```

このコマンドは、次のように分類できます。

* 入力は`gpg`コマンドに渡されます。
* `--armor` はバイナリではなくASCII装甲出力を生成します。これにより、JSONを使用した値の転送が簡単になります。
* `--encrypt` は、GPGにデータの暗号化を指示します。
* `-r` は、データの受信者を設定します。受信者（公開鍵に対応する秘密鍵の保持者）のみがデータを復号化できます。 `gpg --list-keys`の出力を調べると、目的のキーの受信者名が見つかります。

上記のコマンドは、`Tags Data Encryption <launch@adobe.com>`の公開鍵を使用して、ASCII装甲形式の値`Example value`を暗号化します。

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

この出力は、秘密鍵を持つシステムによってのみ復号化できます。
は、`Tags Data Encryption <launch@adobe.com>`公開鍵に対応します。

この出力は、Reactor APIにデータを送信する際にで指定する値です。 システムは、この暗号化された出力を保存し、必要に応じて一時的に復号します。 例えば、システムは、サーバーへの接続を開始するのに十分な長さのホスト資格情報を復号化し、その後すぐに、復号化された値のすべてのトレースを削除します。

>[!NOTE]
>
>装甲、暗号化された値の形式は重要です。 リクエストで指定された値で行の戻り値が適切にエスケープされていることを確認します。
