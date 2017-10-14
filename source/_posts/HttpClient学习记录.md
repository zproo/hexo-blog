---
title: HttpClient学习记录
comment: true
date: 2017-10-14 16:55:10
tags: [HttpClient, Java]
---

# 一、前期准备
> [HttpClient](https://hc.apache.org/httpcomponents-client-ga/index.html) is a HTTP/1.1 compliant HTTP agent implementation based on HttpCore. It also provides reusable components for client-side authentication, HTTP state management, and HTTP connection management. HttpComponents Client is a successor of and replacement for [Commons HttpClient 3.x](http://hc.apache.org/httpclient-legacy/index.html). Users of Commons HttpClient are strongly encouraged to upgrade.

**总结:** HttpClient 是apache 组织下面的一个用于处理HTTP 请求和响应的开源工具。

需要的jar包：

​	httpclient-4.3.6.jar、httpcore-4.3.3.jar、httpmime-4.3.6.jar、commons-codec-1.6.jar。

# 二、操作步骤

由于HttpClient从4.X开始，基本的get/post 等http请求实现方式与之前版本的写法有很大的差异，所以在此总结一下4.X版本之后的官方推荐写法。

**操作顺序：** 

1. 创建CloseableHttpClient对象
2. 创建请求方法实例：HttpGet/HttpPost
3. （可选）需要请求参数时
   - HttpGet/HttpPost都可以使用setParams方法添加参数（deprecated 已过时）
   - 对于HttpPost方法可以使用SetEntity(HttpEntity entity)参加请求参数
   - 对于HttpGet方法，可以通过拼接url的方式添加请求参数
4. 调用CloseableHttpClient对象的execute(HttpUriRequest request)发送请求，该方法返回一个CloseableHttpResponse。
5. 通过CloseableHttpResponse对象获取服务器的响应内容。
6. 释放连接。无论执行方法是否成功，都必须释放连接

# 三、代码实例

```java
package com.learn.http;

import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.util.ArrayList;
import java.util.List;

import org.apache.http.HttpEntity;
import org.apache.http.NameValuePair;
import org.apache.http.ParseException;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;

public class HttpSend {

	public static void main(String[] args) {
		// post请求百度地址，返回404
		new HttpSend().post("http://www.baidu.com");
		new HttpSend().get("http://www.baidu.com");
	}

	/**
	 * 发送 post请求访问本地应用并根据传递参数不同返回不同结果
	 */
	public void post(String url) {

		// 创建默认的httpClient实例.
		CloseableHttpClient httpclient = HttpClients.createDefault();
		// 创建httppost
		HttpPost httppost = new HttpPost(url);
		
		// 创建参数队列
		List<NameValuePair> formparams = new ArrayList<NameValuePair>();
		// 添加参数
		// formparams.add(new BasicNameValuePair("gg", "1"));

		UrlEncodedFormEntity uefEntity;
		try {
			//传入空参数，创建实体对象
			uefEntity = new UrlEncodedFormEntity(formparams, "UTF-8");
			//设置post对象的实体对象
			httppost.setEntity(uefEntity);
			System.out.println("executing post request " + httppost.getURI());
			//执行post请求
			CloseableHttpResponse response = httpclient.execute(httppost);
			try {
				//获取请求结果对象
				HttpEntity entity = response.getEntity();
				if (entity != null) {
					System.out.println("--------------------------------------");
					System.out.println("Response content: " + EntityUtils.toString(entity));
					System.out.println("--------------------------------------");
				}
			} finally {
				response.close();
			}
		} catch (ClientProtocolException e) {
			e.printStackTrace();
		} catch (UnsupportedEncodingException e1) {
			e1.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			// 关闭连接,释放资源
			try {
				httpclient.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}

	/**
	 * 发送 get请求
	 */
	public void get(String url) {

		// 创建默认的httpClient实例.
		CloseableHttpClient httpclient = HttpClients.createDefault();
		
		try {
			// 封装请求参数
			List<NameValuePair> params = new ArrayList<>();
			params.add(new BasicNameValuePair("cityEname", "henan"));
			//转换为键值对
			String str = EntityUtils.toString(new UrlEncodedFormEntity(params, "UTF-8"));
			System.out.println(str);
			//创建带参数get请求（示例）
			HttpGet httpGet = new HttpGet(url+"?"+str);

			// 创建不带参数get请求
			HttpGet httpget = new HttpGet(url);
			System.out.println("executing get request " + httpget.getURI());
			// 执行get请求.
			CloseableHttpResponse response = httpclient.execute(httpget);
			try {
				// 获取响应实体
				HttpEntity entity = response.getEntity();
				System.out.println("--------------------------------------");
				// 打印响应状态
				System.out.println(response.getStatusLine());
				if (entity != null) {
					// 打印响应内容长度
					System.out.println("Response content length: " + entity.getContentLength());
					// 打印响应内容
					System.out.println("Response content: " + EntityUtils.toString(entity));
				}
				System.out.println("------------------------------------");
			} finally {
				response.close();
			}
		} catch (ClientProtocolException e) {
			e.printStackTrace();
		} catch (ParseException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			// 关闭连接,释放资源
			try {
				httpclient.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
}
```



