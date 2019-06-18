# 第10章 使用Maven进行测试

**本章内容**

* account-captcha
* maven-surefirre-plugin简介
* 跳过测试
* 动态指定要运行的测试用例
* 包含与排除测试用例
* 测试报告
* 运行TestNG测试
* 重用测试代码
* 小结

随着敏捷开发模式的日益流行，软件开发人员也越来越认识到日常编程工作中单元测试的重要性。Maven的重要职责之一就是自动运行单元测试，它通过maven-surefire-plugin与主流的单元测试框架JUnit3、JUnit4以及TestNG集成，并且能够自动生成丰富的结果报告。本章将介绍Maven关于测试的一些重要特性，但不会深入解释单元测试框架本身及相关技巧，重点是介绍如何通过Maven控制单元测试的进行。除了测试之外，本章还会进一步丰富账户注册服务的这一背景案例，引入其第三个模块：account-chaptcha。

## 10.1 account-captcha

在讨论maven-surefire-plugin之前，本章先介绍实现账户注册服务的account-captcha模块，该模块负责处理账户注册时验证码的key生成、图片生成以及验证等。读者可以回顾第4章的背景案例以获得更具体的需求信息。

### 10.1.1 account-catcha的POM

### 10.1.2 account-captcha的主代码

### 10.1.3 account-captcha的测试代码

```java
package com.juvenxu.mvnbook.account.captcha;

import org.junit.Test;

import java.util.HashSet;
import java.util.Set;

import static org.junit.Assert.assertFalse;

/**
 * RandomGeneratorTest
 *
 * @author shenhuanjie
 * @date 2019/6/12 23:17
 */
public class RandomGeneratorTest {
    @Test
    public void testGetRandomString() throws Exception {
        Set<String> randoms = new HashSet<>(100);
        for (int i = 0; i < 100; i++) {
            String random = RandomGenerator.getRandomString();
            assertFalse(randoms.contains(random));
            randoms.add(random);
        }
    }
}
```

```java
package com.juvenxu.mvnbook.account.captcha;

import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStream;
import java.util.ArrayList;
import java.util.List;

import static org.junit.Assert.*;

/**
 * AccountCaptchaServiceTest
 *
 * @author shenhuanjie
 * @date 2019/6/12 23:07
 */
public class AccountCaptchaServiceTest {
    private AccountCaptchaService service;

    @Before
    public void prepare() throws Exception {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("account-captcha.xml");
        service = (AccountCaptchaService) applicationContext.getBean("accountCaptchaService");
    }

    @Test
    public void testGenerateCaptcha() throws Exception {
        String captchaKey = service.generateCaptchaKey();
        assertNotNull(captchaKey);

        byte[] captchaImage = service.generateCaptchaImage(captchaKey);
        assertTrue(captchaImage.length > 0);

        File image = new File("target/" + captchaKey + ".jpg");
        OutputStream outputStream = null;
        try {
            outputStream = new FileOutputStream(image);
            outputStream.write(captchaImage);
        } finally {
            if (outputStream != null) {
                outputStream.close();
            }
        }
        assertTrue(image.exists() && image.length() > 0);
    }

    @Test
    public void testValidateCaptchaCorrect() throws Exception {
        List<String> preDefinedTexts = new ArrayList<>();
        preDefinedTexts.add("12345");
        preDefinedTexts.add("abcde");
        service.setPreDefinedTexts(preDefinedTexts);

        String captchaKey = service.generateCaptchaKey();
        service.generateCaptchaImage(captchaKey);
        assertTrue(service.validateCaptcha(captchaKey, "12345"));

        captchaKey = service.generateCaptchaKey();
        service.generateCaptchaImage(captchaKey);
        assertTrue(service.validateCaptcha(captchaKey, "abcde"));
    }

    @Test
    public void testValidateCaptchaIncorrect() throws Exception {
        List<String> preDefinedTexts = new ArrayList<String>();
        preDefinedTexts.add("12345");
        service.setPreDefinedTexts(preDefinedTexts);

        String captchaKey = service.generateCaptchaKey();
        service.generateCaptchaImage(captchaKey);
        assertFalse(service.validateCaptcha(captchaKey, "67890"));
    }
}
```

## 10.2 maven-surefire-plugin简介

## 10.3 跳过测试

## 10.4 动态指定要运行的测试用例

## 10.5 包含与排除测试用例

## 10.6 测试报告

### 10.6.1 基本的测试报告

### 10.6.2 测试覆盖率报告

## 10.7 运行TestNG测试

## 10.8 重用测试代码

## 10.9 小结

本章的主题是Maven与测试的集成，不过在讲述具体的测试技巧之前先实现了背景案例的account-captcha模块，这一模块的测试代码也成了本章其他内容良好的素材。maven-surefire-plugin是Maven背后真正执行测试的插件，它有一组默认的文件名模式来匹配并自动运行测试类，用户还可以使用该插件来跳过测试，动态执行测试类、包含或排除测试等。maven-surefire-plugin能生成基本的测试报告，除此之外还能使用cobertura-maven-plugin生成测试覆盖率报告。

除了主流的JUnit之外，本章还讲述了如何与TestNG集成，最后介绍了如何重用测试代码。

