# smart-bi项目总结

1.	 vuex本地存储持久化：使用组件[vuex-persistedstate](https://github.com/robinvdvleuten/vuex-persistedstate)，可配置存储为sessionStorage或者localStorage
2.	dev环境下配置proxy，不可把所有的请求代理到服务器（\*\*/\*），否则所有异步组件也会去访问服务器，导致全部404,一定要加条件，如(\*\*/\*.json)
3.	Vue对象带了大量附属信息，可以用JSON.parse(JSON.stringify())来转化成纯数据，此方法也可以用于简单对象深度拷贝对象，但是不支持NaN，Infinity等数据类型以及循环引用，所以慎用！当然，一般情况异步组件会自动处理无用信息，无需手动剔除。
4.	使用Vue-resource时，一般来说请求带一个参数，当请求方式是"POST|PUT|PATCH"时，以body形式请求，当"GET|HEAD|DELETE|JSONP"时，以quey方式请求。当有些情况比如POST以query形式请求时，可以让请求带两个参数，第一个参数会以query形式传递，第二个以body形式。
	参看源码：
	
		 switch (args.length) {
	        case 2:
	            params = args[0];
	            body = args[1];
	            break;
	        case 1:
	            if (/^(POST|PUT|PATCH)$/i.test(options$$1.method)) {
	                body = args[0];
	            } else {
	                params = args[0];
	            }
	            break;
	        case 0:
	            break;
	        default:
	            throw 'Expected up to 2 arguments [params, body], got ' + args.length + ' arguments';
	    }

5.	全局使用一个变量挂载在Vue下(如`Vue.prototype.socketCallback = []`)，用来存储socket消息回调，这样只要有一次socket连接和监听，有需要新的消息监听，只要往socketCallback里面push fun，收到消息之后对socketCallback进行遍历，找到自己的消息类型进行执行。这样做还有个好处是，哪怕没有停留在当前页面，收到socket消息之后也能对它进行处理，比如提醒有新的用户消息。
6.	Vue2.0版本中，v-model with debounce 被废弃了，要去除输入抖动可以用在watch方法里设置setTimeout

	    'editingTab.unit': function() {
            let _t = this
            if (_t.tabChanges) return false
            clearTimeout(_t.levelTimeout)
            _t.levelTimeout = setTimeout(function() {
                _t.setAxisLabel()
            }, 500)
        },

7.	对数据进行遍历以查找某一个特定值的时候，尽量别使用forEach方法，因为它不会跳出循环，哪怕第一次循环就找到了，也会执行到最后一个。
8.	对于同一个数组(对象)，尽量只遍历一次，提前拿到所有需要的结果，而不是用一次遍历一次。
9.	生成随机字符串：`Math.random().toString(36).substr(2)`
10.	各个类型的chart其实有很多相同的配置，可以把相同的部分提取出来，通过Object.assign或类似的方法来拷贝一份，再进行赋值等操作。注意Object.assign是浅拷贝。
11.	计算代码运行的时间，可以在代码前后分别加`console.time('time passed')` 和`console.timeEnd('time passed')`,参数可以是任意字符串，但是要相同。如：

		  console.time('time pass')
          this.data = preInitSqlResult(sqlData, _t.fieldList)
	      console.timeEnd('time pass')
	      
		  // time pass: 131.05712890625ms
12. 时间运算需要注意，
	`new Date('2017-9-12') => Tue Sep 12 2017 00:00:00 GMT+0800 (CST)`
	`new Date('2017-09-12') => Tue Sep 12 2017 08:00:00 GMT+0800 (CST)`
	`new Date('2017-10-12') => Thu Oct 12 2017 08:00:00 GMT+0800 (CST)`
	`new Date('2017/9/12') => Tue Sep 12 2017 00:00:00 GMT+0800 (CST)`
	`new Date('2017/09/12') => Tue Sep 12 2017 00:00:00 GMT+0800 (CST)`
13. 仅仅改变Array的顺序，Vue不会触发重汇。我的做法是先把数组设置为空，setTimeout之后赋值排序过的数组。
14. 数组中按照属性值进行分类排序，比如要把type=='Number'的值排后面，Stirng类型排前面，可以这样：
				`list.sort(function(a,b){
					return a.type=='Number' && b.type == 'String' ? 1 : -1
		})`
15. CSS-scoped慎用。CSS 作用域不能代替 class。考虑到浏览器渲染各种 CSS 选择器的方式，当 p { color: red } 设置了作用域时 (即与特性选择器组合使用时) 会慢很多倍。如果你使用 class 或者 id 取而代之，比如 .example { color: red }，性能影响就会消除。
16. 深度作用选择器
	如果你希望 scoped 样式中的一个选择器能够作用得“更深”，例如影响子组件，你可以使用 `>>>` 操作符：
			    `<style scoped>
					.a >>> .b { /* ... */ }
		</style>`
上述代码将会编译成：
`.a[data-v-f3f3eg9] .b { /* ... */ }`
有些像 SASS 之类的预处理器无法正确解析 >>>。这种情况下你可以使用 /deep/ 操作符(`.a /deep/ .b { /* ... */ } `)取而代之——这是一个 `>>>` 的别名，同样可以正常工作。
深度作用选择器还可以使用在通过 v-html 创建的 DOM 内容上
参见：[vue-loader之scoped-css](https://vue-loader.vuejs.org/zh-cn/features/scoped-css.html)

17. vue中computed的计算属性不能再进行另外赋值，否则会报错。可以通过定义setter对其依赖的属性进行赋值。如果不想更改其依赖的属性，可以结合$watch一个中间变量。
18. 当对一个队形进行重新赋值，不想引起对象内属性的watch监听，可以设置一个阈值，setTimeout之后重置阈值