### 利用Vue脚手架新建Vue项目

####  1.创建一个基于webpack的vue应用程序

```sh
# 这里的 myvue 是项目名称，可以根据自己的需求起名
vue init webpack myvue
```

#### 2.新建项目时的选项说明

- `Project name`：项目名称，默认 `回车` 即可
- `Project description`：项目描述，默认 `回车` 即可
- `Author`：项目作者，默认 `回车` 即可
- `Install vue-router`：是否安装 `vue-router`，选择 `n` 不安装（后期需要再手动添加）
- `Use ESLint to lint your code`：是否使用 `ESLint` 做代码检查，选择 `n` 不安装（后期需要再手动添加）
- `Set up unit tests`：单元测试相关，选择 `n` 不安装（后期需要再手动添加）
- `Setup e2e tests with Nightwatch`：单元测试相关，选择 `n` 不安装（后期需要再手动添加）
- `Should we run npm install for you after the project has been created`：创建完成后直接初始化，选择 `n`，我们手动执行

#### 3.初始化并且运行

```sh
#进入到项目的根目录
cd myvue
npm install
npm run dev
```

