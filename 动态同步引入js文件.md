---
title: 动态同步引入js文件
date: 2018-04-25 16:59:24
tags:js
---
&nbsp;&nbsp;&nbsp;&nbsp;最近做的项目中，需要根据浏览器类型，动态的引入不同的js文件，最开始采用下面的方式引入，但是经常会出现引入的文件不成功的现象。查找资料研究后，发现下面的方式是异步加载的，并发执行，即在动态加载js文件的同时，也会加载其他的，因此，引入的js内容不能直接使用，否则会报错。
<!-- more -->
	<script>
	//判断浏览器类型
	function getBrowser() {    
		var ua = window.navigator.userAgent;    
		var isIE = window.ActiveXObject != undefined && ua.indexOf("MSIE") != -1;    
	    var isFirefox = ua.indexOf("Firefox") != -1;    
	    var isOpera = window.opr != undefined;    
	    var isChrome = ua.indexOf("Chrome") && window.chrome;    
	    var isSafari = ua.indexOf("Safari") != -1 && ua.indexOf("Version") != -1;    
	    if (isIE) {    
	        return "IE";    
	    } else if (isFirefox) {    
	        return "Firefox";    
	    } else if (isOpera) {    
	        return "Opera";    
	    } else if (isChrome) {    
	        return "Chrome";    
	    } else if (isSafari) {    
	        return "Safari";    
	    } else {    
	        return "Unkown";    
	    }    
	}  
	var browserType = getBrowser();
	//动态加载js（异步方式，并发执行，引入的js文件不能直接使用）
	var scriptNode = document.createElement("script");  
	scriptNode.setAttribute("type", "text/javascript"); 
	if('IE' == browserType ){
		//如果是IE则加载multipartuploadie8.js
		scriptNode.setAttribute("src", "../../js/core/multipartuploadie8.js"); 
	}else{
	    //如果不是IE则加载multipartupload.js
		scriptNode.setAttribute("src", "../../js/core/multipartupload.js"); 
	}
	document.head.appendChild(scriptNode);
	</script>

&nbsp;&nbsp;&nbsp;&nbsp;下面考虑使用同步方式，动态加载js文件，这样会按照顺序加载，js文件加载完成后，引入的js内容可以直接使用。另外静态加载的js文件，默认也是同步加载的。  
&nbsp;&nbsp;&nbsp;&nbsp;下面是我的实现方法，参考了网上的很多资料，感谢强大的网友们。将引入的方法放入到了一个js文件中，在使用时，先在项目中引用该文件，然后在调用js文件中的方法即可。示例如下：  

	//引入的js文件  
	<script src="loadjssync.js" type="text/javascript" charset="utf-8"></script>

	//根据浏览器类型，选择性加载文件上传的js
	var browserType = getBrowser();
	var srcFile = "";
	if('IE' == browserType ){
		//如果是IE则加载multipartuploadie8.js
		srcFile =  "../../js/core/multipartuploadie8.js"; 
	}else{
	    //如果不是IE则加载multipartupload.js
		srcFile =  "../../js/core/multipartupload.js"; 
	}
	//调用js文件中的方法，即可实现同步动态加载js文件
	loadJS("myJS",srcFile);

loadjssync.js文件内容如下：  
	
	/**
	 * 同步加载js脚本
	 * @param id   需要设置的<script>标签的id
	 * @param url   js文件的相对路径或绝对路径
	 * @return {Boolean}   返回是否加载成功，true代表成功，false代表失败
	 */
	function loadJS(id,url){
	    var  xmlHttp = null;
	    //获取浏览器版本
	    var browserType = getBrowser();
	    if (browserType=='IE') {
	    	try {
	            //IE6以及以后版本中可以使用
	            xmlHttp = new ActiveXObject("Msxml2.XMLHTTP");
	        }catch (e) {
	            //IE5.5以及以后版本可以使用
	            xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
	        }
	    } else if (window.XMLHttpRequest){
	    	//Firefox，Opera 8.0+，Safari，Chrome
	        xmlHttp = new XMLHttpRequest();
	    }
	
	    //采用同步加载
	    xmlHttp.open("GET",url,false);
	    //发送同步请求，如果浏览器为Chrome或Opera，必须发布后才能运行，不然会报错
	    xmlHttp.send(null);
	    //4代表数据发送完毕
	    if ( xmlHttp.readyState == 4 ) {
	        //0为访问的本地，200到300代表访问服务器成功，304代表没做修改访问的是缓存
	        if((xmlHttp.status >= 200 && xmlHttp.status <300) || xmlHttp.status == 0 || xmlHttp.status == 304){
	            var myHead = document.getElementsByTagName("HEAD").item(0);
	            var myScript = document.createElement( "script" );
	            myScript.language = "javascript";
	            myScript.type = "text/javascript";
	            myScript.id = id;
	            try{
	                //IE8以及以下不支持这种方式，需要通过text属性来设置
	                myScript.appendChild(document.createTextNode(xmlHttp.responseText));
	            }catch (ex){
	                myScript.text = xmlHttp.responseText;
	            }
	            myHead.appendChild( myScript );
	            return true;
	        }else{
	            return false;
	        }
	    }else {
	        return false;
	    }
	}
	
	//判断浏览器类型
	function getBrowser() {    
	    var ua = window.navigator.userAgent;    
	    var isIE = window.ActiveXObject != undefined && ua.indexOf("MSIE") != -1;    
	    var isFirefox = ua.indexOf("Firefox") != -1;    
	    var isOpera = window.opr != undefined;    
	    var isChrome = ua.indexOf("Chrome") && window.chrome;    
	    var isSafari = ua.indexOf("Safari") != -1 && ua.indexOf("Version") != -1;    
	    if (isIE) {    
	        return "IE";    
	    } else if (isFirefox) {    
	        return "Firefox";    
	    } else if (isOpera) {    
	        return "Opera";    
	    } else if (isChrome) {    
	        return "Chrome";    
	    } else if (isSafari) {    
	        return "Safari";    
	    } else {    
	        return "Unkown";    
	    }    
	}  

	
	
