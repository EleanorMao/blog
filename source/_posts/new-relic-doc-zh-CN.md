---
title: New Relic 中文文档
date: 2017-05-18 17:28:41
tags:
- javascript
- doc
---
# New Relic Node.js代理文档

## 系统要求

* 操作系统：
	*  Linux
	*  SmartOS
	*  MacOSX 10.7 及更高
	*  Windows Server 2008 及更高
* NODE版本： 使用最高稳定版本(现在是6.x版本)的NODE会为New Relic带来更好的体验。支持4.x及最新版本，但不支持不稳定版本的NODE（不支持0.10.34版本）。
* 框架：
	* Express2.0及更高。
	* Restify
	* Connect 1 and Connect 2 (不支持router)
	* Hapi
	* Kraken
	* 如果你使用的是框架的默认路由，那么New Relic的NODE代理是可以读到这些路由名的。但是如果你希望使用自定义的名字，那么你可以用[node.js 事务命名 API](https://docs.newrelic.com/docs/agents/nodejs-agent/supported-features/nodejs-agent-api)
* 数据存储：
	* Cassandra（配合cassandra-driver，而且NODE版本要0.10或更高)
	* Memcached
	* MongoDB
	* MySQL 0.9 and 2.0
	* Redis
	* Postgres
	* Oracle
* 主机服务
	* Heroku
	* Microsoft Azure
	* AWS EC2
* 安全要求
	* 支持SHA-2(256-bit)，不支持SHA-1

## 安装
### 指南
* 在NODE最新的稳定版本上有最好的体验，最低支持0.8版本，不支持不稳定版本
* `npm install newrelic --save`
* 从`node_modules/newrelic`中把`newrelic.js`拷贝到应用的根目录下
* 修改`newrelic.js`，特别重要的是要把`license_key`更换成自己账号的(可以在Account Setting)中找到
* 在你的应用的主模块第一行中`require('newrelic');`
* 选用: 为了保证你应用配置独立于应用，可以在配置`NEW_RELIC_HOME`变量(如果使用的是`bash`，那么在~/.bashrc中配置`export NEW_RELIC_HOME="引用根目录"`)
* 选用: 在New Relic的界面配置服务端需要的参数
* 选用: 安装`@newrelic/native-metrics`包，作为额外的NODE运行状况监测工具

### 可以看你的应用运行状况啦
在 `APM > Applications > (selected app) > Overview`中查看

### 记得及时更新

## NODE代理配置

你可以通过修改`newrelic.js`的配置来设置你的环境变量，你也可以通过管理界面来修改一些，还有记得要把`newrelic.js`放在应用根目录下面哟

### 配置方法和优先级
主要的推荐的配置方法就是修改那个js。
优先级如图
![优先级](https://docs.newrelic.com/sites/default/files/styles/full_size/public/thumbnails/image/nodejs%20config%20cascade_0.png?itok=r_yPD--g)
### 可配置变量
一般就是说在那个js里面配的，可以配在环境变量里面的会另外说明名字什么的。用`exports.config = {`裹住。

<table border="1" cellpadding="1" cellspacing="0">
	<thead>
	<tr>
		<th>名字</th>
		<th>类型</th>
		<th>默认值</th>
		<th>环境变量名</th>
		<th>其他</th>
	</tr>
	</thead>
	<tbody>
		<tr>
			<td>**`app_name`**</td>
			<td>String</td>
			<td>"My Application"</td>
			<td>`NEW_RELIC_APP_NAME`</td>
			<td>必要的，可以写多个。Azure用户还有些其他的嘱咐，自己去[看](https://docs.newrelic.com/docs/agents/nodejs-agent/installation-configuration/nodejs-agent-configuration)吧</td>
		</tr>
		<tr>
			<td>**`license_key`**</td>
			<td>String</td>
			<td></td>
			<td>`NEW_RELIC_LICENSE_KEY`</td>
			<td>在Account Setting里面靠右边可以找到</td>
		</tr>
		<tr>
			<td>`host`</td>
			<td>String</td>
			<td>collector.newrelic.com</td>
			<td>`NEW_RELIC_HOST`</td>
			<td>不要编辑这个变量，除非New Relic Support要求你修改它(这个是[New Relic collector](https://docs.newrelic.com/docs/accounts-partnerships/education/getting-started-new-relic/glossary#collector)用的)</td>
		</tr>
		<tr>
			<td>`port`</td>
			<td>Integer</td>
			<td>443</td>
			<td>`NEW_RELIC_PORT`</td>
			<td>不要编辑这个变量，除非New Relic Support要求你修改它(这个是[New Relic collector](https://docs.newrelic.com/docs/accounts-partnerships/education/getting-started-new-relic/glossary#collector)用的)</td>
		</tr>
		<tr>
			<td>`proxy`</td>
			<td>String</td>
			<td></td>
			<td>`NEW_RELIC_PROXY_URL`</td>
			<td>如果你用了`proxy`配置，它会覆盖其他的代理配置(`proxy_host`, `proxy_port`, `proxy_user`, `proxy_pass`)。同理，`NEW_RELIC_PROXY_URL`环境变量也会覆盖掉其他代理的环境变量配置</td>
		</tr>
		<tr>
			<td>`proxy_host`</td>
			<td>String</td>
			<td></td>
			<td>`NEW_RELIC_PROXY_HOST`</td>
			<td></td>
		</tr>
		<tr>
			<td>`proxy_port`</td>
			<td>String</td>
			<td></td>
			<td>`NEW_RELIC_PROXY_PORT`</td>
			<td></td>
		</tr>
		<tr>
			<td>`proxy_user`</td>
			<td>String</td>
			<td></td>
			<td>`NEW_RELIC_PROXY_USER`</td>
			<td></td>
		</tr>
		<tr>
			<td>`proxy_pass`</td>
			<td>String</td>
			<td></td>
			<td>`NEW_RELIC_PROXY_PASS`</td>
			<td>密码</td>
		</tr>
		<tr>
			<td>`ignore_server_configuration`</td>
			<td>Boolean</td>
			<td>`false`</td>
			<td>`NEW_RELIC_IGNORE_SERVER_CONFIGURATION`</td>
			<td>启用后会忽略服务端配置</td>
		</tr>
		<tr>
			<td>`agent_enabled`</td>
			<td>Boolean</td>
			<td>`true`</td>
			<td>`NEW_RELIC_ENABLED`</td>
			<td>设置成`false`停用代理，可以在开发时临时使用</td>
		</tr>
		<tr>
			<td>`apdex_t`</td>
			<td>Number</td>
			<td>`0.100`</td>
			<td>`NEW_RELIC_APDEX`</td>
			<td>性能指数，服务端配置名是`Apdex T`。单位是秒。你可以在服务端配置它。100毫秒是低于标准New Relic性能配置的。但是node.js比别的语言要延时敏感一些。具体可以看[这边](https://docs.newrelic.com/docs/agents/nodejs-agent/installation-configuration/nodejs-agent-configuration#tracer_threshold)</td>
		</tr>
		<tr>
			<td>`capture_params`</td>
			<td>Boolean</td>
			<td>`false`</td>
			<td>`NEW_RELIC_CAPTURE_PARAMS`</td>
			<td>启用后可以捕获请求参数(配合`事务追踪配置`和 `错误追踪配置`)，因为他会放过一些敏感数据，所以默认是`false`。补充设置可以查看`ssl`和`ignored_params`</td>
		</tr>
		<tr>
			<td>`ignored_params`</td>
			<td>String</td>
			<td></td>
			<td>`NEW_RELIC_IGNORED_PARAMS`</td>
			<td>列一堆不需要补货的参数名，默认是`ignored_params : []`</td>
		</tr>
		<tr>
			<td>`ssl`</td>
			<td>Boolean</td>
			<td>`true`</td>
			<td>`NEW_RELIC_USE_SSL`</td>
			<td>如果你设置了`high_security`一定要启用。一般情况下，你会在`安全v1模式`下的，如果你关了`ssl`还启用了`capture_params`，那你就退出了高安全模式，会组织代理链接高安全账户，这时你的log会出现`ERROR Account Security Violation`错误</td>
		</tr>
		<tr>
			<td>`certificates`</td>
			<td>Array of strings</td>
			<td></td>
			<td></td>
			<td>`certificates: [ fs.readFileSync('myca.crt', {encoding: 'utf8'}) ]`</td>
		</tr>
		<tr>
			<td>`high_security`</td>
			<td>Boolean</td>
			<td>`false`</td>
			<td>`NEW_RELIC_HIGH_SECURITY`</td>
			<td>启用后进入`安全v2模式`，同时必须启用`ssl`，还有在管理界面中启用`high security`</td>
		</tr>
		<tr>
			<td>`browser_monitoring.enable`</td>
			<td>Boolean</td>
			<td>`true`</td>
			<td>`NEW_RELIC_BROWSER_MONITOR_ENABLE`</td>
			<td>服务端标签是`Enable browser monitoring?`。为页面加载计时生成JavaScript标头，但是除非你启用了`New Relic Browser`，这个并没有什么卵用</td>
		</tr>
		<tr>
			<td>`label`</td>
			<td>Object or string</td>
			<td></td>
			<td>`NEW_RELIC_LABELS`</td>
			<td>例子： `Server:One;Data Center:Primary`</td>
		</tr>
	</tbody>
</table>

### 日志变量
在`newrelic.js`里面用`logging`变量申明的
<table border="1" cellpadding="1" cellspacing="0">
	<thead>
		<tr>
			<th>名字</th>
			<th>类型</th>
			<th>默认</th>
			<th>环境变量</th>
			<th>其他</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<td>`level`</td>
		<td>String</td>
		<td>`info`</td>
		<td>`NEW_RELIC_LOG_LEVEL`</td>
		<td>用了[bunyan](https://github.com/trentm/node-bunyan)模块，共有`fatal`, `error`, `warn`, `info`, `debug`, `trace`这几种类型。一般情况下用`info`即可，非必要最好不要用`trance`或`debug`，因为会产生过多的记录</td>
	</tr>
	<tr>
		<td>`filepath`</td>
		<td>String</td>
		<td>`require('path').join(process.cwd(), 'newrelic_agent.log')`</td>
		<td>`NEW_RELIC_LOG`</td>
		<td>如果创建不了这个文件的话进程会挂掉</td>
	</tr>
	</tbody>
</table>

### 审核日志
在`newrelic.js`里面用`audit_log`变量申明的
<table border="1" cellpadding="1" cellspacing="0">
	<thead>
		<tr>
			<th>名字</th>
			<th>类型</th>
			<th>默认</th>
			<th>环境变量</th>
			<th>其他</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<td>`enabled`</td>
		<td>Boolean</td>
		<td>`false`</td>
		<td>`NEW_RELIC_AUDIT_LOG_ENABLED`</td>
		<td>启用时，代理会记录发送给收集器的有效内容。即使日志等级较低也会包含在主日志文件里边</td>
	</tr>
	<tr>
		<td>`endpoints`</td>
		<td>Array</td>
		<td>空数组 (包含所有类型)</td>
		<td>`NEW_RELIC_AUDIT_LOG_ENDPOINTS`</td>
		<td>
		代理在单独的有效负载中向收集器发送几种不同类型的数据。 默认情况下，它们都包含在日志文件中。 此选项可以将日志记录限制到特定类型的数据。可用的类型有：

1. `metric_data`
1. `error_data`
1. `analytic_event_data`
1. `custom_event_data`
1. `error_event_data`
1. `transaction_sample_data`
1. `sql_trace_data`
1. `connect`
1. `agent_settings`
1. `get_redirect_host`
1. `shutdown`
		</td>
	</tr>
	</tbody>
</table>

### 错误收集变量
在`newrelic.js`里面用`error_collector`变量申明的
<table border="1" cellpadding="1" cellspacing="0">
	<thead>
		<tr>
			<th>名字</th>
			<th>类型</th>
			<th>默认</th>
			<th>环境变量</th>
			<th>其他</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<td>`enabled`</td>
		<td>Boolean</td>
		<td>`true`</td>
		<td>`NEW_RELIC_ERROR_COLLECTOR_ENABLED`</td>
		<td>服务端标签是`Enable error collection?`。启用后代理会收集错误信息</td>
	</tr>
	<tr>
		<td>`ignore_status_codes`</td>
		<td>Array</td>
		<td>`404`</td>
		<td>`NEW_RELIC_ERROR_COLLECTOR_IGNORE_ERROR_CODES`</td>
		<td>服务端标签是`Ignore these status codes`。以逗号分隔需要被忽略的HTTP状态码</td>
	</tr>
	</tbody>
</table>

### 事务追踪变量
代理将你的请求分组为事务，用于：

* 可视化引用在事务故障上花费的时间
* 识别缓慢请求
* 组指标
* 隔离其他问题，比如说辣鸡数据库性能

在`newrelic.js`里面用`transaction_tracer`变量申明的
<table border="1" cellpadding="1" cellspacing="0">
	<thead>
		<tr>
			<th>名字</th>
			<th>类型</th>
			<th>默认</th>
			<th>环境变量</th>
			<th>其他</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<td>`enabled`</td>
		<td>Boolean</td>
		<td>`true`</td>
		<td>`NEW_RELIC_TRACER_ENABLED`</td>
		<td>服务端标签是`Enable transaction tracing?`。启用后代理会收集慢事务</td>
	</tr>
	<tr>
		<td>`transaction_threshold`</td>
		<td>Integer 或 `apdex_f`</td>
		<td>`apdex_f`</td>
		<td>`NEW_RELIC_TRACER_THRESHOLD`</td>
		<td>服务端标签是`Threshold`。web事务响应时间（以秒为单位）的阈值，超过该值，事务将有资格进行事务跟踪。 默认值为`apdex_f`; 这将跟踪阈值设置为应用程序的`apdex_f`的四倍。您还可以输入特定的时间值（以毫秒为单位）。</td>
	</tr>
	<tr>
		<td>`top_n`</td>
		<td>Integer</td>
		<td>`20`</td>
		<td>`NEW_RELIC_TRACER_TOP_N`</td>
		<td>定义适用于事务跟踪的最大请求数。要记录最后一分钟的绝对最慢事务，您可以设置top_n：0或top_n：1。但是，这会导致一个非常慢的路由来支配事务跟踪。
		</td>
	</tr>
	<tr>
		<td>`record_sql`</td>
		<td>String (`off`, `obfuscated`, `raw`)</td>
		<td>`off`</td>
		<td>`NEW_RELIC_RECORD_SQL`</td>
		<td>此选项同时影响事务跟踪的慢查询和record_sql。设置为`off`时，不会捕获缓慢的查询，并且事务跟踪中将不包括回溯和SQL。 如果设置为`raw`或`obfuscated`，代理将原始或模糊的SQL和慢速查询样本发送到收集器。 代理也可以在满足其他条件时发送SQL，例如，设置了`slow_sql.enabled`。		</td>
	</tr>
	<tr>
		<td>`explain_threshold`</td>
		<td>Integer</td>
		<td>`500`</td>
		<td>`NEW_RELIC_EXPLAIN_THRESHOLD`</td>
		<td>事务的最小查询长度（以毫秒为单位），适用于慢速查询和事务跟踪。</td>
	</tr>
	</tbody>
</table>

### 调试变量
在`newrelic.js`里面用`debug`变量申明的
<table border="1" cellpadding="1" cellspacing="0">
	<thead>
		<tr>
			<th>名字</th>
			<th>类型</th>
			<th>默认</th>
			<th>环境变量</th>
			<th>其他</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<td>`internal_metrics`</td>
		<td>Boolean</td>
		<td>`false`</td>
		<td>`NEW_RELIC_DEBUG_METRICS`</td>
		<td>不要随便修改这个值，除非New Relic要你改。启用后，代理程序将收集内部可支持性指标和诊断以及性能指标。</td>
	</tr>
	<tr>
		<td>`tracer_tracing`</td>
		<td>Boolean</td>
		<td>`false`</td>
		<td>`NEW_RELIC_DEBUG_TRACER `</td>
		<td>不要随便修改这个值，除非New Relic要你改。启用后，代理会跟踪事务跟踪器本身的内部操作。 需要将日志级别设置为`trace`。</td>
	</tr>
	</tbody>
</table>

### 规则变量
在`newrelic.js`里面用`rules`变量申明的
<table border="1" cellpadding="1" cellspacing="0">
	<thead>
		<tr>
			<th>名字</th>
			<th>类型</th>
			<th>默认</th>
			<th>环境变量</th>
			<th>其他</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<td>`name`</td>
		<td>Strings 或 regular expressions</td>
		<td></td>
		<td>`NEW_RELIC_NAMING_RULES`</td>
		<td>以逗号分隔的规则列表，以匹配传入请求URL并命名关联的New Relic事务。使用格式{pattern：'STRING_OR_REGEX'，name：'NAME'}。正则支持`JavaScript`风格的捕获组以及$1风格的替换字符串。正则表达式只找到第一个匹配结果; 后续匹配被忽略。更多请查看[node.js 事务命名 API](https://docs.newrelic.com/docs/agents/nodejs-agent/supported-features/nodejs-agent-api)。对于`NEW_RELIC_NAMING_RULES `环境变量，将规则作为逗号分隔的JSON对象常量传递，如:`NEW_RELIC_NAMING_RULES='{"pattern":"^t","name":"u"},{"pattern":"^u","name":"t"}'` </td>
	</tr>
	<tr>
		<td>`ignore`</td>
		<td>Strings 或 regular expressions</td>
		<td>正则表达式匹配socket.io长轮询请求`("^\/socket\.io\/.*\/xhr-polling/")`</td>
		<td>`NEW_RELIC_IGNORING_RULES`</td>
		<td>定义希望代理忽略的请求URL列表。</td>
	</tr>
	<tr>
		<td>`enforce_backstop`</td>
		<td> Boolean</td>
		<td>`true`</td>
		<td>`NEW_RELIC_IGNORING_RULES`</td>
		<td>除非你了解[指标分组问题](https://docs.newrelic.com/docs/features/metric-grouping-issues)，否则请勿更改此设置。启用时，代理将不受其他命名逻辑（API，规则(`rules`)或度量标准化规则(`metric normalization rules`)）影响的事务重命名为`NormalizedUri/*`。 如果将此值设置为false，代理将事务名称设置为`Uri/path/to/resource`。</td>
	</tr>
	</tbody>
</table>

### 事务事件变量
在`newrelic.js`里面用`transaction_events`变量申明的。当前没有事务事件的环境变量。
<table border="1" cellpadding="1" cellspacing="0">
	<thead>
		<tr>
			<th>名字</th>
			<th>类型</th>
			<th>默认</th>
			<th>其他</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<td>`enabled`</td>
		<td>Boolean</td>
		<td>`true`</td>
		<td>启用时，代理将事务事件发送到[New Relic Insights](https://docs.newrelic.com/docs/insights)。此事件数据包括事务时间，事务名称和任何自定义参数。如果禁用此选项，则代理程序不会收集此数据或将其发送到Insights。
</td>
	</tr>
	<tr>
		<td>`max_samples_per_minute`</td>
		<td>Integer</td>
		<td>10000</td>
		<td>定义代理每分钟收集的最大事件数。如果有超过此数字，代理将收集统计抽样。</td>
	</tr>
	<tr>
		<td>`max_samples_stored`</td>
		<td> Integer </td>
		<td>20000</td>
		<td>定义代理程序无法与New Relic收集器通信时存储的事件的最大数量。确保此数字大于`max_samples_per_minute`。在增加此值之前，务必考虑内存开销。</td>
	</tr>
	</tbody>
</table>

### 慢查询变量
在`newrelic.js`里面用`slow_sql`变量申明的。
<table border="1" cellpadding="1" cellspacing="0">
	<thead>
		<tr>
			<th>名字</th>
			<th>类型</th>
			<th>默认</th>
			<th>环境变量</th>
			<th>其他</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<td>`enabled`</td>
		<td>Boolean</td>
		<td>`false`</td>
		<td>`NEW_RELIC_SLOW_SQL_ENABLED`</td>
		<td>启用时，代理会收集慢查询的详细内容</td>
	</tr>
	<tr>
		<td>`max_samples`</td>
		<td>Integer</td>
		<td>10</td>
		<td>`NEW_RELIC_MAX_SQL_SAMPLES`</td>
		<td>定义代理每分钟收集的慢速查询的最大数量。在达到限制后，代理将丢弃其他查询。使用增加该变量会增大内存消耗。</td>
	</tr>
	</tbody>
</table>

### 自定义主机名变量
在`newrelic.js`里面用`process_host`变量申明的。这些选项控制有关New Relic APM UI中主机显示名称的行为。
<table border="1" cellpadding="1" cellspacing="0">
	<thead>
		<tr>
			<th>名字</th>
			<th>类型</th>
			<th>默认</th>
			<th>环境变量</th>
			<th>其他</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<td>`display_name`</td>
		<td>String of 255 bytes or less</td>
		<td></td>
		<td>`NEW_RELIC_PROCESS_HOST_DISPLAY_NAME`</td>
		<td>指定要在New Relic UI中显示的自定义主机名。如果不设置此字段，New Relic将调用`os.hostname()`找到的默认主机名。</td>
	</tr>
	<tr>
		<td>`ipv_preference`</td>
		<td>Integer(`4` 或 `6`)</td>
		<td>4</td>
		<td>`NEW_RELIC_IPV_PREFERENCE`</td>
		<td>如果你的`display_name`用的是默认设置，那么当获取不到默认主机名时，会展示ip。</td>
	</tr>
	</tbody>
</table>

### 环境变量重写
本节定义了两个仅通过环境变量可用的配置选项。 这些重写在大多数配置中不使用。
<table border="1" cellpadding="1" cellspacing="0">
	<thead>
		<tr>
			<th>名字</th>
			<th>类型</th>
			<th>默认</th>
			<th>其他</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<td>`NEW_RELIC_HOME`</td>
		<td>String</td>
		<td></td>
		<td>只能作为环境变量配置，配置的是包含`newrelic.js`的文件夹路径</td>
	</tr>
	<tr>
		<td>`NEW_RELIC_NO_CONFIG_FILE`</td>
		<td>Boolean</td>
		<td>`false`</td>
		<td>只能作为环境变量配置，阻止代理从newrelic.js读取配置设置</td>
	</tr>
	</tbody>
</table>

### 数据库追踪变量
在`newrelic.js`里面用`datastore_tracer`变量申明的。
<table border="1" cellpadding="1" cellspacing="0">
	<thead>
		<tr>
			<th>名字</th>
			<th>类型</th>
			<th>默认</th>
			<th>其他</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<td>`instance_reporting.enabled`</td>
		<td>Boolean</td>
		<td>`true`</td>
		<td>启用时，代理会收集某些数据库驱动程序的数据存储实例度量标准（例如主机和端口）。这些报告在慢查询跟踪和事务跟踪。</td>
	</tr>
	<tr>
		<td>`database_name_reporting.enabled`</td>
		<td>Boolean</td>
		<td>`true`</td>
		<td>启用后，代理将在某些数据库驱动程序的慢查询跟踪和事务跟踪上收集数据库名称。</td>
	</tr>
	</tbody>
</table>

## Node.js代理属性
在New Relic产品中，属性是从你应用中事务和事件中收集的键值对。New Relic的Node.js代理从HTTP请求和响应头中收集了一些基础的属性。

### Node.js代理属性
New Relic Node.js代理默认捕获一些基础的HTTP头数据，比如HTTP响应码和请求方式。NNew Relic将此默认数据称为代理属性。New Relic Node.js代理收集的属性的数据显示在：

* 事务事件
* 事务追踪
* 错误追踪
* 错误事件

你也可以添加自定义工具以捕获任何其他属性。
以下是一些默认属性：

1. `request.headers.accept`
2. `request.headers.host`
3. `request.headers.referer`
4. `request.headers.userAgent`
5. `request.method`
6. `httpResponseCode`
7. `httpResponseMessage`
8. `response.headers.contentLength`
9. `response.headers.contentType`

### 创建自定义属性
创建自定义属性可以用以下任意方式：

* 创建一个自定义事件：使用**`recordCustomEvent`**
* 添加到现有事务：使用**`addCustomParameters`**
* 添加多个属性： 使用**`addCustomParameters`**

## 支持的特性

### Node.js代理API
New Relic提供了多个工具帮助获取提供有关Node.js应用程序的有用指标所需的信息。包括：

* 从Express和Restify路由器读取路由名称（如果使用）
* 使用API命名当前请求，或使用简单的名称或操作控制器组
* 支持存储在代理配置中的规则，可以根据请求的原始URL（也可作为API调用）匹配的正则表达式来标记要重命名或忽略的请求。
为保证用户体验，New Relic需要跟踪足够小的名称数量。这也需要足够大以提供适当的信息量（不会使您的数据压倒），以便您可以更容易地识别您的应用程序中的问题点。

#### 请求名称
Node.js代理捕获HTTP方法使用一个潜在参数化的路径，类似于` /user/:id`，或正则，比如`/^/user/([-0-9a-f]+)/`。这些信息会成为请求名的一部分。
如果你支持慢事务追踪或者在配置文件中开启了`capture_params`，那么事务追踪会捕获请求参数和值。如果你对Node.js代理的请求名不满意，你也可以使用API创建描述性的名称。
*如果按通用名称对请求进行分组，则`/*`就足够了，您不需要自定义配置文件或API调用。*

#### 请求
New Relic使用请求名以将请求分组成多个图表。随着不同请求名称的数量增加，这些可视化的值将下降。
例如，不要在创建的请求名称中包含高度可变的信息，如GUID，数字ID或时间戳。如果你的请求速度足以生成事务跟踪，则该跟踪将包含原始URL。如果你启用参数捕捉，那么这些参数也会被捕获。
*避免拥有超过50个不同的事务名称。例如，如果您有超过几百个不同的请求名称，请重新思考您的命名策略。*

#### 避免度量分组问题
请求命名API帮助New Relic避免尝试处理太多指标的问题，有时称为“指标爆炸”。 New Relic有几个解决策略。最严重的是简单地将违规应用程序列入黑名单。你在使用这些请求命名工具时要小心的主要原因是为了防止这种情况发生在你的应用程序中。


#### 指南
定义您的配置规则从最一般到最具体。
配置文件中列出或添加的第一个Node.js事务命名API规则应该是最通用的“通用”规则。
更加详细的规则应该被添加到列表的末尾，因为它们会以相反顺序进行评估。

***例子***
有个网站的url大概长这样

```javascript
  /user/customers/all/prospects
  /user/customers/all/current
  /user/customers/all/returning
  /user/customers/John
  /user/customers/Jane
```
那么他配的规则可以是这样

```javascript
  // newrelic.js
  exports.config={
    //other configuration
    rules:{
      name:[
      { pattern: "/user/customers/.*", name: "/user/customers/:customer" },
      { pattern: "/user/customers/all/.*", name: "/user/customers/all" },
      { pattern: "/user/customers/all/prospects/", name: "/user/customers/all/prospects" }
  ]
}
```
根据这些规则，将会创建三个事务名

```javascript
/user/customers/:customer
/user/customers/all
/user/customers/all/prospects
```
如果把规则写反了的话，就会全部捕获成`:customer`事务。

#### 加载请求名API
确保在应用最前面加载New Relic模块

```javascript
var newrelic = require('newrelic');
```
这返回请求命名API。您可以安全地从应用程序中的多个模块导入模块，因为它只初始化一次。

#### 请求API

* **`newrelic.setTransactionName(name)`**
根据请求命名规则命名当前请求。你可以在任意HTTP请求句柄上下文中调用该函数(需在请求处理开始后和请求结束前)。一般来说，您可设置名称在请求和相应在作用域内。
显式调用`newrelic.setTransactionName()`会将`Express`或`Restify`路由设置的名称覆盖。同时，调用`newrelic.setTransactionName()`和`newrelic.setControllerName()`会互相覆盖。

* **`newrelic.setControllerName(name, [action])`**
使用控制器样式模式命名当前请求，可选择包括当前控制器操作。
如果省略`action`，`New Relic`会把HTTP方法（GET，POST等）作为`action`。调用`newrelic.setControllerName()`的规则与`newrelic.setTransactionName()`相同，包括请求命名要求。
显示调用`newrelic.setControllerName()`会将`Express`或`Restify`路由设置的名称覆盖。同时，调用`newrelic.setTransactionName()`和`newrelic.setControllerName()`会互相覆盖。

#### 自定义检测API
使用这些API来扩展检测工具

* **`createWebTransaction(url, handle)`**
检测指定web事务。调用此函数后，可检测New Relic不会自动检测的事务。
`url`是事务名称并且需要是静态的。不要包括用户ID等变量数据。
`handle`是检测函数。New Relic将捕获将由自动检测捕获的任何指标，以及通过`createTracer()`进行手动监测的指标。
如果在一个web事务中调用，那么代理会新建一个段。如果在一个后台事务中调用，那么改调用会创建一个新的独立事务，并捕获新事务内的`handle`内的任何新调用。必须通过调用`endTransaction()`手动结束自定义事务。New Relic会从调用`createWebTransaction()`计时事务，并在调用`endTransaction()`时结束事务。

* **`createBackgroundTransaction(name, [group], handle)`**
工具指定后台事务。使用此API，您可以扩展New Relic的工具以从后台事务捕获数据。
`name`是事务名称，需要是静态的。不要包括用户ID等变量数据。
`group`是可选的，它允许您通过用户界面中的事务类型将类似的作业分组在一起。需要是静态的。`handle`定义了一个函数，其中包含您要检测的整个后台作业。New Relic将捕获将由自动检测捕获的任何指标，以及通过`createTracer()`进行手动监测的指标。必须通过调用`endTransaction()`手动结束自定义事务。New Relic会从调用`createWebTransaction()`计时事务，并在调用`endTransaction()`时结束事务。

* **`endTransaction()`**
事务结束时需要被调用

* **`createTracer(name, callback)`**
调用特定的回调以提高事务的可见性。使用此函数来改进特定方法的检测，或通过在目标函数及其父异步函数中调用`createTracer()`来跨越异步边界跟踪工作。
`name`是追踪器的名字。该名字会在事务追踪中和New Relic UI中作为新度量可见。`callback`是你希望追踪的回调。代理会在`createTracer`被调用后开始计时段，并`callback `回调完成执行时结束段。

#### 自定义指标API
使用这些API自定义指标

* **`recordMetric(name, value)`**
使用`recordMetric`记录事件基础的度量指标，通常与特定的持续时间相关联。`name`必须是个符合指标命名规则的`string`。`value`通常是个`number`，有时也可以是`object`。
当`value`是个数值时，它应表示与事件关联的测量幅度（如特定方法调用的持续时间）。
当`value`是个对象时，必须包含`count`，`total`，`min`，`max`和`sumOfSquares`的键对值，这些参数都是`number`。如果你希望自己整合指标并定期报告他们(如用`setInterval`)，那么就对表格很有用。这些值将与先前为同一指标收集的值进行汇总。 这些键的名称与平台API使用的键名称相匹配。

* **`incrementMetric(name, [amount])`**
使用`incrementMetric`升级指标作为一个简单计数器。所选指标的计数将默认从1开始递增。

#### 自定义事件API
调用此API记录其他[Insights](https://docs.newrelic.com/docs/insights)事件

* **`recordCustomEvent(eventType, attributes)`**
使用`recordCustomEvent`记录事件基础的指标，通常与特定的持续时间相关联。`eventType`是一个由字母数字字符串，且少于255字符。`attributes`是一个对象。其中的键值必须少于255个字符，其中的值必须是`string`，`number`或`boolean`。

#### 其他API

* **`newrelic.addCustomParameter(name, value)`**
在`New Relic UI`中设置要与事务跟踪一起显示的自定义参数值。必须在事务的上下文中调用。自定义参数将显示在事务跟踪详细信息视图中，并显示事务的错误。
*如果您在Insights中使用自定义参数或属性，请避免使用Insights的任何保留字来命名它们。*

* **`newrelic.addCustomParameters(params)`**
在New Relic UI中设置要与事务跟踪一起显示的多个自定义参数值。参数应作为单个对象传递。必须在事务的上下文中调用。自定义参数将显示在事务跟踪详细信息视图中，并显示事务的错误。*如果您在Insights中使用自定义参数或属性，请避免使用Insights的任何保留字来命名它们。*

* **`newrelic.getBrowserTimingHeader()`**
返回要插入到HTML网页标题中的HTML代码段，以启用网页加载时间。 HTML将指示浏览器获取一个小的JavaScript文件并启动页面计时器。

* **`newrelic.setIgnoreTransaction(ignored)`**
告诉模块是否忽略给定的请求。这允许您显式过滤耗时的长轮询和不相关的路由或请求。这还允许您收集否则将被忽略的请求的指标。
将参数设置为`true`会忽略事务。要防止使用此函数忽略事务，必须传递参数`false`。传递`null`或`undefined`不会更改事务是否被忽略。

* **`newrelic.noticeError(error, [customParameters])`**
如果您的应用程序正在使用域或`try/catch`子句执行自己的错误处理，但您希望有关应用程序中出现的错误数量的集中管理的所有信息，请使用此函数。与其他Node.js调用不同，这可以在路由处理程序之外使用，但如果从事务范围内调用，则会有额外的上下文。

* **`newrelic.shutdown([options], callback)`**
使用此方法正常关闭代理。
`options`:
	*  `options.collectPendingData`布尔类型。告诉代理在关闭之前是否向New Relic服务器发送任何挂起的数据。
	* `options.timeout`数字类型(ms)。强制关闭之前的默认时间。当`collectPendingData`为`true`时，代理程序将在关闭之前等待连接。此超时对于短期进程（如AWS Lambda）非常有用，以防止进程在尝试连接时保持打开时间过长。
例子：

```javascript
newrelic.shutdown({ collectPendingData: true, timeout: 3000}, function(error) {
    process.exit()
  })
```

#### 命名和忽略请求的规则
如果不想将调用到New Relic模块直接放入应用程序代码中，可以使用基于模式的规则来命名请求。有两组规则：一个用于重命名请求，一个用于标记要由New Relic的工具忽略的请求。 这里是New Relic的Node.js代理中的规则的结构。

* **`rules.name`**

格式为`{pattern：“pattern”，name：“name”}`的规则列表，用于将传入的请求URL与模式匹配，并命名匹配的New Relic事务的名称。这用作正则表达式替换，您可以将模式设置为字符串或JavaScript正则表达式文字，`pattern`和`name`都是必需的。
当将正则表达式作为字符串传递时，可以转义反斜杠，因为代理在模式作为字符串给出时不会保留它们。定义您的配置规则从最一般到最具体，因为模式将以相反的顺序被评估，并且是终端性质的。有关详细信息，请参阅命名准则。
也可以通过环境变量`NEW_RELIC_NAMING_RULES`配置：`    NEW_RELIC_NAMING_RULES='{"pattern":"^t","name":"u"},{"pattern":"^u","name":"t"}'`

***可选的规则属性***

<table border="1" cellpadding="1" cellspacing="0">
	<thead>
		<tr>
			<th>属性名</th>
			<th>默认</th>
			<th>描述</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<td>`terminate_chain`</td>
		<td>`true`</td>
		<td>当设置为`true`（默认）时，如果此规则是匹配，则不会评估其他规则。将多个规则一起使用时，将此选项设置为false非常有用。例如，一个规则是可以匹配许多不同URL中的通用模式，同时随后有更详细的规则。</td>
	</tr>
	<tr>
		<td>`replace_all`</td>
		<td>`false`</td>
		<td>当设置为`true`时，将替换模式的所有匹配项。 否则，只有第一个匹配项将被替换。 在正则表达式中使用g标志将有相同的效果。例如`pattern: '[0-9]+',
replace_all: true`。和它有相同作用的是`pattern: /[0-9]+/g`</td>
	</tr>
	<tr>
		<td>`precedence`</td>
		<td>500</td>
		<td>默认情况下，以从最后到第一个相反的顺序评估规则。 如果您发现此行为违反直觉，则可以通过将功能标志`reverse_naming_rules`设置为`false`来反转执行顺序。此外，如果你希望完全控制排序规则，那你可以给每个规则一个`precedence`属性。`precedence`需要是一个整数，且规则按递增顺序评估</td>
	</tr>
	</tbody>
</table>

***测试你的命名规则***

Node.js代理附带了一个用于测试命名规则的命令行工具。有关更多信息，请在安装了应用程序的目录中的终端窗口中运行以下命令：
`node node_modules/.bin/newrelic-naming-rules`

***命名规则例子***

1. 匹配完整URL
`pattern: "^/items/[0-9]+",name: "/items/:id"`
结果：

```
/items/123  =>  /items/:id
/orders/123  =>  /orders/123   (not replaced since the rule is a full match)
```

2. 替换网址中的第一个匹配
`pattern: "[0-9]+",name: ":id"`
结果：

```
/orders/123  =>  /orders/:id
/items/123  =>  /items/:id
/orders/123/items/123  =>  /orders/:id/items/123
```

3. 替换任何网址中的所有匹配项
`pattern: "[0-9]+",name: ":id",replace_all: true`
结果：
`/orders/123/items/123  =>  /orders/:id/items/:id`

4. 匹配组引用
`pattern: '^/(items|orders)/[0-9]+',name: '/\\1/:id'`
结果：

```
/orders/123  =>  /orders/:id
/items/123  =>  /items/:id
```

* **`rules.ignore`**
这也可以通过环境变量`NEW_RELIC_IGNORING_RULES`设置，多个规则作为逗号分隔模式列表传递。目前没有办法在模式中转义逗号。
`NEW_RELIC_IGNORING_RULES='^/socket\.io/\*/xhr-polling,ignore_me'`

* **例子**
	* 命名规则示例

		```
	// newrelic.js
	  exports.config = {
	    // other configuration
	    rules : {
	      name : [
	        { pattern: "/tables/name-here", name: "/name-hererule1" }
	      ]
	    }
		```

	* 忽略规则示例

		```
		  // newrelic.js
		  exports.config = {
		    // other configuration
		    rules : {
		      ignore : [
		        '^\/socket\.io\/.*\/xhr-polling'
		      ]
		    }
		  };
		```

#### API调用规则
使用New Relic的Node.js代理命名和忽略规则的API。

* **`newrelic.addNamingRule(pattern, name)`**
程序化版本的`rules.name`。一旦添加了命名规则，在重新启动Node进程之前不能删除它们。它们也可以通过Node.js代理的配置添加。这两个参数都是必需的。

* **`newrelic.addIgnoringRule(pattern)`**
程序化版本的`rules.ignore`。一旦添加了命名规则，在重新启动Node进程之前不能删除它们。它们也可以通过Node.js代理的配置添加。这两个参数都是必需的。

### Node.js 自定义检测工具
自定义工具仅在 New Relic 1.10.0及更高版本被支持
通过自定义工具可以检测websocket，后台工作，不支持的数据库，还可以指向代码的某部分哟

#### 检测web事务
检测类似于websocket请求的web事务，需要创建一个自定义事务。创建一个自定义事务为您提供与该代理自动处理的事务相同类型的可见性。检测web事务，需要把你需要的检测句柄包裹在**`createWebTransaction()`**中。确保事务结尾调用**`endTransaction()`**。

***例子***

该例子在`socket.io`中检测了一个`/websocket/ping`事务和一个`/websocket/new-message`。`/ping`例子是同步的，`/new-message`例子是异步的。事务结尾调用**`endTransaction()`**。

```javascript
var nr = require('newrelic')
var app = require('http').createServer()
var io = require('socket.io')(app)

io.on('connection', function (socket) {
  socket.on('ping', nr.createWebTransaction('/websocket/ping', function (data) {
    socket.emit('pong')
    nr.endTransaction()
  }))
  socket.on('new-message', nr.createWebTransaction('/websocket/new-message', function (data) {
    addMessageToChat(data, function () {
      socket.emit('message-received')
      nr.endTransaction()
    })
  }))
})
```
#### 检测后台事务
你也可以自定义事务检测后台事务，比如说是应用中周期性的工作或是请求后继续完成的工作。检测后台事务需要将句柄包裹在**`createBackgroundTransaction()`**中。确保**`endTransaction()`**在事务的最后。

***例子***

用`setInterval`检测`update:cache`

```javascript
var nr = require('newrelic')
var redis = require('redis').createClient()

setInterval(nr.createBackgroundTransaction('update:cache', function () {
  var newValue = someDataGenerator()

  redis.set('some:cache:key', newValue, function () {
    nr.endTransaction() // End the transaction once redis is done
  })
}), 30000) // Every 30s
```
#### 扩展事务检测工具
你可以使用自定义工具扩展已有的工具提高web事务的可视化，或了解未自动检测的数据库和其他事务内部工作。你需要将你的回调函数包裹在自定义事务追踪器中。
要调用回调，请使用**`createTracer()`**包装回调。如果要测试在异步函数内调用的函数，则需要使用**`createTracer()`**将目标函数及其父异步函数包装。

***例子***

检测回调

```javascript
// Wrap the callback in a segment
db.createObject(nr.createTracer('db:createObject', function (err, result) {
  // Some error handler that will end the response for us
  if (util.handleError(err, res)) {
    return
  }
  res.write(JSON.stringify(result.rows[0].id))
  res.write('\n')
  res.end()
}))
```

检测异步函数
这个例子同时包裹了`pg.connect`和`client.query`。这是因为`client.query`被一个异步父函数(`pg.connect`)调用。否则，我们将不能从`client.query`中获得任何数据。
这允许**`createTracer()`**在这些异步边界之间传播活动事务。

```javascript
pg.connect(config.db_string, nr.createTracer('pg:connect', function (err, client, done) {
  if (util.handleError(err, '500', res)) {
    return done()
  }
  client.query('SELECT count(*) FROM test_count', nr.createTracer('pg:query', function (err, result) {
    if (util.handleError(err, '500', res)) {
      return done()
    }

    res.write(result.rows[0].count)
    res.write('\n')
  }))
}))
```

### Node.js自定义指标
自定义指标，可通过API调用记录的任意指标，并创建自定义图表。
收集太多的指标可能会影响您的应用程序和New Relic的代理的性能。为了避免数据的问题，保持2000下独特的自定义指标总数。

#### 命名指标
公制名称由路径分隔`/`符。对于自定义指标使用这种模式：
`<category>/<class>/<method>`
对于自定义指标名称，使用`Custom/<class>/<method>`或`Custom/<category>/<name>`。例如，使用`Custom/MyCategory/My_method`）。

#### 记录的自定义指标
记录度量数据的公共API包含在两个方法：

* **`recordMetric`**：用于创建新的自定义指标。
* **`incrementMetric`**：使用更新的自定义指标的数值。

#### 自定义指标例子
```javascript
app.post('/cart/checkout', function(req, res) {
  var total = computeCartTotal(req.user);
  newrelic.recordMetric('Custom/Cart/ChargeAmount', total);
  ...
});
```

#### 查看自定义指标
使用`Insights Metric Explorer`查询，可创建自定义图标等。


### 页面加载时间
使用New Relic的Node.js代理的页面加载时间（有时称为实时用户监测或者RUM），需要是用最新版本的Node.js代理。
在用户接口启用页面加载时间，请使用[浏览器设置]()。然后按照本节设置页面加载时间用了New Relic的Node.js的代理程序。

#### 插入JavaScript头
Node.js代理工具可以在应用程序之外继续监测到最终用户的浏览器。`newrelic`模块可以生成脚本头当插入你的HTML模板时，可以捕获用户的页面加载时间。必须手动注入，但不需要额外的配置。

1. 在你html页面`head`标签中，在任意`CHARSET`的`meta`标签后插入**`newrelic.getBrowserTimingHeader()`**的计算结果。（补充：为了最大的IE兼容性，把**`newrelic.getBrowserTimingHeader()`**的计算结果插在`X-UA-COMPATIBLE HTTP-EQUIV`的`meta`标签后面）
2. 每个请求都调用一下这个头。不要缓存。

#### 例子
**Express & Jade**

在`app.js`里面

```javascript
    var newrelic = require('newrelic');
    var app = require('express')();
    // in express, this lets you call newrelic from within a template
    app.locals.newrelic = newrelic;

    app.get('/user/:id', function (req, res) {
      res.render('user');
    });
    app.listen(process.env.PORT);

```

在`layout.jade`里面

```html
    doctype html
    html
      head
        != newrelic.getBrowserTimingHeader()
        title= title
        link(rel='stylesheet', href='stylesheets/style.css')
      body
        block content
```


**Express & Swig**

在`app.js`里面

```javascript
    var newrelic = require('newrelic');

    var http = require('http')
    var path = require('path')
    var swig = require('swig')

    var app = require('express')();

    app.locals.newrelic = newrelic;

    //taken from http://paularmstrong.github.io/swig/docs/#express
    app.engine('html', swig.renderFile);
    app.set('view engine', 'html');
    app.set('views', __dirname + '/views');

    app.get('/user/:id', function (req, res) {
      res.render('user');
    });

    app.listen(process.env.PORT);

```

在`views/user.html`里面

```html
    <!DOCTYPE html>
    <html>
      <head>
          {{ newrelic.getBrowserTimingHeader() }}
          <title>Hello</title>
      </head>
        <body>
          <h1>Hello World</h1>
        </body>
    </html>
```

**Hapi.js & handlebars**

在`app.js`里面

```javascript
    var newrelic = require('newrelic');
    var Hapi = require('hapi');
    var server = new Hapi.Server(parseInt(PORT), '0.0.0.0', {
      views: {
        engines : {html: 'handlebars' },
        path : __dirname + '/templates'
      }
    });

    function homepage(request, reply) {
      var context = {

        // pass in the header each request
        nreum : newrelic.getBrowserTimingHeader(),
        content : ...
    };

    reply.view('homepage', context);
    };

    server.route({
      method : 'GET',
      path : '/',
      handler : homepage
    });

    server.start();

```

在`templates/homepage.html`里面

```html
    <!DOCTYPE html>
    <html>
      <head>
          {{{ nreum }}}
          <title>Hello</title>
      </head>
        <body>
          {{ content }}
        </body>
    </html>
```


#### 禁止头生成
默认会调用**`newrelic.getBrowserTimingHeader()`**会生成有效的头，当要禁用的时候，需要在模板中移除代码，然后在`newrelic.js`中：
```javascript
    browser_monitoring : {
      enable : false
    }
```
或者把环境变量改改：
`NEW_RELIC_BROWSER_MONITOR_ENABLE=false`

### NodeVM测量
[自己看吧](https://docs.newrelic.com/docs/agents/nodejs-agent/supported-features/node-vm-measurements)

### Node.js代理 v2.0 beta
[自己看吧+1](https://docs.newrelic.com/docs/agents/nodejs-agent/nodejs-agent-v20-instrumentation-api-beta)

## 故障排错

### 安装问题
#### 问题
如果在安装New Relic Node.js代理后遇到任何常见问题，请尝试以下故障排除提示。
#### 解决方案
* **没有看到数据**
为了最大限度地减少Node.js代理使用的带宽量，New Relic仅每分钟报告一次数据
	* 如果测试环境下代理添加到运行不到一分钟的，它将没有时间将数据报告给New Relic。
	* 如果在部署代理后没有看到事务跟踪或其他数据，这可能是由于配置，框架或Apdex设置。
* **安装问题**
检查顺序
	* 主模块： 确认是否在应用开头`require('newrelic');`。
	* 条件逻辑： 如果你在你的`require`时添加了条件逻辑，把你的条件逻辑改到`newrelic.js`里面去
	* 框架： 确认你的框架是New Relic支持的
	* Apdex： 试试调整你在配置文件的配置和面板的配置
* **日志文件**
Node.js代理会将日志写在应用根目录的`newrelic_agent.log`中，除非你修改了日志配置。如果代理不发送数据或崩溃退出应用。你可以生成一个有错误报告和支持请求的故障排除日志文件，可以帮助确定New Relic什么地方出了错。
* **缺少VM指标**
代理可以收集与垃圾收集（GC），内存和CPU相关的VM指标。其中一些度量标准需要安装额外的本机模块。
	* 问题： 有如下报错时

		```
		gyp ERR! configure error
gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
		```
		```
		gyp ERR! build error
gyp ERR! stack Error: not found: make
		```
		```
		make: g++: Command not found
		```

	* 解决方案： 确保已安装`node-gyp`模块。在Debian / Ubuntu平台上，请使用该命令：`apt-get install build-essential`

### 内存增大问题
#### 问题
安装New Relic Node.js代理后使内存使用量增加。
#### 解决方案
* **将代理版本升级到1.18.0及更高**
Node.js代理版本1.18.0缓解了在TLS连接期间可能发生的Node.js内存泄漏。Node.js Core有一个涉及TLS连接时有突出的内存泄漏问题。指定证书的客户端会快速显露此泄露，包括New Relic代理。代理版本1.18.0及更高版本通过尽可能使用默认客户端证书来缓解此问题。当无法使用TLS内存泄漏解决方法时（如使用带有HTTPS代理的自定义证书时），将打印新的日志消息。
* **因SSL造成的增加**
为减少因TLS造成的内存溢出，停用SSL。如果停用SSL，需要手动将`port`设`80`。
* **因TLS内存缓冲区分配引起的增加**
Node.js应用首次使用任何形式的加密（包括SSL和HTTPS）时，将创建slab缓冲区，默认大小为10 MB。
	* 解决方案： 把缓冲区大小设为小于10 MB，可使用[`tls.SLAB_BUFFER_SIZE`](http://nodejs.org/api/tls.html#tls_tls_slab_buffer_size)。当应用运行环境在SSL终止入站请求发生在单独路由层时，通常不会产生此开销。在入站请求的SSL终止发生在单独的路由器层中的环境中运行的应用程序。像Heroku和AWS这样的云服务通常以这种方式运行。 但是，Node.js代理通过HTTPS将出站数据发送到New Relic服务时，将触发slab缓冲区的分配。*使用New Relic代理时，不要将slab缓冲区大小设置为低于128 KB。 对于使用SSL，HTTPS或任何其他形式的加密技术与服务或客户端进行通信的应用程序，不应减少slab缓冲区分配。在使用SSL，HTTPS或任何其他形式的加密技术与服务或客户端进行通信的应用程序时，不应减少slab缓冲区分配。*
* **因集群工作线程分配引起的增加**
Node.js提供了集群模块。这允许通过使用服务器上可用的所有处理器核心并行处理请求。但是，每个集群工作者为SSL事务分配自己的slab缓冲区，并保留其自己的Node.js代理数据副本。这会将内存开销乘以使用的集群工作程序数。如果服务器同时运行多个Node.js应用程序，也是如此。
	* 解决方案：减少集群工作者数量或不使用集群支持，可能会降低内存使用率但不会影响性能。
* **因日志信息存储在硬盘引起的增加**
因为代理默认将日志写在硬盘
	* 解决方案：根据Node.js版本，日志级别默认为`info`或`trace`。将日志级别降低到`info`或`warn`级别，以显着降低内存使用和花在垃圾收集中的时间。
* **因MongoDB游标溢出引起的增加**
	* 解决方案： 通过在应用程序完成处理查询结果后调用`cursor.close()`，确保在应用程序中创建的每个游标都已关闭。
* **因代理数据存储导致的增加**
	* 解决方案：如果代理数据存储被识别为内存使用增加的原因，则可以通过向服务器添加更多内存或切换到更大的云服务器实例来解决此问题。

### 页面加载时间问题
#### 问题
如果在为页面加载计时（有时称为实时用户监视或RUM）检测New Relic Node.js代理时遇到问题，请按照标准故障排除步骤操作。 这里有一些Node.js的额外提示。
#### 解决方案
错误代码会自动出现在网站源代码和你的Node.js代理日志里。可以查询[NREUM](https://docs.newrelic.com/docs/new-relic-browser/troubleshooting-page-view-monitoring#javascript)找到这些代码。

* 错误代码0 :已显示禁用浏览器监控。可在配置`newrelic.js`和环境变量中修改。（`NEW_RELIC_BROWSER_MONITOR_ENABLE`默认是`true`）
* 错误代码1 :页面加载时间在Web事务之外调用。这可能发生在尝试生成页面加载计时数据一次，然后缓存它，或者如果您在后台任务中调用它。
* 错误代码2 :发生意外事件。
* 错误代码3 :事务未命名。 如果您不使用`Express`或`Restify`，并且没有显式地命名事务，则会出现此错误。 这是为了避免将事务名称滚动到`/*`。 有关更多信息，请参阅事务命名。
* 错误代码4 :Node.js代理还没有与收集器进行“握手”。 应用程序已启动，用户在收集器可以与代理通信之前访问该站点。 这可能是因为：
	* 浏览器页面在代理初始化于New Relic的链接前加载
	* `license_key`不可用（如果错误持续超过1分钟，很可能是这个问题）
	* 发生了一些阻止应用ID可用的问题
* 错误代码5 :在收集器端禁用了浏览器监视。 例如，收集器未返回足够的数据以启用页面加载计时。 这是一个收集器问题，因为当前的Node.js的服务器端配置不可用。

### 模拟旧参数问题
#### 问题
您想要模仿在New Relic UI中启用`Capture`属性或`Capture`参数选项的传统行为。
#### 解决方案
可在配置中修改，例如在配置文件中修改为：
`capture_params : true`
