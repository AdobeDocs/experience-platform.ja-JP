---
title: SFTP ソースコネクタの概要
description: API またはユーザーインターフェイスを使用して SFTP サーバーを Adobe Experience Platform に接続する方法について説明します。
exl-id: d5bced3d-cd33-40ea-bce0-32c76ecd2790
source-git-commit: 4816a6b627dc6551e351bfe3cdc4bc8c8ea8b17e
workflow-type: tm+mt
source-wordcount: '1215'
ht-degree: 42%

---

# SFTP コネクタ

Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL SFTP] アカウントをExperience Platformに正常に接続するために必要な前提条件の手順については、このドキュメントを参照してください。

>[!TIP]
>
>接続する前に、SFTP サーバー設定でキーボードインタラクティブ認証を無効にする必要があります。 この設定を無効にすると、パスワードをサービスやプログラムを通じて入力するのではなく、手動で入力できるようになります。

## 前提条件 {#prerequisites}

[!DNL SFTP] ソースをExperience Platformに正常に接続するために必要な前提条件の手順については、この節を参照してください。

### IP アドレスの許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 許可リストに加える詳しくは、[IP アドレス &#x200B;](../../ip-address-allow-list.md) ページを参照してください。

### ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストを次に示します。

* ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
* ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
* 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
* 次の文字は使用できません。`" \ / : | < > * ?`
* 無効な URL パス文字は使用できません。`\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
* 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

### [!DNL SFTP] 用の Base64 にエンコードされた OpenSSH 秘密鍵の設定

[!DNL SFTP] ソースは [!DNL Base64] にエンコードされた OpenSSH 秘密鍵を使用した認証をサポートしています。Base64 にエンコードされた OpenSSH 秘密鍵を生成し、Experience Platformに接続する方法については、以下の手順を参照し [!DNL SFTP] ください。

>[!BEGINTABS]

>[!TAB Windows]

### [!DNL Windows] ユーザー

[!DNL Windows] コンピューターを使用している場合は、**スタート**&#x200B;メニューを開き、「**設定**」を選択します。

![settings](../../images/tutorials/create/sftp/settings.png)

表示された&#x200B;**設定**&#x200B;メニューで「**アプリ**」を選択します。

![apps](../../images/tutorials/create/sftp/apps.png)

次に「**オプション機能**」を選択します。

![optional-features](../../images/tutorials/create/sftp/optional-features.png)

オプション機能のリストが表示されます。 **OpenSSH クライアント**&#x200B;がお使いのコンピューターに事前にインストールされている場合は、「**オプション機能**」の「**インストールされている機能**」のリストに含まれているはずです。

![open-ssh](../../images/tutorials/create/sftp/open-ssh.png)

インストールされていない場合は、「**インストール**」を選択して **[!DNL Powershell]** を開き、次のコマンドを実行して秘密鍵を生成します。

```shell
PS C:\Users\lucy> ssh-keygen -t rsa -m pem
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\lucy/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\lucy/.ssh/id_rsa.
Your public key has been saved in C:\Users\lucy/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:osJ6Lg0TqK8nekNQyZGMoYwfyxNc+Wh0hYBtBylXuGk lucy@LAPTOP-FUJT1JEC
The key's randomart image is:
+---[RSA 3072]----+
|.=.*+B.o.        |
|=.O.O +          |
|+o+= B           |
|+o +E .          |
|.o=o  . S        |
|+... . .         |
| *o .            |
|o.B.             |
|=O..             |
+----[SHA256]-----+
```

次に、秘密鍵のファイルパスを指定して次のコマンドを実行し、秘密鍵を [!DNL Base64] でエンコードします。

```shell
C:\Users\lucy> [convert]::ToBase64String((Get-Content -path "C:\Users\lucy\.ssh\id_rsa" -Encoding byte)) > C:\Users\lucy\.ssh\id_rsa_base64
```

上記のコマンドにより、[!DNL Base64] でエンコードされた秘密鍵が指定したファイルパスに保存されます。これで、その秘密鍵を使用して [!DNL SFTP] への認証を行い、Experience Platformに接続できます。

>[!TAB Mac]

### [!DNL Mac] ユーザー

[!DNL Mac] を使用している場合は、**ターミナル**&#x200B;を開き、次のコマンドを実行して秘密鍵を生成します（この場合、秘密鍵は `/Documents/id_rsa` に保存されます）。

```shell
ssh-keygen -t rsa -m pem -f ~/Documents/id_rsa
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/vrana/Documents/id_rsa.
Your public key has been saved in /Users/vrana/Documents/id_rsa.pub.
The key fingerprint is:
SHA256:s49PCaO4a0Ee8I7OOeSyhQAGc+pSUQnRii9+5S7pp1M vrana@vrana-macOS
The key's randomart image is:
+---[RSA 2048]----+
|o ==..           |
|.+..o            |
|oo.+             |
|=.. +            |
|oo = .  S        |
|+.+ +E . = .     |
|o*..*.. . o      |
|.o*=.+   +       |
|.oo=Oo  ..o      |
+----[SHA256]-----+
```

次に、以下のコマンドを実行して、秘密鍵を [!DNL Base64] でエンコードします。

```shell
base64 ~/Documents/id_rsa > ~/Documents/id_rsa_base64
 
 
# Print Content of base64 encoded file
cat ~/Documents/id_rsa_base64
LS0tLS1CRUdJTiBPUEVOU1NIIFBSSVZBVEUgS0VZLS0tLS0KYjNCbGJuTnphQzFyWlhrdGRqRUFBQUFBQkc1dmJtVUFBQUFFYm05dVpRQUFBQUFBQUFBQkFBQUJGd0FBQUFkemMyZ3RjbgpOaEFBQUFBd0VBQVFBQUFRRUF0cWFYczlXOUF1ZmtWazUwSXpwNXNLTDlOMU9VYklaYXVxbVM0Q0ZaenI1NjNxUGFuN244CmFxZWdvQTlCZnVnWDJsTVpGSFl5elEzbnp6NXdXMkdZa1hkdjFjakd0elVyNyt1NnBUeWRneGxrOGRXZWZsSzBpUlpYWW4KVFRwS0E5c2xXaHhjTXg3R2x5ejdGeDhWSzI3MmdNSzNqY1d1Q0VIU3lLSFR5SFFwekw0MEVKbGZJY1RGR1h1dW1LQjI5SwpEakhwT1grSDdGcG5Gd1pabTA4Uzc2UHJveTVaMndFalcyd1lYcTlyUDFhL0E4ejFoM1ZLdllzcG53c2tCcHFQSkQ1V3haCjczZ3M2OG9sVllIdnhWajNjS3ZsRlFqQlVFNWRNUnB2M0I5QWZ0SWlrYmNJeUNDaXV3UnJmbHk5eVNPQ2VlSEc0Z2tUcGwKL3V4YXNOT0h1d0FBQThqNnF6R1YrcXN4bFFBQUFBZHpjMmd0Y25OaEFBQUJBUUMycHBlejFiMEM1K1JXVG5Rak9ubXdvdgowM1U1UnNobHE2cVpMZ0lWbk92bnJlbzlxZnVmeHFwNkNnRDBGKzZCZmFVeGtVZGpMTkRlZlBQbkJiWVppUmQyL1Z5TWEzCk5TdnY2N3FsUEoyREdXVHgxWjUrVXJTSkZsZGlkTk9rb0QyeVZhSEZ3ekhzYVhMUHNYSHhVcmJ2YUF3cmVOeGE0SVFkTEkKb2RQSWRDbk12alFRbVY4aHhNVVplNjZZb0hiMG9PTWVrNWY0ZnNXbWNYQmxtYlR4THZvK3VqTGxuYkFTTmJiQmhlcjJzLwpWcjhEelBXSGRVcTlpeW1mQ3lRR21vOGtQbGJGbnZlQ3pyeWlWVmdlL0ZXUGR3cStVVkNNRlFUbDB4R20vY0gwQiswaUtSCnR3aklJS0s3Qkd0K1hMM0pJNEo1NGNiaUNST21YKzdGcXcwNGU3QUFBQUF3RUFBUUFBQVFBcGs0WllzMENSRnNRTk9WS0sKYWxjazlCVDdzUlRLRjFNenhrSGVydmpJYk9kL0lvRXpkcHlVa28rbm41RmpGK1hHRnNCUXZnOFdTaUlJTk1oU3BNYWI1agpvWXlka2gvd0ovWElOaDlZaE5QVXlURi9NNkFnMkNYd21KS2RxN1VKWjZyNjloV3V0VVN6U05QbkVYWTZLc29GeVUwTEFvCko0OHJMT1pMZldtMHFhWDBLNUgzNmJPaHFXSWJwMDNoZk94eno5M0MrSDM5MFJkRkp4bzJVZ0FVY3UvdHREb0REVldBdmEKVkVyMWEzak9LenVHbThrK21WeXpPZERjVFY4ckZIT0pwRnRBU3l6Q24yVld1MjV0TWtrcGRPRjNKcVdMZHdOY3loeG1URApXZGVDNWh4V0Fiano0WDZ5WXpHcFcwTmptVkFoWUVVZGNBSVlXWWM3OGEvQkFBQUFnRm8wakl4aGhwZkJ6QjF6b09FMDJBClpjTC9hcUNuYysrdmJ1a2V0aFg5Zzhlb0xQMTQyeUgzdlpLczl3c1RtbVVsZ0prZURaN2hUcklwOGY2eEwzdDRlMXByY1kKb2ZLd0gwckNGOTFyaldPbGZOUmxEempoR1NTTEVMczZoNlNzMEdBQXE2Z0ZQTVF2dTB4TDlQUTlGQ21YZVVKazJpRm1MWgpEWWJGc0NyVUxEQUFBQWdRRGF0a1pMamJaSTBFM0ZuY2dTOVF5Y3lVWmtkZ1dVNjBQcG9ud3BMQXdUdHRpOG1EQXE5cHYwClEvUlk1WE9UeGF3VXNHa0tYMjNtV1BYR0grdUlBSzhrelVVM2dGM1dRWGVkTWw4NHVCVFZCTEtUdStvVVAvZmIvMEE0dE0KSE9BSythbXZPMkZuYzFiSmVwd05USTE2cjZXWk9sZWV2ZklJQVpXcEgxVVpIdkVRQUFBSUVBMWNwcStDNUVXSFJwbnVPZQpiNHE4T0tKTlJhSUxIRUN6U0twWlFpZDFhRmJYWlVKUXpIQU85YzhINVZMcjBNUjFkcW1ORkNja2ZsZzI2Y3BEUEl3TjBYCm5HMFBxcmhKbXp0U3ZQZ3NGdkNPallncXF6U0RYUjkxd1JQTEN5cU8zcGMyM2kzZnp2WkhtMGhIdWdoNVJqV0loUlFZVkwKZUpDWHRqM08vY3p1SWdzQUFBQVJkbkpoYm1GQWRuSmhibUV0YldGalQxTUJBZz09Ci0tLS0tRU5EIE9QRU5TU0ggUFJJVkFURSBLRVktLS0tLQo=
```

[!DNL Base64] でエンコードされた秘密鍵が指定したフォルダーに保存されたら、公開鍵ファイルの内容を [!DNL SFTP] ホスト認証済みキーの新しい行に追加する必要があります。コマンドラインで次のコマンドを実行します。

```shell
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
```

公開鍵が正しく追加されたかどうかを確認するには、コマンドラインで次のコマンドを実行します。

```shell
more ~/.ssh/authorized_keys
```

>[!ENDTABS]

### 必要な資格情報の収集 {#credentials}

[!DNL SFTP] サーバーをExperience Platformに接続するには、次の資格情報の値を指定する必要があります。

>[!BEGINTABS]

>[!TAB  基本認証 ]

基本認証を使用して [!DNL SFTP] サーバーを認証するには、次の資格情報に適切な値を指定します。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL SFTP] サーバーに関連付けられた名前または IP アドレス。 |
| `port` | 接続先の [!DNL SFTP] サーバーポート。 指定しない場合、値はデフォルトで `22` になります。 |
| `username` | [!DNL SFTP] サーバーにアクセスできるユーザー名。 |
| `password` | [!DNL SFTP] サーバーのパスワード。 |
| `maxConcurrentConnections` | このパラメーターを使用すると、Experience Platformが SFTP サーバーへの接続時に作成する同時接続数の上限を指定できます。 この値は、SFTP で設定された制限以下に設定する必要があります。 **注意**：既存の SFTP アカウントに対してこの設定が有効になっている場合、既存のデータフローではなく、今後のデータフローにのみ影響します。 |
| `folderPath` | アクセス権を付与するフォルダーへのパス。 ソース [!DNL SFTP]、フォルダーパスを指定して、選択したサブフォルダーへのユーザーアクセスを指定できます。 |
| `disableChunking` | データ取り込み時に、[!DNL SFTP] ソースは最初にファイル長を取得し、ファイルを複数の部分に分割してから、並行して読み取ることができます。 この値を有効または無効にして、[!DNL SFTP] サーバーがファイル長を取得できるか、特定のオフセットからデータを読み取れるかを指定できます。 |
| `connectionSpec.id` | （API のみ）接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL SFTP] の接続仕様 ID は `b7bf2577-4520-42c9-bae9-cad01560f7bc` です。 |

>[!TAB SSH 公開鍵認証 ]

SSH 公開鍵認証を使用して [!DNL SFTP] サーバーを認証するには、次の資格情報に適切な値を指定します。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL SFTP] サーバーに関連付けられた名前または IP アドレス。 |
| `port` | 接続先の [!DNL SFTP] サーバーポート。 指定しない場合、値はデフォルトで `22` になります。 |
| `username` | [!DNL SFTP] サーバーにアクセスできるユーザー名。 |
| `password` | [!DNL SFTP] サーバーのパスワード。 |
| `privateKeyContent` | Base64 にエンコードされた SSH 秘密鍵の内容。 サポートされている OpenSSH キータイプは、`ed25519`、`RSA`、`DSA` です。 |
| `passPhrase` | キーファイルまたはキーの内容がパスフレーズによって保護されている場合に秘密鍵を復号化するためのパスフレーズまたはパスワード。 PrivateKeyContent がパスワードで保護されている場合、このパラメーターは、PrivateKeyContent のパスフレーズを値として使用する必要があります。 |
| `maxConcurrentConnections` | このパラメーターを使用すると、Experience Platformが SFTP サーバーへの接続時に作成する同時接続数の上限を指定できます。 この値は、SFTP で設定された制限以下に設定する必要があります。 **注意**：既存の SFTP アカウントに対してこの設定が有効になっている場合、既存のデータフローではなく、今後のデータフローにのみ影響します。 |
| `folderPath` | アクセス権を付与するフォルダーへのパス。 ソース [!DNL SFTP]、フォルダーパスを指定して、選択したサブフォルダーへのユーザーアクセスを指定できます。 |
| `disableChunking` | データ取り込み時に、[!DNL SFTP] ソースは最初にファイル長を取得し、ファイルを複数の部分に分割してから、並行して読み取ることができます。 この値を有効または無効にして、[!DNL SFTP] サーバーがファイル長を取得できるか、特定のオフセットからデータを読み取れるかを指定できます。 |
| `connectionSpec.id` | （API のみ）接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL SFTP] の接続仕様 ID は `b7bf2577-4520-42c9-bae9-cad01560f7bc` です。 |

>[!ENDTABS]

## SFTP のExperience Platformへの接続

以下のドキュメントでは、API またはユーザーインターフェイスを使用して、SFTP サーバーをExperience Platformに接続する方法に関する情報を提供します。

### API の使用

* [Flow Service API を使用して SFTP ベース接続を作成](../../tutorials/api/create/cloud-storage/sftp.md)
* [Flow Service API を使用して、クラウドストレージソースのデータ構造とコンテンツを探索](../../tutorials/api/explore/cloud-storage.md)
* [Flow Service API を使用して、クラウドストレージソースのデータフローを作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

* [UI で SFTP ソース接続を作成](../../tutorials/ui/create/cloud-storage/sftp.md)
* [UI でクラウドストレージ接続のデータフローを作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
