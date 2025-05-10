# **Spring resources**

Spring дает вам возможность доступа к ресурсам с помощью приятного небольшого синтаксиса. Интерфейс ресурса имеет несколько интересных методов:

- boolean exists();
- String getFilename();
- File getFile() throws IOException;
- InputStream getInputStream() throws IOException;

Использование
- `Resource aClasspathTemplate = ctx.getResource("classpath:somePackage/application.properties");`
- `Resource aFileTemplate = ctx.getResource("file:///someDirectory/application.properties");`
- `Resource anHttpTemplate = ctx.getResource("[https://marcobehler.com/application.properties](https://marcobehler.com/application.properties)");`
- `Resource depends = ctx.getResource("myhost.com/resource/path/myTemplate.txt");`
- `Resource s3Resources = ctx.getResource("s3://myBucket/myFile.txt");`

**@PropertySources**
Установка путей к свойствам

```java
@PropertySources({
		@PropertySource("classpath:/com/${my.placeholder:default/path}/app.properties"),
		@PropertySource("file://myFolder/app-production.properties")
})
```