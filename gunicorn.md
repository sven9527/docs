#gunicorn
2018-11-27

    gunicorn 是wsgi容器，常见的还有uwsgi
    用来管理维护web框架，有一个Master和多个worker，支持Gevent、Eventlet异步，支持Tornado。

    特点：
    1.Gunicorn是基于prefork模式的Python wsgi应用服务器，支持 Unix like的系统
    2.采用epoll (Linux下) 非阻塞网络I/O 模型
    3.多种Worker类型可以选择 同步的，基于事件的（gevent tornado等），基于多线程的
    4.高性能，比之uwsgi不相上下


    常用命令：
    gunicorn --worker 8 --bind 0.0.0.0:8000 --worker-class=gevent wsgi:application 


    关系：
    web_platform <-> WSGI <-> NGINX <-> web_client
    (nginx收到客户端发来的请求，根据nginx中配置的路由，将其转发给WSGI) nginx：”WSGI，找你的来了！” (WSGI服务器根据WSGI协议解析请求，配置好环境变量，调用start_response方法呼叫flask框架) WSGI服务器：”flask,快来接客，客户资料我都给你准备好了！” (flask根据env环境变量，请求参数和路径找到对应处理函数，生成html) flask：”!@#$%^……WSGI，html文档弄好了，拿去吧。” (WSGI拿到html，再组装根据env变量组装成一个http响应，发送给nginx) WSGI服务器：”nginx,刚才谁找我来着？回他个话，!@#$%^…..” (nginx再将响应发送给客户端)

