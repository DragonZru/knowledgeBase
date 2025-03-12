TODO: GC日志分析，可以自己打出一个jar包在服务器上运行（指定-XX:HeapDumpPath=/path/logs/java.hprof -XX:+HeapDumpOnOutOfMemoryError）导出文件由visualVM分析



-jar gr-user-web-1.1.0-RELEASE.jar 启动的 Spring Boot 应用 JAR 包。
--spring.profiles.active=produce   激活produce环境配置（生产环境）。

疑问？
SkyWalking
-javaagent:/home/app/skywalking/gr-skywalking-agent/skywalking-agent.jar   SkyWalking Agent 路径。
-Dskywalking.agent.service_name=produce::gr-user                           服务名称（用于 SkyWalking 监控）。
-Dskywalking.collector.backend_service=10.209.217.79:30202                 SkyWalking 后端服务地址。
-Dskywalking.agent.authentication=e6b09a564a2946f69c33c2e471911f5f         认证 Token。
-Dskywalking.logging.level=WARN                                            日志级别设为 WARN。
-Dskywalking.logging.dir=/home/app/gr-user/logs/skywalking                 SkyWalking 日志路径。
-Dskywalking.agent.sample_n_per_3_secs=100                                 每 3 秒采样 100 条追踪数据。

疑问？
-Dsun.net.client.defaultConnectTimeout=10000      默认连接超时 10 秒。
-Dsun.net.client.defaultReadTimeout=30000         默认读取超时 30 秒。
-Djava.awt.headless=true                          启用无头模式（无图形界面）。

疑问？
-Djasypt.encryptor.password=2Jw3p1L9GrgcgSlAfKGB4Q==       Jasypt 加密密码（用于配置文件加密）。
-Djasypt.encryptor.algorithm=PBEWithMD5AndDES              指定加密算法。
-Djasypt.encryptor.iv-generator-classname=org.jasypt.iv.NoIvGenerator     禁用初始化向量（IV）。

dubbo学习

nginx -> gateway -> server 每一层怎么做负载均衡，从入口开始压测

有哪些大数据组件？ 学习

HAProxy 或 Traefik 使用
https://www.readfog.com/a/1665375657830486016
https://peihsinsu.gitbooks.io/docker-note-book/content/xin-yi-dai-lb-traefik.html

https://jaminzhang.github.io/lb/HAProxy-Get-Started/
https://www.cnblogs.com/you-men/p/12979599.html


什么是 http 1,2 区别，WebSocket 入门
HTTP/1.1、HTTP/2 和 WebSocket 等协议的处理效率不同，HTTP/2 通常比 HTTP/1.1 更高效


nginx:
upstream backend {
//负载均衡轮训策略
//Round Robin 默认，无需配置
//Weighted Round Robin 加权轮询                                 适用：服务器性能差异大
server backend1.example.com weight=5;
server backend2.example.com weight=3;
server backend3.example.com weight=2;
//ip_hash IP 哈希                                              会话保持
ip_hash;
server backend1.example.com;
server backend2.example.com;
server backend3.example.com;
//least_conn 最少连接                                           长连接或响应时间差异大
least_conn;
server backend1.example.com;
server backend2.example.com;
server backend3.example.com;
//hash $request_uri consistent;  # 使用一致性哈希算法   URL 哈希   缓存一致性要求高
hash $request_uri consistent;  # 使用一致性哈希算法
server backend1.example.com;
server backend2.example.com;
server backend3.example.com;
//random 随机                                                   无状态服务
random;
server backend1.example.com;
server backend2.example.com;
server backend3.example.com;
//fair 响应时间权重                                               动态优化性能
fair;
server backend1.example.com;
server backend2.example.com;
server backend3.example.com;
}