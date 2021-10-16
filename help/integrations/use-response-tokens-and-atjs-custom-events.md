---
title: レスポンストークンと at.js カスタムイベントの使用方法
description: レスポンストークンと at.js カスタムイベントを使用して、Target のプロファイル情報をサードパーティのシステムと共有する方法を説明します。
role: Developer
level: Experienced
topic: Personalization, Architecture, Development
feature: Implementation
doc-type: technical video
kt: null
author: Daniel Wright
exl-id: d6ce5367-a453-4e6c-8545-9fa676977f04
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 3%

---

# Adobe Targetでのレスポンストークンと at.js カスタムイベントの使用

レスポンストークンと `at.js` カスタムイベントを使用すると、[!DNL Target] のプロファイル情報をサードパーティのシステムと共有できます。 カスタムプロファイル属性、地理情報、アクティビティの詳細、組み込みプロファイルなど、 [!DNL Target] 訪問者プロファイル内の任意のオブジェクトを [!DNL Target] 応答に追加して、カスタム JavaScript を使用してサードパーティと統合できます。

>[!VIDEO](https://video.tv.adobe.com/v/23253/?quality=12)

## レスポンストークンと at.js カスタムイベントの使用方法

1. [!DNL Target] から必要なデータを決定
1. 設定/レスポンストークン画面で切り替えを反転して、必要なデータのレスポンストークンをオンにします。
1. 使用する必要があるイベントリスナーを決定します。
1. Adobe Targetイベントをリッスンし、レスポンストークンを読み取り、統合に必要な操作を行うために必要な JavaScript を記述します
1. 「Load Target」アクションの後に、Launch のカスタムコードアクションを使用してイベントリスナー JavaScript をデプロイするか、セットアップ/実装画面で at.js の「ライブラリフッター」セクションに追加して、新しい at.js ファイルを保存します。
1. 統合の QA および公開

## その他のリソース

* [Adobe TargetでのExperience Cloud Debuggerの使用](../troubleshooting/troubleshoot-with-the-experience-cloud-debugger.md)
* [レスポンストークンのドキュメント](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=en)
* [at.js カスタムイベントのドキュメント](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/atjs-custom-events.html?lang=en)
* [Adobe Target でのデータプロバイダーの使用](use-data-providers-to-integrate-third-party-data.md)
