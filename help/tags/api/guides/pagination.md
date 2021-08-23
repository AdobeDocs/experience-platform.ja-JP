---
title: Reactor API での応答のページ付け
description: Reactor API でリソースをリストする際に結果をページ付けする方法を説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: ht
source-wordcount: '101'
ht-degree: 100%

---

# Reactor API での応答のページ付け

Reactor API から返される応答はページ付けされます。 デフォルトのページサイズは 25 個です。ページネーションについて詳しくは、API 応答オブジェクトの `meta.pagination ` セクションにレポートされます。

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

リクエストパスに `page` クエリパラメーターを含めることで、特定のページを取得し、ページのサイズを変更できます。

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
