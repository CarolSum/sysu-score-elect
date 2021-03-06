## 直接运行

需要 OCR 程序 tesseract

```sh
# ubuntu
apt install tesseract
# mac
brew install tesseract
```

```sh
npm i
# 直接启动
node -r esm elect.js
# 或者通过 pm2
npm i -S -g pm2
pm2 startOrReload deploy.json
```

## Docker 方式运行

在宿主机需要对 ppp 进行 NAT

```sh
# ubuntu
modprobe ip_nat_pptp
modprobe ip_conntrack_pptp
```

up

```sh
docker-compose up -d
```

down

```sh
docker-compose down
```

## config

### docker-compose.yml

挂载点有 ``config`` 文件夹和 ``deploy.json``、``logrotate`` 里指定的 ``/var/log/elect`` 等

### deploy.json

```json
{
  "apps" : [{
    "name"      : "elect",
    "script"    : "elect.js",
    "instances" : "1",
    "exec_mode" : "cluster",
    "node_args": ["-r", "esm"],
    "cwd": "/sysu-score-elect",
    "watch"             : false,
    "log_date_format"   : "YYYY-MM-DD HH:mm:ss.SSS",
    "merge_logs"        : true,
    "error_file"        : "/var/log/elect/error.log",
    "out_file"          : "/var/log/elect/output.log",
    "max_memory_restart": "1G"
  }, {
    "name"      : "score",
    "script"    : "score.js",
    "instances" : "1",
    "exec_mode" : "cluster",
    "node_args": ["-r", "esm"],
    "cwd": "/sysu-score-elect",
    "watch"             : false,
    "log_date_format"   : "YYYY-MM-DD HH:mm:ss.SSS",
    "merge_logs"        : true,
    "error_file"        : "/var/log/score/error.log",
    "out_file"          : "/var/log/score/output.log",
    "max_memory_restart": "1G"
  }]
}
```

### index.js

```js
import path from 'path';

export default {
  // TODO
  credentials: ['netid', 'pass'],
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // -------------------------------------------------------------------
  // TODO
  // 注释掉 vpn 则不使用 vpn
  vpn: {
    remote: 'vpn.sysu.org.cn',
    // 可选，如果不填，则使用上面的 credentials
    // username: 'username',
    // password: 'password',
  },
  // TODO
  courses: [
    {
      // 公必、公选、专必、专选
      type: '专选',
      // 公必、公选、专必、专选类型 id，看浏览器地址栏里的同名 query string
      xkjdszid: '2017231004001',
      // 匹配课程信息
      match: ({ time, course, teacher, classId }) => String(course).match(/数据挖掘/),
      // 可不填，表示选课前要先退出某门课
      // unelect: '62000098172001',
      // 可不填，表示人满了的话是否还要继续抢课
      // force: true,
    },
  ],
  // TODO
  pollInterval: {
    elect: 20 * 1000,
    score: 60 * 1000,
  },
  notification: {
    // TODO
    // 不需要邮箱提醒，就把邮箱这个字段注释掉
    mail: {
      login: {
        // 详见 https://nodemailer.com/smtp/well-known/#supported-services
        service: 'QQ',
        auth: {
          user: '邮箱',
          pass: '密码/token',
        },
      },
      send: {
        from: '发件人 <发件人邮箱>',
        to: '收件人邮箱',
      },
      path: path.join(__dirname, '..', 'notification.pug'),
    },
    // 不需要微信提醒，就把微信这个字段注释掉，像这样
    // wechat: {
    //   // http://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login
    //   appid: 'appid',
    //   secret: 'secret',
    //   // 你的微信在公众号的 openid
    //   openid: 'openid',
    //   elect: {
    //     // 主要是模板标题不同。模板内容都得是 {{content.DATA}}
    //     progressing_template_id: 'template_id',
    //     success_template_id: 'template_id',
    //     failure_template_id: 'template_id',
    //   },
    //   score: {
    //     // 模板内容都得 {{content.DATA}}
    //     score_template_id: 'template_id',
    //   },
    // },
  },
};
```
