# cc-mock
[Chinese Document](README.md)

## Introduction

cc-mock is an intelligent mock tool. It supports the automatic generation of interface response data in Spring Boot projects. It is convenient to use for front-end joint debugging data before the interface development is completed.

## Software Architecture

- **Project Environment:** JDK 1.8+


## Installation Tutorial

1.  Add the dependency:
    ```xml
    <dependency>
        <groupId>io.github.boren07</groupId>
        <artifactId>cc-mock-api-starter</artifactId>
        <version>1.0.0</version>
    </dependency>
    ```
2.  Enable mock configuration in your configuration file. Remember to disable mock after development is complete.
    ```yaml
    ccmock:
      enabled: true
    ```

## Usage

### Supported Mock Types

| Java Type | Supported Types | Mock Configuration | Notes |
| :--- | :--- | :--- | :--- |
| **Primitive & Wrapper Types** | byte, short, int, long, Byte, Short, Integer, Long, double, float, Double, Float, boolean, Boolean | Refer to `number` configuration | |
| **Character/String** | String, char, Character | Refer to `string` configuration | |
| **Date Types** | Date, LocalDate, LocalTime, LocalDateTime | Refer to `date` configuration | |
| **Decimal** | BigDecimal | Refer to `number` configuration | |
| **Java Objects** | Any Java Bean object, supports nested generics. | | |

### Spring Boot Development

1.  Develop your interface. **Note:** The return type of the interface should use generics to specify the actual data type whenever possible, or use the `@MockResponse` annotation to specify the data type. If `Object` is used to set the return data, cc-mock cannot automatically simulate the response content.

    ```java
    /**
     * Response with generic parameters (Recommended approach)
     */
    @GetMapping("/test2")
    public MyResult<Foo> test2(){
        // Custom logic here, e.g., read from database or other sources
        Foo data = new Foo();
        return new MyResult<>(data);
    }
    ```

2.  Start the project and access `http://localhost:8080/test2`. You will receive the mock result:
    ```json
    {
        "message": "yes",
        "status": "0000000",
        "data": {
            "account": 339.07,
            "age": 681,
            "attrs": [
                {
                    "age": 775,
                    "gender": 379,
                    "name": "架殃桅因蕊险稿刷裕隘"
                },
                {
                    "age": 343,
                    "gender": 4,
                    "name": "夏恍握滇势搓疲泣酉有"
                }
            ],
            "createTime": "1985-09-17 03:52:16",
            "gender": 429,
            "list": [
                "丧冒橇抿诈亡穆樱挺签",
                "娄炉撒盖岂狱彝弧仆弹",
                "锯中应孽榜挂确含狭硷"
            ],
            "map": {
                "棠那苯鳃而廷膊潘古由": 994.50,
                "伐园覆低巧沾萤大性泡": 566.98,
                "泄链宙诗寅仙漏莽击腊": 235.57,
                "凳鸣疏痰绽棉钒银蛔扁": 481.40
            },
            "nickName": "址逃虾霄憨椽苑职貌犀",
            "password": "戳狈单兢杆汪创型慷豁",
            "username": "押相缸冷嫉恶捣卧块射"
        }
    }
    ```
3.  [View the Spring Boot integration case for cc-mock。](https://github.com/boren07/cc-mock/tree/master/cc-mock-samples/spring-boot-sample)


### Custom Mock Example

```java
@Data
public class User {
    private String username;
    private String password;
    private String nickName;
    private List<String> roles;
}

/**
 * Use custom configuration to mock a User object
 */
public static void main(String[] args) {
    MockConfig mockConfig = new MockConfig();
    MockConfig.String string = new MockConfig.String();
    string.setLength(5);
    string.setStringType(StringType.NUMBER_CHAR_MIX);
    mockConfig.setString(string);
    System.out.println(CcMock.mock(mockConfig, User.class));
}
//output result
/**
 * User(username=cxy7x, password=s7n3v, nickName=j3sgy, roles=[s0osm, 6x5mu, 1m6bw, 26wto, jo52h, ro8zy, n6e84, dx5dm, ueke7, 9jhgm])
 */
```
####  Advantages
- Non-invasive: cc-mock enhances the existing Spring Boot framework for mocking, without affecting the current code or architecture.
- Simple & Lightweight: Requires almost only one configuration to enable or disable the mock function, with a large number of automatic mock strategies. There is no performance overhead when mock is disabled.
- Extensible: Supports custom mock strategies and custom mock response structures.
####  Contributing
1. Fork this repository.
2. Create a new branch named Feat_xxx.
3. Submit your code.
4. Open a Pull Request.
####  Version Update Notes
1.0.0: First official release version.