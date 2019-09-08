# springboot设置session超时和session监听

2018年09月20日 20:27:23 [陌筱明](https://me.csdn.net/ming19951224) 阅读数 3805

###                              2.0版本以下设置session超时时间

 

1.  springboot 2.0版本以下配置session超时

​     1.1 application.properties配置文件： spring.session.store-type=none

​     1.2 引入 spring-boot和spring-session 2个依赖包

------

```
        <!--session管理-->
        <dependency>
            <groupId>org.springframework.session</groupId>
            <artifactId>spring-session</artifactId>
            <version>1.3.2.RELEASE</version>
        </dependency>
        <!--引入配置-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot</artifactId>
            <version>1.5.4.RELEASE</version>
        </dependency>
```

​     1.3在springboot启动类注入以下bean对象

```
package com.sinosoft.session;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.context.embedded.ConfigurableEmbeddedServletContainer;
import org.springframework.boot.context.embedded.EmbeddedServletContainerCustomizer;
import org.springframework.context.annotation.Bean;

import java.util.concurrent.TimeUnit;

@SpringBootApplication
public class SessionApplication {
    public static void main(String[] args) {

        SpringApplication.run(SessionApplication.class, args
        );
    }

    @Bean
    public EmbeddedServletContainerCustomizer embeddedServletContainerCustomizer() {
        return new EmbeddedServletContainerCustomizer() {
            @Override
            public void customize(ConfigurableEmbeddedServletContainer container) {
                //设置session超时时间为1分钟
                container.setSessionTimeout(1, TimeUnit.MINUTES);
            }
        };
    }
}
```

 

2.添加session管理器和监听器

```
package com.sinosoft.session.server;

import java.util.HashMap;
import javax.servlet.http.HttpSession;

/**
 * Created by  lijunming
 * on  date 2018-09-20
 * session管理器
 * time 20:01
 */
public class MySessionContext {
    private static HashMap mymap = new HashMap();
    
    public static synchronized void AddSession(HttpSession session) {
        if (session != null) {
            mymap.put(session.getId(), session);
        }
    }

    public static synchronized void DelSession(HttpSession session) {
        if (session != null) {
            mymap.remove(session.getId());
        }
    }

    public static synchronized HttpSession getSession(String session_id) {
        if (session_id == null)
            return null;
        return (HttpSession) mymap.get(session_id);
    }
}
```

 

```
package com.sinosoft.session.server;

import org.springframework.stereotype.Component;
import javax.servlet.http.HttpSession;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

/**
 * Created by  lijunming
 * on  date 2018-09-20
 * session监听器
 * time 20:02
 */

@Component
public class MySessionListener implements HttpSessionListener {
	@Override
    public void sessionCreated(HttpSessionEvent httpSessionEvent) {
        System.out.println("session正在創建");
        MySessionContext.AddSession(httpSessionEvent.getSession());
    }
    @Override
    public void sessionDestroyed(HttpSessionEvent httpSessionEvent) {
        HttpSession session = httpSessionEvent.getSession();
        System.out.println("session注銷中");
        MySessionContext.DelSession(session);
    }
```