---
title: Reactor APIでの応答のページ付け
description: Reactor APIでリソースをリストする際に結果をページ番号付けする方法を説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 0%

---

# Reactor APIでの応答のページ付け

Reactor APIから返される応答はページ分割されます。 デフォルトのページサイズは25個です。 ページネーションに関する詳細は、API応答オブジェクトの`meta.pagination `セクションにレポートされます。

```json
"meta": {
  "pagination": {
      "current_page": 1,
      "next_page": 2,
      "prev_page": null,
      "total_pages": 4,
      "total_count": 90
  }
}
```

リクエストパスに`page`クエリパラメーターを含めることで、特定のページを取得し、ページのサイズを変更できます。

## 特定のページの取得

特定のページを取得するには：

```http
GET /{RESOURCE_TYPE}/{RESOURCE_ID}?page[number]={PAGE_NUMBER}
```

## ページサイズの変更

ページサイズを変更するには：

```http
GET /{RESOURCE_TYPE}/{RESOURCE_ID}?page[size]={PAGE_SIZE}
```

様々なオプションを組み合わせることができます。

```http
GET /{RESOURCE_TYPE}/{RESOURCE_ID}?page[size]={PAGE_SIZE}&page[number]={PAGE_NUMBER}
```
