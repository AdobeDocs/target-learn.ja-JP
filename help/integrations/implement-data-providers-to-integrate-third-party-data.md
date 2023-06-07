---
title: データプロバイダーを実装してサードパーティデータを統合する方法
description: このチュートリアルでは、Adobe Targetのデータプロバイダー機能を使用してサードパーティのデータプロバイダーからデータを取得し、Target リクエストで渡す方法の実装の詳細と例を示します。
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
source-wordcount: '285'
ht-degree: 0%

---

# 実装方法 [!UICONTROL データプロバイダー] サードパーティデータをAdobe Targetに統合するには

実装の詳細とAdobe Targetの使用方法の例 [!UICONTROL データプロバイダー] サードパーティのデータプロバイダーからデータを取得し、Target リクエストに渡す機能が追加されました。

>[!NOTE]
>
>[!UICONTROL データプロバイダー] が必要です `at.js` 1.3 以降

## データプロバイダーの基本コンポーネントの実装

>[!VIDEO](https://video.tv.adobe.com/v/22348/?quality=12)

の基本的なコンポーネントの概要 `dataProvider` 適切な順序でコードを取得する方法も\
ビデオで使用されるコードの実際の例を次に示します。
[https://target.enablementadobe.com/data-providers/simple.html](https://target.enablementadobe.com/data-providers/simple.html)

## サードパーティ API との統合

>[!VIDEO](https://video.tv.adobe.com/v/22345/)

より現実的な例です。天気 API の統合を参照してください。\
ビデオで使用されるコードの実際の例を次に示します。
[https://target.enablementadobe.com/data-providers/3rdparty.html](https://target.enablementadobe.com/data-providers/3rdparty.html)

## 複数のプロバイダーとの統合

>[!VIDEO](https://video.tv.adobe.com/v/22346/)

複数のプロバイダーのデータをグローバルに組み込む方法 [!DNL Target] リクエスト。\
ビデオで使用されるコードの実際の例を次に示します。
[https://target.enablementadobe.com/data-providers/combined.html](https://target.enablementadobe.com/data-providers/combined.html)

## ページ読み込みの影響を最小化

>[!VIDEO](https://video.tv.adobe.com/v/22347/)

データをセッションストレージオブジェクトに保存することで、ページ読み込み時間への影響を最小限に抑えます。 または、 `profile.` というプレフィックスを付け、最初の [!DNL Target] セッションのリクエスト。 ただし、リクエストごとに 50 個のプロファイルパラメーターを渡すように制限されます。

ビデオで使用されるコードの実際の例を次に示します。 [https://target.enablementadobe.com/data-providers/reducedCalls.html](https://target.enablementadobe.com/data-providers/reducedCalls.html)

## サポート資料

* [Adobe Targetでのデータプロバイダーの使用](use-data-providers-to-integrate-third-party-data.md)
