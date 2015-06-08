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