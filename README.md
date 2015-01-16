# asset-cache-control
###基于maven插件的缓存控制工具，通过修改资源url的请求参数，比如在url后面添加版本号或者时间戳的形式，来有效的防止浏览器缓存。

目前该功能可用于避免js、css、image 三种文件类型缓存

###用法：

1.添加插件asset-cache-control 到pom文件中：

```xml
<build>
	<plugins>
		<plugin>
			<groupId>org.zt</groupId>
			<artifactId>asset-cache-control</artifactId>
			<version>0.0.2</version>
			<executions>
				<execution>
					<id>version</id>
					<phase>prepare-package</phase>
					<goals>
						<goal>version</goal>
					</goals>
					<configuration>
						<!-- 资源URL前缀,可选 -->
						<resourcesURL>http://res.github.com/</resourcesURL>
						<!-- 后缀，可选，默认支持jsp、html、htm、ftl，如果填写则覆盖默认文件后缀，只会处理指定的文件后缀 -->
						<suffixs>
							<suffix>jsp</suffix>
						</suffixs>
						<!-- 版本号，给资源url添加的版本号，如果为空，则打上当前时间戳 -->
						<version>${project.version}</version>
						<!-- 需要打包的静态资源目录，可选，package命令需要，如果指定，则只打当前指定目录下面的文件到静态资源包中 -->
						<resourcesDirs>
							<resourcesDir>css</resourcesDir>
							<resourcesDir>font</resourcesDir>
							<resourcesDir>html</resourcesDir>
							<resourcesDir>images</resourcesDir>
							<resourcesDir>js</resourcesDir>
						</resourcesDirs>
					</configuration>
				</execution>
			</executions>
		</plugin>
	</plugins>
</build>
```


2.执行命令：
执行maven命令，用来替换工程中所有的动态文件中引用的静态资源URL路径。
```html
 打版本命令： mvn asset-cache-control:version
打静态资源到独立war包命令：mvn asset-cache-control:package
```

该命令会自动添加版本号或者时间戳到静态资源URL后面，自动添加静态资源域名在url前面（如果有配置静态资源域名），例如 ：

原始：
```html
<script type="text/javascript" src="/javascripts/jquery-1.10.2.min.js"></script>
<link href="/css/bootstrap.min.css" rel="stylesheet">
```

执行后效果：

版本号模式
```html
	<script type="text/javascript" src="http://res.github.com/javascripts/jquery-1.10.2.min.js?v=1.1.0"></script>
	<link href="http://res.github.com/css/bootstrap.min.css?v=1.1.0" rel="stylesheet">
```

时间戳模式
```html
  <script type="text/javascript" src="http://res.github.com/javascripts/jquery-1.10.2.min.js?v=14298124845"></script>
  <link href="http://res.github.com/css/bootstrap.min.css?v=14298124845" rel="stylesheet">
```

