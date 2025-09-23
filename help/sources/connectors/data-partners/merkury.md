---
title: Merkury エンタープライズ Id 解決Sourceの概要
description: ユーザーインターフェイスを使用して Merkury Enterprise Identity Resolution をAdobe Experience Platformに接続する方法を説明します。
exl-id: c5eaa561-d620-4c82-bce1-972d0a422c3f
source-git-commit: e402a58f51de49b26f9d279cebf551ec11e4698f
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 34%

---

# [!DNL Merkury Enterprise Identity Resolution]

Adobe Experience Platformは、データパートナーアプリケーションからのデータ取り込みをサポートしています。 データパートナーのサポートには、[!DNL Merkury Enterprise Identity Resolution] が含まれます。

[!DNL Merkury] by [!DNL Merkle] を使用すると、Cookie を使用しなくても、より多くのデジタル訪問者を認識し、顧客のニーズに合わせてパーソナライズされた適切なエクスペリエンスを提供できます。

**ユーザー ID** を [!DNL Merkury] ソースの一部として利用し、組織が個人について知っているすべてを 1 つの包括的なプロファイルに結合できます。 これには、次のような詳細が含まれます。

- デジタル行動
- 購入環境設定
- 識別情報（名前、メールアドレス、物理アドレス、デバイス ID など）。

取り込んだデータを、Experience Data Model （XDM） JSON、XDM Parquet または区切り形式で書式設定できます。 プロセスのすべての手順がソース作業に統合されます

![Merkury ソースのデータ処理ワークフローの図。](../../images/tutorials/create/merkury-enterprise-identity-resolution-assets/architecture.png)

## IP アドレスの許可リスト

ソースコネクタを使用する前に、地域に必要な IP アドレスを許可リストに追加する必要があります。 これらの IP アドレスを追加しないと、ソースコネクタが正しく動作しないか、エラーが発生する可能性があります。 許可リストに加える詳細な手順と許可する IP アドレスの一覧については、[IP アドレス ](../../ip-address-allow-list.md) を参照してください。

## ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストを次に示します。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。`\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## 前提条件

[!DNL Merkury] ソースの使用を開始するには、次の前提条件を満たしている必要があります。

- [!DNL Merkury] の設定を [!DNL Merkury] チームと完了する必要があります。
- [!DNL Merkury] チームから資格情報（アクセスキー、秘密鍵、バケット名）を取得する必要があります。 

>[!NOTE]
>
>`myBucket/folder/subfolder/subsubfolder/abc.csv` のようなファイルパスを使用すると、`subsubfolder/abc.csv` にのみアクセスする場合があります。 サブフォルダーにアクセスする場合は、バケットパラメーターを myBucket に、folderPath を folder/subfolder に指定して、ファイルの探索を `subsubfolder/abc.csv` ではなくサブフォルダーで開始できるようにします。

## 次の手順

このドキュメントでは、[!DNL Merkury] アカウントからExperience Platformにデータを取り込むために必要な、前提条件の設定を完了しました。 これで、ユーザーインターフェイスを使用した [Experience Platformへの接続  [!DNL Merkury]  に関するガイドに進むことができ ](../../tutorials/ui/create/data-partners/merkury.md) す。
