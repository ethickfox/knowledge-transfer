# **HTTP**

Так как HTTP — протокол прикладного уровня передачи данных, изначально — в виде гипертекстовых документов в формате HTML, в настоящее время используется для передачи произвольных данных. Это клиент-серверный протокол,

- `GET` запрашивает представление ресурса. Запросы с использованием этого метода могут только извлекать данные.
- `POST` используется для отправки сущностей к определенному ресурсу. Часто вызывает изменение состояния или какие-то побочные эффекты на сервере.HTTP-метод POST предназначен для отправки данных на сервер. Тип тела запроса указывается в заголовке Content-Type. Разница между PUT и POST состоит в том, что PUT является идемпотентным: повторное его применение даёт тот же результат, что и при первом применении (то есть у метода нет побочных эффектов), тогда как повторный вызов одного и того же метода POST может иметь такие эффекты, как например, оформление одного и того же заказа несколько раз. Запрос POST обычно отправляется через форму HTML и приводит к изменению на сервере. В этом случае тип содержимого выбирается путём размещения соответствующей строки в атрибуте enctype элемента `<form>` или formenctype атрибута элементов `<input>` или `<button>`: `application/x-www-form-urlencoded`: значения кодируются в кортежах с ключом, разделённых символом '&', с '=' между ключом и значением. Не буквенно-цифровые символы - percent encoded: это причина, по которой этот тип не подходит для использования с двоичными данными (вместо этого используйте multipart/form-data) multipart/form-data: каждое значение посылается как блок данных ("body part"), с заданными пользовательским клиентом разделителем ("boundary"), разделяющим каждую часть. Эти ключи даются в заголовки Content-Disposition каждой части text/plain Когда запрос POST отправляется с помощью метода, отличного от HTML-формы, — например, через XMLHttpRequest — тело может принимать любой тип. Как описано в спецификации HTTP 1.1, POST предназначен для обеспечения единообразного метода для покрытия следующих функций: Аннотация существующих ресурсов Публикация сообщения на доске объявлений, в новостной группе, в списке рассылки или в аналогичной группе статей; Добавление нового пользователя посредством модальности регистрации; Предоставление блока данных, например, результата отправки формы, процессу обработки данных; Расширение базы данных с помощью операции добавления.
- `PUT` заменяет все текущие представления ресурса данными запроса. Метод запроса HTTP PUT создаёт новый ресурс или заменяет представление целевого ресурса, данными представленными в теле запроса.Разница между PUT и POST в том, что PUT является идемпотентным, т.е. единичный и множественные вызовы этого метода, с идентичным набором данных, будут иметь тот же результат выполнения (без сторонних эффектов), в случае с POST, множественный вызов с идентичным набором данных может повлечь за собой сторонние эффекты. Если целевой ресурс не содержит отправляемой сущности и PUT запрос создаёт её, то сервер должен проинформировать клиентское приложение о создании, отправив в ответ 201 (Created). HTTP/1.1 201 Created Content-Location: /new.html Если целевой ресурс содержит отправляемую сущность и сущность была успешно мутирована (обновлена), в соответствии с прилагаемыми в теле запроса данными, то сервер должен отправить или 200 (OK), или 204 (No Content) для информирования об успешном завершении запроса. HTTP/1.1 204 No Content Content-Location: /existing.html
- `DELETE` удаление If a DELETE method is successfully applied, there are several response status codes possible: A 202 (Accepted) status code if the action will likely succeed but has not yet been enacted. A 204 (No Content) status code if the action has been enacted and no further information is to be supplied. A 200 (OK) status code if the action has been enacted and the response message includes a representation describing the status.
- **HEAD** запрашивает ресурс так же, как и метод GET, но без тела ответа. The HTTP HEAD method requests the headers that would be returned if the HEAD request's URL was instead requested with the HTTP GET method. For example, if a URL might produce a large download, a HEAD request could read its Content-Length header to check the filesize without actually downloading the file.Warning: A response to a HEAD method should not have a body. If it has one anyway, that body must be ignored: any representation headers that might describe the erroneous body are instead assumed to describe the response which a similar GET request would have received.If the response to a HEAD request shows that a cached URL response is now outdated, the cached copy is invalidated even if no GET request was made.
- **OPTIONS** используется для описания параметров соединения с ресурсом.
- **CONNECT** The HTTP CONNECT method starts two-way communications with the requested resource. It can be used to open a tunnel. For example, the CONNECT method can be used to access websites that use SSL (HTTPS). The client asks an HTTP Proxy server to tunnel the TCP connection to the desired destination. The server then proceeds to make the connection on behalf of the client. Once the connection has been established by the server, the Proxy server continues to proxy the TCP stream to and from the client. CONNECT is a hop-by-hop method.

HTTP определяет множество методов запроса, которые указывают, какое желаемое действие выполнится для данного ресурса. Несмотря на то, что их названия могут быть существительными, эти методы запроса иногда называются HTTP глаголами. Каждый реализует свою семантику, но каждая группа команд разделяет общие свойства: так, методы могут быть безопасными, идемпотентными или кешируемыми.

### **Безопасный метод**

Метод HTTP является безопасным, если он не меняет состояние сервера. Другими словами, безопасный метод проводит операции "только чтение" (read-only). Несколько следующих методов HTTP безопасные: GET, HEAD или OPTIONS. Все безопасные методы являются также идемпотентными, как и некоторые другие, но при этом небезопасные, такие как PUT или DELETE.

Даже если безопасные методы являются по существу "только для чтения", сервер всё равно может сменить своё состояние: например, он может сохранять статистику. Что существенно, так то, когда клиент вызывает безопасный метод, то он не запрашивает никаких изменений на сервере, и поэтому не создаёт дополнительную нагрузку на сервер. Браузеры могут вызывать безопасные методы, не опасаясь причинить вред серверу: это позволяет им выполнять некоторые действия, например, предварительная загрузка без риска. Поисковые роботы также полагаются на вызовы безопасных методов.

Безопасные методы не обязательно должны обрабатывать только статичные файлы; сервер может генерировать ответ "на-лету", пока скрипт, генерирующий ответ, гарантирует безопасность: он не должен вызывать внешних эффектов, таких как формирование заказов, отправка писем и др..

Правильная реализация безопасного метода - это ответственность серверного приложения, потому что сам веб-сервер, будь то Apache, nginx, IIS это соблюсти не сможет. В частности, приложение не должно разрешать изменение состояния сервера запросами GET.

### **Идемпотентный метод**

Метод HTTP является идемпотентным, если повторный идентичный запрос, сделанный один или несколько раз подряд, имеет один и тот же эффект, не изменяющий состояние сервера. Другими словами, идемпотентный метод не должен иметь никаких побочных эффектов (side-effects), кроме сбора статистики или подобных операций. Корректно реализованные методы GET, HEAD, PUT и DELETE идемпотентны, но не метод POST. Также все безопасные методы являются идемпотентными.

Для идемпотентности нужно рассматривать только изменение фактического внутреннего состояния сервера, а возвращаемые запросами коды статуса могут отличаться: первый вызов DELETE вернёт код 200, в то время как последующие вызовы вернут код 404. Из идемпотентности DELETE неявно следует, что разработчики не должны использовать метод DELETE при реализации RESTful API с функциональностью удалить последнюю запись.

Обратите внимание, что идемпотентность метода не гарантируется сервером, и некоторые приложения могут нарушать ограничение идемпотентности.

GET /pageX HTTP/1.1 идемпотентен. Вызвавший несколько раз подряд этот запрос, клиент получит тот же результат:

## **Кешируемые методы**

Кешируемые ответы - это HTTP-ответы, которые могут быть закешированы, то есть сохранены для дальнейшего восстановления и использования позже, тем самым снижая число запросов к серверу. Не все HTTP-ответы могут быть закешированы. Вот несколько ограничений:

Метод, используемый в запросе, кешируемый, если это GET или HEAD. Ответ для POST или PATCH запросов может также быть закеширован, если указан признак "свежести" данных и установлен заголовок Content-Location (en-US), но это редко реализуется. (Например, Firefox не поддерживает это согласно https://bugzilla.mozilla.org/show_bug.cgi?id=109553.) Другие методы, такие как PUT и DELETE не кешируемые, и результат их выполнения не кешируется.

Коды ответа, известные системе кеширования, которые рассматриваются как кешируемые: 200, 203, 204, 206, 300, 301, 404, 405, 410, 414, 501.

Отсутствуют специальные заголовки в ответе, которые предотвращают кеширование: например, Cache-Control.

Обратите внимание, что некоторые некешируемые запросы-ответы к определённым URI могут сделать недействительным (инвалидируют) предыдущие закешированные ответы на тех же URI. Например, PUT к странице pageX.html инвалидируют все закешированные ответы GET или HEAD запросов к этой странице.

Когда и метод запроса и статус ответа кешированы, то ответ к запросу тоже может быть закеширован:

Запрос PUT не может быть закеширован. Более того, он инвалидирует закешированные данные запросов к тому же URI, сделанных через HEAD или GET:

Специальный заголовок Cache-Control в ответе может предотвратить кеширование:

[![](https://lh5.googleusercontent.com/cF-OW31Gzwf4y0Vw249X6F7cErG3qblHY-3tbnRARUfjUQgXKzlHh64dW_vgiWkr_SBoYSOETQcp4CqziWQOd-2mppoc4RJQ-n20FMrlrqy7NjavSPSUO02pndVRWsAqGEvM_lxtG9hJW4DsVYHCokvm41jjsTZK4W2YMEKHedCxDwyaFd7wUDrrkjgM)](https://lh5.googleusercontent.com/cF-OW31Gzwf4y0Vw249X6F7cErG3qblHY-3tbnRARUfjUQgXKzlHh64dW_vgiWkr_SBoYSOETQcp4CqziWQOd-2mppoc4RJQ-n20FMrlrqy7NjavSPSUO02pndVRWsAqGEvM_lxtG9hJW4DsVYHCokvm41jjsTZK4W2YMEKHedCxDwyaFd7wUDrrkjgM)

## **Коды ошибок**

1хх - запрос был получен

2хх - запрос успешно получен и обработан

3хх - redirection

4хх - запрос клиента неверный и не может быть выполнен

5хх - ошибка сервера

## **HTTP сессия**

1. Клиент устанавливает TCP соединения (или другое соединение, если не используется TCP транспорт).
2. Клиент отправляет запрос и ждёт ответа.
3. Сервер обрабатывает запрос и посылает ответ, в котором содержится код статуса и соответствующие данные.

Начиная с версии HTTP/1.1, после третьей фазы соединение не закрывается, так как клиенту позволяется инициировать другой запрос. То есть, вторая и третья фазы могут повторяться.

[![](https://lh6.googleusercontent.com/OTl9mhoBpNhDpUQWhhZROSJ_b01VyLp5moJcQjo7ilQoOZEiLhoIAM2ApTLSJ4oM_OpXbKQH476a-9xew2nPTg3XB3Lzon_5D6J3r7xNg9uMAc8bu_RMRsDVSrSlEQL4TmNjff-u7E0kKskAwKc1P7SqWyhpvFnNoESVSXflSKnTELURLj4ZG3vuuWr7)](https://lh6.googleusercontent.com/OTl9mhoBpNhDpUQWhhZROSJ_b01VyLp5moJcQjo7ilQoOZEiLhoIAM2ApTLSJ4oM_OpXbKQH476a-9xew2nPTg3XB3Lzon_5D6J3r7xNg9uMAc8bu_RMRsDVSrSlEQL4TmNjff-u7E0kKskAwKc1P7SqWyhpvFnNoESVSXflSKnTELURLj4ZG3vuuWr7)

HTTP protocol and Web Servers are stateless, what it means is that for web server every request is a new request to process and they can’t identify if it’s coming from client that has been sending request previously.

But sometimes in web applications, we should know who the client is and process the request accordingly. For example, a shopping cart application should know who is sending the request to add an item and in which cart the item has to be added or who is sending checkout request so that it can charge the amount to correct client.

Session is a conversional state between client and server and it can consists of multiple request and response between client and server. Since HTTP and Web Server both are stateless, the only way to maintain a session is when some unique information about the session (session id) is passed between server and client in every request and response.

There are several ways through which we can provide unique identifier in request and response.

User Authentication – This is the very common way where we user can provide authentication credentials from the login page and then we can pass the authentication information between server and client to maintain the session. This is not very effective method because it wont work if the same user is logged in from different browsers.HTML Hidden Field – We can create a unique hidden field in the HTML and when user starts navigating, we can set its value unique to the user and keep track of the session. This method can’t be used with links because it needs the form to be submitted every time request is made from client to server with the hidden field. Also it’s not secure because we can get the hidden field value from the HTML source and use it to hack the session.URL Rewriting – We can append a session identifier parameter with every request and response to keep track of the session. This is very tedious because we need to keep track of this parameter in every response and make sure it’s not clashing with other parameters.Cookies – Cookies are small piece of information that is sent by web server in response header and gets stored in the browser cookies. When client make further request, it adds the cookie to the request header and we can utilize it to keep track of the session. We can maintain a session with cookies but if the client disables the cookies, then it won’t work.Session Management API – Session Management API is built on top of above methods for session tracking. Some of the major disadvantages of all the above methods are:Most of the time we don’t want to only track the session, we have to store some data into the session that we can use in future requests. This will require a lot of effort if we try to implement this.All the above methods are not complete in themselves, all of them won’t work in a particular scenario. So we need a solution that can utilize these methods of session tracking to provide session management in all cases.

That’s why we need Session Management API and J2EE Servlet technology comes with session management API that we can use.

## **Content Negotiation**

Generally, the REST resources can have multiple presentations, mostly because there may be different clients expecting different representations. Asking for a suitable presentation by a client is referred to as content negotiation.

- Server-driven - if the selection of the best representation for a response is made by an algorithm located at the server
- Agent-driven - if that selection is made at the agent or client-side

### **Implementations**

- Using HTTP Headers
    - At server side, an incoming request may have an entity attached to it. To determine it’s type, server uses the HTTP request header Content-Type: application/json
    - Determine what type of representation is desired on the client-side, an HTTP header: Accept: application/json
- Using URL Patternshttp://rest.api.com/v1/employees/20423.xml

# **URL**

### **URI**

имя и адрес ресурса в сети, включает в себя URL и URN (https://wiki.merionet.ru/images/vse-chto-vam-nuzhno-znat-pro-devops/1.png)

### **URL**

Адрес ресурса в сети, определяет местонахождение и способ обращения к нему (https://wiki.merionet.ru)

### **URN**

Имя ресурса в сети, определяет только название ресурса, но не говорит как к нему подключиться (images/vse-chto-vam-nuzhno-znat-pro-devops/1.png)

# **Протоколы**

## **UDP**

Способ передачи данных, при котором не требуется предварительное соединение(без явных “рукопожатий”)Таким образом данный способ не является надежным и может быть потеряна часть датаграмм и их дублирование.

## **TCP**

Протокол передачи данных, который требует предварительного соединения

## **DHCP**

Протокол, позволяющий сетевым устройствам автоматически получать IP.

# **DNS**

Система доменных имен представляет собой распределенную систему хранения и обработки информации о доменных зонах. Она необходима для соотнесения ip адресов и соответствующих им символьных имен.