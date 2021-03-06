---
title: Schnittebenen
description: Erläutert, was Schnittebenen sind und wie sie verwendet werden.
author: jakrams
ms.author: jakras
ms.date: 02/06/2020
ms.topic: article
ms.openlocfilehash: 7adf9a9701eb2492f0b13a26af1dbaf8de631373
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84021363"
---
# <a name="cut-planes"></a>Schnittebenen

Eine *Schnittebene* ist eine visuelle Funktion, die Pixel auf einer Seite einer virtuellen Ebene beschneidet und das Innere von [Gittermodellen](../../concepts/meshes.md) enthüllt.
In der folgenden Abbildung wird der Effekt veranschaulicht. Auf der linken Seite wird das ursprüngliche Gittermodell angezeigt, auf der rechten Seite sehen Sie das Innere des Gittermodells:

![Schnittebene](./media/cutplane-1.png)

## <a name="limitations"></a>Einschränkungen

* Zurzeit unterstützt Azure Remote Rendering **maximal acht aktive Schnittebenen**. Sie können weitere Schnittebenenkomponenten erstellen, aber wenn Sie versuchen, mehrere Komponenten gleichzeitig zu aktivieren, wird die Aktivierung ignoriert. Deaktivieren Sie zuerst andere Ebenen, wenn Sie die Komponente austauschen möchten, die die Szene beeinflussen soll.
* Jede Schnittebene besitzt Auswirkungen auf alle remote gerenderten Objekte. Es gibt derzeit keine Möglichkeit, bestimmte Objekte oder Gittermodelle auszuschließen.
* Schnittebenen sind ein rein visuelles Feature, das sich nicht auf das Ergebnis [räumlicher Abfragen](spatial-queries.md) auswirkt. Wenn Sie einen Strahl in ein aufgeschnittenes Gittermodell werfen möchten, können Sie den Ausgangspunkt des Strahls so anpassen, dass er auf der Schnittebene liegt. Auf diese Weise kann der Strahl nur auf sichtbare Teile treffen.

## <a name="performance-considerations"></a>Überlegungen zur Leistung

Jede aktive Schnittebene verursacht beim Rendern geringfügige Kosten. Deaktivieren oder löschen Sie Schnittebenen, wenn Sie nicht benötigt werden.

## <a name="cutplanecomponent"></a>CutPlaneComponent

Sie fügen der Szene eine Schnittebene hinzu, indem Sie eine *CutPlaneComponent* erstellen. Die Position und die Ausrichtung der Ebene werden von der [Besitzerentität](../../concepts/entities.md) der Komponente bestimmt.

```cs
void CreateCutPlane(AzureSession session, Entity ownerEntity)
{
    CutPlaneComponent cutPlane = (CutPlaneComponent)session.Actions.CreateComponent(ObjectType.CutPlaneComponent, ownerEntity);
    cutPlane.Normal = Axis.X; // normal points along the positive x-axis of the owner object's orientation
    cutPlane.FadeColor = new Color4Ub(255, 0, 0, 128); // fade to 50% red
    cutPlane.FadeLength = 0.05f; // gradient width: 5cm
}
```

```cpp
void CreateCutPlane(ApiHandle<AzureSession> session, ApiHandle<Entity> ownerEntity)
{
    ApiHandle<CutPlaneComponent> cutPlane = session->Actions()->CreateComponent(ObjectType::CutPlaneComponent, ownerEntity)->as<CutPlaneComponent>();;
    cutPlane->Normal(Axis::X); // normal points along the positive x-axis of the owner object's orientation
    Color4Ub fadeColor;
    fadeColor.channels = { 255, 0, 0, 128 }; // fade to 50% red
    cutPlane->FadeColor(fadeColor);
    cutPlane->FadeLength(0.05f); // gradient width: 5cm
}
```


### <a name="cutplanecomponent-properties"></a>CutPlaneComponent-Eigenschaften

Die folgenden Eigenschaften werden für eine Schnittebenenkomponente bereitgestellt:

* `Enabled`: Sie können Schnittebenen temporär ausschalten, indem Sie die Komponente deaktivieren. Deaktivierte Schnittebenen verursachen keinen Renderingmehraufwand und werden auch nicht in den globalen Grenzwert von Schnittebenen einbezogen.

* `Normal`: Gibt an, welche Richtung (+X, -X, +Y, -Y, +Z, -Z) als normale Ebene verwendet wird. Diese Richtung ist relativ zur Ausrichtung der Besitzerentität. Verschieben und drehen Sie die Besitzerentität zur genauen Platzierung.

* `FadeColor` und `FadeLength`:

  Wenn der Alphawert von *FadeColor* ungleich NULL ist, werden Pixel in der Nähe der Schnittebene in Richtung des RGB-Anteils von FadeColor ausgeblendet. Die Stärke des Alphakanals bestimmt, ob die Ausblendung vollständig bis in die Ausblendfarbe oder nur teilweise erfolgt. *FadeLength* definiert, über welche Entfernung diese Ausblendung stattfindet.

## <a name="next-steps"></a>Nächste Schritte

* [Einseitiges Rendering](single-sided-rendering.md)
* [Räumliche Abfragen](spatial-queries.md)
