---
title: wireshark抓包常用规则
date: 2020-09-19 18:03:16
tags:
---
## 1.协议过滤：
tcp udp arp icmp http
smtp ftp dns msnms
ip ssl oicq bootp

## 2.mac过滤：
eth.dst == A0:00:00:04:C5:84
eth.src eq A0:00:00:04:C5:84
eth.dst==A0:00:00:04:C5:84
eth.dst==A0-00-00-04-C5-84

## 3.ip过滤：
<!--more-->
ip.addr == 10.43.54.65
// 常量的研究两者间的通信
ip.addr== 192.168.8.54 || ip.addr== 112.80.248.74
ip.src == 10.43.54.65 or ip.dst == 10.43.54.65

## 4.tcp和udp过滤：
tcp.port == 80
tcp.port eq 80 or udp.port eq 80
tcp.port eq 25 or icmp
tcp.port >= 1 and tcp.port <= 80
tcp.window_size == 0 && tcp.flags.reset != 1
udp.length == 26

tcp类型和内容：
tcp[13] & 000 = 0: No flags set (null scan)
tcp[13] & 001 = 1: FIN set and ACK not set
tcp[13] & 003 = 3: SYN set and FIN set
tcp[13] & 005 = 5: RST set and FIN set
tcp[13] & 006 = 6: SYN set and RST set
tcp[13] & 008 = 8: PSH set and ACK not set
    
包长过滤：
udp.length == 26 这个长度是指udp本身固定长度8加上udp下面那块数据包之和
tcp.len >= 7  指的是ip数据包(tcp下面那块数据),不包括tcp本身
ip.len == 94 除了以太网头固定长度14,其它都算是ip.len,即从ip本身到最后
frame.len == 119 整个数据包长度,从eth开始到最后

## 5.http过滤：
// 常用的域名
http.host == party.syyx.com
http.response.code == 404
http.content_type contains "javascript"
http.request.uri matches "gl=se$"
http.request.method == "GET"
http.request.method == "POST"
http.request.uri == "/img/logo-edu.gif"
http contains "GET"
http contains "HTTP/1."

// GET包
http.request.method == "GET" && http contains "Host: "
http.request.method == "GET" && http contains "User-Agent: "
// POST包
http.request.method == "POST" && http contains "Host: "
http.request.method == "POST" && http contains "User-Agent: "
// 响应包
http contains "HTTP/1.1 200 OK" && http contains "Content-Type: "
http contains "HTTP/1.0 200 OK" && http contains "Content-Type: "

## 6.例子

tcp dst port 3128  //捕捉目的TCP端口为3128的封包。
ip src host 10.1.1.1  //捕捉来源IP地址为10.1.1.1的封包。
host 10.1.2.3  //捕捉目的或来源IP地址为10.1.2.3的封包。
ether host e0-05-c5-44-b1-3c //捕捉目的或来源MAC地址为e0-05-c5-44-b1-3c的封包。如果你想抓本机与所有外网通讯的数据包时，可以将这里的mac地址换成路由的mac地址即可。
src portrange 2000-2500  //捕捉来源为UDP或TCP，并且端口号在2000至2500范围内的封包。
not imcp  //显示除了icmp以外的所有封包。（icmp通常被ping工具使用）
src host 10.7.2.12 and not dst net 10.200.0.0/16 //显示来源IP地址为10.7.2.12，但目的地不是10.200.0.0/16的封包。
(src host 10.4.1.12 or src net 10.6.0.0/16) and tcp dst portrange 200-10000 and dst net 10.0.0.0/8  //捕捉来源IP为10.4.1.12或者来源网络为10.6.0.0/16，目的地TCP端口号在200至10000之间，并且目的位于网络 10.0.0.0/8内的所有封包。
src net 192.168.0.0/24 
src net 192.168.0.0 mask 255.255.255.0  //捕捉源地址为192.168.0.0网络内的所有封包。
