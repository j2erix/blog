# 我从mroderick的PubSubJS库学到的

GitHub：[https://github.com/mroderick/PubSubJS](https://github.com/mroderick/PubSubJS)  
作者：[mroderick](http://roderick.dk/)

## 支持AMD, CommonJS的模块基础脚手架

以PubSub为例的模块脚手架代码：

	(function( root ) {
		'use strict';
		// 模块代码
		var PubSub = {};
		PubSub.version = '1.1.0';
		PubSub.publish = function() {};
		PubSub.subscribe = function() {};
		PubSub.unsubscribe = function() {};

		// CommonJS支持，或添加到全局对象
		if ( typeof exports !== 'undefined' ) {
			if ( typeof module !== 'undefined' && module.exports ) {
				module.exports = PubSub;
			}
			exports.PubSub = PubSub;
		} else {
			root.PubSub = PubSub;
		}

		// AMD模块支持
		if ( typeof define === 'function' && define.adm ) {
			define( 'pubsub', function() {
				return PubSub;
			} );
		}
	})( typeof window !== 'undefined' && window || this );
	
## 命名空间迭代
Commit版本: `993e251e218d9f4186c53b73881d18978d9f34e9`

PubSub中使用过一个循环的方式来实现message的命名空间化。  
比如注册了a.b的一个message的监听器，那么a.b.c这个message在publish的时候也会触发该监听器，因为他们在命名空间上属于一个包含关系。相关代码如下：

	function messageHasSubscribers( message ){
		var topic = String( message ),
			found = messages.hasOwnProperty( topic ),
			position = topic.lastIndexOf( '.' );

		while ( !found && position !== -1 ){
			topic = topic.substr( 0, position );
			position = topic.lastIndexOf('.');
			found = messages.hasOwnProperty( topic );
		}

		return found;
	}
	
## “能记住参数”的闭包
Commit版本：`a52be6f6e28d61fb5d2de561f91faee97f7fe343`

PubSubJS中的这段代码巧妙地利用了闭包，构造一个能记住参数的函数：

	function createDeliveryFunction( message, data ){
		return function deliverNamespaced(){
			if ( messages.hasOwnProperty( message ) ) {
				deliverMessage(message, message, data);
			}

			namespaceIterator(message, function( name ){
				if ( messages.hasOwnProperty( name ) ){
					deliverMessage(message, name, data );
				}
				return true;
			});
		};
	}
	
这样就能避免在下面调用的时候还要传递参数：

	function publish( message, data, sync ){
		var deliver = createDeliveryFunction( message, data ),
			hasSubsribers = messageHasSubscribers( message );

		if ( !hasSubsribers ){
			return false;
		}

		if ( sync === true ){
			deliver();
		} else {
			setTimeout( deliver, 0 );
		}
		return true;
	}
	
## 与iframe的事件通信
Commit版本：`058899f25cbfadc89e77c4edd967ea6c0e3e0029`

通过在子iframe中引入一个桥接文件：

	function setPubSub( PubSub ){
		window.PubSub = PubSub;
	}

将父window的PubSub实例传入共享，做到与iframe通信。

## version脚本，用于版本更新，打tag等
Commit版本：`d01184a37ba5ad86b5d33c1e367be0147a551a96`

新增了一个version脚本，用于打版本号。使用了node中的child_process异步处理git的提交：

    #!/usr/bin/env node
    var newVersion = process.argv[2],
        fs = require('fs'),
        child_process = require('child_process'),
        packageJson = require('./package.json'),
        bowerJson = require('./bower.json');

    if(!/^[0-9]+\.[0-9]+\.[0-9]+$/.test(newVersion)){
        console.error('Invalid version', newVersion);
        return process.exit(1);
    }

    console.log('tagging version', newVersion);

    packageJson.version = bowerJson.version = newVersion;

    fs.writeFileSync('./package.json', JSON.stringify(packageJson, null, 2));
    fs.writeFileSync('./bower.json', JSON.stringify(bowerJson, null, 2));

    child_process.exec('git commit -am "v' + newVersion + '"', function(code){
        if(code > 0){
            console.error('Could not commit new version');
            return process.exit(1);
        }
        child_process.spawn('git', ['tag', 'v' + newVersion], {stdio: 'inherit'});
    });
    
## 包装成jQuery插件
Commit: `334ced1f6c8046793046b62ca054130d6ad2394b`

PubSub源码中有两个txt文件，包含了jQuery插件的包装部分。  
开始是pubsub.js.pre.txt：

	/*
	Copyright (c) 2010,2011,2012 Morgan Roderick http://roderick.dk
	License: MIT - http://mrgnrdrck.mit-license.org

	https://github.com/mroderick/PubSubJS
	*/
	/*jslint white:true, plusplus:true */
	/*global
		jQuery,
		PubSub
	*/
	;(function($){
		"use strict";
		var self = this;

中间是PubSub代码  

	PubSub代码...

最后是pubsub.js.post.txt：


		if ( $.pubsub ){
			$.error( 'pubsub is already defined on jQuery, skipping integration of PubSubJS' );
			return false;
		}

		function pubsub( method ){
			if ( PubSub.hasOwnProperty( method ) && typeof PubSub[ method ] === 'function' ){
				return PubSub[ method ].apply( self, Array.prototype.slice.call( arguments, 1 ) );
			} else {
				$.error( 'Method ' +  method + ' does not exist on jQuery.pubsub' );
			}    
		}

		// set the version property of the function to be a concatenated version of PubSub's name and version strings
		pubsub.version = PubSub.name + ' ' + PubSub.version;

		// export into jQuery
		$.pubsub = pubsub;
	}(jQuery));
	
用这种方式，在陪着grunt中的concat，就能将pubsub封装成jQuery插件使用：

	concat: {
		options: {
			// overrides the default linefeed separator
			separator: ''
		},
	    jquery: {
	    	src: ['wrappers/jquery/pubsub.js.pre.txt', 'src/pubsub.js', 'wrappers/jquery/pubsub.js.post.txt'],
	    	dest: 'jquery.pubsub.js'
	    }
	}