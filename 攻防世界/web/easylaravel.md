![图片](https://github.com/user-attachments/assets/680af25b-6f0c-4034-b3c6-47942f8eb900)

点击f12查看到
https://github.com/qazxcv123/easy_laravel

下载下来审计

首先审计到一个文件包含flag位置
th1s1s_F14g_2333333


一个admin邮箱
admin@qvq.im
![图片](https://github.com/user-attachments/assets/dc4ad763-87d0-40c7-b100-1bd8fbf4a869)

一个文件上传
白名单
![图片](https://github.com/user-attachments/assets/31b5a1f3-9742-4167-b402-d10dfa5a42b1)

笔记位置
        $username = Auth::user()->name;
        $notes = DB::select("SELECT * FROM `notes` WHERE `author`='{$username}'");
用户名sql注入
![图片](https://github.com/user-attachments/assets/a278184a-0555-4074-ada1-4065ee6673a3)

有了sql有了用户名直接sql注入读密码
查一下已知的数据库notes 

