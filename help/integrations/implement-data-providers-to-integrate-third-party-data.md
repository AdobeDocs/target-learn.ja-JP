---
title: データプロバイダーを実装してサードパーティのデータを統合する方法
description: このチュートリアルでは、Adobe Targetのデータプロバイダー機能を使用して、サードパーティのデータプロバイダーからデータを取得し、Target リクエストに渡す方法の実装詳細と例を示します。
role: Developer
level: Experienced
topic: Personalization, Integrations
feature: Implementation, Integrations, APIs/SDKs
doc-type: technical video
kt: null
author: Daniel Wright
exl-id: fcf6d1a8-e2a7-41ce-9c1c-02985b7afb5a
source-git-commit: 80208b3ecbc0d627d2afe72f882e91c9800d2726
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# [!UICONTROL Data Providers] を実装してサードパーティのデータをAdobe Targetに統合する

実装の詳細と、Adobe Targetの [!UICONTROL Data Providers] 機能を使用して、サードパーティのデータプロバイダーからデータを取得し、Target リクエストに渡す方法の例です。

>[!NOTE]
>
>[!UICONTROL Data Providers] には `at.js` 1.3 以上が必要です

## データプロバイダーの基本コンポーネントの実装

>[!VIDEO](https://video.tv.adobe.com/v/34061/?quality=12&captions=jpn)

`dataProvider` の基本的なコンポーネントの概要と、コードを正しい順序で取得する方法です。\
ビデオで使用されているコードの実際の例については、次を参照してください。
[https://target.enablementadobe.com/data-providers/simple.html](https://target.enablementadobe.com/data-providers/simple.html)

## サードパーティ API との統合

>[!VIDEO](https://video.tv.adobe.com/v/34062?captions=jpn)

より現実的な例としては、天気 API の統合があります。\
ビデオで使用されているコードの実際の例については、次を参照してください。
[https://target.enablementadobe.com/data-providers/3rdparty.html](https://target.enablementadobe.com/data-providers/3rdparty.html)

## 複数のプロバイダーとの統合

>[!VIDEO](https://video.tv.adobe.com/v/36804?captions=jpn)

複数のプロバイダーからのデータをグローバル [!DNL Target] リクエストに組み込む方法。\
ビデオで使用されているコードの実際の例については、次を参照してください。
[https://target.enablementadobe.com/data-providers/combined.html](https://target.enablementadobe.com/data-providers/combined.html)

## ページ読み込みの影響を最小化

>[!VIDEO](https://video.tv.adobe.com/v/36805?captions=jpn)

セッションストレージオブジェクトにデータを保存することで、ページ読み込み時間への影響を最小限に抑えます。 または、`profile.` プレフィックスを使用して、プロファイルパラメーターとして値を渡し、セッションの最初の [!DNL Target] リクエストで渡すこともできます。 ただし、リクエストごとに 50 個のプロファイルパラメーターを渡すように制限されます。

ビデオで使用されているコードの実際の例については、次を参照してください。[https://target.enablementadobe.com/data-providers/reducedCalls.html](https://target.enablementadobe.com/data-providers/reducedCalls.html)

## サポート資料

* [Adobe Targetでのデータプロバイダーの使用 ](use-data-providers-to-integrate-third-party-data.md)
