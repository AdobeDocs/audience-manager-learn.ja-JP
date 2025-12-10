---
title: アルゴリズム（類似）モデルを使用して ROAS を増やす
description: Audience Managerの類似モデリングの利点は、セカンドパーティおよびサードパーティのデータソースを使用する、まったく新しい質の高いユーザーセットに対して、ベースラインオーディエンスを拡大しようとするときです。 このチュートリアルでは、このデータからモデルを作成する手順を説明します。
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
source-git-commit: d47848370e7bf7617f2b706041c911161a6479cd
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 0%

---

# Audience Managerのアルゴリズム（類似）モデルを使用した ROAS の向上 {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Managerの類似 [!UICONTROL Modeling] ールの真の威力は、セカンドパーティおよびサードパーティのデータソースから、質の高い新しいユーザーセットに対してベースラインオーディエンスを拡大しようとするときにあります。 このチュートリアルでは、このデータからモデルを作成するために必要な手順を説明します。

## Audience Marketplaceからのサードパーティまたはサードパーティのデータストリームを有効にする {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

セカンドパーティのデータとサードパーティのデータを類似モデルで使用するには、まずこのデータをAudience Manager インターフェイスに有効にする必要があります。 Adobeには、多数のセカンドパーティおよびサードパーティのデータプロバイダーがあり、ここから選択できます。 これらは、AAMのセルフサービスインターフェイスでAudience Marketplaceから使用できます。 Audience Marketplaceに移動し、可能性を参照します。 次のビデオでは、これを行う方法を説明します。これには、無料の「try before you buy」ストリームを有効にして、データプロバイダーの価格にコミットする前に、組織にとって最も役立つデータをロックインできるようにする方法が含まれます。

また、使用するデータプロバイダーを調べて決定するのに役立つ、優れたリソースが [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/) です。

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 理想的なユーザー（コンバージョン）特性やセグメントの特定または作成 {#identify-create-an-ideal-user-conversion-trait-or-segment}

サイトでユーザーに何を実行させるつもりですか？ コンバージョンイベントとは何ですか？ もちろん、この質問に対する回答は、サイトのタイプ/業種や組織の目標に応じて様々です。 いずれにせよ、AAMでは、これらの条件を満たした訪問者に対して特性を作成するのが一般的です。

以下のビデオでは、コンバージョン特性の作成方法を説明します。このチュートリアルを続行して類似モデルを作成する際に、コンバージョン特性を適切に設定する必要があります。

また、Adobe Analytics イベントを使用して特性を作成する場合は、必要以上のユーザーを特性に集めないように、注意する必要がある重要な問題があります。 詳しくは、次のビデオをご覧ください。 :）

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 上記のビデオでは、以下に示す例は、Adobe Analyticsを持っていることを前提としています。 明らかに、これは当てはまらない可能性があります。 Google Analytics（GA）がある場合、AAMにデータを送信するために使用できるモジュールがあります（[ ドキュメント ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html) を参照）。サイト上のコンバージョンアクティビティが GA によってAAMに送信される場合は、そこからコンバージョン特性を作成できます。 別の分析ソリューションがある（または分析ソリューションがない）場合でも、DIL コードや `submit` 関数などを介してAAMにデータを送信できます。 （[ ドキュメント ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)）を参照してください。 次に、サイトでコンバージョンアクティビティが実行されたときに送信されたデータに基づいて、コンバージョン特性を作成します。

## セカンドパーティまたはサードパーティのデータからの類似モデルの作成 {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

上記の手順を完了すると、アルゴリズム（類似）モデルを作成する準備が整います。 モデルを設定する際には、コンバージョン特性を基本特性（複製する主な訪問者）として使用し、有効なサードパーティデータストリームを取り込み元の人物のプールとして使用します。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要なベストプラクティス {#an-important-best-practice}

Audience Managerでアルゴリズムモデルを作成する場合、明らかに、モデルをできるだけ効果的なものにします。 モデルはベースとなる特性/セグメントのメンバーが属するすべての特性を考慮しているので、すべての人が特性/セグメントに含まれている場合、モデルは役に立ちません。 したがって、非常に一般的な特性（サイトにアクセスした人、サイトから広告を受け取った人など）がある場合は、それらが属するデータソースがモデルのデータソースに含まれていないことを確認します。 この記事のユースケースでは、新しいルックエイクのためにサードパーティデータを調べることに重点を置いているので、そうする可能性は低いですが、とにかく言及する価値があり、すべてのアルゴリズムモデルに適用されます。

## [!UICONTROL Algorithmic Trait] の作成 {#creating-an-algorithmic-trait}

次に、モデルの結果を使用できるように、[!UICONTROL Algorithmic Trait] を作成する必要があります。 特性を作成しないと、モデルは役に立ちません。 モデルを実行したら、必ず特性ダイアログに移動して [!UICONTROL Algorithmic Trait] を作成してください。 次のビデオでは、順を追って説明し、いくつかのヒントを示します。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## モデルデータからセグメントを作成して DSP に送信する {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

[!UICONTROL Algorithmic Trait] ータを作成したら、新しいセグメントを作成して配置し、データをアクティブ化できます（特性をアクティブ化することはできませんが、[!UICONTROL Algorithmic Trait] ータを含んだ新しい単一特性セグメントを作成して、セグメントをアクティブ化（使用）できます）。

このアルゴリズム特性からセグメントを作成すると、サイトで既にコンバージョンを行った人物のように見える潜在的なクライアントのオーディエンスを得ることができます。 これで、このセグメントをAudience Managerの任意のDSPの宛先にマッピングできます。 マーケティングのターゲットを、通常の一般公開よりもサイト上でコンバージョンする可能性が高いルックエイクに設定し、広告費用対効果を高めることができます。
