# 交换日记项目1.0

数据库期末Project，交换日记。

前端部分代码地址：<https://github.com/wu-kan/exchange-journal_front-end/>

## 项目人员及分工

按学号顺序排列。

|学号|姓名|分工|
|-|-|-|
|17341131|[彭继贤](https://github.com/shadw3002)|后端部分|
|17341155|[王永康](https://github.com/)|前端部分/发现、登陆、注册|
|17341163|[吴坎](https://github.com/wu-kan)|前端部分/写日记、个人中心|

## 项目目的

初步了解开发一款程序应用的基本流程，并初步熟悉开发程序应用所需的基本知识：前端和后端开发，并在其中了解数据库知识在其中的实际应用。

## 项目内容

开发一款写日记的APP，APP内容如下：

- 名字：交换日记（Exchange Journal）
- 主要功能：
  - 用户可以写日记，并发布
  - 两个用户随机配对
  - 配对的两个用户可以查看对方的日记

## 项目方案和流程

开发过程分为三个部分：前端开发，后端开发，前后端结合。

```mer

```

### 前端开发

前端开发的具体流程为：

- 讨论应该实现的功能
- 根据功能绘制相应的界面
- 确定各个界面之间的业务逻辑，数据流向
- 利用现有的vue框架来实现界面视图和界面的数据交互

初始设计界面，应该至少包含三个界面：主要功能的发现主界面，匹配方日记的主界面，最后是写日记的主要界面。

我们的前端部分使用了[uni-app框架](https://uniapp.dcloud.io/)进行开发。它的好处和优势是一次编写，多平台发布（安卓app、IOSapp、H5页面、微信小程序、支付宝小程序、百度小程序、头条小程序、支付宝小程序）。uni-app框架也是一个基于vue的框架，涉及html,css,javascript等多种语言。

我们设计的界面描述如下：

应用启动后的第一页面是发现页，也是我们项目的核心功能。用户可以在发现页查看自己已经匹配的另一个用户的日志。对于初次注册尚未匹配的用户，它会看到的是匹配页面，可以看到一些其他用户的公开信息。其中，点击`ta`则会进入匹配方的日记界面。

作为日记类应用，写日记是其最基本的功能，我在设计页面的时候希望尽可能的简洁，使得用户可以专注于自己要分享的内容。同时增加了日记心情、上传图片、图片压缩的功能，可以用于对日记做一些基础分类（未来用户数量足够多的时候，可以考虑对日记心情和文本内容做一些分析，从而更智能地为用户推送消息）。

最后是用户个人中心页面，由于目前还在一个很初级的版本，此处功能较少。未来可能会继续为其增加一些功能，如用户等级、用户相册、用户推荐、私信（目前是消息通知）、日记归档等等。

### 后端开发

- 后端部分代码地址：<https://github.com/wu-kan/exchange-journal_front-end/>

最初前后端交互想采用 python-grpc / grpc-web, 但是 grpc-web 有一些问题, 所以最后转移到了 Python 实现 http 服务器的方式.

目前的后端是这样实现的:
- 打开 server socket
- 接收一个连接请求
- 将新创建的 client socket 传给子线程
- 解析 http 请求包格式
- 从 url 解析调用的方法和参数
- 与 mysql 数据库进行交互
- 以 json 形式返回响应结果

```python
class HTTPServer(object):
    def __init__(self):
        pass

    def start(self):
        pass

    def handle_client(self, client_socket):
        pass

    def bind(self, port):
        pass
```

实现的接口的 rpc 描述为
```rpc
service MyDiary {
  // 获取自己或对方的文章数
  rpc GetPostNum(UserIDT) returns (PostNum) {}

  // 获取自己或对方的文章
  rpc GetPost(PostIDT) returns (Post) {}

  // 获取自己或对方的全部文章 ID
  rpc GetPostIDs(UserIDT) returns (stream PostID) {}
  
  // 获取对方的 ID
  rpc RequestPair(Token) returns (UserID) {}

  // 进行配对 TOOD
  rpc WantPair(Token) returns (Result) {}

  // 破坏配对 TODO
  rpc BreakPair(Token) returns (Result) {}

  // 
  rpc Login(LoginInfo) returns (Token) {}

  rpc UpdatePost(PostT) returns (PostID) {}
}
```

另外, 登陆机制的实现为:
- 用户登陆时, 服务器会产生一个随机的 token
- 用户随后使用这个 token 与服务器进行交互
- 服务器会在用户重新登陆后刷新这个 token, 服务器此外会定时刷新 token

### 前后端结合

前后端的结合通过javascript定义的数据接口来进行交互，当然这一部分由于遇到了一些困难，没有完全对接好所有的数据接口之间的数据交互。

## 项目结果

目前，我们只实现一个简单的demo版本，功能尚不完全，实现主要功能有写日记，预览日记，查看日记。
#### 界面预览

我们实现的App前端部分包含如下几个模块。

| ![发现](docs/assets/images/发现.jpg) | ![写日记](docs/assets/images/写日记.jpg) | ![个人中心](docs/assets/images/我的.jpg) |
| ------------------------------------ | ---------------------------------------- | ---------------------------------------- |
| 发现                                 | 写日记                                   | 个人中心                                 |

| ![日记详情](docs/assets/images/日记详情.jpg) | ![注册](docs/assets/images/注册.jpg) | ![登陆](docs/assets/images/登陆.jpg) |
| -------------------------------------------- | ------------------------------------ | ------------------------------------ |
| ta的日记                                     | 注册                                 | 登录                                 |

## 项目总结

这一次的交换日记的项目开发过程，真是感触良多。

从最初的设计一款什么样的APP的选题到最后动手去实现，这个过程真的可以说比较曲折。

我主要有以下的一些收获：团队合作的重要性，自己学习到了很多，开发计划流程的制定时间线的制定，收获到了许多知识，熟悉了当今软件开发的一个流程，前端开发的难点，和挑战性，以及和我们这门课的联系在哪？

- 团队合作和分工的重要性：

  这一次数据库项目的实现，由于我们三个人都没有开发APP的成熟经验，所以对于开发的整个流程是比较模糊的，也是由于这个原因，导致我们很晚才正式着手开始实践。后面，我们逐渐清楚流程之后，每个人有自己在不同方面的长处，所以进行了合理的分工，使得整个项目得以运转起来。期间，我们在界面设计和实现上也遇到了不少困难，都是在大家的相互交流和合作下，逐渐解决了这些困难。

- 制定合理的开发时间线的重要性：

  我们这一次项目的实现比较仓促，原因有上面提到的对开发的流程不太熟悉，另一方面也是没有制定合理的开发时间线。因为这次交换日记开发的项目持续周期相对较长，时间超过大半个学期的时间。可能是时间战线实在是过于漫长，我们组前期方面的进展甚是缓慢，这就导致后面的开发工作任务比较繁重。

- 我们遇到的困难：
  - 技术上的困难：我们都没有相关的开发经验，所以很多东西都要去熟悉，需要接触许多未知的领域，需要花费大量的时间学习一些新的知识。
  - 开发项目交流上的困难：因为大家没有相关的开发经验，所以在很多问题的交流上，往往不能直接解决问题，实际上正是这个原因，导致大家交流比较少，这也是使项目的进展比较缓慢的原因之一。
- 收获：大家这一次通过合作开发交换日记的项目过程，学习到了许多的应用程序开发的知识，包括开发的流程，前后端交互的方式，以及前端界面的设计等等知识，认识到了许多实际开发过程中的技术的运用，积累了一定的开发经验。

- 后续：我们小组计划在考完试之后，再继续完善交换日记的其余功能，并对界面进行美工处理。

## 参考文献

uni-app官网：https://uniapp.dcloud.io/

grpc官网: https://grpc.io/docs/