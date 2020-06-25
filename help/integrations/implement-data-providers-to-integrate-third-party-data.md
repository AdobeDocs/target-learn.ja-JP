---
title: データプロバイダーの導入によるサードパーティデータのAdobe Targetへの統合
seo-title: データプロバイダーの導入によるサードパーティデータのAdobe Targetへの統合
description: Adobe Targetのデータプロバイダー機能を使用して、サードパーティのデータプロバイダーからデータを取得し、Targetリクエストに渡す方法の実装の詳細と例です。
audience: developer
difficulty: 5
author: Daniel Wright
doc-type: implement
activity-type: technical-video
translation-type: tm+mt
source-git-commit: 37443ae4c1cdda387c8db0053201d520fa1ec224
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---


# サードパーティ [!UICONTROL のデータをAdobe Targetに統合するための] データプロバイダーの実装

Implementation details and examples of how to use Adobe Target&#39;s [!UICONTROL Data Providers] feature to retrieve data from third-party data providers and pass it in the Target request.

>[!NOTE]
>
>[!UICONTROL Data Providers] requires `at.js` 1.3 or be the

## データプロバイダーの基本コンポーネントの実装

>[!VIDEO](https://video.tv.adobe.com/v/22348/?quality=12)

の基本的なコンポーネントの概要と、コードを適切な順序で取得する方法 `dataProvider` を簡単に説明します。\
このビデオで使用されているコードの実際の例は、次を参照してください。
[https://target.enablementadobe.com/data-providers/simple.html](https://target.enablementadobe.com/data-providers/simple.html)

## サードパーティAPIとの統合

>[!VIDEO](https://video.tv.adobe.com/v/22345/)

より現実的な例で、weather APIの統合を参照してください。\
このビデオで使用されているコードの実際の例は、次を参照してください。
[https://target.enablementadobe.com/data-providers/3rdparty.html](https://target.enablementadobe.com/data-providers/3rdparty.html)

## 複数のプロバイダーとの統合

>[!VIDEO](https://video.tv.adobe.com/v/22346/)

複数のプロバイダーのデータをグローバル [!DNL Target] リクエストに組み込む方法。\
このビデオで使用されているコードの実際の例は、次を参照してください。
[https://target.enablementadobe.com/data-providers/combined.html](https://target.enablementadobe.com/data-providers/combined.html)

## ページロードへの影響を最小化

>[!VIDEO](https://video.tv.adobe.com/v/22347/)

データをセッションストレージオブジェクトに格納することで、ページ読み込み時間への影響を最小限に抑えます。 または、プレフィックスを使用して値をプロファイルパラメーターとして渡し、セッションの最初の `profile.`[!DNL Target] リクエストで渡すこともできます。 ただし、リクエスト1回につき50個のプロファイルパラメーターを渡すことに制限されます。

このビデオで使用されているコードの実際の例は、次を参照してください。 [https://target.enablementadobe.com/data-providers/reducedCalls.html](https://target.enablementadobe.com/data-providers/reducedCalls.html)

## サポート資料

* [Adobe Targetでのデータプロバイダーの使用](use-data-providers-to-integrate-third-party-data.md)

* [データプロバイダードキュメント](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/targetgobalsettings.html#data-providers)