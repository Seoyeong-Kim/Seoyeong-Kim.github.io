---
layout: post
title: RestTemplate SocketTimeoutException catch
categories: [spring]
excerpt: ""
comments: true
share: true
tags: [rest-template, java, spring]
date: 2020-03-17
---

RestTemplate 사용하여 다른 서비스에 api 호출을 하는데 연동부분에서 누락데이터가 발생했다.
호출한 서비스의 api는 정상적으로 처리되었는데 우리쪽 api는 정상처리가 되지 않았고, TimeOut 문제일까 싶어 찾아보게되었다.  
결과적으로, Spring RestTemplate에서는 SocketTimeoutException을 ResourceAccessException으로 던지므로 아래와 같이
try/catch 할 수 있다.
connectTimeout, readTimeout, connectionRequestTimeout을 변경해가며 테스트를 해 보았다. 
<br>

코드
~~~java
class Test {
    public void test() {
        try {
            CloseableHttpClient httpClient = HttpClientBuilder.create().build();
            HttpComponentsClientHttpRequestFactory requestFactory = new HttpComponentsClientHttpRequestFactory(httpClient);
            requestFactory.setConnectTimeout(1000);
            requestFactory.setReadTimeout(1000);
            requestFactory.setConnectionRequestTimeout(1000);
            RestTemplate restTemplate = new RestTemplate(requestFactory);
        
            restTemplate.getForEntity("http://localhost:8080/v1/codes", Object.class);
        
        } catch (ResourceAccessException e) {
            if(e.getCause() instanceof ConnectTimeoutException) {
                System.out.println("test");
            }
        
            if(e.getCause() instanceof SocketTimeoutException) {
                System.out.println("test");
            }
        
            if(e.getCause() instanceof InterruptedException) {
                System.out.println("test");
            }
        
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
~~~