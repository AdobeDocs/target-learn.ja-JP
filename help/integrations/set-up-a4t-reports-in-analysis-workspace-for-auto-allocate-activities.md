---
title: ' [!DNL Analysis Workspace]  での[!UICONTROL 自動配分]アクティビティ用 A4T レポートの設定方法'
description: ' [!DNL Analysis Workspace]  での[!UICONTROL 自動配分]アクティビティの実行時に予期した結果が得られるように、A4T レポートを設定する方法について説明します。'
role: User
level: Intermediate
topic: Personalization, Integrations
feature: Analytics for Target (A4T), Auto-Target, Integrations
doc-type: tutorial
kt: null
exl-id: 7d53adce-cc05-4754-9369-9cc1763a9450
source-git-commit: ef9e4667ab6e264f0dd324bfd8a7a14783952078
workflow-type: tm+mt
source-wordcount: '1290'
ht-degree: 51%

---

# [!DNL Analysis Workspace] での [!DNL Auto-Allocate] アクティビティ用 A4T レポートの設定

An [!DNL Auto-Allocate] 「 」アクティビティでは、2 つ以上のエクスペリエンスの中から勝者を特定し、テストの実施と学習が続く間に、自動的にその勝者にトラフィックを配分し直します。 [!UICONTROL 自動配分]のための [!UICONTROL Analytics for Target]（A4T）統合により、[!DNL Adobe Analytics] でレポートデータを確認できるほか、[!DNL Analytics] で定義されたカスタムイベントや指標を最適化することもできます。

豊富な分析機能は [!DNL Adobe Analytics] [!DNL Analysis Workspace]（デフォルトに対するいくつかの変更） **[!UICONTROL Analytics for Target]** パネルが正しく解釈するために必要になる場合があります [!DNL Auto-Allocate] アクティビティ、 [最適化指標の基準](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-at-aa.html?lang=ja#supported){target=_blank}.

このチュートリアルでは、[!DNL Analysis Workspace] の [!DNL Auto-Allocate] アクティビティ分析に推奨される変更について、順番に説明します。主な概念は次のとおりです。

* [!UICONTROL 訪問者数]は、[!DNL Auto-Allocate] アクティビティの正規化指標として常に使用する必要があります。
* 指標が [!DNL Adobe Analytics] 指標の場合、コンバージョン率の計算は、アクティビティの設定時に定義した最適化条件のタイプに応じて異なります。
   * 「個別訪問者コンバージョン率を最大化」コンバージョン率の分子は、個別訪問者の数です *指標の正の値を持つ*.
   * このメソッドでは、 [!DNL Target] UI
* 「訪問者ごとの指標値を最大化」コンバージョン率の分子は、 [!DNL Adobe Analytics]. この指標は、デフォルトで [!DNL Analytics for Target] パネル内 [!DNL Analysis Workspace].
   * つまり、コンバージョンに至った訪問者の数を最大化します（「訪問者あたり 1 回カウント」）。
   * この方法では、レポートで、 [!DNL Target] UI
* 最適化指標が [!DNL Target] 定義済みのコンバージョン指標である場合、[!DNL Analysis Workspace] のデフォルトの **[!UICONTROL Analytics for Target]** パネルでパネルの設定を処理します。
* [!DNL Target Standard/Premium] 23.3.1 リリース（2023年3月30日（PT））より前に作成したすべての[!UICONTROL 自動配分]アクティビティでは、[!DNL Analytics Workspace] と [!DNL Target] の[!UICONTROL 信頼性]に同じ値が表示されます。

  すべて [!UICONTROL 自動配分] 2023 年 3 月 30 日以降に作成されたアクティビティ。 [!DNL Analysis Workspace] 反映しない [～が使うより保守的な統計 [!UICONTROL 自動配分]](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=ja#section_98388996F0584E15BF3A99C57EEB7629){target=_blank} 次のアクティビティがある場合は *両方* 次の条件を満たす場合：

   * レポートソースとしての [!DNL Analytics]（A4T）
   * [!DNL Analytics] 最適化指標

  信頼性指標は、A4T パネルから削除する必要があります。 代わりに、[!DNL Target] レポートでこれらの値を参照してください。

## [!DNL Analysis Workspace] での [!DNL Auto-Allocate] パネル用の A4T の作成

用の A4T パネルを作成するには、以下を実行します。 [!DNL Auto-Allocate] レポートは **[!UICONTROL Analytics for Target]** パネル内 [!DNL Analysis Workspace]、以下に示すように。 次に、以下の選択を行います。

1. 「 」アクティビティを追加します。
1. **[!UICONTROL コントロールエクスペリエンス]**：任意のエクスペリエンスを選択できます。
1. **[!UICONTROL 指標の標準化]**:「訪問者」を選択します（訪問者はデフォルトで A4T パネルに含まれます）。 [!DNL Auto-Allocate] では、ユニーク訪問者ごとのコンバージョン率が常に正規化されます。
1. **[!UICONTROL 成功指標]**：アクティビティの作成時に使用した指標と同じ指標を選択します。これが [!DNL Target] 定義済みのコンバージョン指標である場合、「**アクティビティのコンバージョン**」を選択します。それ以外の場合は、使用した [!DNL Adobe Analytics] 指標を選択します。

[!DNL Auto-Allocate] アクティビティの ![[!UICONTROL Analytics for Target] パネルの設定。](assets/AAFigure1.png)

*図 1：[!DNL Auto-Allocate] アクティビティの [!UICONTROL Analytics for Target] パネルの設定。*

[!DNL Adobe Target] のレポート画面からリンクをクリックすると、事前定義済みの **[!UICONTROL Analytics for Target]** パネルにアクセスすることもできます。

## 「訪問者あたりの指標値の最大化」の最適化条件を使用した、[!DNL Target] の[!UICONTROL コンバージョン]指標または [!DNL Analytics] 指標

目標指標が次のいずれかの場合：

* Target コンバージョン指標
* Analytics 指標と最適化基準「訪問者あたりの指標値を最大化」

デフォルトの A4T パネルでは、レポートが自動的に設定されます。

このパネルの例の 1 つは、[!UICONTROL 収益]指標に対して示されます。ここでは、アクティビティ作成時に最適化条件として「訪問者あたりの指標値の最大化」が選択されています。前述したように、[!DNL Auto-Allocate] では、**[!UICONTROL Analytics for Target]** パネルで使用する計算と比較して、より保守的な信頼性の計算を使用します。アドビでは、A4T パネルから信頼性指標に加え、関連する下限および上限の上昇率指標も削除することをお勧めします。代わりに、信頼値を [!DNL Target] レポート。

>[!NOTE]
>
>A4T レポートの信頼性の値は、 [!DNL Target] レポートを作成する際に、 [!UICONTROL 自動配分] アクティビティ。


![[!UICONTROL Analytics for Target - 自動配分レポート]パネル](assets/AAFigure2.png)

*図 2：[!DNL Analytics] 指標の「訪問者あたりの指標値の最大化」の最適化条件を使用した [!DNL Auto-Allocate] アクティビティの推奨レポート。これらのタイプの指標および [!DNL Target] 定義済みのコンバージョン指標については、[!DNL Analysis Workspace] のデフォルトの&#x200B;**[!UICONTROL Analytics for Target]**パネルを使用できます。*

## [!DNL Analytics] 「個別訪問者数を最大化」最適化条件を使用する指標

最適化基準の「コンバージョン率で個別訪問者を最大化」は、指標値が正の訪問者数を指します。 例えば、コンバージョン率が売上高と定義されている場合、「コンバージョン率を使用して個別訪問者を最大化」の基準は、売上高が 0 より大きい個別訪問者の数に関して最適化されます。 つまり、この条件は、売上高自体の価値ではなく、売上高を生み出す訪問者の数を最大化します。

>[!NOTE]
>
>ここで参照しているコンバージョン率は、クリック数、インプレッション数など、注文以外のアクションを指す場合があります。 この場合、条件は引き続き、クリックまたはページを表示した訪問者の数を最大化することになります。

The [!DNL Analytics for Target] パネル内 [!DNL Analysis Workspace] この最適化条件を [!DNL Adobe Analytics] 指標。

この最適化基準を使用すると、成功指標はコンバージョン指標が正の数であった個別訪問者の数になります。 したがって、この値を表示するには、指標に正の値を持つヒットをフィルタリングする新しいセグメントを作成する必要があります。

次のように、このセグメントを作成します。

1. [!DNL Analysis Workspace] ツールバーで&#x200B;**[!UICONTROL コンポーネント]**／**[!UICONTROL セグメントを作成]**&#x200B;オプションを選択します。
1. アクティビティの作成時に使用する指標を、左側のパネルからセグメントの「**[!UICONTROL 定義]**」ボックスにドラッグします。
1. 0 の数値&#x200B;**より大きい**&#x200B;指標の値を選択します。
1. **[!UICONTROL 含める]**&#x200B;ドロップダウンから「**[!UICONTROL 訪問者数]**」を選択します。
1. セグメントに適切な名前を付けます。

セグメントの作成例を次の図に示します。ここで、成功指標は [!UICONTROL 正の売上高を持つ訪問者].

[!DNL Analysis Workspace]](assets/AAFigure3.png) での![[!UICONTROL 収益がプラスの訪問者数]セグメント

*図 3：「[!UICONTROL ユニーク訪問者のコンバージョン率の最大化]」に等しい最適化条件を使用した、[!DNL Adobe Analytics] 指標のセグメント作成。この例では、指標は[!UICONTROL 収益]であり、最適化の目標は収益がプラスの訪問者数を最大化することです。*

適切なセグメントが作成されたら、デフォルトの  **[!UICONTROL Analytics for Target]** パネル内 [!DNL Analysis Workspace] をクリックして、最適化基準の値を表示します。 これをおこなうには、次の手順を実行します。

1. 既存の[!UICONTROL 訪問者数]指標列の横に 2 番目の&#x200B;**ユニーク訪問者**&#x200B;指標を追加します。
2. 新しく作成したセグメントを最初の列の下にドラッグして、図 4 のようなパネルを生成します。列の値の違いに注意してください。正の売上高を持つ個別訪問者の数は、（下図のように）各エクスペリエンスに割り当てられた個別訪問者の合計数の一部である必要があります。

   ![Figure4.png](assets/AAFigure4.png)

   *図 4：新しく作成したセグメントによる[!UICONTROL ユニーク訪問者]のフィルタリング*

3. コンバージョン率は、1 列目と 2 列目の両方をハイライト表示して右クリックし、**[!UICONTROL 選択から指標を作成]**／**[!UICONTROL 除算]**&#x200B;を選択することで[すばやく計算](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/components/calculated-metrics/quick-calculated-metrics-in-analysis-workspace.html?lang=ja)できます。

   次の図に示すように、デフォルトのコンバージョン率を削除し、この新しい計算指標に置き換える必要があります。図のように、新しく作成した計算指標を編集して、**[!UICONTROL 形式]**／**[!UICONTROL パーセント]**&#x200B;として小数点以下 2 桁まで表示する必要がある場合があります。

   ![Figure5.png](assets/AAFigure5.png)

   *図 5：2 値化された収益コンバージョン指標のコンバージョン率を示す最後の[!UICONTROL 自動配分]パネル*

## まとめ

このチュートリアルの手順では、[!UICONTROL 自動配分]レポートデータを表示するように [!DNL Analysis Workspace] を正しく設定する方法を説明しました。

まとめると、次のようになります。

* 指標が [!DNL Target] 定義済みのコンバージョン指標である場合や、「訪問者あたりの指標値の最大化」という最適化条件を備えた [!DNL Adobe Analytics] 指標である場合は、正規化指標として訪問者数を使用して設定されたデフォルトのワークスペースパネルを使用する必要があります。
* 指標が [!DNL Adobe Analytics] 指標と最適化基準「個別訪問者コンバージョン率を最大化」を使用する場合は、合計訪問者に対して正の指標値を持つ訪問者の割合を決定する必要があります。 これは、 [!UICONTROL 個別訪問者] を選択します。
