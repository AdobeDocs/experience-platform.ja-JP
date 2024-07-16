---
title: ソース UI のWorkspaceでの暗号化されたデータの取り込み
description: ソース UI ワークスペースで暗号化されたデータを取り込む方法を説明します。
hide: true
hidefromtoc: true
exl-id: 34aaf9b6-5c39-404b-a70a-5553a4db9cdb
source-git-commit: a11f469cb54421e0ca30c7c5878128e216470f7f
workflow-type: tm+mt
source-wordcount: '111'
ht-degree: 54%

---

# ソース UI での暗号化されたデータの取り込み

このガイドでは、バッチデータのクラウドストレージソースを使用して、暗号化されたデータをAdobe Experience Platformに取り込む方法について説明します。

## 前提条件

* データの暗号化

## 暗号化されたデータの取り込み {#ingest-encrypted-data}

>[!CONTEXTUALHELP]
>id="platform_sources_encrypted_isFileEncrypted"
>title="ファイルは暗号化されていますか？"
>abstract="既に暗号化されているファイルを取り込む場合は、この切替スイッチを選択します。"


>[!CONTEXTUALHELP]
>id="platform_sources_encrypted_sampleFile"
>title="サンプルファイルの選択"
>abstract="マッピングを作成するには、暗号化されたデータを取り込む際にサンプルファイルを取り込む必要があります。"

>[!CONTEXTUALHELP]
>id="platform_sources_encrypted_createCustomKey"
>title="カスタムキーの作成"
>abstract="オプションで、署名検証キーペアを作成して、暗号化されたデータ用のカスタムキーを作成することができます。"
