---
title: "Пример: Устранение неполадок динамического программирования | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 42ed860a-a022-4682-8b7f-7c9870784671
caps.latest.revision: 8
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 8
---
# Пример: Устранение неполадок динамического программирования
> [!NOTE]
>  В этом разделе рассматривается предварительная версия программного обеспечения для разработчиков машинного кода .NET.  Предварительную версию можно загрузить на [веб\-сайте Microsoft Connect](http://go.microsoft.com/fwlink/?LinkId=394611) \(требуется регистрация\).  
  
 Не все метаданных поиска ошибок в приложениях, разработанных с использованием цепочки инструментов [!INCLUDE[net_native](../../../includes/net-native-md.md)], приводят к исключению.  Некоторые могут проявляться в непредсказуемом виде в приложении.  В следующем примере показано нарушение доступа, вызванное ссылкой на пустой объект:  
  
```  
  
Access violation - code c0000005 (first chance)  
App!$3_App::Core::Util::NavigationArgs.Setup  
App!$3_App::Core::Util::NavigationArgs..ctor  
App!$0_App::Gibbon::Util::DesktopNavigationArgs..ctor  
App!$0_App::ViewModels::DesktopAppVM.NavigateToPage  
App!$3_App::Core::ViewModels::AppViewModel.NavigateToFirstPage  
App!$3_App::Core::ViewModels::AppViewModel::<HandleLaunch>d__a.MoveNext  
App!$43_System::Runtime::CompilerServices::AsyncMethodBuilderCore.CallMoveNext  
App!System::Action.InvokeClosedStaticThunk  
App!System::Action.Invoke  
App!$43_System::Threading::Tasks::AwaitTaskContinuation.InvokeAction  
App!$43_System::Threading::SendOrPostCallback.InvokeOpenStaticThunk  
[snip]  
  
```  
  
 Давайте попробуем устранить данное исключение, используя трехшаговый подхода, описанный в разделе «Устранение вручную проблем с отсутствующими метаданными» [Начало работы](../../../docs/framework/net-native/getting-started-with-net-native.md).  
  
## Что делало это приложение?  
 В первую очередь следует отметить механизм обработки ключевого слова `async` в начале стека.  Определить, что приложение действительно делало в методе `async` может быть проблематично, так как стек потерял контекст исходного вызова и выполнял код `async` в другом потоке.  Тем не менее, мы можем предположить, что приложение пытается загрузить свою первую страницу.  В реализации для `NavigationArgs.Setup` нарушение доступа было вызвано следующим кодом:  
  
```  
AppViewModel.Current.LayoutVM.PageMap  
```  
  
 В этом случае свойство `LayoutVM` на `AppViewModel.Current` было **нулевым**.  Отсутствие некоторых метаданных вызвало небольшую разницу в поведении и привело к инициализации свойства вместо набора, как ожидало приложение.  Задание точки останова в коде, в котором должно было быть реализовано `LayoutVM`, может пролить свет на ситуацию.  Тем не менее, обратите внимание, что типом `LayoutVM` является `App.Core.ViewModels.Layout.LayoutApplicationVM`.  Единственной директивой метаданных, присутствующей в файле rd.xml на настоящий момент, является:  
  
```  
  
<Namespace Name="App.ViewModels" Browse="Required Public" Dynamic="Required Public" />  
  
```  
  
 Вероятной причиной для сбоя является то, что в `App.Core.ViewModels.Layout.LayoutApplicationVM` не хватает метаданных, так как он находится в другом пространстве имен.  
  
 В этом случае добавления директивы среды выполнения для `App.Core.ViewModels` устранило проблему.  Основной причиной был вызов API для метода <xref:System.Type.GetType%28System.String%29?displayProperty=fullName>, который вернул **значение null,**, и приложение молча проигнорировало проблему до возникновения сбоя.  
  
 В динамическом программировании хорошей практикой при использовании API отражения в [!INCLUDE[net_native](../../../includes/net-native-md.md)] считается использование перегрузок <xref:System.Type.GetType%2A?displayProperty=fullName>, которые создает исключение при сбое.  
  
## Это изолированный случай?  
 Другие проблемы также могут возникнуть при использовании `App.Core.ViewModels`.  Необходимо решить, стоит ли определять и устранять каждое исключение отсутствующих метаданных или лучше сэкономить время и добавить директивы для большего класса типов.  Здесь, добавление метаданных `dynamic` для `App.Core.ViewModels` может быть лучше, если результирующее увеличение размера выходного двоичного файла не проблема.  
  
## Можно переписать код?  
 Если приложение использовало `typeof(LayoutApplicationVM)` вместо `Type.GetType(“LayoutApplicationVM”)`, то цепочка инструментов могла сохранить метаданные `browse`.  Тем не менее, оно по\-прежнему не создало метаданные `invoke`, которые бы привели к исключению [MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md) при создании экземпляра типа.  Чтобы предотвратить это исключение, как и раньше необходимо добавить директиву среды выполнения для пространства имен или тип, который задает политику `dynamic`.  Дополнительные сведения о директивах среды выполнения см. в разделе [Ссылка на файл конфигурации директив среды выполнения \(rd.xml\)](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md).  
  
## См. также  
 [Начало работы](../../../docs/framework/net-native/getting-started-with-net-native.md)   
 [Пример: Обработка исключений при привязке данных](../../../docs/framework/net-native/example-handling-exceptions-when-binding-data.md)