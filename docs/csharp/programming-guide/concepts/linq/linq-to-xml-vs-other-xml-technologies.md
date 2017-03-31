---
title: "LINQ to XML или Другие XML-технологии | Документы Майкрософт"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: 01b8e746-12d3-471d-b811-7539e4547784
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 8ac37ce1a225a66069e34abedd2ee0c273b8f8a9
ms.lasthandoff: 03/13/2017

---
# <a name="linq-to-xml-vs-other-xml-technologies"></a>LINQ to XML или Другие XML-технологии
В этом разделе [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] сравнивается со следующими XML-технологиями: <xref:System.Xml.XmlReader>, XSLT, MSXML и XmlLite. Данные сведения могут помочь в выборе технологии.  
  
 Сравнение [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] с моделью DOM см. в разделе [Сравнение LINQ to XML или DOM (C#)](../../../../csharp/programming-guide/concepts/linq/linq-to-xml-vs-dom.md).  
  
## <a name="linq-to-xml-vs-xmlreader"></a>LINQ to XML или XmlReader  
 <xref:System.Xml.XmlReader> — это быстрое однопроходное средство синтаксического анализа без кэширования.  
  
 Технология [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] реализована на основе <xref:System.Xml.XmlReader>, и эти средства тесно связаны друг с другом. В то же время <xref:System.Xml.XmlReader> можно использовать отдельно.  
  
 Предположим, что необходимо создать веб-службу, которая будет выполнять в каждую секунду анализ сотен XML-документов, имеющих одинаковую структуру, так что для анализа XML потребуется записать лишь одну реализацию кода. В этом случае может потребоваться использовать объект <xref:System.Xml.XmlReader> самостоятельно.  
  
 Напротив, если создается система, предназначенная для синтаксического анализа множества небольших XML-документов, отличающихся друг от друга, то целесообразно воспользоваться средствами повышения производительности, которые предоставляет технология [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)].  
  
## <a name="linq-to-xml-vs-xslt"></a>LINQ to XML или XSLT  
 Технология [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] и язык XSLT предоставляют широкие возможности по преобразованию XML-документа. XSLT представляет собой декларативный подход, основанный на правилах. Опытные XSLT-программисты записывают код XSLT в стиле функционального программирования, в основе которого лежит подход без сохранения состояния. Преобразования могут быть написаны с использованием чистых функций, реализованных без побочных эффектов. С этим подходом, основанным на правилах или функциональным, незнакомы многие разработчики, и для его изучения потребуется много времени и усилий.  
  
 Технология XSLT может стать основой весьма продуктивной системы, позволяющей создавать высокопроизводительные приложения. Например, несколько крупных компаний, работающих в сфере веб-технологий, используют XSLT для создания HTML-кода из XML-кода, полученного из различных хранилищ данных. Управляемое ядро XSLT компилирует XSLT в код CLR и в некоторых сценариях работает даже лучше собственного ядра XSLT.  
  
 Однако многие разработчики, переходя к использованию языка XSLT, теряют такое свое преимущество, как знание языков C# и Visual Basic. Разработчики вынуждены использовать при написании кода совсем другой и сложный язык программирования. Использование двух не связанных между собой систем разработки, таких как C# (или Visual Basic) и XSLT, приводит к появлению программных систем, которые сложнее разрабатывать и поддерживать.  
  
 После успешного овладения выражениями запроса [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] становится ясно, что преобразования [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] представляют собой мощную и простую в использовании технологию. В сущности, процесс формирования XML-документа состоит из использования функционального построения, получения по запросу данных из разных источников, динамического построения объектов <xref:System.Xml.Linq.XElement> и сборки всех элементов в новое XML-дерево. Это преобразование позволяет создать совершенно новый документ. Создание преобразований в [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] происходит относительно легко и интуитивно понятно, а конечный код легко читается. Это снижает расходы на разработку и обслуживание.  
  
 [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] не предназначен для замены XSLT. XSLT все еще является лучшим средством для сложных XML-преобразований, в основе которых лежат документы, особенно если структура документа определена не полностью.  
  
 Преимущество XSLT заключается в том, что эта технология определена стандартом консорциума W3C. Поэтому если должно быть учтено требование по использованию только тех технологий, которые являются стандартными, то XSLT может оказаться более подходящей.  
  
 Код XSLT представляет собой код XML, поэтому с ним можно проводить манипуляции программным путем.  
  
## <a name="linq-to-xml-vs-msxml"></a>LINQ to XML или MSXML  
 MSXML - это основанная на модели COM технология обработки XML, включенная в ОС Microsoft Windows. MSXML обеспечивает собственную реализацию DOM с поддержкой XPath и XSLT. Она также содержит средство синтаксического анализа SAX2, основанное на событиях, без кэширования.  
  
 MSXML обеспечивает высокую производительность, по умолчанию обеспечивает безопасность в большинстве сценариев и может использоваться в обозревателе Internet Explorer для выполнения обработки XML на стороне клиента в приложениях AJAX. Возможность применения технологии MSXML предусмотрена в любом языке программирования, поддерживающем COM, включая C++, JavaScript и [!INCLUDE[vbprvb](../../../../csharp/programming-guide/concepts/linq/includes/vbprvb_md.md)] 6.0.  
  
 Не рекомендуется использовать MSXML в управляемом коде, основанном на среде CLR.  
  
## <a name="linq-to-xml-vs-xmllite"></a>LINQ to XML или XmlLite  
 XmlLite представляет собой запрашивающее средство синтаксического анализа с последовательным доступом, без кэширования. В основном разработчики используют XmlLite с языком C++. Не рекомендуется использовать XmlLite с управляемым кодом.  
  
 Основным преимуществом такого средства синтаксического анализа XML, как XmlLite, является его простота, быстродействие и безопасность в большинстве сценариев. Его контактная зона, подверженная угрозам, очень мала. Если требуется проведение анализа документов, не заслуживающих доверия, и необходимо защититься от атак типа «отказ в обслуживании» или раскрытия данных, то XmlLite является хорошим выбором.  
  
 XmlLite не поддерживает интеграцию с [!INCLUDE[vbteclinqext](../../../../csharp/getting-started/includes/vbteclinqext_md.md)]. Применение этого инструмента, в отличие от технологии [!INCLUDE[vbteclinq](../../../../csharp/includes/vbteclinq_md.md)], не приводит к повышению производительности программиста, что является одним из побудительных мотивов к использованию этой технологии.  
  
## <a name="see-also"></a>См. также  
 [Начало работы (LINQ to XML)](../../../../csharp/programming-guide/concepts/linq/getting-started-linq-to-xml.md)