**Познакомимся с библиотекой GSON**

- С помощью **GsonBuilder** можно настроить и создать обработчик  
    JSONGson gson = new GsonBuilder() .setPrettyPrinting() // настраиваем формат вывода .create(); // создаем обработчик  
    
- **Gson** основной класс для работы с Json  
    gson.toJson(users); // Объект в Json  
    gson.fromJson(jsonString, User.class) // Объект из Json  
    

**Все объекты можно разделить на:**

- **JsonElement**
- JsonObject
- JsonArray
- JsonPrimitive

[![](https://lh6.googleusercontent.com/ZG2I_v9pXr7-yB8tOmok_rT4RqM_SbUZCRRuNc5w9X05Pwd01mll43-27_z9y68v8Cn-DhzVPrGO9U8AvduSfBYV-TNJaXwFL6Oxdxk5huNKkeRozQdX8hWF8dJqiJE2j_khs1nDDXaa9sjnfMm6qL5pKUBO64XRuDUsidXJ3ODkZVp_stc7trYL7JYPpSJV)](https://lh6.googleusercontent.com/ZG2I_v9pXr7-yB8tOmok_rT4RqM_SbUZCRRuNc5w9X05Pwd01mll43-27_z9y68v8Cn-DhzVPrGO9U8AvduSfBYV-TNJaXwFL6Oxdxk5huNKkeRozQdX8hWF8dJqiJE2j_khs1nDDXaa9sjnfMm6qL5pKUBO64XRuDUsidXJ3ODkZVp_stc7trYL7JYPpSJV)

**Чтение данных**

- Используя fromJson можно прочитать данные из строки в объект с соответствующими полями. Это может быть объект любого типа, главное, чтобы совпадали поля.  
      
    **gson.fromJson(data, Parameter[].class)  
    gson.fromJson(jsonElement.toString(), User.class)  
    **
- Так же прочитать можно в объекты Gson, для дальнейшей обработки  
      
    **gson.fromJson(usersJson, JsonObject.class)**

**Работа с объектами Json**

- JsonObject можно конвертировать в любой другой объект, а также получить значения из конкретного поля.  
    resultsObjects.getAsJsonArray();  
    resultsObjects.getAsJsonArray("results");  
    resultsObjects.getAsJsonNull();  
    resultsObjects.getAsJsonPrimitive("results");  
    

**Запись данных**

- Достаточно указать переменную для сериализации, gson автоматически ее преобразует и вернет Вам строку

String usersJson = gson.toJson(users)