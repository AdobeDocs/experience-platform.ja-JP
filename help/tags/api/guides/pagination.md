---
title: Reactor API での応答のページ分割
description: Reactor API でリソースをリストする際に結果のページ分割をおこなう方法を説明します。
exl-id: bccb6e78-4ac8-4786-b398-6e55109d99dd
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 100%

---

# Reactor API での応答のページ分割

Reactor API から返される応答は、ページ分割されます。デフォルトのページサイズは 25 エレメントです。ページネーションに関する詳細は、API 応答オブジェクトの `meta.pagination `セクションに記載されています。

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
