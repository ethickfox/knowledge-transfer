Hexagonal architecture tries to solve problem of having too messy project structure
Example of hexagonal project
```
    └── yourcompany/
            ├── adapter/      # All required for external communications  
			│   ├── in/                 # Incoming requests adapters      
			│   │   ├── http/       
		    │   │   └── kafka/        
		    │   └── out/                # Outgoing requests adapters      
		    │       ├── persistense/        
		    │       ├── cache/        
		    │       └── kafka/        
		    ├── application/                     
		    │   ├── domain/             # Core logic        
	        │   │   ├── model/          
	        │   │   ├── service/          
		    │   └── port/               # Core API        
		    │       ├── in/        
		    │       └── out/        
		    └── common/           # Neither business logic nor adapters        
```
![](Software_Architecture/_img/Pasted%20image%2020250526210338.png)
## Что это даёт?

Короткий ответ - Слабая Связность

Простой ответ - это отделение “скучного” кода от “логики”. Отделение на таком уровне, что логику можно безболезненно вынести в другой проект.

- Можно полноценно протестировать каждый адаптер, бизнес логику в отрыве от всего остального приложения.
    
- Проще заменить один адаптер на другой, например если вы жили на hibernate, а хотите перейти на jdbc. Или с memcached на redis. У вас есть требования к адаптеру в виде его интерфесов, и эти требования обложены тестами (я надеюсь).
    

Про это и так будет написано в любой книжке по чистому коду