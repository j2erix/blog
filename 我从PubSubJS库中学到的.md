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
	})( this );
	
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
	
