---
title: "Использование клиентского канала обнаружения | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 1494242a-1d64-4035-8ecd-eb4f06c8d2ba
caps.latest.revision: 6
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 6
---
# Использование клиентского канала обнаружения
При написании клиентского приложения WCF необходимо знать адрес конечной точки вызываемой службы.Во многих случаях адрес конечной точки службы неизвестен заранее или может измениться со временем.Клиентский канал обнаружения позволяет создать клиентское приложение WCF, описать службу, которую необходимо вызвать, после чего клиентский канал автоматически отправит зондирующий запрос.После ответа службы клиентский канал обнаружения извлекает адрес конечной точки службы из ответа запроса и пользуется им для вызова службы.  
  
## Использование клиентского канала обнаружения  
 Чтобы задействовать клиентский канал обнаружения, добавьте экземпляр <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> в стек клиентских каналов.Можно также использовать конечную точку <xref:System.ServiceModel.Discovery.DynamicEndpoint>, в этом случае элемент <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> будет автоматически добавляться к привязке, если он еще не присутствует.  
  
> [!CAUTION]
>  Рекомендуется в качестве самого верхнего элемента в стеке клиентских каналов задать <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement>.Для всех элементов привязки, добавляемых поверх <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement>, убедитесь, что ни <xref:System.ServiceModel.ChannelFactory>, ни создаваемый им канал не используют адрес конечной точки или адрес `Via` \(передаваемый методу `CreateChannel`\), поскольку правильный адрес в них может отсутствовать.  
  
 Класс <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> содержит два открытых свойства.  
  
1.  <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.FindCriteria%2A>, которое используется для описания вызываемой службы.  
  
2.  <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.DiscoveryEndpoint%2A>, которое указывает конечную точку обнаружения, которой необходимо отправлять сообщения.  
  
 Свойство <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.FindCriteria%2A> позволяет указать искомый контракт службы, все необходимые URI области и максимальное количество попыток открытия канала.Тип контракта указывается при вызове конструктора <xref:System.ServiceModel.Discovery.FindCriteria%2A>.URI области могут быть добавлены в свойство <xref:System.ServiceModel.Discovery.FindCriteria.Scopes%2A>.Свойство <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A> позволяет задать максимальное количество результатов, с которыми клиент пытается установить соединение.При получении ответа на зондирующий запрос клиент выполняет попытку открытия канала по адресу конечной точки, полученному из ответа запроса.Если возникло исключение, то клиент переходит к следующему ответу зондирующего запроса и при необходимости ожидает получения дополнительных ответов.Это продолжается до тех пор, пока не будет успешно открыт канал или достигнуто максимальное количество результатов.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] о данных параметрах см. в разделе <xref:System.ServiceModel.Discovery.FindCriteria%2A>.  
  
 Свойство <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.DiscoveryEndpoint%2A> позволяет указать конечную точку обнаружения для использования.Обычно это <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>, но может быть также использована любая действительная конечная точка.  
  
 При создании привязки для взаимодействия со службой должна быть указана та же привязку, что и в службе.Единственное различие заключается в том, что привязка клиента содержит <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> в вершине стека.Если служба использует одну из привязок, предоставленных системой, создайте новую <xref:System.ServiceModel.Channels.CustomBinding> и передайте привязку, предоставленную системой, конструктору <xref:System.ServiceModel.CustomBinding.CustomBinding%2A>.После этого можно добавить <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement>, вызвав `Insert` для свойства <xref:System.ServiceModel.Channels.Binding.Elements%2A>.  
  
 После добавления к привязке и настройки <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> можно создать экземпляр класса клиента WCF, открыть его и обращаться к его методам.В следующем примере показан клиентский канал обнаружения для обнаружения службы WCF, которая реализует класс `ICalculator` \(используется в учебнике «Приступая к работе с WCF»\) и вызывает его метод `Add`.  
  
```  
// Create the DiscoveryClientBindingElement  
DiscoveryClientBindingElement bindingElement = new DiscoveryClientBindingElement();  
// Search for a service that implements the ICalculator interface, attempting to open  
// the channel a maximum of 2 times  
bindingElement.FindCriteria = new FindCriteria(typeof(ICalculator)) { MaxResults = 2 };  
// Use the UdpDiscoveryEndpoint  
bindingElement.DiscoveryEndpoint = new UdpDiscoveryEndpoint();  
  
// The service uses the BasicHttpBinding, so use that and insert the DiscoveryClientBindingElement at the   
// top of the stack  
CustomBinding binding = new CustomBinding(new BasicHttpBinding());  
binding.Elements.Insert(0,bindingElement);  
  
try  
{  
    // Create the WCF client and call a method  
    CalculatorClient client = new CalculatorClient(binding, new EndpointAddress("http://schemas.microsoft.com/dynamic"));  
    client.Open();  
    client.Add(1, 1);  
}  
catch (EndpointNotFoundException ex)  
{  
    Console.WriteLine("An exception occurred: " + ex.Message);  
}  
```  
  
## Безопасность и клиентский канал обнаружения  
 При использовании клиентского канала обнаружения указываются две конечные точки.Одна из этих точек служит для передачи сообщений обнаружения \(обычно это <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>\), а другая является конечной точкой приложения.При реализации службы безопасности необходимо соблюдать предосторожность и защитить обе конечные точки.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] о безопасности см. в разделе [Защита служб и клиентов](../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md).