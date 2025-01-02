
# 简介

SpringSecurity是Spring安全框架的一种，这里是[哔哩哔哩视频](https://www.bilibili.com/video/BV1mm4y1X7Hc/)，以下内容是对SpringSecurity补充。

# Spring Security替换WebSecurityConfigurerAdapter(Deprecated)的方法

## 简介

在本文中，我将提供一个解决方案来配置Spring 安全性，而无需 WebSecurityConfigurerAdapter 类。从 Spring Security 5.7 开始，WebSecurityConfigurerAdapter类已被弃用，Spring 团队鼓励用户转向基于组件的安全配置。

## 使用 WebSecurityConfigurerAdapter

在WebSecurityConfigurerAdapter类被弃用之前，我们正在编写这样的代码。我们创建了一个Spring Java配置类，它扩展了WebSecurityConfigurerAdapter类并覆盖了几个configure()方法：

```java
@Configuration
@EnableWebSecurity
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // configure HTTP security...         
    }
 
    @Override
    public void configure(WebSecurity web) throws Exception {         
        // configure Web security...         
    }      
}
```

从春季安全 5.7.0-M2 开始。WebSecurityConfigurerAdapter类已被弃用，Spring 团队鼓励用户转向基于组件的安全配置。

## 没有网络安全配置器适配器

在使用基于组件的 Spring 安全配置的新方法中，您需要遵循以下非常简单的步骤：

+ 删除 WebSecurityConfigurerAdapter 类（不要扩展WebSecurityConfigurerAdapter）

+ 删除网络安全配置器适配器类的所有重写方法

+ 使用SecurityFilterChain 配置 HttpSecurity，使用WebSecurityCustomizer 配置 WebSecurity：

```java
@Configuration
@EnableWebSecurity
public class SecurityConfiguration {
         
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
      // configure HTTP security...     
    }
     
    @Bean
    public WebSecurityCustomizer webSecurityCustomizer() {
          // configure Web security...   
    }    
}
```

让我们看一个完整的示例供您参考。

## 没有网络安全的 Spring 安全性配置器适配器示例

考虑我们在下面使用WebSecurityConfigurerAdapter类进行 Spring 安全配置，稍后我们将看到如何将此安全配置迁移到基于组件的方法。

```java
package net.javaguides.springboot.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.dao.DaoAuthenticationProvider;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.util.matcher.AntPathRequestMatcher;

import net.javaguides.springboot.service.UserService;

@Configuration
@EnableWebSecurity
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {

	@Autowired
	private UserService userService;
	
	@Bean
    public BCryptPasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
	
	@Bean
    public DaoAuthenticationProvider authenticationProvider() {
        DaoAuthenticationProvider auth = new DaoAuthenticationProvider();
        auth.setUserDetailsService(userService);
        auth.setPasswordEncoder(passwordEncoder());
        return auth;
    }
	
	@Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.authenticationProvider(authenticationProvider());
    }
	
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http.authorizeRequests().antMatchers(
				 "/registration**",
	                "/js/**",
	                "/css/**",
	                "/img/**").permitAll()
		.anyRequest().authenticated()
		.and()
		.formLogin()
		.loginPage("/login")
		.permitAll()
		.and()
		.logout()
		.invalidateHttpSession(true)
		.clearAuthentication(true)
		.logoutRequestMatcher(new AntPathRequestMatcher("/logout"))
		.logoutSuccessUrl("/login?logout")
		.permitAll();
	}

}
```

接下来，这是没有 WebSecurityConfigurerAdapter 的基于组件的替代方法：

```java
package net.javaguides.springboot.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.util.matcher.AntPathRequestMatcher;

@Configuration
@EnableWebSecurity
public class SpringSecurity {

//    @Autowired
//    private UserDetailsService userDetailsService;

    @Bean
    public static PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    // configure SecurityFilterChain
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf().disable()
                .authorizeRequests()
                .antMatchers("/register/**").permitAll()
                .antMatchers("/index").permitAll()
                .antMatchers("/users").hasRole("ADMIN")
                .and()
                .formLogin(
                        form -> form
                                .loginPage("/login")
                                .loginProcessingUrl("/login")
                                .defaultSuccessUrl("/users")
                                .permitAll()
                ).logout(
                        logout -> logout
                                .logoutRequestMatcher(new AntPathRequestMatcher("/logout"))
                                .permitAll()

                );
        return http.build();
    }

//    @Autowired
//    public void configureGlobal(AuthenticationManagerBuilder builder) throws Exception {
//        builder.userDetailsService(userDetailsService)
//                .passwordEncoder(passwordEncoder());
//    }
}
```

在上面的例子中，我们遵循最佳实践，使用 Spring Security lambda DSL 和方法HttpSecurity#authorizeHttpRequests来定义我们的授权规则。如果你不熟悉lambda DSL，你可以在这篇博文中阅读它。

重要：我们不再需要手动将UserDetailsService和PasswordEncoder设置为AuthenticationManager实例，它只需要存在于spring上下文中。一旦我们将UserDetailsService和PasswordEncoder配置为Spring bean，Spring Security就会自动设置为AuthenticationManager。

## Spring Security JWT （JSON Web Token） without WebSecurityConfigurerAdapter

考虑我们有以下的 spring 安全性和使用WebSecurityConfigurerAdapter类的 JWT 配置，稍后我们将看到如何将此安全配置迁移到基于组件的方法。

```java
package com.springboot.blog.config;

import com.springboot.blog.security.CustomUserDetailsService;
import com.springboot.blog.security.JwtAuthenticationEntryPoint;
import com.springboot.blog.security.JwtAuthenticationFilter;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpMethod;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private CustomUserDetailsService userDetailsService;

    @Autowired
    private JwtAuthenticationEntryPoint authenticationEntryPoint;

    @Bean
    public JwtAuthenticationFilter jwtAuthenticationFilter(){
        return  new JwtAuthenticationFilter();
    }

    @Bean
    PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .csrf().disable()
                .exceptionHandling()
                .authenticationEntryPoint(authenticationEntryPoint)
                .and()
                .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                .antMatchers(HttpMethod.GET, "/api/v1/**").permitAll()
                .antMatchers("/api/v1/auth/**").permitAll()
                .antMatchers("/v2/api-docs/**").permitAll()
                .antMatchers("/swagger-ui/**").permitAll()
                .antMatchers("/swagger-resources/**").permitAll()
                .antMatchers("/swagger-ui.html").permitAll()
                .antMatchers("/webjars/**").permitAll()
                .anyRequest()
                .authenticated();
        http.addFilterBefore(jwtAuthenticationFilter(), UsernamePasswordAuthenticationFilter.class);
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService)
                .passwordEncoder(passwordEncoder());
    }

    @Override
    @Bean
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }

    //    @Override
//    @Bean
//    protected UserDetailsService userDetailsService() {
//        UserDetails ramesh = User.builder().username("ramesh").password(passwordEncoder()
//                .encode("password")).roles("USER").build();
//        UserDetails admin = User.builder().username("admin").password(passwordEncoder()
//                .encode("admin")).roles("ADMIN").build();
//        return new InMemoryUserDetailsManager(ramesh, admin);
//    }
}
```

接下来，这是没有 WebSecurityConfigurerAdapter 的基于组件的替代方法：

```java
package com.springboot.blog.config;

import com.springboot.blog.security.CustomUserDetailsService;
import com.springboot.blog.security.JwtAuthenticationEntryPoint;
import com.springboot.blog.security.JwtAuthenticationFilter;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpMethod;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig {

    @Autowired
    private CustomUserDetailsService userDetailsService;

    @Autowired
    private JwtAuthenticationEntryPoint authenticationEntryPoint;

    @Bean
    public JwtAuthenticationFilter jwtAuthenticationFilter(){
        return  new JwtAuthenticationFilter();
    }

    @Bean
    PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Bean
    protected SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
                .csrf().disable()
                .exceptionHandling()
                .authenticationEntryPoint(authenticationEntryPoint)
                .and()
                .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests((authorize) -> authorize
                        .antMatchers(HttpMethod.GET, "/api/v1/**").permitAll()
                        .antMatchers("/api/v1/auth/**").permitAll()
                        .antMatchers("/v2/api-docs/**").permitAll()
                        .antMatchers("/swagger-ui/**").permitAll()
                        .antMatchers("/swagger-resources/**").permitAll()
                        .antMatchers("/swagger-ui.html").permitAll()
                        .antMatchers("/webjars/**").permitAll()
                        .anyRequest()
                        .authenticated()
                );
        http.addFilterBefore(jwtAuthenticationFilter(), UsernamePasswordAuthenticationFilter.class);
        return http.build();
    }


//    @Override
//    protected void configure(HttpSecurity http) throws Exception {
//        http
//                .csrf().disable()
//                .exceptionHandling()
//                .authenticationEntryPoint(authenticationEntryPoint)
//                .and()
//                .sessionManagement()
//                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
//                .and()
//                .authorizeRequests()
//                .antMatchers(HttpMethod.GET, "/api/v1/**").permitAll()
//                .antMatchers("/api/v1/auth/**").permitAll()
//                .antMatchers("/v2/api-docs/**").permitAll()
//                .antMatchers("/swagger-ui/**").permitAll()
//                .antMatchers("/swagger-resources/**").permitAll()
//                .antMatchers("/swagger-ui.html").permitAll()
//                .antMatchers("/webjars/**").permitAll()
//                .anyRequest()
//                .authenticated();
//        http.addFilterBefore(jwtAuthenticationFilter(), UsernamePasswordAuthenticationFilter.class);
//    }

    @Bean
    public AuthenticationManager authenticationManager(
            AuthenticationConfiguration authenticationConfiguration) throws Exception {
        return authenticationConfiguration.getAuthenticationManager();
    }

//    @Override
//    @Bean
//    public AuthenticationManager authenticationManagerBean() throws Exception {
//        return super.authenticationManagerBean();
//    }

    //    @Override
//    @Bean
//    protected UserDetailsService userDetailsService() {
//        UserDetails ramesh = User.builder().username("ramesh").password(passwordEncoder()
//                .encode("password")).roles("USER").build();
//        UserDetails admin = User.builder().username("admin").password(passwordEncoder()
//                .encode("admin")).roles("ADMIN").build();
//        return new InMemoryUserDetailsManager(ramesh, admin);
//    }
}
```

# SpringSecurity邮箱验证码登录配置方式

## 简介

实现邮箱认证码登录步骤：

+ 电子邮件原理

+ 开发邮箱验证码接口

+ 校验邮箱验证码并登录

+ 重构代码

## 发送接收电子邮件原理

**电子邮件服务器**

用户要在Internet上提供电子邮件功能，必须有专门的电子邮件服务器。

邮件服务器就好像是互联网世界的邮局。可以划分为两种类型：

+ SMTP邮件服务器：用户替用户发送邮件外面发送给本地用户的邮件。(邮递员)

+ POP3/IMAP邮件服务器：用户帮助用户读取SMTP邮件服务器接收进来的邮件。(门前邮递箱)

**电子邮箱**

而电子邮箱(163、qq、gmail)其实就是用户在电子邮件服务器上申请的账户

**邮件客户端**

而邮件客户端(FoxMail、Outlook等)集发送和收发功能于一体，帮助用户将邮件发送给SMTP邮件服务器和从POP3/IMAP邮件服务器读取用户的电子邮件。

**邮件传输协议**

+ 简单邮件传输协议（Simple Mail Transfer Protocol，SMTP）：定义了客户端和SMTP邮件发送服务器之间，以及两台SMTP邮件服务器之间的通信规则。

+ 邮局协议（Post Office Protocol，POP3）：定义了客户端和POP3邮件接收服务器的通信规则。

+ 消息访问协议 （Internet Message Access Protocol，IMAP）：对POP3协议的一种扩展，也是定义了客户端和IMAP邮件服务器的通信规则。

**邮件格式**

邮件内容的基本格式和具体细节分别是由RFC822文档和MIME协议定义的。

+ RFC822文档

    定义的文件格式包括两个部分：邮件头、邮件体。

+ 复杂邮件体的格式（Multipurpose Internet Mail Extensions，MIME）

    + 可以表达多段平行的文本内容和非文本的邮件内容，例如，在邮件体中内嵌的图像数据和邮件附件等。

    + MIME协议的数据格式也可以避免邮件内容在传输过程中发生信息丢失。

    + MIME协议不是对RFC822邮件格式的升级和替代，而是基于RFC822邮件格式的扩展应用。

一言以蔽之，RFC822定义了邮件内容的格式和邮件头字段的详细细节，MIME协议则是定义了如何在邮件体部分表达出的丰富多样的数据内容。

**电子邮件发送和接收流程**

![1](https://github.com/user-attachments/assets/1305a4ab-6944-45eb-a9a9-9c6391a094bc)

+ 用户A的电子邮箱为：xx@qq.com，通过邮件客户端软件写好一封邮件，交到QQ的邮件服务器，这一步使用的协议是SMTP,对应图示的①；

+ QQ邮箱服务器 会根据用户A发送的邮件进行解析，根据收件地址判断是否是自己管辖的账户

    如果收件地址也是 QQ邮箱服务器 内，那么会直接存放到自己的存储空间。

    如果发送给其他服务器（如163），那么QQ邮箱就会将邮件转发到163邮箱服务器，转发使用SMTP协议，对应图示的②；

+ 163邮箱服务器接收到QQ邮箱服务器转发过来的邮件

    会判断收件地址是否在自己内部，发现是自己的账户，那么就会将QQ邮箱转发过来的邮件存放到自己的内部存储空间，对应图示的③；

+ 用户B会通过邮件客户端软件先向163邮箱服务器请求，要求收取自己的邮件，对应图示的④；使用的协议是POP3

+ 163邮箱服务器收到用户B的请求后，会从自己的存储空间中取出B未收取的邮件，对应图示⑤；使用的协议是POP3

+ 163邮箱服务器取出用户B未收取的邮件后，将邮件发给用户B，对应图示的⑥；使用的协议是POP3

## 开发邮箱验证码接口

**邮箱服务器打开smtp服务**

![2](https://github.com/user-attachments/assets/351543f4-f119-4d68-b15c-332841e6d216)

![3](https://github.com/user-attachments/assets/7644f7e3-7ad9-4c14-8253-d5e7716a12a4)

**引入依赖**

```xml
<!--发送邮件:实现邮箱验证码-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

发送email的依赖，springboot帮我们封装好了

可以看到，底层用的sun公司的api

![4](https://github.com/user-attachments/assets/1917cdf7-a60a-4816-8024-6bfe1a3beeca)

```yml
spring:
  mail:
    # 设置邮箱主机
    host: smtp.163.com
    # 非SSL的端口
    port: 25
    # 默认即为smtp
    protocol: smtp
    # 设置用户名
    username: xxxxxxxxxxxxxxxxxx@163.com
    # 设置密码，该处的密码是QQ邮箱开启SMTP的授权码而非登录密码
    password: xxxxxxxxxxxxxxxxxxxxxxxx
    # 默认即为utf8
    default-encoding: utf-8
```

**编写EmailCodeSender实现**

对springboot的sender进一步封装，通过接口能快速更换api

```java
import cn.vshop.security.core.properties.SecurityProperties;
import lombok.Setter;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSender;

/**
 * 邮箱发送类的实现
 *
 * @author alan smith
 * @version 1.0
 * @date 2020/4/6 0:47
 */
@Slf4j
@Setter
public class DefaultEmailCodeSender implements EmailCodeSender {

    @Value("${spring.mail.username}")
    private String from;

    @Autowired
    private JavaMailSender mailSender;

    @Autowired
    private SecurityProperties securityProperties;

    /**
     * 简单文本邮件
     */
    @Override
    public void send(String to, String code) {
        SimpleMailMessage message = new SimpleMailMessage();
        // 设置邮件主题
        //message.setSubject("登录验证码");
        // 邮件的内容
        message.setText(contentBild(code, securityProperties.getCode().getEmail().getExpireIn()));
        // 设置接收者邮箱
        message.setTo(to);
        // 设置发送者邮箱
        message.setFrom(from);
        // 发送
        mailSender.send(message);
    }

    /**
     * 生成邮箱信息
     */
    private String contentBild(String code, int expireIn) {
        StringBuilder sb = new StringBuilder();
        sb.append("您的验证码:");
        sb.append(code);
        sb.append(",有效时效:");
        sb.append(expireIn);
        return sb.toString();
    }


}
```

**编写EmailCodeSender接口**

```java
/**
 * 邮箱发送器接口
 *
 * @author alan smith
 * @version 1.0
 * @date 2020/4/6 0:42
 */
public interface EmailCodeSender {
    /**
     * 发送验证码到目标邮箱
     *
     * @param to 目标邮箱
     * @param code  验证码
     */
    void send(String to, String code);
}
```

**编写校验码生成器EmailCodeGenerator**

```java
import cn.vshop.security.core.properties.SecurityProperties;
import cn.vshop.security.core.validate.code.ValidateCode;
import cn.vshop.security.core.validate.code.ValidateCodeGenerator;
import org.springframework.beans.factory.annotation.Autowired;

import javax.servlet.http.HttpServletRequest;
import java.util.Random;

/**
 * @author alan smith
 * @version 1.0
 * @date 2020/4/6 11:33
 */
public class EmailCodeGenerator implements ValidateCodeGenerator {

    @Autowired
    private SecurityProperties securityProperties;

    @Override
    public ValidateCode generate(HttpServletRequest request) {
        return new ValidateCode(getCode(), securityProperties.getCode().getEmail().getExpireIn());
    }

    private Random random = new Random();

    private String getCode() {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < securityProperties.getCode().getEmail().getLength(); i++) {
            sb.append(random.nextInt(10));
        }
        return sb.toString();
    }

}
```

**修改ValidateCodeController**

在ValidateCodeController中添加接口，和相应的依赖

按情况修改前面代码的名称，使代码读起来更加合理

```java
import cn.vshop.security.core.validate.code.email.EmailCodeSender;
import cn.vshop.security.core.validate.code.image.ImageCode;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.social.connect.web.HttpSessionSessionStrategy;
import org.springframework.social.connect.web.SessionStrategy;
import org.springframework.web.bind.ServletRequestBindingException;
import org.springframework.web.bind.ServletRequestUtils;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.context.request.ServletWebRequest;

import javax.imageio.ImageIO;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author alan smith
 * @version 1.0
 * @date 2020/4/5 1:33
 */
@RestController
public class ValidateCodeController {

    /**
     * 图片校验码信息存入session中的key
     */
    public static final String IMAGE_SESSION_KEY = "SESSION_KEY_IMAGE_CODE";

    /**
     * 邮箱校验码信息存入session中的key
     */
    public static final String EMAIL_SESSION_KEY = "SESSION_KEY_EMAIL_CODE";

    /**
     * 注入接口，接口的实现完成【图片】验证码的生成和封装
     */
    @Autowired
    @Qualifier("imageCodeGenerator")
    private ValidateCodeGenerator imageCodeGenerator;

    /**
     * 注入接口，接口的实现完成【邮箱】验证码的生成和封装
     */
    @Autowired
    @Qualifier("emailCodeGenerator")
    private ValidateCodeGenerator emailCodeGenerator;

    /**
     * 发送邮箱的工具实现
     */
    @Autowired
    @Qualifier("emailCodeSender")
    private EmailCodeSender emailCodeSender ;

    /**
     * Spring 的工具类，用以操作session
     */
    private SessionStrategy sessionStrategy = new HttpSessionSessionStrategy();

    /**
     * 图片验证码接口
     */
    @GetMapping("/code/image")
    public void createCode(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // 生成随机的验证码，并封装为 ImageCode
        ImageCode imageCode = (ImageCode) imageCodeGenerator.generate(request);
        // 获取session，并把键值对存入session
        sessionStrategy.setAttribute(
                // ServletWebRequest 是一个适配器（Adapter），把servlet封装成spring的WebRequest（继承了RequestAttributes）
                // 通过把请求传进来，sessionStrategy会从请求中获取session
                new ServletWebRequest(request),
                // 存入session中的key
                IMAGE_SESSION_KEY,
                // 存入session中的值
                imageCode);
        // javax的io工具包
        // 将BufferedImage以指定格式写入输出流中
        ImageIO.write(
                // 以BufferedImage类型的图片
                imageCode.getImage(),
                // 输出的图片格式
                "JPEG",
                // 输出流，输出到响应体中
                response.getOutputStream());

    }

    /**
     * 邮箱验证码接口
     */
    @GetMapping("/code/email")
    public void createSmsCode(HttpServletRequest request, HttpServletResponse response) throws ServletRequestBindingException {
        // 生成邮箱形式的验证码(普通的验证码)
        ValidateCode emailCode = emailCodeGenerator.generate(request);
        // 将验证码放入session
        sessionStrategy.setAttribute(new ServletWebRequest(request), EMAIL_SESSION_KEY, emailCode);
        // 请求参数中获取目标eamil
        String email = ServletRequestUtils.getRequiredStringParameter(request, "email") ;
        // 发送短信
        emailCodeSender.send(email, emailCode.getCode());
    }

}
```

**打开接口的访问权限**

![5](https://github.com/user-attachments/assets/e5d59a91-3cbf-43a2-b23e-de116ca06275)

**修改application.yml**

把登录的界面改成新写的界面

```yml
v:
  security:
    browser:
      # 登录页面
      loginPage: /login3.html
```

**前端代码**

创建新的登录页面 login3.html

![6](https://github.com/user-attachments/assets/a629904f-06e0-4c68-8743-063c33b05750)

前端代码bug不深究

![7](https://github.com/user-attachments/assets/de58af5d-45f7-4198-beed-f685fd8dc581)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<h2>邮箱登录</h2>
<form id="loginForm" method="post" action="/authentication/email">
    邮箱: <input type="text" name="email" value="1191693505@qq.com"><br>
    验证码:<input type="text" name="emailCode">
    <button onclick="sendCode(this)">发送验证码</button>
    <br>
    <input type="checkbox" value="true" name="remember-me">记住我<br>
    <input type="submit" value="登录">
</form>

<script>
    let form = new FormData(document.getElementById("loginForm"));

    function sendCode(o) {
        let xhr = new XMLHttpRequest();
        xhr.open('GET', path());
        xhr.send(null);
        xhr.onload = function () {
            alert("验证码发送成功!");
        }
        time(o);
    }

    function path() {
        let email = form.get("email");
        return "/code/email?email=" + email;
    }

    let wait = 60 ;
    let content = "发送验证码"

    function time(o) {
        if (wait == 0) {
            o.removeAttribute("disabled");
            if (content) o.innerHTML = content;
            wait = 60;
        } else {
            o.setAttribute("disabled", true);
            if (o.innerHTML) content = o.innerHTML;
            o.innerHTML = wait + "秒后可以重新发送";
            wait--;
            setTimeout(() => {
                time(o)
            }, 1000);
        }
    }
</script>

</body>
</html>
```

**访问测试**

http://localhost:8080/login3.html

写入邮箱，

![8](https://github.com/user-attachments/assets/65821308-f03e-4f57-b162-0a33791d7413)

## 重构代码（一）

重构思路是用模板方法把代码（分级）抽出

+ 声明ValidateCodeProcessor接口（处理验证码生成整个流程）

    生成验证码过程抽象为三步：生成、存储、发送

    如果整个验证码逻辑发生改变，只需要借助一级创建一个新接口实现即可。

+ 接口有一个抽象的实现 AbstractValidateCodeProcessor

    实现两步：生成、存储

    其中生成逻辑封装在：ValidateCodeGenerator，根据不同类型认证码给出不同实现

+ 而不一样的部分：发送（邮箱发送、图片发送）则写到子类ImageCodeProcessor和EmailCodeProcessor

![9](https://github.com/user-attachments/assets/b977658c-56a6-445d-b8f5-1dcd2f21a591)

**重构抽象类 generate方法时，用到 spring 常见的开发技巧：依赖查找**

因为图形验证码和邮箱验证码的生成逻辑都是封装在ValidateCodeGenerator接口下面，当我们（下图）形式注入时

![10](https://github.com/user-attachments/assets/464a1add-14d5-4812-94d5-4048b67d56a7)

spring会把实现了接口的bean，以名字作为key，bean的值作为value存入

当收到请求，会从请求中截取认证类型的部分，调用相应的认证类型的 ValidateCodeGenerator

![11](https://github.com/user-attachments/assets/9a3f8c3a-be50-482a-bd9d-77b0485234bb)

## 校验邮箱验证码并登录

仿照（下图）用户密码登录流程，给出邮箱验证码验证流程

![12](https://github.com/user-attachments/assets/cafddc75-3723-496b-b30a-c297295c01d3)

要创建下面几个类：

+ EmailCodeFilter：拦截邮件认证请求，校验邮箱认证码是否正确

+ EmailAuthenticationFilter：拦截邮件认证请求，通过邮箱获取角色认证

+ EmailAuthenticationToken：短信认证Token，封装邮件登录信息

+ EmailAuthenticaionProvider：能对邮件认证Token处理的Provider解析成UserDetails

+ EmailUserDetailsService：根据邮箱获取UserDetails

因为无论是浏览器亦或者手机端均会使用短信验证，因此写在 core 模块中。

**编写EmailAuthenticationToken**

直接复制 UsernamePasswordAuthenticationToken,并且去掉Credential(密码)部分即可

![13](https://github.com/user-attachments/assets/4770a1a6-b8a9-47f0-892e-ec4200e5c329)

```java
import org.springframework.security.authentication.AbstractAuthenticationToken;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.SpringSecurityCoreVersion;

import java.util.Collection;

/**
 * 封装登录信息
 * <p>
 * 直接复制 {@link UsernamePasswordAuthenticationToken},并且去掉Credential(密码)部分
 *
 * @author alan smith
 * @version 1.0
 * @date 2020/4/6 22:26
 */
public class EmailCodeAuthenticationToken extends AbstractAuthenticationToken {

    private static final long serialVersionUID = SpringSecurityCoreVersion.SERIAL_VERSION_UID;

    // ~ Instance fields
    // ================================================================================================

    /**
     * 登录前：邮箱
     * 登录后：用户信息
     */
    private final Object principal;

    // ~ Constructors
    // ===================================================================================================

    /**
     * This constructor can be safely used by any code that wishes to create a
     * <code>UsernamePasswordAuthenticationToken</code>, as the {@link #isAuthenticated()}
     * will return <code>false</code>.
     */
    public EmailCodeAuthenticationToken(Object email) {
        super(null);
        this.principal = email;
        setAuthenticated(false);
    }

    /**
     * This constructor should only be used by <code>AuthenticationManager</code> or
     * <code>AuthenticationProvider</code> implementations that are satisfied with
     * producing a trusted (i.e. {@link #isAuthenticated()} = <code>true</code>)
     * authentication token.
     *
     * @param principal
     * @param authorities
     */
    public EmailCodeAuthenticationToken(Object principal, Collection<? extends GrantedAuthority> authorities) {
        super(authorities);
        this.principal = principal;
        super.setAuthenticated(true); // must use super, as we override
    }

    // ~ Methods
    // ========================================================================================================


    @Override
    public Object getCredentials() {
        return null;
    }

    @Override
    public Object getPrincipal() {
        return this.principal;
    }

    @Override
    public void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException {
        if (isAuthenticated) {
            throw new IllegalArgumentException(
                    "Cannot set this token to trusted - use constructor which takes a GrantedAuthority list instead");
        }

        super.setAuthenticated(false);
    }

    @Override
    public void eraseCredentials() {
        super.eraseCredentials();
    }

}
```

**编写EmailAuthenticationFilter**

同样的，复制 UsernamePasswordAuthenticationFilter，并改成 Email 形式

![14](https://github.com/user-attachments/assets/289c6871-5d66-4415-be0e-a0c64030edca)

```java
import org.springframework.security.authentication.AuthenticationServiceException;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.AbstractAuthenticationProcessingFilter;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import org.springframework.security.web.util.matcher.AntPathRequestMatcher;
import org.springframework.util.Assert;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 拦截邮箱验证码请求，并且组装Token
 * <p>
 * 直接复制 {@link UsernamePasswordAuthenticationFilter},并作出相依修改
 *
 * @author alan smith
 * @version 1.0
 * @date 2020/4/6 22:46
 */
public class EmailCodeAuthenticationFilter extends AbstractAuthenticationProcessingFilter {
    // ~ Static fields/initializers
    // =====================================================================================

    // 请求中携带参数的名字
    public static final String SPRING_SECURITY_FORM_EMAIL_KEY = "email";

    private String emailParameter = SPRING_SECURITY_FORM_EMAIL_KEY;
    private boolean postOnly = true;

    // ~ Constructors
    // ===================================================================================================

    public EmailCodeAuthenticationFilter() {
        super(new AntPathRequestMatcher(
                // 匹配的请求
                "/authentication/email", "POST"));
    }

    // ~ Methods
    // ========================================================================================================

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request,
                                                HttpServletResponse response) throws AuthenticationException {
        // 判断当前请求是否为POST请求，如果不是就抛出异常
        if (postOnly && !request.getMethod().equals("POST")) {
            throw new AuthenticationServiceException(
                    "Authentication method not supported: " + request.getMethod());
        }

        String email = obtainEmail(request);

        if (email == null) {
            email = "";
        }

        email = email.trim();

        EmailCodeAuthenticationToken authRequest = new EmailCodeAuthenticationToken(email);

        // 把请求信息放入Token，比如说IP、session
        // Allow subclasses to set the "details" property
        setDetails(request, authRequest);

        // 使用AuthenticationManager进行认证流程
        return this.getAuthenticationManager().authenticate(authRequest);
    }


    /**
     * Enables subclasses to override the composition of the username, such as by
     * including additional values and a separator.
     *
     * @param request so that request attributes can be retrieved
     * @return the username that will be presented in the <code>Authentication</code>
     * request token to the <code>AuthenticationManager</code>
     */
    protected String obtainEmail(HttpServletRequest request) {
        return request.getParameter(emailParameter);
    }

    /**
     * Provided so that subclasses may configure what is put into the authentication
     * request's details property.
     *
     * @param request     that an authentication request is being created for
     * @param authRequest the authentication request object that should have its details
     *                    set
     */
    protected void setDetails(HttpServletRequest request, EmailCodeAuthenticationToken authRequest) {
        authRequest.setDetails(authenticationDetailsSource.buildDetails(request));
    }

    /**
     * Sets the parameter name which will be used to obtain the username from the login
     * request.
     *
     * @param emailParameter the parameter name. Defaults to "username".
     */
    public void setEmailParameter(String emailParameter) {
        Assert.hasText(emailParameter, "Username parameter must not be empty or null");
        this.emailParameter = emailParameter;
    }


    /**
     * Defines whether only HTTP POST requests will be allowed by this filter. If set to
     * true, and an authentication request is received which is not a POST request, an
     * exception will be raised immediately and authentication will not be attempted. The
     * <tt>unsuccessfulAuthentication()</tt> method will be called as if handling a failed
     * authentication.
     * <p>
     * Defaults to <tt>true</tt> but may be overridden by subclasses.
     */
    public void setPostOnly(boolean postOnly) {
        this.postOnly = postOnly;
    }

    public final String getEmailParameter() {
        return emailParameter;
    }

}
```

**编写EmailCodeAuthenticationProvider**

提供对我们自定义的EmailAuthenticationToken的认证提供者。

![15](https://github.com/user-attachments/assets/f0b4b28f-4e53-48b0-9a6e-1050b16ace39)

```java
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.InternalAuthenticationServiceException;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;

/**
 * 邮箱验证码认证的提供者，
 *
 * @author alan smith
 * @version 1.0
 * @date 2020/4/6 23:05
 */
public class EmailCodeAuthenticationProvider implements AuthenticationProvider {

    private UserDetailsService userDetailsService ;

    /**
     * 认证的主要逻辑
     *
     * @param authentication 我们自定义的 EmailCodeAuthenticationToken
     * @return
     * @throws AuthenticationException
     */
    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        // supports方法通过已经说明token为EmailCodeAuthenticationToken，因此可以强转
        EmailCodeAuthenticationToken authenticationToken = (EmailCodeAuthenticationToken) authentication;
        // 此时principal为email，调用（自定义）UserDetailsService，通过email获取UserDetails
        UserDetails user = userDetailsService.loadUserByUsername((String) authenticationToken.getPrincipal());

        if(user==null){
            // 如果查找不到数据，抛出内部服务异常
            // 这个InternalAuthenticationServiceException异常将被视为可处理异常，不会被最终抛出
            throw new InternalAuthenticationServiceException("无法获取用户信息") ;
        }

        // 重新生成（已认证）Token
        EmailCodeAuthenticationToken authenticationResult = new EmailCodeAuthenticationToken(user, user.getAuthorities());
        // 将（未认证）Token中的IP、session等信息放入（已认证）Token中
        authenticationResult.setDetails(authenticationToken.getDetails());

        return authenticationResult;
    }

    /**
     * 判断是否当前认证请求是否是EmailCodeAuthenticationToken
     *
     * @param authentication 当前的请求Token
     * @return 如果是EmailCodeAuthenticationToken或其子类，则返回true，表示支持当前认证
     */
    @Override
    public boolean supports(Class<?> authentication) {
        return EmailCodeAuthenticationToken.class.isAssignableFrom(authentication);
    }
}
```

**编写EmailCodeFilter**

拦截邮件认证请求，校验邮箱认证码是否正确

参考之前写的ImageCodeFilter

![16](https://github.com/user-attachments/assets/a1ba5287-299b-42f3-8384-fdd9b3df35a2)

```java
import cn.vshop.security.core.properties.SecurityProperties;
import cn.vshop.security.core.validate.code.ValidateCode;
import cn.vshop.security.core.validate.code.ValidateCodeException;
import cn.vshop.security.core.validate.code.ValidateCodeProcessor;
import lombok.Setter;
import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang.StringUtils;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.social.connect.web.HttpSessionSessionStrategy;
import org.springframework.social.connect.web.SessionStrategy;
import org.springframework.util.AntPathMatcher;
import org.springframework.web.bind.ServletRequestBindingException;
import org.springframework.web.bind.ServletRequestUtils;
import org.springframework.web.context.request.ServletWebRequest;
import org.springframework.web.filter.OncePerRequestFilter;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.HashSet;
import java.util.Set;

/**
 * 拦截邮箱认证请求，校验邮箱认证码是否正确
 * <p>
 * 每次请求只拦截一次 OncePerRequestFilter
 * 需要初始化 InitializingBean
 *
 * @author alan smith
 * @version 1.0
 * @date 2020/4/7 0:51
 */
@Slf4j
@Setter
public class EmailCodeFilter extends OncePerRequestFilter implements InitializingBean {

    /**
     * spring的工具类，用来匹配Ant风格路径，如：“/user/*”
     */
    private AntPathMatcher pathMatcher = new AntPathMatcher();

    /**
     * 操作session的工具类
     */
    private SessionStrategy sessionStrategy = new HttpSessionSessionStrategy();

    /**
     * 需要拦截的URL
     * (默认添加 /authentication/email)
     */
    private Set<String> urls = new HashSet<>();

    /**
     * (需要手动注入)
     */
    private SecurityProperties securityProperties;

    /**
     * 如果在Spring环境中，会在配置加载后执行
     * （这里需要手动执行）
     *
     * @throws ServletException
     */
    @Override
    public void afterPropertiesSet() throws ServletException {
        super.afterPropertiesSet();
        String[] configUrls = securityProperties.getCode().getEmail().getUrls();
        for (String url : configUrls) {
            urls.add(url);
        }
        urls.add("/authentication/email");

    }

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        // 循环判断是否执行过滤
        boolean action = false;
        for (String url : urls) {
            if (pathMatcher.match(url, request.getRequestURI())) {
                action = true;
                break;
            }
        }

        // 如果是邮箱校验请求，执行邮箱校验逻辑
        if (action) {
            // 尝试校验
            validate(new ServletWebRequest(request, response));
        }

        // 校验通过or不是email校验请求
        filterChain.doFilter(request, response);
    }

    /**
     * 邮箱校验码存储在session中对应的key
     */
    private final static String SESSION_KEY_EMAIL = ValidateCodeProcessor.SESSION_KEY_PREFIX + "EMAIL";

    /**
     * 校验的逻辑，emailCode
     *
     * @param request
     */
    private void validate(ServletWebRequest request) throws ServletRequestBindingException {
        // 从session中获取封装好的ValidateCode
        ValidateCode codeInSession = (ValidateCode) sessionStrategy.getAttribute(request, SESSION_KEY_EMAIL);
        // 从request中获取请求参数ValidateCode
        String codeInRequest = ServletRequestUtils.getStringParameter(request.getRequest(), "emailCode");
        if (StringUtils.isBlank(codeInRequest)) {
            throw new ValidateCodeException("验证码的值不能为空");
        }
        if (codeInSession == null) {
            throw new ValidateCodeException("验证码不存在");
        }
        if (codeInSession.isExpired()) {
            sessionStrategy.removeAttribute(request, SESSION_KEY_EMAIL);
            throw new ValidateCodeException("验证码已过期");
        }
        if (!StringUtils.equalsIgnoreCase(codeInSession.getCode(), codeInRequest)) {
            throw new ValidateCodeException("验证码不匹配");
        }
        sessionStrategy.removeAttribute(request, SESSION_KEY_EMAIL);
    }
}
```

**编写配置类EmailCodeAuthenticationSecurityConfig**

专门进行EmailCode（邮箱验证码）的配置

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.SecurityConfigurerAdapter;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.web.DefaultSecurityFilterChain;
import org.springframework.security.web.authentication.AuthenticationFailureHandler;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import org.springframework.stereotype.Component;

/**
 * 关于短信验证码的配置
 * <p>
 * (因为既要在浏览器中用，也要在app中用，因此写在core内)
 *
 * 写好后只需在应用配置(如BrowserSecurityConfig)中导入配置即可生效
 *
 * @author alan smith
 * @version 1.0
 * @date 2020/4/7 1:47
 */
@Component
public class EmailCodeAuthenticationSecurityConfig
        // HttpSecurity关于DefaultSecurityFilterChain的配置适配器
        extends SecurityConfigurerAdapter<DefaultSecurityFilterChain, HttpSecurity> {

    @Autowired
    private AuthenticationSuccessHandler authenticationSuccessHandler;
    @Autowired
    private AuthenticationFailureHandler authenticationFailureHandler;
    @Autowired
    private UserDetailsService userDetailsService;

    /**
     * 对FilterChain的配置
     * 同{@link WebSecurityConfigurerAdapter}的configure(HttpSecurity http)
     *
     * @param http 封装http的请求响应，可以操作FilterChain
     * @throws Exception
     */
    @Override
    public void configure(HttpSecurity http) throws Exception {
        http
                // 将我们自定义的provider添加到AuthenticationManager管理的provider集合内
                .authenticationProvider(emailCodeAuthenticationProvider())
                // 将我们自定义的filter添加到UsernamePasswordAuthenticationFilter的后面
                // 为什么是后面？
                // 因为其他配置均已UsernamePasswordAuthenticationFilter为基准，把校验码的校验如EmailCodeFilter配置在其之前，
                // 对应的这类的认证过滤器就应配置在其之后
                .addFilterAfter(getEmailCodeAuthenticationFilter(http), UsernamePasswordAuthenticationFilter.class);
    }

    /**
     * 构造邮箱验证码校验过滤器
     *
     * @param http
     */
    private EmailCodeAuthenticationFilter getEmailCodeAuthenticationFilter(HttpSecurity http) {
        // 创建邮箱验证码校验过滤器
        EmailCodeAuthenticationFilter filter = new EmailCodeAuthenticationFilter();
        // 设置认证管理器
        filter.setAuthenticationManager(http.getSharedObject(AuthenticationManager.class));
        // 注册successHandler
        filter.setAuthenticationSuccessHandler(authenticationSuccessHandler);
        // 注册failureHandler
        filter.setAuthenticationFailureHandler(authenticationFailureHandler);
        return filter;
    }

    /**
     * 构造邮箱验证码的provider
     *
     * @return
     */
    private EmailCodeAuthenticationProvider emailCodeAuthenticationProvider() {
        EmailCodeAuthenticationProvider provider = new EmailCodeAuthenticationProvider();
        provider.setUserDetailsService(userDetailsService);
        return provider;
    }
}
```

**修改BrowserSecurityConfig**

在应用配置中导入新加的配置

![17](https://github.com/user-attachments/assets/f275a9c1-5a79-4979-9501-05a6803df59d)

![18](https://github.com/user-attachments/assets/131bf86b-374b-4a0c-bbad-4bc90f585867)

**登录测试**

把两种登录方式放入了同一页面login4.html

**不输入验证码，直接邮箱登录**

![19](https://github.com/user-attachments/assets/0f7184b1-4350-4a3f-a9c5-037eb8a945b8)

![20](https://github.com/user-attachments/assets/ebeb83a7-2b4e-4550-8e71-25c4d9ba5d7a)

随便乱输入

![21](https://github.com/user-attachments/assets/af627fc5-1cc7-4ac3-b812-9422cd506dc6)

![22](https://github.com/user-attachments/assets/0ccbb405-78dd-4211-99a1-d6468961b806)

输入正确验证码

![23](https://github.com/user-attachments/assets/37d827fd-2312-4b8c-b8c9-5e8ec187756f)

![24](https://github.com/user-attachments/assets/1aff7571-fee2-4e8a-94eb-804a7377eb02)

等待60s（验证码过期）

![26](https://github.com/user-attachments/assets/53cd5106-e2d0-4624-9fec-d1ae1fda531a)

同时，图片验证码也是可用的

## 重构代码（二）

**图形验证码和邮箱验证码重复的配置部分**

将重复部分抽出为一个个的类，用apply方法应用其配置，使其生效

![27](https://github.com/user-attachments/assets/554c13e0-c586-49ef-96c2-d23eafb76749)

图形和短信校验码的校验过滤器（validateFilter）合并为一个。

![28](https://github.com/user-attachments/assets/f36d1512-a517-4d9b-b764-d520e2dfdfa3)

![29](https://github.com/user-attachments/assets/dbde8267-bb9a-4f74-834e-6ea4f61dd457)

![30](https://github.com/user-attachments/assets/18e1292c-4301-4a6e-9c4e-5227348073f3)

![31](https://github.com/user-attachments/assets/5674c3e4-b80d-42af-a776-3b0e16370cd8)

![32](https://github.com/user-attachments/assets/81be3fd6-dc7a-41a7-9ef5-57e382ebf52d)

![33](https://github.com/user-attachments/assets/3e52cdb2-8ef2-425c-bc4d-d7210dd68bc1)

![34](https://github.com/user-attachments/assets/71a8537b-4930-4345-813a-a6c92dd2123a)

![35](https://github.com/user-attachments/assets/aca2efcd-6d4c-4942-a166-abbe3d84f2e9)

![36](https://github.com/user-attachments/assets/674221c6-6c17-45ab-8c22-5da8e95f567d)

![37](https://github.com/user-attachments/assets/5f8cadcc-22b7-476f-ac57-9c91a2122b43)