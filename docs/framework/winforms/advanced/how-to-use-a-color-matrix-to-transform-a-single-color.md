---
title: "Практическое руководство. Использование матрицы цветов для преобразования отдельного цвета | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "матрицы цветов, использование"
  - "цвета изображения, преобразование"
ms.assetid: 44df4556-a433-49c0-ac0f-9a12063a5860
caps.latest.revision: 17
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 17
---
# Практическое руководство. Использование матрицы цветов для преобразования отдельного цвета
В [!INCLUDE[ndptecgdiplus](../../../../includes/ndptecgdiplus-md.md)] имеются классы <xref:System.Drawing.Image> и <xref:System.Drawing.Bitmap>, которые служат для хранения изображений и управления ими.  В объектах <xref:System.Drawing.Image> и <xref:System.Drawing.Bitmap> в виде 32\-битового числа хранятся значения цветов каждого пикселя: по 8 бит на красный, зеленый, синий и альфа\-компоненты.  Каждый из четырех указанных компонентов представляет собой число от 0 до 255, где 0 соответствует нулевой интенсивности, а 255 — наибольшей интенсивности.  Альфа\-компонент определяет прозрачность цвета: 0 соответствует полной прозрачности, а 255 — полной непрозрачности.  
  
 Цветовой вектор является кортежем вида \(красный, зеленый, синий, альфа\).  Например, цветовой вектор \(0, 255, 0, 255\) соответствует непрозрачному цвету, не имеющему красного и синего компонентов, а зеленый компонент которого имеет наибольшую интенсивность.  
  
 При другом способе представления цветов число 1 соответствует наибольшей интенсивности.  При использовании этого способа описанный в предыдущем абзаце цвет соответствует вектору \(0, 1, 0, 1\).  В [!INCLUDE[ndptecgdiplus](../../../../includes/ndptecgdiplus-md.md)] единица в качестве значения, соответствующего наибольшей интенсивности, используется при преобразовании цветов.  
  
 К цветовым векторам можно применять линейные преобразования \(поворот, масштабирование и т. п.\), умножая эти векторы на матрицу размерностью 4×4.  Однако такую матрицу 4×4 нельзя использовать для осуществления сдвига цветов \(нелинейного\).  Если добавить фиктивную пятую координату \(например, число 1\) к каждому из цветовых векторов, то можно использовать матрицу размерностью 5×5 для осуществления любого сочетания линейных преобразований и сдвигов.  Преобразование, состоящее из линейного преобразования, за которым следует сдвиг, называется аффинным преобразованием.  
  
 Предположим, например, что нам нужно начать с цвета \(0,2; 0,0; 0,4; 1,0\) и применить к нему следующие преобразования.  
  
1.  Увеличить в два раза красный компонент.  
  
2.  Добавить 0,2 к красному, зеленому и синему компонентам.  
  
 Следующее матричное произведение осуществляет эти два преобразования в указанном порядке.  
  
 ![Перекрашивание](../../../../docs/framework/winforms/advanced/media/recoloring01.png "recoloring01")  
  
 Индексы элементов матрицы цветов соответствуют номера строк и столбцов \(начиная с нуля\).  Например, элемент в пятой строке и третьем столбце матрицы M обозначается M\[4\]\[2\].  
  
 Единичная матрица размерностью 5×5 \(приведенная на следующем рисунке\) имеет единицы на главной диагонали, а все другие ее элементы равны нулю.  При умножении вектора на единичную матрицу он не изменяется.  Удобным способом создания матрицы преобразования цветов является использование единичной матрицы в качестве начальной и внесение в нее небольших изменений, приводящих к желаемому преобразованию.  
  
 ![Перекрашивание](../../../../docs/framework/winforms/advanced/media/recoloring02.gif "recoloring02")  
  
 Дополнительные сведения о матрицах и их преобразованиях см. в разделе [Системы координат и преобразования](../../../../docs/framework/winforms/advanced/coordinate-systems-and-transformations.md).  
  
## Пример  
 В следующем примере к изображению, содержащему только один цвет \(0,2; 0,0; 0,4; 1,0\), применяется преобразование, описанное выше.  
  
 На следующем рисунке показаны как исходное изображение \(слева\), так и преобразованное изображение \(справа\).  
  
 ![Цвета](../../../../docs/framework/winforms/advanced/media/colortrans1.png "colortrans1")  
  
 В приведенном ниже примере коде для осуществления перекрашивания выполняются следующие действия.  
  
1.  Инициализация объекта <xref:System.Drawing.Imaging.ColorMatrix>.  
  
2.  Создание объекта <xref:System.Drawing.Imaging.ImageAttributes> и передача объекта <xref:System.Drawing.Imaging.ColorMatrix> методу <xref:System.Drawing.Imaging.ImageAttributes.SetColorMatrix%2A> объекта <xref:System.Drawing.Imaging.ImageAttributes>.  
  
3.  Передача объекта <xref:System.Drawing.Imaging.ImageAttributes> методу <xref:System.Drawing.Graphics.DrawImage%2A> объекта <xref:System.Drawing.Graphics>.  
  
 [!code-csharp[System.Drawing.RecoloringImages#21](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.RecoloringImages/CS/Class1.cs#21)]
 [!code-vb[System.Drawing.RecoloringImages#21](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.RecoloringImages/VB/Class1.vb#21)]  
  
## Компиляция кода  
 Предыдущий пример предназначен для работы с Windows Forms, для него необходим объект <xref:System.Windows.Forms.PaintEventArgs> `e`, передаваемый в качестве параметра обработчику события <xref:System.Windows.Forms.Control.Paint>.  
  
## См. также  
 [Перекрашивание изображений](../../../../docs/framework/winforms/advanced/recoloring-images.md)   
 [Системы координат и преобразования](../../../../docs/framework/winforms/advanced/coordinate-systems-and-transformations.md)