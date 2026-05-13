---
title: 応答トークンとat.js カスタムイベントの使用方法
description: 応答トークンとat.js カスタムイベントを使用して、プロファイル情報をTargetからサードパーティシステムに共有する方法を説明します。
role: Developer
level: Experienced
topic: Personalization, Architecture, Development
feature: Implementation
doc-type: technical video
kt: null
author: Daniel Wright
exl-id: d6ce5367-a453-4e6c-8545-9fa676977f04
TQID: https://experienceleague.adobe.com/gJfFi9mC3iKY8pEdvE1Tuk7Mk2rUOdTKtv67vXQwkO8
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: c0b4abf2d4ead4d58a8db6e8970857b7b50dbe5c
workflow-type: tm+mt
source-wordcount: 230
ht-degree: 3%

---

# Adobe Targetでの応答トークンとat.js カスタムイベントの使用

応答トークンと`at.js` カスタムイベントを使用すると、[!DNL Target]からサードパーティシステムにプロファイル情報を共有できます。 カスタムプロファイル属性、地理情報、アクティビティの詳細、組み込みプロファイルなど、[!DNL Target]訪問者プロファイル内のオブジェクトを[!DNL Target]応答に追加すると、カスタム JavaScriptを使用してサードパーティと統合できます。

>[!VIDEO](https://video.tv.adobe.com/v/34066/?captions=jpn&quality=12)

## 応答トークンとat.js カスタムイベントの使用方法

1. [!DNL Target]から必要なデータを決定
1. 設定/レスポンストークン画面のトグルを反転して、必要なデータのレスポンストークンをオンにします
1. 使用するイベントリスナーを決定する
1. Adobe Target イベントをリッスンするために必要なJavaScriptを書き、応答トークンを読み、統合に必要なことを行います
1. 「ターゲットを読み込む」アクションの後、Launchのカスタムコードアクションを使用してイベントリスナーJavaScriptをデプロイするか、セットアップ/実装画面のat.jsの「ライブラリフッター」セクションに追加して、新しいat.js ファイルを保存します
1. 統合のQAと公開

## その他のリソース

* [Adobe TargetでのExperience Cloud Debuggerの使用](../troubleshooting/troubleshoot-with-the-experience-cloud-debugger.md)
* [応答トークンのドキュメント](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=ja)
* [Adobe Target でのデータプロバイダーの使用](use-data-providers-to-integrate-third-party-data.md)
