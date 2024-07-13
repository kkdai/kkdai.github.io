---
layout: post
title: "[Google Cloud/Firebase] 關於 Python Firebase Admin 認證方式 "
description: ""
category: 
- Python 
- TIL
tags: ["python", "firebase", "GoogleCloud"]
---

## 前提

將 Python Firebase 的套件改成官方的 FirebaseAdmin ，以下有兩種認證方式。



## 透過 GOOGLE_CREDENTIALS

- 將 services_account.json 放在環境變數 `GOOGLE_CREDENTIALS`
- 透過以下程式碼啟動
- 這個方式可以在本地端，遠端連線去 firebase 測試。

```
# 从环境变量中读取服务账户密钥 JSON 内容
service_account_info = json.loads(os.environ['GOOGLE_CREDENTIALS'])
# 使用服务账户密钥 JSON 内容初始化 Firebase Admin SDK
cred = credentials.Certificate(service_account_info)

# 啟動 firebase realtime database (透過 firebase_url )
firebase_admin.initialize_app(cred, {'databaseURL': firebase_url})
```



## 透過 GCP 系統啟動

- 如果服務是部署在 Cloud Function 或是 Cloud Run 
- 其實可以透過預設的 Services Account 來取得相關權限
  - My Project 可以取得 My Project 在 firebase 的權限。
- 這個設定方式如法在本地端連接到 Firebase 測試

```
# 直接取得 Google Cloud 的參數
cred = credentials.ApplicationDefault()

# 啟動 firebase realtime database (透過 firebase_url )
firebase_admin.initialize_app(cred, {'databaseURL': firebase_url})
```



## 增加 Firebase 相關安全性設定 (Firebase Realtime Database Security Rules)



```
{
  "rules": {
    ".read": "auth != null && auth.token.admin === true",
    ".write": "auth != null && auth.token.admin === true"
  }
}
```

這樣就可以了。



## 相關程式碼

放一些基本常用到的 

#### 查詢全部

```
def get_all_cards(u_id):
    try:
        # 引用 "namecard" 路径
        ref = db.reference(f'{namecard_path}/{u_id}')

        # 获取数据
        namecard_data = ref.get()
        if namecard_data:
            for key, value in namecard_data.items():
                print(f'{key}: {value}')
            return namecard_data
    except Exception as e:
        print(f"Error fetching namecards: {e}")
```

#### 新增資料

```
def add_namecard(namecard_obj, u_id):
    """
    将名片数据添加到 Firebase Realtime Database 的 "namecard" 路径下。

    :param namecard_obj: 包含名片信息的字典对象
    """
    try:
        # 引用 "namecard" 路径
        ref = db.reference(f'{namecard_path}/{u_id}')

        # 推送新的名片数据
        new_ref = ref.push(namecard_obj)

        print(f'Namecard added with key: {new_ref.key}')
    except Exception as e:
        print(f'Error adding namecard: {e}')
```



#### 移除重複資料

```
def remove_redundant_data(u_id):
    """
    删除 "namecard" 路径下具有相同电子邮件地址的冗余数据。
    """
    try:
        # 引用 "namecard" 路径
        ref = db.reference(f'{namecard_path}/{u_id}')

        # 获取所有名片数据
        namecard_data = ref.get()

        if namecard_data:
            email_map = {}
            for key, value in namecard_data.items():
                email = value.get('email')
                if email:
                    if email in email_map:
                        # 如果电子邮件已经存在于 email_map 中，则删除该名片数据
                        ref.child(key).delete()
                        print(f'Deleted redundant namecard with key: {key}')
                    else:
                        # 如果电子邮件不存在于 email_map 中，则添加到 email_map
                        email_map[email] = key
        else:
            print('No data found in "namecard"')
    except Exception as e:
        print(f'Error removing redundant data: {e}')
```



#### 查詢某個資料是否已經存在

```
def check_if_card_exists(namecard_obj, u_id):
    """
    检查名片数据是否已经存在于 "namecard" 路径下。

    :param namecard_obj: 包含名片信息的字典对象
    :return: 如果名片存在返回 True, 否则返回 False
    """
    try:
        # 获取名片对象中的电子邮件地址
        email = namecard_obj.get('email')
        if not email:
            print('No email provided in the namecard object.')
            return False

        # 引用 "namecard" 路径
        ref = db.reference(f'{namecard_path}/{u_id}')

        # 获取所有名片数据
        namecard_data = ref.get()

        if namecard_data:
            for key, value in namecard_data.items():
                if value.get('email') == email:
                    print(
                        f'Namecard with email {email} already exists: {key}')
                    return True
        print(f'Namecard with email {email} does not exist.')
        return False
    except Exception as e:
        print(f'Error checking if namecard exists: {e}')
        return False
```

