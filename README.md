Автозагрузчик модулей Битрикс, который поможет вам навсегда забыть про вызовы CModule::IncludeModule и Loader::includeModule

Будте осторожны: это бетта-версия и в реальных боевых условиях она пока не тестировалась!

Как использовать:

1 Установите через composer: 

`composer require webarchitect609/bitrix-neverinclude`

2 В init.php после подключения `vendor/autoload.php` добавьте вызов: 

`\WebArch\BitrixNeverInclude\BitrixNeverInclude::registerModuleAutoload();`

3 И наслаждайтесь более интересными заботами, чем подключение модулей то здесь, то там! :)

Особенности реализации

1 Классы не из глобального namespace разбираются динамически и превращаются в название модуля, 
который тут же подключается.

2 Классы из глобальной области проверяются по маппингу "имя класса => имя модуля", для вычисления которого делается 
подключение всех установленных в системе модулей и производится сбор внутренних данных, которые потом кешируются. 

Возможные неприятности:
 
1 Если происходит установка нового модуля, использующего классы в глобальной области, кеш маппинга 
"имя класса => имя модуля" будет неактуальным. Рекомендуется сбросить его по тегу следующим образом: 

```

$tagCache = \Bitrix\Main\Application::getInstance()->getTaggedCache();
$tagCache->clearByTag(\WebArch\BitrixNeverInclude\BitrixNeverInclude::CACHE_TAG);

```

2 После сброса кеша рекомендуется вызвать `\WebArch\BitrixNeverInclude\BitrixNeverInclude::getClassMapping();` , чтобы 
при следующем хите уже существовал маппинг "имя класса => имя модуля". 


