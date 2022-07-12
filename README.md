## Readme

中国地质大学（武汉） 大学生安全微课

**本人未学习过前端相关知识，如有问题，请自行解决**

**仅供学习使用，后果自负**

## 0. 进入调试界面

参见 https://developers.weixin.qq.com/community/develop/doc/00086ef5e2ceb0e167ade728351c00

## 1. console 界面执行脚本

### 1.1 课程

**注意：使用此函数会导致进度溢出，慎用**

建议自行爬取所有 `course_id` 后执行

```javascript
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

for (var i = 0; i < 200; i++) {
    finished_courses(i);
}
```

### 1.2 考试

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

