---
title: 応答トークンと at.js カスタムイベントの使用方法
description: 応答トークンと at.js カスタムイベントを使用して、Target からサードパーティシステムにプロファイル情報を共有する方法を説明します。
role: Developer
level: Experienced
topic: Personalization, Architecture, Development
feature: Implementation
doc-type: technical video
kt: null
author: Daniel Wright
exl-id: d6ce5367-a453-4e6c-8545-9fa676977f04
source-git-commit: fcd2273ba373dc2b3bc59a77f1925cdb7b2ed3ee
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 3%

---

# Adobe Targetでの応答トークンと at.js カスタムイベントの使用

レスポンストークンと `at.js` カスタムイベントを使用すると、[!DNL Target] ーザーからサードパーティシステムにプロファイル情報を共有できます。 カスタムプロファイル属性、地理情報、アクティビティの詳細、組み込みプロファイルなど、[!DNL Target] 訪問者プロファイルの任意のオブジェクトを [!DNL Target] レスポンスに追加できます。このレスポンスでは、カスタムJavaScriptを使用してサードパーティと統合できます。

>[!VIDEO](https://video.tv.adobe.com/v/23253/?quality=12)

## 応答トークンと at.js カスタムイベントの使用方法

1. 必要なデータを [!DNL Target] から決定
1. 設定 – > 応答トークン画面のトグルを切り替えて、必要なデータの応答トークンをオンにします
1. 使用する必要があるイベントリスナーを決定する
1. Adobe Target イベントをリッスンするために必要なJavaScriptを記述し、レスポンストークンを読み取り、統合に必要な操作を実行します
1. 「Load Target」アクションの後に Launch のカスタムコードアクションを使用してイベントリスナーJavaScriptをデプロイするか、設定/「Implementation」画面で at.js のライブラリフッターセクションに追加して、新しい at.js ファイルを保存します。
1. 統合の QA と公開

## その他のリソース

* [Experience Cloud Debugger をAdobe Targetで使用する](../troubleshooting/troubleshoot-with-the-experience-cloud-debugger.md)
* [ 応答トークンのドキュメント ](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=en)
* [Adobe Target でのデータプロバイダーの使用](use-data-providers-to-integrate-third-party-data.md)
