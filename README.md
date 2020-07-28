一、几个基本概念
====
### 1.单点登录：
单点登录（Single Sign On），简称为 SSO，是比较流行的企业业务整合的解决方案之一。SSO的定义是在多个应用系统中，用户只需要登录一次就可以访问所有相互信任的应用系统。
* 所有应用系统共享一个身份认证系统。
<br>统一的认证系统是SSO的前提之一。认证系统的主要功能是将用户的登录信息和用户信息库相比较，对用户进行登录认证；认证成功后，认证系统应该生成统一的认证标志（ticket），返还给用户。另外，认证系统还应该对ticket进行效验，判断其有效性。
* 所有应用系统能够识别和提取ticket信息
<br>要实现SSO的功能，让用户只登录一次，就必须让应用系统能够识别已经登录过的用户。应用系统应该能对ticket进行识别和提取，通过与认证系统的通讯，能自动判断当前用户是否登录过，从而完成单点登录的功能。


### 2.前后端分离：
* 前端：html、css，JavaScript，面向的是用户，如果载体是浏览器或移动设备，语言主要是JavaScript。
* 后端：基于web容器提供应用服务，可以是java、net语言，还可以是nodejs等等。
<br>最后前后端如何衔接，无非是ajax url请求把数据传递给web应用服务。

前后端分离，分成两个概念，一个是工作职责角色的区分，一个是架构分离的区分。
<br>常见的前后端分离： JAVA写rest风格的后端接口，安卓 IOS的 web端都是调用统一的接口来实现的。

### 3.微服务：
一种软件开发技术- 面向服务的体系结构（SOA）架构样式的一种变体，将应用程序构造为一组松散耦合的服务。在微服务体系结构中，服务是细粒度的，协议是轻量级的。
<br>微服务（或微服务架构）是一种云原生架构方法，其中单个应用程序由许多松散耦合且可独立部署的较小组件或服务组成。
* 有自己的堆栈，包括数据库和数据模型；
* 通过REST API，事件流和消息代理的组合相互通信；
* 和它们是按业务能力组织的，分隔服务的线通常称为有界上下文。

尽管有关微服务的许多讨论都围绕体系结构定义和特征展开，但它们的价值可以通过相当简单的业务和组织收益更普遍地理解：
* 可以更轻松地更新代码。
* 团队可以为不同的组件使用不同的堆栈。
* 组件可以彼此独立地进行缩放，从而减少了因必须缩放整个应用程序而产生的浪费和成本，因为单个功能可能面临过多的负载。 

三、Docker和容器
=====
### 1.什么是容器？
容器就是将软件打包成标准化单元，以用于开发、交付和部署。
* 容器镜像是轻量的、可执行的独立软件包 ，包含软件运行所需的所有内容：代码、运行时环境、系统工具、系统库和设置。
* 容器化软件适用于基于Linux和Windows的应用，在任何环境中都能够始终如一地运行。
* 容器赋予了软件独立性，使其免受外在环境差异（例如，开发和预演环境的差异）的影响，从而有助于减少团队间在相同基础设施上运行不同软件时的冲突。
### 2.什么是Docker?
* Docker是世界领先的软件容器平台。
* Docker使用Google公司推出的Go语言进行开发实现，基于Linux内核的cgroup，namespace，以及AUFS类的UnionFS等技术，对进程进行封装隔离，属于操作系统层面的虚拟化技术。由于隔离的进程独立于宿主和其它的隔离的进程，因此也称其为容器。Docker最初实现是基于LXC。
* Docker能够自动执行重复性任务，例如搭建和配置开发环境，从而解放了开发人员以便他们专注在真正重要的事情上：构建杰出的软件。
* 用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行版本管理、复制、分享、修改，就像管理普通的代码一样。
### 3.Docker的基本概念
* 镜像（Image）——一个特殊的文件系统<br>
&nbsp; &nbsp; Docker镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。 镜像不包含任何动态数据，其内容在构建之后也不会被改变。<br>
&nbsp; &nbsp; Docker设计时，就充分利用Union FS的技术，将其设计为分层存储的架构。 镜像实际是由多层文件系统联合组成。<br>
&nbsp; &nbsp; 镜像构建时，会一层层构建，前一层是后一层的基础。每一层构建完就不会再发生改变，后一层上的任何改变只发生在自己这一层。比如，删除前一层文件的操作，实际不是真的删除前一层的文件，而是仅在当前层标记为该文件已删除。在最终容器运行的时候，虽然不会看到这个文件，但是实际上该文件会一直跟随镜像。因此，在构建镜像的时候，需要额外小心，每一层尽量只包含该层需要添加的东西，任何额外的东西应该在该层构建结束前清理掉。
