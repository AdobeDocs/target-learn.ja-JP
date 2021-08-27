---
title: レスポンストークンとat.jsカスタムイベントの使用方法
description: レスポンストークンとat.jsカスタムイベントを使用して、Targetからサードパーティシステムにプロファイル情報を共有する方法を説明します。
role: Developer
level: Experienced
topic: Personalization, Architecture, Development
feature: Implementation
doc-type: technical video
kt: null
thumbnail: null
author: Daniel Wright
exl-id: d6ce5367-a453-4e6c-8545-9fa676977f04
source-git-commit: d1517f0763290eb61a9e4eef4f2eb215a9cdd667
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 3%

---

# Adobe Targetでのレスポンストークンとat.jsカスタムイベントの使用

レスポンストークンと`at.js`カスタムイベントを使用すると、[!DNL Target]のプロファイル情報をサードパーティシステムと共有できます。 カスタムプロファイル属性、地理情報、アクティビティの詳細、組み込みプロファイルなど、[!DNL Target]訪問者プロファイルの任意のオブジェクトを[!DNL Target]応答に追加して、カスタムJavaScriptを使用してサードパーティと統合できます。

>[!VIDEO](https://video.tv.adobe.com/v/23253/?quality=12)

## レスポンストークンとat.jsカスタムイベントの使用方法

1. [!DNL Target]から必要なデータを特定する
1. 設定/レスポンストークン画面で切り替えを反転して、必要なデータのレスポンストークンをオンにします
1. 使用する必要があるイベントリスナーを決定します。
1. Adobe Targetイベントをリッスンし、レスポンストークンを読み取り、統合に必要な操作を行うために必要なJavaScriptを記述します
1. 「Load Target」アクションの後に、Launchのカスタムコードアクションを使用してイベントリスナーJavaScriptをデプロイするか、セットアップ/実装画面でat.jsの「ライブラリフッター」セクションに追加して、新しいat.jsファイルを保存します。
1. 統合のQAと公開

## その他のリソース

* [Adobe TargetでのExperience Cloud Debuggerの使用](../troubleshooting/troubleshoot-with-the-experience-cloud-debugger.md)
* [レスポンストークンのドキュメント](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=en)
* [at.jsカスタムイベントのドキュメント](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/atjs-custom-events.html?lang=en)
* [Adobe Target でのデータプロバイダーの使用](use-data-providers-to-integrate-third-party-data.md)
