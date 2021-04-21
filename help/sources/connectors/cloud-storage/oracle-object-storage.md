---
keywords: Experience Platform；ホーム；人気の高いトピック；Oracleオブジェクトストレージ;oracleオブジェクトストレージ
solution: Experience Platform
title: Oracleオブジェクトストレージソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してOracleオブジェクトストレージをAdobe Experience Platformに接続する方法について説明します。
exl-id: 5e8b85c8-9f01-49a6-9556-7b9c7518fb4b
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 3%

---

# Oracleオブジェクトストレージコネクタ

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]などのクラウドプロバイダーにネイティブの接続を提供し、これらのシステムのデータをプラットフォームに取り込み、ダウンストリームのサービスや宛先で使用できます。

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、プラットフォームにデータを取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parket、または区切り形式で形式設定できます。 プロセスのすべての手順がソースワークフローに統合されます。 プラットフォームでは、[!DNL Oracle Object Storage]からバッチを通じてデータを取り込むことができます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、[IPアドレス許可リスト](../../ip-address-allow-list.md)ドキュメントを参照してください。

## ファイルとディレクトリの命名規則

次に、クラウドストレージファイルやディレクトリに名前を付ける際に考慮する必要がある制約のリストを示します。

- ディレクトリ名とファイルコンポーネント名は255文字以内にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ(`/`)を付けることはできません。 指定した場合は、自動的に削除されます。
- 次の予約済みURL文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`.
- 無効なURLパス文字は使用できません。 `\uE000`のようなコードポイントは、NTFSファイル名では有効ですが、有効なUnicode文字ではありません。 また、制御文字（0x00 ～ 0x1F、\u0081など）など、ASCII文字やUnicode文字には使用できないものもあります。 HTTP/1.1でUnicode文字列を扱うルールについては、[RFC 2616, Section 2.2を参照してください。基本規則](https://www.ietf.org/rfc/rfc2616.txt)と[RFC 3987](https://www.ietf.org/rfc/rfc3987.txt)。
- 次のファイル名は使用できません。LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、prn、AUX、NUL、CON、CLOCK$、ドット文字(.)および2つのドット文字(..)。

## [!DNL Oracle Object Storage]をプラットフォームに接続

以下のドキュメントは、APIまたはユーザーインターフェイスを使用してOracleオブジェクトストレージをAdobe Experience Platformに接続する方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用してOracleオブジェクトストレージソース接続を作成する](../../tutorials/api/create/cloud-storage/oracle-object-storage.md)
- [Flow Service APIを使用したクラウドストレージシステムの調査](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service APIを使用してクラウドストレージデータを収集する](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UIでのOracleオブジェクトストレージソース接続の作成](../../tutorials/ui/create/cloud-storage/oracle-object-storage.md)
- [UIでのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/batch/cloud-storage.md)
