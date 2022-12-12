---
layout: post
title: "[TIL] 為蛇魔 Passkeys 流程在建立帳號的時候，依舊需要帳號跟密碼？"
description: ""
category: 
- TodayILearn
- Golang
tags: ["TIL", "login", "golang"]
---

<img src="../images/2021/image-20220608151748839.png" alt="image-20220608151748839" style="zoom:50%;" />



# 前情提要:

我曾經在 WWDC 結束後有寫了一篇關於 [Passkeys 的整理文章](https://www.evanlin.com/til-apple-passkeys/)。結果前幾天就看到一個有趣的 tweet ，讓我重新的檢視自己熟悉的程度如何？到底真正 Passkeys 的流程（第一次與之後的）有什麼不同？ 這一篇文章將對這個部分做一個整理。



# (2022/12/10更新)如果是第一次創建帳號，需要幾次手續？ 為什麼？

<img src="../images/2022/image-20221212081920686.png" alt="image-20221212081920686" style="zoom:50%;" />

之前看到的 tweet ，裡面有提到為什麼看到 [Yubico/java-webauthn-server 有接近 21 個步驟](https://github.com/Yubico/java-webauthn-server#architecture)？ 這也讓我好奇地去檢查了一下相關流程的原意：

<img src="../images/2022/demo-sequence-diagram.svg" alt="WebAuthn ceremony sequence diagram" style="zoom:80%;" />

(Pic from [https://github.com/Yubico/java-webauthn-server#architecture]( https://github.com/Yubico/java-webauthn-server#architecture))

根據這個元件的說明圖，大家會很好奇：

- **Step: 1~ 4:** 
  - 帳號建立的時候，由於需要支援比較舊型的瀏覽設備（瀏覽器/OS) ，所以這裡還是要帳號跟密碼。整個流程跟以往的資料庫一樣，但是接下來流程有一些不同。
- **Step: 5:**
  - 這裡是參考 [PKCE 的方式](https://www.evanlin.com/go-oauth-pkce/)來生成Challenge ，也就事某個 AES 加密過後的數值。
- **Step 6:**
  - 










# Reference

-  [Apple Doc: Supporting Passkeys](https://developer.apple.com/documentation/authenticationservices/public-private_key_authentication/supporting_passkeys)
-  [WWDC22 Session: Meet passkeys](https://developer.apple.com/videos/play/wwdc2022/10092/)
-   [WWDC21 議程: Move beyond passwords](https://developer.apple.com/videos/play/wwdc2021/10106/)
-  [FIDO2: Web Authentication (WebAuthn)](https://fidoalliance.org/fido2-2/fido2-web-authentication-webauthn/)
-  [Passkeys for web authentication](https://www.hanko.io/blog/passkeys-part-1)
-  [What Apple's WWDC Passkeys Announcement Means for Enterprise IAM](https://blog.hypr.com/what-apples-wwdc-passkeys-announcement-means-for-enterprise-iam)
-  [https://github.com/duo-labs/webauthn](https://github.com/duo-labs/webauthn)
- [WebAuthn.io: A demo of the WebAuthn specification](https://webauthn.io/)
- [What is WebAuthn? How to Authenticate Users Without a Password](https://www.freecodecamp.org/news/intro-to-webauthn/)





