---
title: アルゴリズム型（類似）モデルを利用してROASを向上
description: Audience Managerの類似モデリングの真の力は、2ndおよび3rd パーティのデータソースから得られる質の高い新しいユーザーセットに対して、ベースラインオーディエンスを拡大しようとすることで得られます。 このチュートリアルでは、このデータからモデルを作成する手順を説明します。
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: User, Developer, Admin, Leader
level: Intermediate
exl-id: 6626ae11-8709-4302-9e03-0d55878d2409
TQID: https://experienceleague.adobe.com/rQ-djjfEOZDjR3IdvvJnO1Hb2tu6IUhz0uFhp-xuZq8
product_v2:
  - id: df80eeb1-8d72-467e-b0df-9d51c7d3a0a1
feature_v2:
  - id: c814092e-2730-45e8-a12d-e084529f52cb
  - id: d8f86c1e-15ad-457f-9d6f-5e756573fad4
subfeature_v2:
  - id: d921db59-bd4a-43dc-97e6-4ff4611f1ae8
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: 3152e8fc51e0e06c90c17dce0aa203a27995e88d
workflow-type: tm+mt
source-wordcount: 943
ht-degree: 0%

---

# Audience Managerのアルゴリズムモデル（類似）を利用したROASの向上 {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Managerの類似[!UICONTROL Modeling]機能の真の力は、2nd パーティデータソースとサードパーティデータソースから得られる質の高い、まったく新しいユーザーセットに対して、ベースラインオーディエンスを拡大しようとしたときに発揮されます。 このチュートリアルでは、このデータからモデルを作成するために必要な手順を説明します。

## Audience Marketplaceから2nd パーティデータまたはサードパーティデータのストリームを有効にする {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

類似モデルで2nd パーティデータとサードパーティデータを使用するには、まずAudience Managerのインターフェイスでこのデータを有効にする必要があります。 Adobeには、選択できる多数の2nd パーティデータプロバイダーとサードパーティデータプロバイダーがあります。 これらは、Audience Marketplaceを介したAAMのセルフサービスインターフェイスで使用できます。 Audience Marketplaceに移動し、その可能性を参照します。 次のビデオでは、無料の「購入する前に試す」ストリームを有効にする方法など、この方法を説明します。これにより、データプロバイダーの価格に確定する前に、組織にとって最も有用なデータをロックインできます。

また、どのデータプロバイダーを使用するかを調査し、決定するのに役立つ優れたリソースは[[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/)です。

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 理想的なユーザー（コンバージョン）特性またはセグメントの特定または作成 {#identify-create-an-ideal-user-conversion-trait-or-segment}

サイトでのオーディエンスの行動を促進していますか？ コンバージョンイベントとは？ もちろん、サイトの種類や業種、組織の目標に応じて、この質問に対する答えはたくさんあります。 いずれにせよ、AAMでは、これらの条件を満たした訪問者の特性を作成するのが一般的です。

以下のビデオでは、コンバージョン特性を作成する方法を説明します。このチュートリアルを続行して類似モデルを作成する際に、この特性を配置しておきます。

また、Adobe Analyticsのイベントを使用して特性を作成する場合は、特性に追加するユーザーを収集しないようにする必要がある大きな注意点があります。 次のビデオで大きな明らかをご覧ください。 :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**メモ：**&#x200B;上記のビデオでは、Adobe Analyticsを使用していることを前提としています。 明らかに、そんなことはありません。 Google Analytics（GA）をお持ちの場合は、AAMにデータを送信するために使用できるモジュールがあります（[&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html)を参照）。サイト上のコンバージョンアクティビティがGAまでにAAMに送信される場合は、そのモジュールからコンバージョン特性を作成できます。 別の分析ソリューションがある場合（または分析ソリューションがない場合）でも、DIL コードや`submit`関数などを介してAAMにデータを送信できます（[&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)を参照）。 そして、サイトでコンバージョンアクティビティが実行されたときに送信されたデータに基づいて、コンバージョン特性を作成します。

## 2nd パーティデータまたはサードパーティデータから類似モデルを作成する {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

上記の手順を完了した後、アルゴリズム（類似）モデルを作成する準備が整いました。 モデルを設定する際には、コンバージョン特性を基本特性（複製したい主要な訪問者）として使用し、有効なサードパーティデータストリームを人物のプールとして使用します。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要なベストプラクティス {#an-important-best-practice}

Audience Managerでアルゴリズムモデルを作成する際には、可能な限り効果的なモデルにしたいと考えています。 モデルは、基本の特性/セグメントのメンバーが属するすべての特性を考慮するため、すべての人物が特性/セグメント内にある場合はモデルを助けません。 したがって、超汎用的な特性がある場合（サイトにアクセスした全員、または広告を受け取った全員など）、それらが属するデータソースがモデルのデータソースに含まれていないことを確認してください。 この記事のユースケースでは、新しい類似（look-alike）のためにサードパーティデータの確認に注力しているため、おそらくそうではないかもしれませんが、それでも言及する価値があり、すべてのアルゴリズムモデルに適用されます。

## [!UICONTROL Algorithmic Trait]を作成 {#creating-an-algorithmic-trait}

次に、モデルの結果を使用できるように[!UICONTROL Algorithmic Trait]を作成する必要があります。 特性を作成しなければ、モデルは役に立ちません。 モデルの実行後、特性ダイアログに移動して[!UICONTROL Algorithmic Trait]を作成してください。 次のビデオでは、いくつかのヒントを紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## モデルデータからセグメントを作成し、DSPに送信します {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

[!UICONTROL Algorithmic Trait]を作成したら、データをアクティブ化するために新しいセグメントを作成できます（特性をアクティブ化することはできませんが、[!UICONTROL Algorithmic Trait]を含む新しい1つの特性セグメントを作成して、セグメントをアクティブ化（使用）できます）。

このアルゴリズム特性からセグメントを作成すると、すでにサイトでコンバージョンした人のように見える潜在的な顧客のオーディエンスが表示されます。 これで、このセグメントをAudience Managerの任意のDSP宛先にマッピングできます。 これにより、自社サイトでコンバージョンする可能性が高い類似オーディエンスをターゲットにして、広告費用対効果を向上させることができます。
