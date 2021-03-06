---
title: 配色の検出 - Computer Vision
titleSuffix: Azure Cognitive Services
description: Computer Vision API を使用したイメージの配色の検出に関連する概念。
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 90eacdb120603fdb68c0f6c3de3457964ec5fd75
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/18/2018
ms.locfileid: "53579364"
---
# <a name="detect-color-schemes-in-images"></a>イメージ内の配色を検出する

Computer Vision では、イメージから色を抽出します。 次に、前景のドミナント カラー、背景のドミナント カラー、イメージ全体のドミナント カラーの 3 種類のコンテキストで色が分析されます。 これらは 12 色のドミナント アクセント カラーにグループ分けされます。 そのアクセント カラーは、黒、青、茶色、灰色、緑、オレンジ、ピンク、紫、赤、青緑、白、黄色です。 Computer Vision が、イメージから抽出された色を分析し、イメージの最も鮮やかな色を表すアクセント カラーをドミナント カラーと彩度の組み合わせでビューアーに返します。 イメージの色に応じて、シンプルな白黒またはアクセント カラーが 16 進数のカラー コードで返される場合があります。 Computer Vision でも、イメージが白黒であるかどうかを示すブール値を返します。

## <a name="color-scheme-detection-examples"></a>配色の検出の例

次の例は、イメージの例の配色を検出するときに Computer Vision によって返される JSON 応答を示しています。 ここでは、イメージの例は白黒ではありませんが、前景と背景のドミナント カラーは黒で、イメージ全体のドミナント カラーは白黒です。

![屋外の山](./Images/mountain_vista.png)

```json
{
    "color": {
        "dominantColorForeground": "Black",
        "dominantColorBackground": "Black",
        "dominantColors": ["Black", "White"],
        "accentColor": "BB6D10",
        "isBwImg": false
    },
    "requestId": "0dc394bf-db50-4871-bdcc-13707d9405ea",
    "metadata": {
        "height": 202,
        "width": 300,
        "format": "Jpeg"
    }
}
```

### <a name="dominant-color-examples"></a>ドミナント カラーの例

次の表は、Computer Vision によって返されるイメージの例それぞれの前景、背景、イメージのドミナント カラーを示しています。

| イメージ | ドミナント カラー |
|-------|-----------------|
|![白い花と緑の背景](./Images/flower.png)| 前景:黒<br/>背景:白<br/>色:黒、白、緑|
![駅を通過中の電車](./Images/train_station.png) | 前景:黒<br/>背景:黒<br/>色:黒 |

### <a name="accent-color-examples"></a>アクセント カラーの例

 次の表は、Computer Vision によって返されるイメージの例それぞれのアクセント カラーを 16 進数の HTML カラー値で示しています。

| イメージ | アクセント カラー |
|-------|--------------|
|![日没時に山頂の岩の上に立っている人物](./Images/mountain_vista.png) | #BB6D10 |
|![白い花と緑の背景](./Images/flower.png) | #C6A205 |
|![駅を通過中の電車](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>白黒の検出の例

次の表は、Computer Vision によって返されるイメージの例それぞれが白黒であるかどうかを示しています。

| イメージ | 白黒かどうか |
|-------|----------------|
|![マンハッタンのビル群の白黒写真](./Images/bw_buildings.png) | true |
|![青い家と前庭](./Images/house_yard.png) | false |

## <a name="next-steps"></a>次の手順

[イメージの種類の検出](concept-detecting-image-types.md)に関する概念を理解します。