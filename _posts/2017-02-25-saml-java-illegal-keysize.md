---
layout: page
title: "Java Security: Illegal key size exception"
date: 2017-02-25
categories: Lab
---

During the decryption of SAML IdP response the following exception was thrown:

```java
org.springframework.security.saml.log.SAMLDefaultLogger log
INFO: AuthNRequest;SUCCESS;
org.opensaml.common.binding.security.SAMLProtocolMessageXMLSignatureSecurityPolicyRule evaluate
INFO: SAML protocol message was not signed, skipping XML signature processing
org.opensaml.xml.encryption.Decrypter decryptDataToDOM
SEVERE: Error decrypting the encrypted data element
org.apache.xml.security.encryption.XMLEncryptionException: Illegal key size
Original Exception was java.security.InvalidKeyException: Illegal key size
        at org.apache.xml.security.encryption.XMLCipher.decryptToByteArray(XMLCipher.java:1822)
        at org.opensaml.xml.encryption.Decrypter.decryptDataToDOM(Decrypter.java:596)
        at org.opensaml.xml.encryption.Decrypter.decryptUsingResolvedEncryptedKey(Decrypter.java:795)
        at org.opensaml.xml.encryption.Decrypter.decryptDataToDOM(Decrypter.java:536)
        at org.opensaml.xml.encryption.Decrypter.decryptDataToList(Decrypter.java:459)
        at org.opensaml.xml.encryption.Decrypter.decryptData(Decrypter.java:414)
        at org.opensaml.saml2.encryption.Decrypter.decryptData(Decrypter.java:141)
        at org.opensaml.saml2.encryption.Decrypter.decrypt(Decrypter.java:70)
        at org.springframework.security.saml.websso.WebSSOProfileConsumerImpl.processAuthenticationResponse(WebSSOProfileConsumerImpl.java:199)
        at org.springframework.security.saml.SAMLAuthenticationProvider.authenticate(SAMLAuthenticationProvider.java:82)
        at org.springframework.security.authentication.ProviderManager.authenticate(ProviderManager.java:156)
        at org.springframework.security.saml.SAMLProcessingFilter.attemptAuthentication(SAMLProcessingFilter.java:84)
        at org.springframework.security.web.authentication.AbstractAuthenticationProcessingFilter.doFilter(AbstractAuthenticationProcessingFilter.java:194)
        at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:323)
        at org.springframework.security.web.FilterChainProxy.doFilter(FilterChainProxy.java:174)
        at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:323)
        at org.springframework.security.web.context.SecurityContextPersistenceFilter.doFilter(SecurityContextPersistenceFilter.java:87)
        at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:323)
        at org.springframework.security.saml.metadata.MetadataGeneratorFilter.doFilter(MetadataGeneratorFilter.java:88)
        at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:323)
        at org.springframework.security.web.FilterChainProxy.doFilter(FilterChainProxy.java:174)
        at org.springframework.web.filter.DelegatingFilterProxy.invokeDelegate(DelegatingFilterProxy.java:347)
        at org.springframework.web.filter.DelegatingFilterProxy.doFilter(DelegatingFilterProxy.java:260)
        at weblogic.servlet.internal.FilterChainImpl.doFilter(FilterChainImpl.java:61)
        at weblogic.servlet.internal.WebAppServletContext$ServletInvocationAction.wrapRun(WebAppServletContext.java:3748)
        at weblogic.servlet.internal.WebAppServletContext$ServletInvocationAction.run(WebAppServletContext.java:3714)
        at weblogic.security.acl.internal.AuthenticatedSubject.doAs(AuthenticatedSubject.java:321)
        at weblogic.security.service.SecurityManager.runAs(SecurityManager.java:120)
        at weblogic.servlet.internal.WebAppServletContext.securedExecute(WebAppServletContext.java:2283)
        at weblogic.servlet.internal.WebAppServletContext.execute(WebAppServletContext.java:2182)
        at weblogic.servlet.internal.ServletRequestImpl.run(ServletRequestImpl.java:1491)
        at weblogic.work.ExecuteThread.execute(ExecuteThread.java:256)
        at weblogic.work.ExecuteThread.run(ExecuteThread.java:221)
Caused by: java.security.InvalidKeyException: Illegal key size
        at javax.crypto.Cipher.a(DashoA13*..)
        at javax.crypto.Cipher.init(DashoA13*..)
        at javax.crypto.Cipher.init(DashoA13*..)
        at org.apache.xml.security.encryption.XMLCipher.decryptToByteArray(XMLCipher.java:1820)
        ... 32 more


org.opensaml.xml.encryption.Decrypter decryptDataToDOM
SEVERE: Failed to decrypt EncryptedData using either EncryptedData KeyInfoCredentialResolver or EncryptedKeyResolver + EncryptedKey KeyInfoCredentialResolver
Jan 16, 2017 9:36:08 AM org.opensaml.saml2.encryption.Decrypter decryptData
SEVERE: SAML Decrypter encountered an error decrypting element content
org.opensaml.xml.encryption.DecryptionException: Failed to decrypt EncryptedData
        at org.opensaml.xml.encryption.Decrypter.decryptDataToDOM(Decrypter.java:546)
        at org.opensaml.xml.encryption.Decrypter.decryptDataToList(Decrypter.java:453)
        at org.opensaml.xml.encryption.Decrypter.decryptData(Decrypter.java:414)
        at org.opensaml.saml2.encryption.Decrypter.decryptData(Decrypter.java:141)
        at org.opensaml.saml2.encryption.Decrypter.decrypt(Decrypter.java:70)
        at org.springframework.security.saml.websso.WebSSOProfileConsumerImpl.processAuthenticationResponse(WebSSOProfileConsumerImpl.java:199)
        at org.springframework.security.saml.SAMLAuthenticationProvider.authenticate(SAMLAuthenticationProvider.java:82)
        at org.springframework.security.authentication.ProviderManager.authenticate(ProviderManager.java:156)
        at org.springframework.security.saml.SAMLProcessingFilter.attemptAuthentication(SAMLProcessingFilter.java:84)
        at org.springframework.security.web.authentication.AbstractAuthenticationProcessingFilter.doFilter(AbstractAuthenticationProcessingFilter.java:194)
        at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:323)
        at org.springframework.security.web.FilterChainProxy.doFilter(FilterChainProxy.java:174)
        at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:323)
        at org.springframework.security.web.context.SecurityContextPersistenceFilter.doFilter(SecurityContextPersistenceFilter.java:87)
        at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:323)
        at org.springframework.security.saml.metadata.MetadataGeneratorFilter.doFilter(MetadataGeneratorFilter.java:88)
        at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:323)
        at org.springframework.security.web.FilterChainProxy.doFilter(FilterChainProxy.java:174)
        at org.springframework.web.filter.DelegatingFilterProxy.invokeDelegate(DelegatingFilterProxy.java:347)
        at org.springframework.web.filter.DelegatingFilterProxy.doFilter(DelegatingFilterProxy.java:260)
        at weblogic.servlet.internal.FilterChainImpl.doFilter(FilterChainImpl.java:61)
        at weblogic.servlet.internal.WebAppServletContext$ServletInvocationAction.wrapRun(WebAppServletContext.java:3748)
        at weblogic.servlet.internal.WebAppServletContext$ServletInvocationAction.run(WebAppServletContext.java:3714)
        at weblogic.security.acl.internal.AuthenticatedSubject.doAs(AuthenticatedSubject.java:321)
        at weblogic.security.service.SecurityManager.runAs(SecurityManager.java:120)
        at weblogic.servlet.internal.WebAppServletContext.securedExecute(WebAppServletContext.java:2283)
        at weblogic.servlet.internal.WebAppServletContext.execute(WebAppServletContext.java:2182)
        at weblogic.servlet.internal.ServletRequestImpl.run(ServletRequestImpl.java:1491)
        at weblogic.work.ExecuteThread.execute(ExecuteThread.java:256)
        at weblogic.work.ExecuteThread.run(ExecuteThread.java:221)
org.springframework.security.saml.log.SAMLDefaultLogger log
INFO: AuthNResponse;FAILURE 
        org.opensaml.common.SAMLException: Response doesn't have any valid assertion which would pass subject validation
        at org.springframework.security.saml.websso.WebSSOProfileConsumerImpl.processAuthenticationResponse(WebSSOProfileConsumerImpl.java:229)
        at org.springframework.security.saml.SAMLAuthenticationProvider.authenticate(SAMLAuthenticationProvider.java:82)
        at org.springframework.security.authentication.ProviderManager.authenticate(ProviderManager.java:156)
        at org.springframework.security.saml.SAMLProcessingFilter.attemptAuthentication(SAMLProcessingFilter.java:84)
        at org.springframework.security.web.authentication.AbstractAuthenticationProcessingFilter.doFilter(AbstractAuthenticationProcessingFilter.java:194)
        at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:323)
        at org.springframework.security.web.FilterChainProxy.doFilter(FilterChainProxy.java:174)
        at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:323)
        at org.springframework.security.web.context.SecurityContextPersistenceFilter.doFilter(SecurityContextPersistenceFilter.java:87)
        at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:323)
        at org.springframework.security.saml.metadata.MetadataGeneratorFilter.doFilter(MetadataGeneratorFilter.java:88)
        at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:323)
        at org.springframework.security.web.FilterChainProxy.doFilter(FilterChainProxy.java:174)
        at org.springframework.web.filter.DelegatingFilterProxy.invokeDelegate(DelegatingFilterProxy.java:347)
        at org.springframework.web.filter.DelegatingFilterProxy.doFilter(DelegatingFilterProxy.java:260)
        at weblogic.servlet.internal.FilterChainImpl.doFilter(FilterChainImpl.java:61)
        at weblogic.servlet.internal.WebAppServletContext$ServletInvocationAction.wrapRun(WebAppServletContext.java:3748)
        at weblogic.servlet.internal.WebAppServletContext$ServletInvocationAction.run(WebAppServletContext.java:3714)
        at weblogic.security.acl.internal.AuthenticatedSubject.doAs(AuthenticatedSubject.java:321)
        at weblogic.security.service.SecurityManager.runAs(SecurityManager.java:120)
        at weblogic.servlet.internal.WebAppServletContext.securedExecute(WebAppServletContext.java:2283)
        at weblogic.servlet.internal.WebAppServletContext.execute(WebAppServletContext.java:2182)
        at weblogic.servlet.internal.ServletRequestImpl.run(ServletRequestImpl.java:1491)
        at weblogic.work.ExecuteThread.execute(ExecuteThread.java:256)
        at weblogic.work.ExecuteThread.run(ExecuteThread.java:221)
```
AES is limited to 128 bit key size in a default JDK installation due to US export laws. To solve this problem and use 256 bit AES encryption, you need to replace Java Cryptography Extension (JCE) files with the unlimited ones. 
1. Download JCE unlimited Jurisdiction Policy Files (equivalent to the java version you’re using):
  * [Java 6](http://www.oracle.com/technetwork/java/javase/downloads/jce-6-download-429243.html){:target="_blank"}
  * [Java 7](http://www.oracle.com/technetwork/java/javase/downloads/jce-7-download-432124.html){:target="_blank"}
  * [Java 8](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html){:target="_blank"}
2. Extract the zip file
3. Copy `local_policy.jar` and `US_export_policy.jar` to `${java.home}/jre/lib/security/`.
