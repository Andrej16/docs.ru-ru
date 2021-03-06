---
title: "Интерфейсы в C#. Краткий обзор языка C#"
description: "Интерфейсы определяют контракты, которые реализуются в типах C#"
keywords: ".NET, c#, интерфейсы, множественное наследование, полиморфизм"
author: BillWagner
ms.author: wiwagn
ms.date: 08/10/2016
ms.topic: article
ms.prod: .net
ms.technology: devlang-csharp
ms.devlang: csharp
ms.assetid: a9bf82f4-efd1-4216-bd34-4ef0fa48c968
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 673ac56f3f5732fcda02d313b6f4273708ae365f
ms.contentlocale: ru-ru
ms.lasthandoff: 07/28/2017

---

# <a name="interfaces"></a>Интерфейсы

***Интерфейс*** определяет контракт, который может быть реализован классами и структурами. Интерфейс может содержать методы, свойства, события и индексаторы. Интерфейс не предоставляет реализацию членов, которые в нем определены. Он лишь перечисляет члены, которые должны быть определены в классах или структурах, реализующих этот интерфейс.

Интерфейсы могут применять ***множественное наследование***. В следующем примере интерфейс `IComboBox` наследует одновременно от `ITextBox` и `IListBox`.

[!code-csharp[InterfacesOne](../../../samples/snippets/csharp/tour/interfaces/Program.cs#L5-L17)]

Классы и структуры могут реализовывать несколько интерфейсов. В следующем примере класс `EditBox` реализует одновременно `IControl` и `IDataBound`.

[!code-csharp[InterfacesTwo](../../../samples/snippets/csharp/tour/interfaces/Program.cs#L19-L27)]

Если класс или структура реализует конкретный интерфейс, любой экземпляр этого класса или структуры можно неявно преобразовать в такой тип интерфейса. Пример

[!code-csharp[InterfacesThree](../../../samples/snippets/csharp/tour/interfaces/Program.cs#L33-L35)]

Если в статическом контексте невозможно достоверно знать, что экземпляр реализует определенный интерфейс, можно использовать динамическое приведение типов. Например, следующие операторы используют динамическое приведение типов для получения реализации интерфейсов `IControl` и `IDataBound` для объекта. Приведения выполняются успешно, поскольку во время выполнения объект имеет фактический тип `EditBox`.

[!code-csharp[InterfacesFour](../../../samples/snippets/csharp/tour/interfaces/Program.cs#L40-L42)]

В предыдущем примере для класса `EditBox` метод `Paint` из интерфейса `IControl` и метод `Bind` из интерфейса `IDataBound` реализуются с помощью открытых членов. C# также поддерживает явные ***реализации членов интерфейса***, что позволяет классам и структурам не использовать открытые члены. Явная реализация члена интерфейса записывается с использованием полного имени члена интерфейса. Например, класс `EditBox` может реализовывать методы `IControl.Paint` и `IDataBound.Bind` с использованием явной реализации членов интерфейса, как показано в следующем примере.

[!code-csharp[InterfacesFive](../../../samples/snippets/csharp/tour/interfaces/Program.cs#L60-L64)]

Обращение к явным членам интерфейса можно осуществлять только через тип интерфейса. Например, реализация `IControl.Paint`, представленная в предыдущем примере класса EditBox, может вызываться только с предварительным преобразованием ссылки `EditBox` к типу интерфейса `IControl`.

[!code-csharp[InterfacesFive](../../../samples/snippets/csharp/tour/interfaces/Program.cs#L71-L74)]

>[!div class="step-by-step"]
[Назад](arrays.md)
[Вперед](enums.md)

