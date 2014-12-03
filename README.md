Jenkins
=======
Jenkins 
1 Jenkins
Jenkins由以前的hudson更名而来。Jenkins的主要功能是监视重复工作的执行，例如软件工程的构建或在cron下设置的jobs。具体地如下：

*软件的持续构建和测试，此时Jenkins与CruiseControl或DamageControl相似。本质上提供了一个易于使用的持续集成系统，使得开发人员更容易地将改变集成到工程中，使得用户更容易获得一个新的build。自动化，持续的构建提高了软件开发的效率。
*监视外部运行的job的执行，例如cron jobs或procmail jobs，即使这些jobs是运行在远程的机器上。例如，对于cron，你将会收到email包含job的output，你需要检查email来确认是否job broke。Jenkins将保持这些outputs且使得你更加容易地注意到job的broke。
* 项目源码修改的检测，jenkins能够从项目的Subversion/CVS生成最近修改的集合列表，且改方式非常有效，不会增加Subversion/CVS Repository的负载。
* 可读的永久的链接生成，jenkins对于大部分pages都生成清楚的可读的永久的链接，例如''latest build"/"latest successful build",因此可以容易地在其他的地方引用jenkins的生成的pages。	
* RSS/EMail/IM集成，可以通过RSS，EMail或IM来实时地监视build的失败。
* Build完成后仍然可以tag，支持在build完成后tag或重tag。
* Junit/TestNG 测试报告，能够很好地显示各种测试的报告，且可以生成失败的趋向图。
* 分布式build，jenkins能够分发build/test的负载到多台机器，能够更好地利用硬件资源，提高build的时间。
* 文件标识，jenkins可以标识build产生的文件，例如jars。
* 插件支持，jenkins可以通过第三方的插件来扩展。
* 跨平台，支持几乎所有的平台，例如Windows,Ubuntu/Debian,Red Hat/Fedora/CentOS,Mac OS X,openSUSE,FreeBSD,OpenBSD,Solaris/OpenIndiana.Gentoo。
2 jenkins 的安装
下载jenkins.war, 拷贝到c:\jenkins下(不局限c盘，其他的也可以)，然后运行java -jar jenkins.war. (注意需要先安装JDK，然后设置JAVA_HOME环境变量且将%JAVA_HOME%\bin加入到PATH环境变量中)
运行如下：
c:\jenkins>java -jar jenkins.war
Running from: C:\jenkins\jenkins.war
webroot: $user.home/.jenkins
[Winstone 2011/11/02 17:11:27] - Beginning extraction from war file
Jenkins home directory: C:\Users\AAA\.jenkins found at: $user.home/.jenkins
[Winstone 2011/11/02 17:12:57] - HTTP Listener started: port=8080
[Winstone 2011/11/02 17:12:57] - AJP13 Listener started: port=8009
[Winstone 2011/11/02 17:12:58] - Winstone Servlet Engine v0.9.10 running: controlPort=disabled
Nov 02, 2011 5:12:58 PM jenkins.model.Jenkins$6 onAttained
INFO: Started initialization
Nov 02, 2011 5:13:02 PM jenkins.model.Jenkins$6 onAttained
INFO: Listed all plugins
Nov 02, 2011 5:13:02 PM jenkins.model.Jenkins$6 onAttained
INFO: Prepared all plugins
Nov 02, 2011 5:13:02 PM jenkins.model.Jenkins$6 onAttained
INFO: Started all plugins
Nov 02, 2011 5:13:02 PM jenkins.model.Jenkins$6 onAttained
INFO: Augmented all extensions
Nov 02, 2011 5:13:02 PM jenkins.model.Jenkins$6 onAttained
INFO: Loaded all jobs
Nov 02, 2011 5:13:04 PM jenkins.model.Jenkins$6 onAttained
INFO: Completed initialization
Nov 02, 2011 5:13:04 PM hudson.TcpSlaveAgentListener <init>
INFO: JNLP slave agent listener started on TCP port 37157
Nov 02, 2011 5:13:14 PM hudson.WebAppMain$2 run
INFO: Jenkins is fully up and running
访问http://localhost:8080 , jenkins的主界面如下：

如果出现这样的图片，说明安装成功！

构建Java HelloWorld
注意：我们知道Jenkins通过master/slave来支持分布式的job运行，这里的JavaHelloworld运行在master，即Jenkins所在的机器。

一 Java的HelloWorld程序 

<project name="HelloWorld" basedir="." default="main">
    <property name="src.dir"     value="src"/>
    <property name="build.dir"   value="build"/>
    <property name="classes.dir" value="${build.dir}/classes"/>
    <property name="jar.dir"     value="${build.dir}/jar"/>
    <property name="main-class"  value="oata.HelloWorld"/>
    <target name="clean">
        <delete dir="${build.dir}"/>
    </target>
    <target name="compile">
        <mkdir dir="${classes.dir}"/>
        <javac srcdir="${src.dir}" destdir="${classes.dir}"/>
    </target>
    <target name="jar" depends="compile">
        <mkdir dir="${jar.dir}"/>
        <jar destfile="${jar.dir}/${ant.project.name}.jar" basedir="${classes.dir}">
            <manifest>
                <attribute name="Main-Class" value="${main-class}"/>
            </manifest>
        </jar>
    </target>
    <target name="run" depends="jar">
        <java jar="${jar.dir}/${ant.project.name}.jar" fork="true"/>
    </target>
    <target name="clean-build" depends="clean,jar"/>
    <target name="main" depends="clean,run"/>
</project>

Java的helloworld： c:\JavaHelloWorld\src\oata\helloworld.java

package oata;
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}

二 启动Jenkins且创建job来运行JavaHelloWorld

1） 启动jenkins在8000端口：
 
 
2） 创建JavaHelloWorld的job
在ie中打开http://localhost:8000， 

单击new job链接，为javahelloworld新建job，且编译job的配置如下：

注意jenkins默认已经安装了svn的plugin了。 

 
 
3） 运行JavaHelloWorld的job
进入JavaHelloWorld的主页面，点击build now链接进行build，build后可以在此主页面上看到所有的build历史，如下：

 
然后还可以点击某个build的链接，查看某个build的详细日志，如下：
 

Jenkins 的配置
1  修改jenkins的根目录，默认地在C:\Documents and Settings\AAA\.jenkins 。
.jenkins  
├─jobs
│  └─JavaHelloWorld
│      ├─builds
│      │  ├─2011-11-03_16-48-17
│      │  ├─2011-11-03_16-49-05
│      │  ├─2011-11-03_16-49-29
│      │  ├─2011-11-03_17-01-49
│      │  └─2011-11-03_17-11-42
│      └─workspace
│          ├─build
│          │  ├─classes
│          │  │  └─oata
│          │  └─jar
│          └─src
│              └─oata
├─plugins
├─usercontent
├─war 
可以通过设置环境变量来修改，例如：
set JENKINS_HOME=C:\jenkins
然后重新启动jenkins。
 
2 备份和恢复jenkins
 	只需要备份JENKINS_HOME下的所有文件和文件夹，恢复的时候需要先停止jenkins。
3 移动，删除或修改jobs
对于移动或删除jobs，只需要简单地移动或删除%JENKINS_HOEM%\jobs目录。
对于修改jobs的名字，只需要简单地修改%JENKINS_HOEM%\jobs下对应job的文件夹的名字。
对于不经常使用的job，只需要对%JENKINS_HOEM%\jobs下对应的jobs的目录zip或tar后存储到其他的地方。
4 可以在jenkins的url中执行一些命令来操作jenkins，如下
http://[jenkins-server]/[command] 命令可以为：
exit shutdown jenkins
restart restart jenkins
reload to reload the configuration
5 Jenkins 启动时的命令行参数 
--httpPort=$HTTP_PORT，用来设置jenkins运行时的web端口。
--httpsPort=$HTTP_PORT，表示使用https协议。
--httpListenAddress=$HTTP_HOST，用来指定jenkins监听的ip范围，默认为所有的ip都可以访问此jenkins server。
6 修改jenkins的timezone
如果jenkins所在的server的timezone不同于用户的timezone，这时候需要修改jenkins的timezone，需要在jenkins启动的时候增加下列参数-Dorg.apache.commons.jelly.tags.fmt.timeZone=TZ

7 最好通过一个脚本来启动jenkins，确保jenkins每次都运行在相同的环境下，例如
startjenkins.bat
set JENKINS_HOME=c:\jenkins
cd /d %JENKINS_HOME%
java -jar %JENKINS_HOME%\jenkins.war --httpPort=8000

8 jenkins在后台运行
	如果jenkins是部署在servlet容器中，例如apache，tomcat中。因为servlet容器一般都在后台运行了，所以jenkins也就已经在后台运行了。对于windows用户需要在jenkins的管理页面中点击insall as windows service来将jenkins部署为service。 但是感觉比较好的方法还是手动将启动jenkins的脚本部署为windows service，从而可以更灵活地设置更多的参数。
9 jenkins的系统信息
可以在jenkins的管理页面下的系统信息中，查看所有的jenkins的信息，例如jenkins的启动配置，所依赖的系统的环境变量，所安装的plugins。
10 jenkins内置的环境变量
BUILD_NUMBER， 唯一标识一次build，例如23；
BUILD_ID，基本上等同于BUILD_NUMBER，但是是字符串，例如2011-11-15_16-06-21；
JOB_NAME， job的名字，例如JavaHelloWorld；
BUILD_TAG， 作用同BUILD_ID,BUILD_NUMBER,用来全局地唯一标识一此build，例如jenkins-JavaHelloWorld-23；
EXECUTOR_NUMBER， 例如0；
NODE_NAME，slave的名字，例如MyServer01；
NODE_LABELS，slave的label，标识slave的用处，例如JavaHelloWorld MyServer01；
JAVA_HOME， java的home目录，例如C:\Program Files (x86)\Java\jdk1.7.0_01；
WORKSPACE，job的当前工作目录，例如c:\jenkins\workspace\JavaHelloWorld；
HUDSON_URL = JENKINS_URL， jenkins的url，例如http://localhost:8000/ ；
BUILD_URL，build的url 例如http://localhost:8000/job/JavaHelloWorld/23/；
JOB_URL， job的url，例如http://localhost:8000/job/JavaHelloWorld/；
SVN_REVISION，svn 的revison， 例如4；

Jenkins的Windows Slave的配置
一 创建新的Slave
注意Jenkins中slave称为note。 所以下面文章中的slave和node指的是一回事。

1） 在Manage Jenkins-->Manage Nodes -->New Node下：输入Node Name，且选择Dumb Slave作为Slave的类型，然后OK。
 

2）在Slave的配置页面，输入如下：
*executors的数量，1或多个；
*输入Slave 上的跟目录，例如c:\jenkins；
*Usage选择：Leave this machine for tied jobs only；
*Lunch Method选择：Launch slave agents via Java Web Start ；
* Avaliablitiy选择：Keep this slave online as much as possible；
* 然后保存；


3） 在slave所在的机器登录jenkins master，且进入Manage Jenkins-->Manage Nodes-->新建的Note，点击launch，然后安装slave为service如下：

 
4） 安装成功后显示如下：

二 在slave上运行job
对上面的slave增加label，从而表示此slave的用处，且同时对uage选择leave this machine for tied jobs only：


对Jenkins 构建JavaHelloWorld 中的job修改如下：
选择restrict where this project can be run 且输入note（slave）的label。另外注意SVN的地址因该正确，jenkins会提示输入svn的用户名和密码。

此时job将会在slave所在的机器运行，当然build所需要的环境要在slave上配置好哦，运行如下：


注意： 对slave系统环境变量的修改，jenkins slave不会立即生效，需要重启jenkins slave service。 例如我在slave上装了ant，设置到path中后仍然找不到，需要restart jenkins slave service。
Jenkins的Linux的Slave的配置

1） Linux 的 Slave机器设置 
创建jenkins用户sudo /usr/sbin/useradd -m jenkins -d /home/jenkins；
查看jenkins用户及组的信息id jenkins ：
uid=506(jenkins) gid=506(jenkins) groups=506(jenkins) ；
使用sudo /usr/bin/passwd jenkins来设置用户jenkins的密码为0；
切换到用户jenkins环境下su - jenkins；
确保java安装正确：java --version；
确保sshd正确运行： /sbin/service --status-all | grep ssh；
安装ant，在root下运行yum install ant;

2) 在Slave的linux机器上创建public/private key pair：
确保当前用户为jenkins；
执行ssh-keygen来创建public/private key pair，直接enter，表示key将存储在	/home/jenkins/.ssh/id_rsa下，再直接enter，表示不设置密码，再次enter确认	密码为空；
创建authorized_keys：
cd .ssh
cat id_rsa.pub > authorized_keys
chmod 700 authorized_keys ;
将id_rsa(相当于privatekey）拷贝到jenkins master机器上，例如	c:\jenkins\id_rsa下。将id_rsa(相当于privatekey）拷贝到jenkins master机器	上，例如c:\jenkins\id_rsa下。

3） 创建Slave（note）,配置如下：

确保jenkins 中ssh slave plugin正确安装，一般默认安装。
然后lunch slave，使得master和slave通过ssh成功连接。其实launch的时候jenkins自动地从http://yourserver:port/jnlpJars/slave.jar拷贝slave.jar到slave，然后运行通过命令java -jar slave.jar来运行slave。
4） 在新建的Linux的Slave上运行上节中的JavaHelloWorld（Jenkins 构建JavaHelloWorld），且需要修改JavaHelloWorld job的Label为JavaHelloWorldLinux来使用此slave，运行如下：



Jenkins Master/Slave架构ﾧ
一 Jenkins Master/Slave架构
Master/Slave相当于Server和agent的概念。Master提供web接口让用户来管理job和slave，job可以运行在master本机或者被分配到slave上运行。一个master可以关联多个slave用来为不同的job或相同的job的不同配置来服务。
 当job被分配到slave上运行的时候，此时master和slave其实是建立的双向字节流的连接，其中连接方法主要有如下几种：
1）master通过ssh来启动slave
Jenkins内置有ssh客户端实现，可以用来与远程的sshd通信，从而启动slave agent。这是对*unix系统的slave最方便的方法，因为*unix系统一般默认安装有sshd。在创建ssh连接的slave的时候，你需要提供slave的host名字，用户名和ssh证书。创建public/private keys，然后将public key拷贝到slave的~/.ssh/authorized_keys中，将private key 保存到master上某ppk文件中。jenkins将会自动地完成其他的配置工作，例如copy slave agent的binary，启动和停止slave。但是你的job运行所依赖其他的项目需要你自己设置。
2）master通过WMI+DCOM来启动windows slave
对于Windows的Slave，Jenkins可以使用Windows2000及以后内置的远程管理功能（WMI+DCOM），你只需要提供对slave有管理员访问权限的用户名和密码，jenkins将远程地创建windows service然后远程地启动和停止他们。对于windows的系统，这是最方便的方法，但是此方法不允许运行有显示交互的GUI程序。
注意：不想其他类型的链接方式，此种方式slave（note）的名字非常重要，将被用来当做slave的地址访问slave。
3）实现自己的脚本来启动slave
如果上面成套的方法不够灵活，你可以实现自己的脚本来启动slave。你需要将启动脚本放到master，然后告诉jenkins master在需要的时候调用此脚本来启动slave。典型地，你的脚本使用远程程序执行机制，例如SSH，RSH，或类似的方法（在windows，可以通过cygwin或psexec来完成），在脚本的最后需要执行类似java -jar slave.jar来启动slave。slave.jar可以从http://yourjenkinsserver:port/jnlpjars/slave.jar下载，也可以在脚本的开始先下载此slave.jar从而保证slave.jar正确的版本。 但是如果使用ssh slave plugin的话，此plugin将自动地更新slave.jar。
4）通过Java web start来启动slave
jave web start（jnlp）是另一种启动slave的方法。用这种方法你需要登录到slave，打开浏览器，打开slave的配置页面来连接。还可以安装为windows service来使得slave在后台运行。如果你需要运行的程序需要UI的交互，使用下面的方法：在slave系统上创建jenkins用户，设置自动登录，在系统的startup items增加slave JNLP文件的快捷方式，使得slave在系统登录的时候自动启动。
5）直接启动slave
此方式类似于java web start，可以方便地在*unix系统上将slave运行为daemon。需要配置slave为JNLP类型连接，然后在slave机器上执行
java -jar slave.jar -jnlpUrl http://yourserver:port/computer/slave-name/slave-agent.jnlp

二 Slave配置的好的建议
* 每个slave都有用户jenkins，所有的机器使用相同的UID和GID，使得slave的管理更加简单；
* 每个机器上jenkins用户的home目录都相同/home/jenkins, 拥有相同的目录结构使得维护简单；
* 所有的slave运行sshd，windows运行cygwin sshd；
* 所有的slave安装ntp client，用来与相同的ntp server同步；
* 使用脚本sh来自动地配置slave的环境，例如创建jenkins用户，安装sshd，安装java，ant，maven等；
* 使用脚本来启动slave，保证slave总是运行在相同的参数下：
#!/bin/bash JAVA_HOME=/opt/SUN/jdk1.6.0_04 PATH=$PATH:$JAVA_HOME/bin export PATH java -jar /var/jenkins/bin/slave.jar

Jenkins最佳实践ﾧ
Jenkins最佳实践，其实大部分对于其他的CI工具同样的适用：
* Jenkins的安全。对Jenkins的用户使用授权和访问控制。默认地Jenkins不执行任何的安全检查，这意味着任何人都可以访问Jenkins来配置Jenkins，修改job，和执行build。这对于在企业内部使用也许可以接受，但是存在很高的安全风险，例如其他人错误滴删除了job，错误地配置你的job在每分钟运行，启动太多的builds等。所以一般使用plugin来对Jenkins增加授权和访问控制。
* 有规律地对Jenkins的home目录的备份。
* 使用file fingerprinting来管理依赖关系。当在Jenkins上你的job依赖其他的job时，可以使用file fingerprinting来帮助定位依赖的版本信息。
* 最可靠的build是clean builds，clean builds意思是与build相关的所有的3rd party，build脚本，发布说明等都需要在Source code control。
* 与issue tracking系统紧密的集成，例如JIRA或bugzilla，从来减少对change log的修改。
* 与repository浏览工具紧密的集成，例如FishEye如果你使用Subversion作为source code管理工具。
* 总是配置job产生趋势报告和自动化测试，当你运行一个Java build。趋势报告帮助项目经理和开发人员快速地了解当前项目的进度和状态。
* 确保Jenkins的home目录拥有足够的空间。
* 在删除不使用的job前请先存档。
* 为不同的branch建立不同的job，build来尽早地发现错误。
* 为并行的项目builds分配不同的端口，来避免多个jobs同时启动时所遇到的冲突。
* 为不同的项目的开发人员建立email aliais，使得项目所有相关的人员都第一时间了解项目的状态。
* 增加额外的步骤来尽早地发现失败。例如log检查，微测试等。
* 对于经常的维护性的工作可以使用job来自动地完成，例如对磁盘的清除工作。
* 在build成功后对远代码Tag，label或baseline。
* 配置Jenkins bootstrapper来在build前更新工作目
Jenkins中执行batch和Pythonﾧ
Jenkins的job->build 支持Ant，maven，windows batch和Shell， 但是我们知道python，perl，ruby等脚本其实也是shell脚本，所以这里的Shell可以扩展为python，perl，ruby等。
例如： 下面执行windows batch 和python

执行后的输入如下：

可以看到windows batch和shell脚本被保存到slave上的临时目录下，然后再执行。

Jenkins的授权和访问控制ﾧ
一 Jenkins的授权和访问控制	
默认地Jenkins不包含任何的安全检查，任何人可以修改Jenkins设置，job和启动build等。显然地在大规模的公司需要多个部门一起协调工作的时候，没有任何安全检查会带来很多的问题。 我们可以通过以下2方面来增强Jenkins的安全：
1） Security Realm，用来决定用户名和密码，且指定用户属于哪个组；
	2） Authorization Strategy，用来决定用户对那些资源有访问权限；
在Manage Jenkins -> Configure System -> Enable Security 下可以看到可以使用多种方式来增强Jenkins的授权和访问控制。


二 创建管理员账号+匿名只读
简单地设置一个管理员账号，用来管理jenkins设置，修改job和执行build等。其他的匿名访问的用户将只有只读的权限，不能修改Jenkins的设置，不能修改job，且不能运行build，但是可以访问build的结构，查看build的log等。

1）需要对Jenkins增加如下的启动参数来创建管理员账号：
java -jar jenkins.war --argumentsRealm.passwd.user=password --argumentsRealm.roles.user=admin

例如设置管理员用户为jenkins且密码为swordfish，如下：
java -jar jenkins.war --argumentsRealm.passwd.jenkins=swordfish --argumentsRealm.roles.jenkins=admin
2）在启动后需要在Manage Jenkins -> Configure System来选择enable security，然后选择对Security Realm选择Delegate to servlet container，对Authorization选择Legacy Mode。


3） 然后可以在右上角点击login或者 http://yourhost/jenkins/loginEntry来登录，登录后此时你用户管理员权限，可以执行任何的操作。执行完操作后可以选择logout。

4） 对developers增加rebuild的权限。使用管理员登录对需要developer rebuild的job，选择Trigger builds remotely，且设置Authentication Token，例如设置为devbuild，然后developers可以访问http://jenkinsHost/job/project/build?token=token 来启动build。
其中Project为你需要启动build的job。
其中token为你设置的Authentication Token。
如果你有webserver，你可以创建如下的webpage来让developers来启动build：
<h1>Jenkins Force Build Page</h1>
<ul>
    <li>
    <a href="http://jenkins:8080/job/FOO/build?token=build">Force build of Project FOO on Jenkins</a>
    </li>
</ul>

三 使用Jenkins的数据库管理用户且设置用户的访问权限
1）在Manage Jenkins -> Configure System -> Enable Security下为Security Realm选择Jenkins's own user database，且确保Allow users to sign up选中，为Authentication选择Matrix-based security。如下：


2） 接着在主页上signup创建第一个管理员账号jenkins如下：

3） 除了第一个账号以后signup的账号将为只读账号需要管理员分配权限。例如你可以signup来创建dev账号，然后分配权限使得dev可以启动job的build。如下

注意：
在%JENKINS_HOME%\config.xml中可以查看和修改所有用户的权限设置，但是修改后需要重新启动Jenkins。

Jenkins插件之Perforce访问ﾧ
Perforce Plugin，在Jenkins的管理页面的插件管理下面安装Perforce插件，然后重启Jenkins。
一 使用perforce插件来build
对job的设置如下图：

job执行后的log如下：

可以看到Jenkins在执行的过程中创建了新的clientspec，新的clientspec是拷贝自上面参数workspace设置的clientspec，且修改了新的clientroot目录，其中的view是来自上面参数view->mapping中的设置。
如下：

二 使用perforce插件的poll功能来触发build
配置如下：


查看如下：

三 使用perforce插件在Jenkins中查看最新的修改

四 使用perforce的label功能来对成功的build进行label



五 使用perforce插件的自动label功能


Jenkins插件之triggerﾧ
一 Jenkins内置的trigger插件
1） build after other projects are built 
可以设置多个依赖的jobs，当任意一个依赖的jobs成功后启动此build。  多个依赖的jobs间使用,隔开。
2） Trigger builds remotely (e.g., from scripts)
在Authentication Token中指定TOKEN_NAME，然后可以通过连接JENKINS_URL/job/JOBNAME/build?token=TOKEN_NAME来启动build。
3）  build periodically
在schedule中设置，语法类似于cron中语法。
4） Poll SCM 
在schedule中设置时间间隔来抓取SCM server，如果有新的修改，则启动build。 所以这里的作用相当于continous build。

二 其他的trigger插件
需要手动安装插件。 
Maven Dependency Update trigger ： 当有检测到有Maven dependency 跟新的时候启动build，类似于continuous build。

BuildResultTrigger Plugin： 根据其他的job的成功或失败来启动此build。
Files Found Trigger：检测指定的目录，如果发现指定模式的文件则启动build。

Jenkins插件之构建与MSBuildﾧ
一 Jenkins内置的buildtools
Jenkins已经内置了Ant|Maven|Windows batch|Shell(Perl,Python)的支持。

二 其他的buildtools
cmakebuilder Plugin ： 支持cmake的构建；
Copy Artifact Plugin ： 拷贝依赖的组件；
Job Exporter Plugin ： 将当前的运行参数导出到属性文件， 可以供以后的步骤调用；
MSBuild Plugin： 使用MSBuild来构建.NET工程；
NAnt Plugin： 用来支持NAnt;
Python Plugin : 用来支持python；
qmakebuilder Plugin ： 用来支持qmake；
Rake plugin ： 用来支持rake构建；
SCons Plugin ： 用来支持Scons构建；
Xcode Plugin ： 用来支持MAC，iphone等的构建；

三 使用MSBuild 来构建CsharpHelloWorld
1） CSharp 的console project代码如下：

2） 创建Jenkins job CSharpHelloWorld，设置如下： 需要确保slave机器上msbuild的路径在系统path环境变量中，例如C:\Windows\Microsoft.NET\Framework\v4.0.30319



3） build结果如下：

Jenkins插件之环境变量插件EnvInjectﾧ
一 Master/Slave的Node Properties
用来定义slave特定的变量，例如很多的命令所在的路径。

二 job中的build paramete

设置后在build启动的时候提示修改也可以使用默认值。例如启动改build的时候决定是build release还是debug。

启动build时提示如下：

三 EnvInject插件
需要手动安装此插件，用来对job定义环境变量，还可以定义的ob的step来在build的过程中修改环境变量，例如为job定义公共的post location：


在job的step中修改变量，例如修改buildplatform的值：

 四 运行结果如下：


完
