---
title: java使用jsoup时绕过https证书验证
date: 2021-06-03 20:13:24
tags: "Java"
---
**详细错误信息:**
```
SunCertPathBuilderException: unable to find valid certification path to requested target

```

**问题原因:**
爬相关数据，因该网站有SSL加密，故无法爬取。

**问题解决之核心代码:**
```
 /**
     * 绕过HTTPS验证
     */
    static public void initTSL() {
        try {
            SSLContext context = SSLContext.getInstance("TLS");
            context.init(null, new X509TrustManager[]{new X509TrustManager() {
                @Override
                public void checkClientTrusted(X509Certificate[] chain, String authType) throws CertificateException {
                }

                @Override
                public void checkServerTrusted(X509Certificate[] chain, String authType) throws CertificateException {
                }

                @Override
                public X509Certificate[] getAcceptedIssuers() {
                    return new X509Certificate[0];
                }
            }}, new SecureRandom());
            HttpsURLConnection.setDefaultSSLSocketFactory(context.getSocketFactory());
        } catch (NoSuchAlgorithmException e) {
        } catch (KeyManagementException e) {
        }
    }


```