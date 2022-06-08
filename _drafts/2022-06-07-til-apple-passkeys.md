---
layout: post
title: "[TIL][WWDC][Golang] 關於 WWDC22 公佈的 Passkeys 功能，身為後端開發者你該知道什麼?"
description: ""
category: 
- TodayILearn
- Golang
tags: ["TIL", "login", "golang"]
---

<img src="../images/2021/image-20220608151748839.png" alt="image-20220608151748839" style="zoom:50%;" />



# 前情提要:

[WWDC22](https://developer.apple.com/videos/wwdc2022/) 雖然說是 Apple 的開發者大會，但是當天在看 Keynote 的時候，這個 [Passkeys](https://developer.apple.com/documentation/authenticationservices/public-private_key_authentication/supporting_passkeys) 的功能卻讓我相當的驚喜。 於是等到整個細節的議程[ Meet passkeys 的議程 ](https://developer.apple.com/videos/play/wwdc2022/10092/)，整個議程帶來許多新的想法。於是認真的的研究一下，原來 Apple 在 WWDC 21 就已經開發出來 Passkey 的相關流程跟開發方式（參考 [WWDC21 議程: Move beyond passwords](https://developer.apple.com/videos/play/wwdc2021/10106/))。但是到了 iOS16 才有原生支援在手機與 iOS App 端。

# 認證 (Authentication) 的方式到底有幾種？

Passkey 是一個溝通協定，可以比起舊的 Password 機制來說更佳的安全。現在的認證有以下幾種:

- **Memorized passwords:**
  - 就是輸入 ID, PW 的方式。然後用人力來記錄。非常危險，因為經常怕忘記而全部使用同一個。
- **Password manager:**
  - 舉凡 Apple/Chome AutoFill 都算是，這時候你可能不需要紀錄密碼（甚至會幫你產生一組密碼）。但是困擾點就是很難跨系統，甚至只是跨 browser 使用。
- **Security Key:**
  - 很多市售的 USB Security Key (也可以用其他傳輸方式)，可以直接透過 Security Key 登入一些網站。

以上幾種的相關安全度，可以看一下 WWDC 提出的整理。

- - 

![image-20220608161334928](../images/2021/image-20220608161334928.png)

(From:[WWDC21 議程: Move beyond passwords](https://developer.apple.com/videos/play/wwdc2021/10106/) )

![image-20220608152142705](../images/2021/image-20220608152142705.png)

(from [WWDC22 Session: Meet passkeys](https://developer.apple.com/videos/play/wwdc2022/10092/))

# 什麼是 Passkeys ? 

## **Passkey:**

- 最新提出的 Passkey 則是透跟 WebAuthn 與 FIDO2 的認證方式。

![image-20220608170126374](../images/2021/image-20220608170126374.png)

(Edit on [PlantText](https://www.planttext.com/?text=SoWkIImgAStDuU8goIp9ILLuENtEisbx508IYukpKokB5PxFQdc-RTFprJEVjNS-dxBY-UmT81LT3IyMJdwsjV7vYkx73KrSN4_sxWTAlcXeLR3HrRLJUDRP_MpbV2k5bmtzBnlx5DmyNVoD580YJpks0SrxiQhtnTfEZHVjdI-RL-WeFEjfVxwbMvCBeWbY0CHAAuMdJPjVDZGg19GcvMGcAtYdLiAC34zDSYmjoSXJ05lla9gN0lG30000))

# 身為後端開發者，你該怎麼應用?

![image-20220608152153013](../images/2021/image-20220608152153013.png)

# 









# Reference

-  [Apple Doc: Supporting Passkeys](https://developer.apple.com/documentation/authenticationservices/public-private_key_authentication/supporting_passkeys)
-  [WWDC22 Session: Meet passkeys](https://developer.apple.com/videos/play/wwdc2022/10092/)
-   [WWDC21 議程: Move beyond passwords](https://developer.apple.com/videos/play/wwdc2021/10106/)
-  [FIDO2: Web Authentication (WebAuthn)](https://fidoalliance.org/fido2-2/fido2-web-authentication-webauthn/)
-  [Passkeys for web authentication](https://www.hanko.io/blog/passkeys-part-1)
-  [What Apple's WWDC Passkeys Announcement Means for Enterprise IAM](https://blog.hypr.com/what-apples-wwdc-passkeys-announcement-means-for-enterprise-iam)





