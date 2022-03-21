---
keywords: SFTP;sftp
title: SFTP 接続
description: SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的にAdobe Experience Platformから書き出します。
exl-id: 27abfc38-ec19-4321-b743-169370d585a0
source-git-commit: fa393b2bd8085a17653962b5a8b112a5db10df83
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 1%

---

# SFTP 接続

## 概要 {#overview}

SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的にAdobe Experience Platformから書き出します。

>[!IMPORTANT]
>
> Adobeは SFTP サーバーへのデータの書き出しをサポートしていますが、データを書き出す際に推奨されるクラウドストレージの場所は次のとおりです [!DNL Amazon S3] および [!DNL Azure Blob].

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：（電子メールアドレス、電話番号、姓）。「プロファイル属性を選択」画面で選択します。 [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳細を表示 [バッチファイルベースの宛先](/help/destinations/destination-types.md#file-based). |

{style=&quot;table-layout:auto&quot;}

![SFTP プロファイルベースの書き出しタイプ](../../assets/catalog/cloud-storage/sftp/catalog.png)

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_sftp_rsa"
>title="RSA 公開鍵"
>abstract="必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、Base64 エンコードされた文字列として書き込む必要があります。"

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_sftp_ssh"
>title="SSH キー"
>abstract="SSH キーには Base64 文字列が必要です。"

条件 [接続](../../ui/connect-destination.md) をこの宛先に追加するには、次の情報を指定する必要があります。

#### 認証情報 {#authentication-information}

次を選択した場合、 **[!UICONTROL 基本認証]** SFTP の場所に接続するには、次のように入力します。

![SFTP 宛先の基本認証](/help/destinations/assets/catalog/cloud-storage/sftp/stfp-basic-authentication.png)

* **[!UICONTROL ホスト]**:SFTP ストレージの場所のアドレス。
* **[!UICONTROL ユーザー名]**:SFTP ストレージの場所にログインするユーザー名。
* **[!UICONTROL パスワード]**:SFTP ストレージの場所にログインするためのパスワード。
* **[!UICONTROL 暗号化キー]**:必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、 [!DNL Base64] エンコードされた文字列。


次を選択した場合、 **[!UICONTROL SSH キーを使用した SFTP]** SFTP ロケーションに接続するための認証タイプ：

![SFTP 宛先の SSH キー認証](/help/destinations/assets/catalog/cloud-storage/sftp/sftp-ssh-key-authentication.png)

* **[!UICONTROL ドメイン]**:SFTP アカウントの IP アドレスまたはドメイン名を入力します
* **[!UICONTROL ポート]**:SFTP ストレージの場所で使用されるポート。
* **[!UICONTROL ユーザー名]**:SFTP ストレージの場所にログインするユーザー名。
* **[!UICONTROL SSH キー]**:SFTP ストレージの場所にログインするための SSH キー。
* **[!UICONTROL 暗号化キー]**:必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、 [!DNL Base64] エンコードされた文字列。

#### 宛先の詳細 {#destination-details}

SFTP の場所への認証接続を確立したら、宛先に次の情報を指定します。

![SFTP 宛先に対して使用可能な宛先の詳細](/help/destinations/assets/catalog/cloud-storage/sftp/sftp-destination-details.png)

* **[!UICONTROL 名前]**:Experience Platformユーザーインターフェイスでこの宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**:SFTP でファイルを書き出す場所のフォルダーのパスを入力します。

## 書き出されたデータ {#exported-data}

の場合 [!DNL SFTP] 宛先、Platform は、 `.csv` ファイルを指定したストレージの場所に保存します。 ファイルの詳細については、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) （セグメントのアクティベーションに関するチュートリアル）。

## IP アドレスの許可リスト

参照： [クラウドストレージの宛先の IP アドレス許可リスト](ip-address-allow-list.md) 許可リストにAdobeIP を追加する必要がある場合。
