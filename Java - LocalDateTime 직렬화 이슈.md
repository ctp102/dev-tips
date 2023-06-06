## Java8에서 LocalDateTime을 직렬화할 때 발생하는 이슈
원인
- Java 8에서는 java.time.LoaclDateTime 형식의 날짜/시간 유형을 지원하지 않는다.
해결
- com.fasterxml.jackson.datatype.jsr310 패키지를 추가하여 JavaTimeModule 클래스를 ObjectMapper에 등록해준다.
```java
    @Bean
    public ObjectMapper objectMapper() {
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.disable(SerializationFeature.INDENT_OUTPUT);
        objectMapper.disable(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);
        objectMapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS); // 이 옵션 추가
        objectMapper.registerModule(new JavaTimeModule()); // 이 옵션 추가
        return objectMapper;
    }
```
