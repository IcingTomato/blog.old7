---
layout: post
title:  "Make Windows Great Again"
tags: windows git zh-cn
---

# 让 Windows 伟大起来

## WSL 完全使用技巧



```shell
#!/bin/bash
hostip=$(cat /etc/resolv.conf | grep nameserver | awk '{ print $2 }')
wslip=$(hostname -I | awk '{print $1}')
port=7890

PROXY_HTTP="http://${hostip}:${port}"

set_proxy(){
    export http_proxy="${PROXY_HTTP}"
    export HTTP_PROXY="${PROXY_HTTP}"

    export https_proxy="${PROXY_HTTP}"
    export HTTPS_PROXY="${PROXY_HTTP}"

    git config --global http.proxy "${PROXY_HTTP}"
    git config --global https.proxy "${PROXY_HTTP}"

    # 设置APT代理
    echo "Acquire::http::Proxy \"${PROXY_HTTP}\";" | sudo tee /etc/apt/apt.conf.d/95proxies > /dev/null
    echo "Acquire::https::Proxy \"${PROXY_HTTP}\";" | sudo tee -a /etc/apt/apt.conf.d/95proxies > /dev/null
}

unset_proxy(){
    unset http_proxy
    unset HTTP_PROXY

    unset https_proxy
    unset HTTPS_PROXY

    git config --global --unset http.proxy
    git config --global --unset https.proxy

    # 移除APT代理设置
    sudo rm /etc/apt/apt.conf.d/95proxies
}

test_setting(){
    echo "Host ip:" ${hostip}
    echo "WSL ip:" ${wslip}
    echo "Current proxy:" $https_proxy
}

if [ "$1" = "set" ]
then
    set_proxy

elif [ "$1" = "unset" ]
then
    unset_proxy

elif [ "$1" = "test" ]
then
    test_setting
else
    echo "Unsupported arguments."
fi
```

## 倒叙之前因后果

11月月初俺爹斥巨资给我换了个笔记本电脑

<center>
    <img src="http://icing.fun/img/post/2023/11/29/order.jpg" alt="Order" title="Order" width="300" />
</center>

可能有人要“素质三连”了

> Surface Pro 9 不是有酷睿版吗？SQ3 版不是很垃圾吗？有这个钱为什么不买苹果？

首先就是全功能 `Type-C` 接口，两个接口可以提供视频，音频，数据传输，更重要的是可以充电。这不比原装的那个方便？带个小米67W就可以搞定手机和电脑的充电了。酷睿版就八行，只能拿那个笨笨的充电器充电。

<center>
    <img src="http://icing.fun/img/post/2023/11/29/type-c.jpg" alt="Type-C" title="Type-C" width="300" />
</center>

5G 网络说实在真的很香，之前在学校办的移动的号，大流量卡，装这个上面，基本上是全天在线，很舒服。

之前去上海面试的时候，在高铁上就可以用电脑来办公，还能追追剧。

随身WiFi稍微麻烦了点，比如我现在也在用华为的。

<center>
    <img src="http://icing.fun/img/post/2023/11/29/5g.jpg" alt="5G" title="5G" width="300" />
</center>

再来就是这个屏幕了，这个屏幕香不香我不知道，主要是能触屏，MacBook那么多年也不能触屏。（估计有人就要骂我，说苹果触控板好用，我只能说，我不喜欢，我喜欢触屏）

<center>
    <img src="http://icing.fun/img/post/2023/11/29/touch.jpg" alt="Touch" title="Touch" width="300" />
</center>

说它是个大号平板也不为过，我现在就是拿着它在写这篇文章，很舒服。安卓平板根本打不过，如果说我要用安卓应用，Windows还有安卓子系统。

<center>
    <img src="http://icing.fun/img/post/2023/11/29/android.jpg" alt="Android" title="Android" width="300" />
</center>

8cx Gen 3 跑分啥的对我来说确实不重要，抛开实际体验的跑分都是耍流氓。

实际体验下来没啥用不了的应用，转译慢就慢我还能摸摸鱼。Python脚本可以跑，我用的是 Anaconda 的环境，没啥问题。IDE用的 VSCode，有原生 ARM64 版本，也没啥问题。VS也能用，还能编译x64/x86的程序，也没啥问题。

<center>
    <img src="http://icing.fun/img/post/2023/11/29/vscode.jpg" alt="VSCode" title="VSCode" width="300" />
</center>