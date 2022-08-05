## Readme

中国地质大学（武汉） 大学生安全微课

**本人未学习过前端相关知识，如有问题，请自行解决**

**仅供学习使用，后果自负**

## 0. 开启微信网页调试

受微信限制，需 **安卓手机**

参见 https://developers.weixin.qq.com/community/develop/doc/00086ef5e2ceb0e167ade728351c00

> 1. 手机用usb连接至电脑
>
> 2. 手机微信内点击http://debugxweb.qq.com/?inspector=true（只要跳转过微信首页就是开启了调试）
>
> 3. 微信内打开所需调试网址
>
> 4. chrome 浏览器打开 `chrome://inspect/#devices`，会看到 `com.tencent.mm` 下是我们打开的网址
>
> 5. 在点击 chrome 里的 inspect 直接调试﻿



由 @Pull256 补充详细教程 https://blog.csdn.net/Jioho_chen/article/details/90636641



## 1. console 界面执行脚本

### 1.1 课程

**注意：使用此函数会导致进度溢出，慎用**

~~建议自行爬取所有 `course_id` 后执行~~

由 @Pull256 补充 `course_id`，供参考

```javascript
let course_id=[69,73,72,16,74,174,173,57,71,70,170,144,147,212,207,185,186,145,146,196,164,208,79,139,138,59,119,210,80,81,124,62,44,187,166,83,150,112,45,117,46,114,120,178,184,200,197,198,199,42,35,189,211,32,151,34,41,37,126,39,68,204,205,2,209,192,10,191,190,15,152,7,135,3,136,183,132,165,177,131,129,88,130,90,89,153,22,182,181,30,180,137,179,29,94,188,201,202,203,167,168,169,157,28,156,25,158,23,102,154,175,155]

function finished_course(course_id) {
    $.ajax({
        url: "http://study.weusstore.com/index/course/finished?cid=" + course_id,
        type: "GET",
        timeout: 5000,
        success: function (data) {
            console.log(data);
            console.log(course_id + " 完成学习！");
        },
        error: function (XMLHttpRequest, textStatus, errorThrown) {
        }
    });
}

for (var i = 0; i < course_id.length; i++) {
    finished_courses(course_id[i]);
}

// for (var i = 0; i < 240; i++) {
//     finished_course(i);
// }
```



### 1.2 考试

进入考试页面，执行函数

```javascript
function solve() {
    $.ajax({
        type: 'POST',
        url: "/addons/rexam/answer/question.html",
        data: {
            'qnum': qnum,
            'rid': "5",
        },
        dataType: 'json',
        success: function (res) {
            console.log("每一道题的", res)
            if (res.code == 1) {
                btn_fresh();
                var question = res.data.question;
                if (question.options_json != null) {
                    // 选择题
                    var options = res.data.question.options_json;
                    for (var i = 0; i < options.length; i++) {
                        if (options[i].answer != null) {
                            console.log("答案", options[i].value)
                        }
                    }
                } else {
                    // 判断题
                    var judgedata = res.data.question.judgedata;
                    if (judgedata == 0) {
                        console.log("答案", "错误")
                    } else {
                        console.log("答案", "正确")
                    }
                }
            }
        }
    })
}

solve()
```
