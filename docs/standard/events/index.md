---
title: "Обработка и вызов событий | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "разработка приложений [платформа .NET Framework], события"
  - "модель делегата для событий"
  - "события [платформа .NET Framework]"
ms.assetid: b6f65241-e0ad-4590-a99f-200ce741bb1f
caps.latest.revision: 23
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 23
---
# Обработка и вызов событий
События в платформе .NET Framework основаны на модели делегата.  Модель делегата, соответствующая шаблону разработки наблюдателя, позволяет подписчику зарегистрироваться у поставщика и получать от него уведомления.  Отправитель события отправляет уведомление о том, что событие произошло, а приемник событий получает это уведомление и определяет ответ на него.  В этом разделе описываются основные компоненты модели делегата, использование событий в приложениях и реализация событий в коде.  
  
 Дополнительные сведения об обработке событий в приложениях [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] см. в разделе [Обзор событий и перенаправленных событий \(Приложения для Магазина Windows\)](http://go.microsoft.com/fwlink/p/?LinkId=261485).  
  
## События  
 Событие представляет собой сообщение, посылаемое объектом, чтобы сигнализировать о совершении какого\-либо действия.  Это действие может быть вызвано в результате взаимодействия с пользователем, например при нажатии кнопки, или оно может быть вызвано другой логикой программы, например, изменением значения свойства.  Объект, вызывающий событие, называется *отправителем события*.  Отправителю событий не известен объект или метод, который будет получать \(обрабатывать\) сформированные отправителем события.  Обычно событие является членом отправителя события; например, событие <xref:System.Web.UI.WebControls.Button.Click> \- член класса <xref:System.Web.UI.WebControls.Button> и событие <xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged> \- член класса, реализующего интерфейс <xref:System.ComponentModel.INotifyPropertyChanged>.  
  
 Чтобы определить событие, используется ключевое слово `event` \(в C\#\) или `Event` \(в Visual Basic\) в сигнатуре класса событий и задается тип делегата для события.  Делегаты описываются в следующем разделе.  
  
 Обычно, чтобы вызвать событие, нужно добавить метод, помеченный как `protected` и `virtual` \(в C\#\) или `Protected` и `Overridable` \(в Visual Basic\).  Назовите этот метод `On`*EventName*; например, `OnDataReceived`.  Метод должен принимать один параметр, который определяет объект данных события.  Этот метод предоставляется, чтобы производные классы могли переопределять логику для вызова события.  Производный класс должен всегда вызывать метод `On`*EventName* базового класса, чтобы зарегистрированные делегаты гарантированно получили событие.  
  
 В следующем примере показан способ объявления события `ThresholdReached`.  Событие связано с делегатом <xref:System.EventHandler> и возникает в методе `OnThresholdReached`.  
  
 [!code-csharp[EventsOverview#1](../../../samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programtruncated.cs#1)]
 [!code-vb[EventsOverview#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1truncated.vb#1)]  
  
## Делегаты  
 Делегат — это тип, содержащий ссылку на метод.  Делегат объявлен с сигнатурой, которая указывает тип возвращаемого значения и параметры для методов, на которые он ссылается, и может содержать ссылки только на методы, соответствующие его сигнатуре.  Таким образом, делегат эквивалентен типобезопасному указателю функции или обратному вызову.  Объявление делегата является достаточным для определения класса делегата.  
  
 Делегаты имеют множество применений в платформе .NET Framework.  В контексте событий делегат \- посредник \(или механизм, подобный указателю\) между источником событий и кодом, обрабатывающим событие.  Делегат связывается с событием посредством включения типа делегата в объявление события, как показано в примере в предыдущем разделе.  Дополнительные сведения о делегатах см. в классе <xref:System.Delegate>.  
  
 Платформа .NET Framework предоставляет делегаты <xref:System.EventHandler> и <xref:System.EventHandler%601> для поддержки большинства сценариев событий.  Используйте делегат <xref:System.EventHandler> для всех событий, не содержащих данных.  Используйте делегат <xref:System.EventHandler%601> для событий, которые включают данные о событии.  Эти делегаты не имеют типа возвращаемого значения и принимают 2 параметра \(объект для источника события и объект для данных события\).  
  
 Делегаты являются многоадресными. Это означает, что они могут хранить ссылки на несколько методов обработки событий.  Дополнительные сведения см. справочные сведения о классе <xref:System.Delegate>.  Делегаты позволяют осуществлять гибкий и детальный контроль при обработке событий.  Делегат действует как диспетчер событий для класса, вызвавшего событие, обслуживая для события список зарегистрированных обработчиков событий.  
  
 Для сценариев, где делегаты <xref:System.EventHandler> и <xref:System.EventHandler%601> не работают, можно определить делегат.  Сценарии, которые требуют определить делегат, встречаются очень редко, например при необходимости работать с кодом, не распознает универсальные шаблоны.  При объявлении делегат необходимо пометить ключевым словом `delegate` \(в C\#\) или `Delegate` \(в Visual Basic\).  В следующем примере показано, как определить делегат с именем `ThresholdReachedEventHandler`.  
  
 [!code-csharp[EventsOverview#4](../../../samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programtruncated.cs#4)]
 [!code-vb[EventsOverview#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1truncated.vb#4)]  
  
## Данные события  
 Данные, связанные с событием, могут быть предоставлены с помощью класса данных события.  Платформа .NET Framework предоставляет множество классов данных событий, которые можно использовать в приложениях.  Например, класс <xref:System.IO.Ports.SerialDataReceivedEventArgs> \- класс данных события <xref:System.IO.Ports.SerialPort.DataReceived?displayProperty=fullName>.  В платформе .NET Framework названия всех классов данных событий оканчиваются ключевым словом `EventArgs`.  Определить, какой класс данных события связан с событием, можно по делегату этого события.  Например, делегат <xref:System.IO.Ports.SerialDataReceivedEventHandler> содержит класс <xref:System.IO.Ports.SerialDataReceivedEventArgs> в качестве одного из своих параметров.  
  
 Класс <xref:System.EventArgs> является базовым типом для всех классов данных события.  Класс <xref:System.EventArgs> также используется, если событие не содержит связанных с ним данных.  При создании события, которое только уведомляет другие классы о том, что что\-то произошло и не передает никаких данных, используйте класс <xref:System.EventArgs> в качестве второго параметра в делегате.  Можно передать значение <xref:System.EventArgs.Empty?displayProperty=fullName>, если никакие данные не предоставляются.  Делегат <xref:System.EventHandler> содержит класс <xref:System.EventArgs> в качестве параметра.  
  
 Если требуется создать пользовательский класс данных события, создайте класс, производный от <xref:System.EventArgs>, а затем укажите все члены, необходимые для передачи данных, связанных с событием.  Как правило, следует использовать такой же шаблон именования, как .NET Framework и завершать название класса данных события ключевым словом `EventArgs`.  
  
 В следующем примере показан класс данных события с именем `ThresholdReachedEventArgs`.  Он содержит свойства, относящиеся только к вызываемому событию.  
  
 [!code-csharp[EventsOverview#3](../../../samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programtruncated.cs#3)]
 [!code-vb[EventsOverview#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1truncated.vb#3)]  
  
## Обработчики событий  
 Для обработки события в приемнике события необходимо указать метод обработчика событий.  Этот метод должен соответствовать сигнатуре делегата обрабатываемого события.  В обработчике событий выполняются действия, необходимые при возникновении события, например, сбор введённых пользователем данных после того, как пользователь нажал кнопку.  Чтобы получать уведомления при возникновении события, метод обработчика событий должен подписаться на событие.  
  
 В следующем примере показан метод обработчика событий с именем `c_ThresholdReached`, который соответствует сигнатуре делегата <xref:System.EventHandler>.  Метод подписывается на событие `ThresholdReached`.  
  
 [!code-csharp[EventsOverview#2](../../../samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programtruncated.cs#2)]
 [!code-vb[EventsOverview#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1truncated.vb#2)]  
  
## Обработчики статических и динамических событий  
 .NET Framework позволяет подписчикам регистрироваться для получения уведомлений о событиях как статически, так и динамически.  Статические обработчики событий действуют на протяжении всего срока существования класса, события которого они обрабатывают.  Динамические обработчики событий явным образом активируются и деактивируются во время выполнения программы. Обычно это происходит при выполнении каких\-либо условий, определяемых логикой программы.  Например, их можно использовать в случаях, когда уведомление о событиях требуются только в определенных случаях или когда в приложении используется несколько обработчиков событий и нужный из них определяется во время выполнения программы.  В примере в предыдущем разделе показано, как динамически добавлять обработчик событий.  Дополнительные сведения см. в разделах [События](../Topic/Events%20\(Visual%20Basic\).md) и [События](../Topic/Events%20\(C%23%20Programming%20Guide\).md).  
  
## Вызов нескольких событий  
 Если класс создает несколько событий, компилятор создает одно поле для каждого экземпляра делегата события.  Если количество событий велико, то расходы ресурсов на хранение одного поля для каждого делегата могут стать неприемлемыми.  Для таких случаев в среде .NET Framework предоставляются свойства события, которые можно использовать вместе с другой структурой данных \(по вашему выбору\) для хранения делегатов события.  
  
 Свойства событий состоят из объявлений событий, сопровождаемых методами доступа к событиям.  Методы доступа к событиям представляют собой определяемые пользователем методы, позволяющие добавлять или удалять экземпляры делегата события из структуры данных хранения.  Обратите внимание, что использование свойств события снижает быстродействие по сравнению с полями события, поскольку перед вызовом каждого делегата события необходимо извлечь его.  Поэтому должен быть достигнут компромисс между занимаемой памятью и быстродействием.  Если класс определяет множество событий, которые вызываются нечасто, то пользователь может реализовать свойства событий.  Для получения дополнительной информации см. [Практическое руководство. Обработка нескольких событий с помощью их свойств](../../../docs/standard/events/how-to-handle-multiple-events-using-event-properties.md).  
  
## Связанные разделы  
  
|Название|Описание|  
|--------------|--------------|  
|[Практическое руководство. Вызов и прием событий](../../../docs/standard/events/how-to-raise-and-consume-events.md)|Содержит примеры вызова и приема событий.|  
|[Практическое руководство. Обработка нескольких событий с помощью их свойств](../../../docs/standard/events/how-to-handle-multiple-events-using-event-properties.md)|Показывает, как использовать свойства событий для обработки нескольких событий.|  
|[Шаблон разработки Observer](../../../docs/standard/events/observer-design-pattern.md)|Описывает шаблон разработки, который позволяет подписчику зарегистрироваться у поставщика и получать от него уведомления.|  
|[Практическое руководство. Прием событий в приложениях Web Forms](../../../docs/standard/events/how-to-consume-events-in-a-web-forms-application.md)|Описывает способы обработки событий, вызванных элементом управления Web Forms.|  
  
## См. также  
 <xref:System.EventHandler>   
 <xref:System.EventHandler%601>   
 <xref:System.EventArgs>   
 <xref:System.Delegate>   
 [Обзор событий и перенаправленных событий \(Приложения для Магазина Windows\)](http://go.microsoft.com/fwlink/?LinkId=261485)   
 [События](../Topic/Events%20\(Visual%20Basic\).md)   
 [События](../Topic/Events%20\(C%23%20Programming%20Guide\).md)