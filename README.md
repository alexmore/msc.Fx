# alexmore.Fx
Применение принципа CQRS для замещения Repository и XxxService при предметно-ориентированном проектировании.

#### Когда?
  
 - Если Repository начал превращаться в монстра с кучей GetXXX, FindXXX, AddXXX и, уж тем более, если его реализация разделена 
по partial классам;
 - Если в XxxService конструктор принимает все больше и больше Repository и служба становится God-объектом;
 - Если код для обеспечения тестов Repository и XxxService в несколько раз больше самих тестов.
 
#### Почему?
  
 - Четкое разделение ответственности: запросы возвращают, команды изменяют;
 - Возможность создавать различные реализации и управлять ими через IoC-контейнер или другим способом;
 - Слабая связность;
 - Горизонтальное масштабирование "из коробки";
 - Отличная тестируемость.
  
#### А конкретнее?
  
 - Вот так выглядит [простейший контроллер WebApi](source/alexmore.Fx.Tests/Domain/WebApiControllerSample.cs);
 - Пример [команд](source/alexmore.Fx.Tests/Domain/Commands);
 - Пример [запросов](source/alexmore.Fx.Tests/Domain/Queries);
 - А вот [настройка IoC (StructureMap)](source/alexmore.Fx.Tests/Domain/Infrastructure/DefaultRegistry.cs) и [реализация резолверов для комманд и запросов](source/alexmore.Fx.Tests/Domain/Infrastructure/Resolvers.cs);
 - А здесь можно заглянуть [под капот](source/alexmore.Fx/Domain).
 
#### А на практике?
  
 - Подобное решение на практике пережило безболезненную замену БД. Изменений потребовали только комнады и запросы, использующие БД-специфичный код. Если бы это было на поздней стадии побочным эффектом была бы поддержка сразу двух БД, с переключением в настройках;
 - Распределение нагрузки с минимальными вложениями: горизонтальное масштабирование одной из подсистем потребовало только изменение реализации "точки входа" для запросов и команд ([IDataSource](source/alexmore.Fx/Domain/IDataSource.cs)).
