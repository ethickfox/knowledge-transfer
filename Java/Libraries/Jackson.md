Jackson is a library for interaction with json. It allows to work with it, by representing JSON data as:

- tokens
- tree
- object

# Tokens

## JsonParser

JsonParser is a base parsing implementation, that used by other classes. It uses tokenization for identification of items in JSON

The JsonFactory is used to create JsonParser instances. The JsonFactory class contains several createParser methods, each taking a different JSON source as parameter.

```Java
JsonFactory factory = new JsonFactory();
JsonParser  parser  = factory.createParser(carJson);
```

Once you have created a Jackson JsonParser you can use it to parse JSON. The way the JsonParser works is by breaking the JSON up into a sequence of tokens which you can iterate one by one.

```Java
try (JsonParser parser = factory.createParser(src)) {
    JsonToken jsonToken;
    do {
        jsonToken = parser.nextToken();
        System.out.println(jsonToken);
    } while (jsonToken != null);
} catch (IOException exception) {
    throw new RuntimeException("Failed to parse file");
}
```

You can use this JsonToken instance to inspect the given token. The token types are represented by a set of constants in the JsonToken class. Such as:

```Plain
START_OBJECT
END_OBJECT
START_ARRAY
END_ARRAY
FIELD_NAME
VALUE_EMBEDDED_OBJECT
VALUE_FALSE
VALUE_TRUE
VALUE_NULL
VALUE_STRING
VALUE_NUMBER_INT
VALUE_NUMBER_FLOAT
```

You can use these constants to find out what type of token the current JsonToken is. To get value of the current token, use getValueAs*(). Here is an example:

```Java
try (JsonParser parser = factory.createParser(src)) {
    JsonToken jsonToken;
    do {
        jsonToken = parser.nextToken();
        if (FIELD_NAME.equals(jsonToken)) {
            String key = parser.getValueAsString();
            jsonToken = parser.nextToken();
            if (jsonToken.isScalarValue()) {
                jsonData.put(key, parser.getValueAsString());
            }
        }
    } while (jsonToken != null);
} catch (IOException exception) {
    throw new RuntimeException("Failed to parse file");
}
```

# Tree

## Using ObjectMapper

JSON can be parsed into a JsonNode object and used to retrieve data from a specific node:

```Java
JsonNode jsonNode = objectMapper.readTree(json);
String color = jsonNode.get("color").asText();
```

### Array

```Java
JsonNode personJsonArray = mapper.readTree(src);
if (personJsonArray.isArray()) {
    for (JsonNode personNode : personJsonArray) {
        people.add(new Person(personNode.get("name").asText(), personNode.get("job").asText()));
    }
}
```

# Object

### **JSON to Java Object**

```Java
Car car = objectMapper.readValue(new File("src/test/resources/json_car.json"), Car.class);
```

### **Java List From a JSON Array String**

```Java
JavaType type = mapper.getTypeFactory().constructCollectionType(List.class, Person.class);
TypeReference<List<Person>> typeReference = new TypeReference<List<Person>>() {};

List<Car> listCar = objectMapper.readValue(jsonCarArray, new TypeReference<List<Car>>(){});
```

**TypeReference**

This generic abstract class is used for obtaining full generics type information by sub-classing; it must be converted to [`ResolvedType`](https://fasterxml.github.io/jackson-core/javadoc/2.2.0/com/fasterxml/jackson/core/type/ResolvedType.html) implementation (implemented by `JavaType` from "databind" bundle) to be used.

  

## Annotations

The **_@JsonAnyGetter_** annotation allows for the flexibility of using a _Map_ field as standard properties.

```Java
public class ExtendableBean {
    public String name;
    private Map<String, String> properties;

    @JsonAnyGetter
    public Map<String, String> getProperties() {
        return properties;
    }
}
======================
{
    "name":"My bean",
    "attr2":"val2",
    "attr1":"val1"
}
```

We can use the **@JsonPropertyOrder** annotation to specify the order of properties on serialization.

```Java
@JsonPropertyOrder({ "name", "id" })
public class MyBean {
    public int id;
    public String name;
}
```

The **_@JsonRawValue_** annotation can instruct Jackson to serialize a property exactly as is.

**_@JsonValue_** indicates a single method that the library will use to serialize the entire instance.

```Java
public enum TypeEnumWithValue {
    TYPE1(1, "Type A"), TYPE2(2, "Type 2");

    private Integer id;
    private String name;

    // standard constructors

    @JsonValue
    public String getName() {
        return name;
    }
}
```

_@JsonRootName_ annotation to indicate the name of this potential wrapper entity

```Java
@JsonRootName(value = "user")
public class UserWithRoot {
    public int id;
    public String name;
}

============
{
    "User": {
        "id": 1,
        "name": "John"
    }
}
```

_@JsonAnySetter_  
 allows us the flexibility of using a   
_Map_  
 as standard properties. On deserialization, the properties from JSON will simply be added to the map.  

```Java
public class ExtendableBean {
    public String name;
    private Map<String, String> properties;

    @JsonAnySetter
    public void add(String key, String value) {
        properties.put(key, value);
    }
}
```

  

Stream API

[https://www.baeldung.com/jackson-streaming-api](https://www.baeldung.com/jackson-streaming-api)

Marshalling

[https://stackoverflow.com/questions/770474/what-is-the-difference-between-serialization-and-marshaling](https://stackoverflow.com/questions/770474/what-is-the-difference-between-serialization-and-marshaling)

Tokenization