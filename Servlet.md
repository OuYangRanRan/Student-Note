##Servlet学习笔记##
--------
	1.Servlet
	服务器端小应用程序
	它运行在Servlet容器中，它是由服务器直接解析运行。		
	2.怎样创建一个Servlet
		1.通过查看示例，发现Servlet在创建时它会继承一个HttpServlet类。
		2.重写doGet,doPost等方法。
	3.创建一个servlet步骤:
		1.创建一个类，去继承HttpServlet（实现Servlet接口 或继承 GenericServlet）
		2.重写方法  doPost  doGet  (接口中需要将所有方法重写，但是service方法是处理请求)
		3.在web.xml文件中配置
		
	4.servlet生命周期
		
		1.创建servlet，并调用init方法
		2.通过service方法来处理请求。
		3.通过destroy来销毁servlet。
		
		当我们创建一个servlet时，第一次访问，会创建servlet对象，并调用init方法，init方法只调用一次。
		开启一个线程来调用service方法。
		当下一次在访问时，又开启一个线程来调用service方法。
		servlet对象，一般情况只创建一次，它常驻内存。
		当服务停止，或servlet对象销毁时会调用destroy方法。
		
		注意:如果在servlet中创建了成员变量，就可以出现线程安全问题
		在开发中，尽量避免在servlet中创建成员变量。
		如果创建了，就需要同步。
		
	5.关于Servlet接口中service方法与HttpServlet中的doPost,doGet什么关系?
	
		一个servlet被访问时，它默认处理请求的是service方法，这个方法是在Servlet接口中定义的。
		
		根据请求的url，在web.xml文件中查找到具体的servlet,
		会默认调用service方法。
		对于我们来说，它在HttpServlet中对service方法进行了重写，执行其内容

		在HttpServlet的service方法中，会根据请求方式的不同，分别调用不同的方法。
		根据java中的多态，在调用doPost,doGet时，会调用的是我们自己的servlet中的方法。(回调doPost和doGet)

		这里面涉及到一个java模式:模板方法模式(在父类中调用子类的内容 )
			

	5.模板方法模式:
		模板模式是类的行为模式。
	1.定义：定义一个操作中算法的骨架（或称为顶级逻辑），将一些步骤（或称为基本方法）的执行延迟到其子类中.
	2.模板模式与继承
        模板方法估计恰当地使用继承。此模式可以用来改写一些拥有相同功能的相关的类，将可复用的一般性行为代码移到基类里面，而把特殊化的行为代码移到子类里面。熟悉模板方法模式是重新学习继承的开始。
	3.模板模式中的方法
    1)模板方法：必须由抽象类实现，该方法是一个顶级逻辑，调用任意多个基本方法。子类不应该修改该方法
    2)基本方法：模板方法所调用的方法，有可细分为抽象方法，具体方法，钩子方法
        抽象方法：强迫子类重写的
        具体方法：不需要子类重写的，最好声明为final
        钩子方法：子类可以重写的，一般是个空方法(钩子方法的命名应该以do开头，这是一个通用规范)
    补充：模板模式的设计理念是尽量减少必须由子类置换掉的基本方法的数量(可以理解为尽量减少抽象方法和钩子方法的数量。)
	4.重构的原则
	总的原则：行为上移，状态下移(抽象类中的具体方法应该尽量多，而成员变量应该尽量少)
    1)应当根据行为而不是状态定义一个类
    2)在实现行为时，应该尽量用取值方法获取成员变量，而不是直接应用成员变量
    3)给操作划分层次。一个类的行为应当放到一个小组核心方法里面，这些方法可以很方便地在子类中置换
    4)将状态的确认推迟到子类中去。
	5.使用模板模式，用多态取代条件转移(也可以使用策略模式)
	6.问题：模板模式和策略模式的区别
    补充：个人认为这个模式比较好理解，而且在实际编程中是十分常用。虽然如此，但是学习这个模式的过程中，我还是有许多收获的，主要是加深了对继承的理解，对OO的核心思想有了新的认识，就像阎博士说的“熟悉模扳方法模式便成为一个重新学习继承的好地方”
    模板模式：原理为：子类对象的方法覆盖了父类的方法（父类对象调用子类方法）。
    策略模式：确定操作的方法和策略，策略的具体实现行为不同。通过组合实现。原理就是：组合大于继承，实现更大的灵活性。
	例子：
	类加载，dao实现对object的增删改查。
	适用的范围：许多应用的实现有许多公共的部分，但细节有差异。
	1.从一张数据表，生成许多统计报表。
	2.severlet对象service方法中doget，dopost方法。
	
	7,load-on-startup标签的作用是跟随服务器一起启动
	<load-on-startup>2</load-on-startup>
	作用:用于加载一些资源,提前加载在加载时对内存消耗较大的资源,为了提高效率
	<load-on-startup>的取值为1-10的优先级,优先级默认为10,是服务器底层预定的,数值越小优先级越高
	8,Servlet的url-pattern配置问题:
		1),一个Servlet运行被多个路径映射
		2),url-pattern的写法?
		   1.完全匹配  要求必须以/开始
		   2.使用通配符
			目录匹配:必须以/开始,以.*结束
			<url-pattern>/Servlet/*</url-pattern>
			扩展名匹配:不能以/开头,必须以*.xxxx结束.
			<url-pattern>*.action</url-pattern>
			
			最经典的错误  :/*.do

			url-pattern的配置有优先级的:完全匹配>目录匹配>扩展名匹配
	9,Servlet接口中的方法:
		在Servlet接口中定义了init方法是有参数的,它的参数类型是ServletConfig,而我们重写的init方法没有参数,因为弗雷中已经将init(ServletConfig config)这个方法进行了重写,而我们在自己的Servlet中就不用重写了
		config.getServletName();
		<init-pram></init-pram><!--初始化配置-->
	10,ServletConfig类的功能:		
		功能:
			1.可以获取Servlet的name。
			2.可以获取初始化参数
				是我们可以在Servlet的配置中通过<init-param>来进行配置，
				对于这些配置，我们可以通过ServletConfig来获取.
				
				通过初始化参数，我们可以进行预定义信息的加载，这种操作一般情况是在框架中应用。
			3.可以获取ServletContext对象	
			  ServletContext:每个web应用都具有一个ServletContext对象,ServletContext对象是Servlet所共有的,所有的Servletconfig对象都能得到它	
			常用API总结:
				public String getServletName();
				用于获取Servlet名称
				
				public String getInitParameter(String name);
				用于获取init-param中指定名称的value值.
				
				public Enumeration getInitParameterNames();
				用于获取所有的init-param中的name名称.
				
				public ServletContext getServletContext();
				用于获取一个ServletContext对象。

		ServletConfig要求掌握内容:
			1.它是由服务器创建，通过init方法传递到我们的Servlet中。
			  我们在Servlet中通过  this.getServletConfig()获取ServletConfig对象.
			  
			2.ServletConfig对象是用于获取当前Servlet的配置信息.
				1.Servlet的名称
				2.Servlet的初始化参数。		
			每一个Servlet都具有自己的ServletConfig对象。它们之间是不能共享的。

	11.关于ServletContext对象.
		ServletContext它代表的是一个web应用。
		
		怎样获取一个ServletContext对象?
			可以通过ServletConfig对象获取.
			
			ServletConfig.getServletContext();
			
		作用:
			1.它可以获取全局初始化参数
				String getInitParameter(String name)  
				Enumeration getInitParameterNames()  
				
				在web.xml文件中可以配置我们的全局初始化参数
				<!-- 配置全局初始化参数 -->
				<context-param>
					<param-name>name</param-name>
					<param-value>tom</param-value>
					<param-name>name1</param-name>
					<param-name>jack</param-name>
				</context-param>
				
				它的配置是针对于整个web应用.
				然后通过代码中的 ServletContext来获得全局参数
				servletContext.getInitParameter("tom");
			2.它可以帮助我们让servlet实现信息共享.
			
				ServletContext是一个域对象，提到域对象想到两件事,一个是它相当于是一个Map，另一个它有作用域，对于
				ServletContext它的作用域是整个web应用。
				
				示例:统计站点访问次数
					问题:怎样判断是第一次访问
					
				setAttribute(String name,Object value);
				getAttribute(String name);
				removeAttribute(String name);
			
			3.可以获取路径（资源）***********************
			
				在web开发中，如果要想获取资源，必须使用绝对的磁盘路径。
				
				怎样可以得到在服务器上的文件的绝对磁盘路径?
					
				1.String getRealPath(String s);
					
					ServletContext.getRealPath("/"); 得到当前工程在磁盘上的绝对路径。
				
				2.URL  gerResource(String s);
					得到一个URL路径，它也是指向我们的web工程下的资源。
					
				3.InputStream getResourceAsStream(String s);
					得到一个指向资源的输入流.
					
				扩展：对于classes目录下的文件，它有其它的方式可以获取到.
				
					Class.getResource("/").getPath();  获取的就是class文件所有的目录的绝对磁盘路径。
				
					对于存在于classes目录下的文件，我们可以通过classpath的方式去获取其绝对磁盘路径。				
					
				
				
			4.其它功能（了解）
				1.获取MIME类型
					String getMimeType(String file)   文件下载时会用到.
					
				2.分发请求
					 RequestDispatcher getRequestDispatcher(String path)  
					 在开发中不常使用，如果使用通过request对象来操作。
				3.日志
					log方法是用来处理日志.	