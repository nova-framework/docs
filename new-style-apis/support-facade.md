
## Introduction
Facades provide a "static" interface to classes that are available in the application's service container. Nova ships with many facades, and you have probably been using them without even knowing it! Nova "facades" serve as "static proxies" to underlying classes in the service container, providing the benefit of a terse, expressive syntax while maintaining more testability and flexibility than traditional static methods.

## Facade Class Reference

Below you will find every facade and its underlying class. This is a useful tool for quickly digging into the API documentation for a given facade root. The service container binding key is also included where applicable.

|Facade|Class|Service Container Binding|
|---|---|---|
|Auth|Auth\Guard|Auth|
|Cookie|Cookie\CookieJar|Cookie|
|Crypt|Crypt\Encrypter|Crypt|
|DB|Database\Connection|DB|
|Event|Events\Dispatcher|Event|
|Hash|Support\Facades\Hash|Hash|
|Input|Support\Facades\Input|Input|
|Language|Core\Language|Language|
|Mailer|Mail\Mailer|Mailer|
|Paginator|Pagination\Factory|Paginator|
|Password|Auth\Reminders\PasswordBroker|Password|
|Redirect|Routing\Redirector|Redirect|
|Request|Http\Request|Request|
|Response|Http\Response|Response|
|Session|Session\Store|Session|
|Validator|Validation\Factory|Validator|
