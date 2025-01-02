# 简介

SpringDoc是Swagger的优化版本，比较好用，这是[SpringDoc的官方文档](https://springdoc.org/)

# 依赖

```xml
<!--swagger-ui-->
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-ui</artifactId>
    <version>1.5.12</version>
</dependency>
```

# 配置类

```java
import io.swagger.v3.oas.models.Components;
import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Contact;
import io.swagger.v3.oas.models.info.Info;
import io.swagger.v3.oas.models.info.License;
import io.swagger.v3.oas.models.security.SecurityRequirement;
import io.swagger.v3.oas.models.security.SecurityScheme;
import org.springdoc.core.GroupedOpenApi;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;


/**
 * springDoc-swagger标准配置
 *
 * @author huang cheng
 * 2021/8/13
 */
@Configuration
public class SpringDocSwaggerConfig {

    private static final String basePackage = "com.cheng.sunnyday.controller";//需要扫描api路径
    private static final String headerName = "Authorization";//请求头名称

    @Bean
    public GroupedOpenApi usersGroup() {
        return GroupedOpenApi.builder()
                .group("users")
                .addOperationCustomizer((operation, handlerMethod) -> {
                    operation.addSecurityItem(new SecurityRequirement().addList(headerName));
                    return operation;
                })
                .packagesToScan(basePackage)
                .build();
    }

    @Bean
    public OpenAPI customOpenAPI() {
        Components components = new Components();
        //添加右上角的统一安全认证
        components.addSecuritySchemes(headerName,
                new SecurityScheme()
                        .type(SecurityScheme.Type.APIKEY)
                        .scheme("basic")
                        .name(headerName)
                        .in(SecurityScheme.In.HEADER)
                        .description("请求头")
        );

        return new OpenAPI()
                .components(components)
                .info(apiInfo());
    }

    private Info apiInfo() {
        Contact contact = new Contact();
        contact.setEmail("1003816735@qq.com");
        contact.setName("cheng");
        contact.setUrl("https://blog.csdn.net/qq_42495847?spm=1000.2115.3001.5343");
        return new Info()
                .title("sunnyDay-swagger文档")
                .version("1.0")
                .contact(contact)
                .description("博客请关注：https://blog.csdn.net/qq_42495847?spm=1000.2115.3001.5343")
                .license(new License().name("Apache 2.0").url("http://springdoc.org"));
    }

}
```

ApiKey是对请求的header进行设置，第一、二个参数是header的key，第三个参数是用户输入

# 常用注解

|springdoc|注解位置|
|:--|:--:|
|@Tag(tags = “接口类描述”)|Controller类上|
|@Operation(summary =“接口方法描述”)|Controller方法上|
|@Parameters|Controller方法上|
|@Parameter(description=“参数描述”)|Controller方法上@Parameters里|
|@Parameter(description=“参数描述”)|Controller方法的参数上|
|@Parameter(hidden = true)或@Operation(hidden = true)或@Hidden|-|
|@Schema(description = “dto类描述”)|DTO类上|
|@Schema(description = “属性描述”)|DTO属性上|

# 实体类

```java
@Data
@Schema(description ="日记更新参数")
public class JournalUpdateDto {

    @Schema(description ="日记id")
    @NotBlank(message = "日记id不能为空")
    private String id;

    /**
     * 日记内容
     */
    @Schema(description ="日记内容")
    @NotBlank(message = "日记内容不能为空")
    private String content;

    /**
     * 标签
     */
    @Schema(description ="标签")
    private String label;

}
```

# 统一返回类

```java
/**
 * 通用返回类型
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
public class CResponse<T> implements Serializable {

    private static final long serialVersionUID = 1L;
    private String code;//状态码
    private String message;//文字描述
    private T data;//数据

    public CResponse(String code, String message) {
        this(code,message,null);
    }

}
```

# 控制层

```java
import com.cheng.sunnyday.common.constant.SecurityConstant;
import com.cheng.sunnyday.pojo.system.UserInfo;
import com.cheng.sunnyday.pojo.dto.LoginDto;
import com.cheng.sunnyday.pojo.dto.RegisterDto;
import com.cheng.sunnyday.common.http.CResponse;
import com.cheng.sunnyday.pojo.vo.TokenVo;
import com.cheng.sunnyday.service.LoginService;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.Parameter;
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.web.bind.annotation.*;

import javax.annotation.Resource;
import javax.validation.Valid;

/**
 * 用户登录控制
 *
 * @author huang cheng
 * 2021/8/11
 */
@Tag(name = "权限管理")
@RestController
@RequestMapping("/auth")
public class LoginController {

    @Resource
    private LoginService loginService;

    @Operation(summary = "注册")
    @PostMapping("/register")
    public CResponse<TokenVo> register(@RequestBody @Valid RegisterDto registerDto) {
        return loginService.register(registerDto);
    }

    /**
     * 获取token 并更新/插入用户信息
     *
     * @param loginDto 传入该用户可获取到的用户信息
     * @return token 放到Header中的Authorization作为值
     */
    @Operation(summary = "得到token")
    @PostMapping("/getToken")
    public CResponse<TokenVo> getToken(@RequestBody @Valid LoginDto loginDto) {
        return loginService.getToken(loginDto);
    }

    /**
     * 得到当前token中包含的用户信息
     *
     * @param token 令牌
     * @return 用户信息
     */
    @Operation(summary = "得到当前token中包含的用户信息")
    @PostMapping("/getUserInfo")
    public CResponse<UserInfo> getUserInfo(@Parameter(description = "请求头：Authorization") @RequestHeader(SecurityConstant.TOKEN_HEADER) String token) {
        return loginService.getUserInfo(token);
    }


}
```

# 控制器放行地址

如果有spring-Security或者拦截器过滤器之类的配置，需要对以下地址进行放行

```java
    /**
     * 放行Swagger
     */
    public static final String[] SWAGGER_WHITELIST = {
            "/swagger-ui.html",
            "/swagger-ui/**",
            "/swagger-resources/**",
            "/v2/api-docs",
            "/v3/api-docs",
            "/v3/api-docs/swagger-config",
            "/webjars/**",
            "/doc.html",
    };
```

# 常用配置

SpringDoc还有一些常用的配置可以了解下，更多配置可以参考官方文档。

```yml
springdoc:
  swagger-ui:
    # 修改Swagger UI路径
    path: /swagger-ui.html
    # 开启Swagger UI界面
    enabled: true
  api-docs:
    # 修改api-docs路径
    path: /v3/api-docs
    # 开启api-docs
    enabled: true
  # 配置需要生成接口文档的扫描包
  packages-to-scan: com.macro.mall.tiny.controller
  # 配置需要生成接口文档的接口路径
  paths-to-match: /brand/**,/admin/**
```

# 启动

访问localhost:8080/swagger-ui.html

<img width="1673" alt="1" src="https://github.com/user-attachments/assets/a235c7e7-82fc-4060-8da5-57c5b3259a0e" />

基本功能就ok了