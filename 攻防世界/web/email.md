开局看到一个登陆一个注册
注册test看到

![图片](https://github.com/user-attachments/assets/aa48acc1-4988-4b26-9adf-0f600346ad51)

有一个ID：3
一个Hint: 	Try to Be `admin` 
现在看到id想到两个知识点 sql id= 和越权 id 1，2,3
所以我们现在抓包看一下有没有明文id的传输造成的越权

![图片](https://github.com/user-attachments/assets/4ff23baa-7680-4562-b0b8-abd738e5c8a1)

很明显没有
那么现在登陆就有爆破和sql注入
我们试一下sql注入

提示： only letter and number is valid in username and password! 
只能数字和字母 
然后看一下注册 因为注册是多了一个email的
加单引号直接500
![图片](https://github.com/user-attachments/assets/ac44fed1-76d4-4675-9262-661e116666a2)

本来跑不出注入 但是我改了一下用户名发现又可以
![图片](https://github.com/user-attachments/assets/1438138c-5477-4e58-8bb4-719eb08ff87b)

布尔注入

所以要写个sqlmap的脚本 让他用户名每次都变
但是发现一直注入不出来 
