# AI推理服务框架开源版本v2设计(ASE OpenSource)


## 整体组件架构

如图: ![架构](athena.png)


注:

* Kong Nginx为推介组件,使用者可以根据自身需求更换合适的接入层网关

* 配置中心、LB为可选组件

* 根据部署方式不同[见下节],docker-compose 和 kuberentes方式 实际部署架构可能存在不同的组合方式: kubernetes方式可配置使用polaris配置中心，或者使用k8s/configmap


## 命令行工具[asectl]设计


为方便用户快速搭建部署整个AI推理服务框架，推出 ***asectl*** 工具

工具包含功能点:

* AI服务框架部署初始化: 支持 *docker-compose* , kubernetes 部署

* 支持APISchema定义

* AI引擎相关功能: 启动AI引擎，AI引擎镜像构建, AI引擎本地仿真



### 命令行工具详细设计

**asectl**

#### api create

执行 `asectl api create -f ai_api.yml`

```yaml
region: dx # 配置中心区域
group: gas # 配置中心group
metadata:
  serviceName: awake # 服务名
  projectName: guiderAllService
  version: "1.0.0"
  configFilname: xxxx.json # 配置文件名称
  creationTimestamp: null
  name: my-api-schema
spec:
  request:
    paramters:
      properties:
        vcn: 
          description: "a word that xxx"
          type: string
        speed:
          description: "a speed that xxx"
          type: integer
          format: int32
          minimum: 50
          maximum: 200000
        isauto:
          description: "boolean value"
          type: boolean
        someother:
          description: "a objcet key"
          properties:
            value1:
              type: string
            value2: 
              type: string
          type: object
      required:
        - vcn
        - someother
    payload:
      properties:
        audio1:
          compress: raw
          type: audio
          encoding: speex-wb
          format: plain
        text1:
          compress: raw
          type: text
          encoding: utf-8
          format: json
        image1:
          compress: raw
          type: base64
          format: jpg
          raw: string
        vedio1:
          type: vedio
          compress: raw
          format: ""
          raw: string
      required:
        - image1
  response:
    palyload:
      properties:
        audio1:
          type: audio
          encoding: speex-wb

```

执行完次命令后在当前目录/schemas下 生成一条schema.json 并提示是否推送到配置中心


#### config upload
配置推送

#### config edit
配置编辑

#### config list
配置列表

#### config 查看
配置查看


#### loader build
辅助构建加载器镜像

#### loader test
辅助测试加载器镜像运行测试用例

#### loader run
辅助运行1个加载器服务 并注册到推理服务框架，并生成api地址，api调用文档地址




#### setup

配置asectl 工具， 如配置中心地址，账户密码， 区域配置

