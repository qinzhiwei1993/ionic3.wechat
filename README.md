# ionic3.wechat
ionic3  微信端网页开发

# ionic3使用微信sdk过程详解(js接口安全域名已经是配置好的)
   a:首先引入sdk的js文件，ionic3是使用typescript,是一个单页面引用程序，所以在index.html中引入该文件即可全局使用
   b:生命wx的全局变量。  在declartion.d.ts中添加一行
          declare var wx: any;我见过有的人使用在单独使用一个文件.d.ts进行生命wx类型，这样的话在页面中使用的时候还需要引用这个.d.ts文件，当在declartion.d.ts
          文件中声明时，wx即为全局变量，无需在单独引用

   c:通过config接口注入权限验证配置
        wx.config({
            debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
            appId: '', // 必填，公众号的唯一标识
            timestamp: , // 必填，生成签名的时间戳
            nonceStr: '', // 必填，生成签名的随机串
            signature: '',// 必填，签名，见附录1
            jsApiList: [] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2
        });

   d:ready接口处理验证成功
        wx.ready(function(){
            // config信息验证后会执行ready方法，所有接口调用都必须在config接口获得结果之后，config是一个客户端的异步操作，所以如果需要在页面加载时就调用相关接口，则须把相关接口放在ready函数中调用来确保正确执行。对于用户触发时才调用的接口，则可以直接调用，不需要放在ready函数中。
        });

   e:error接口处理失败验证
        wx.error(function(res){
            // config信息验证失败会执行error函数，如签名过期导致验证失败，具体错误信息可以打开config的debug模式查看，也可以在返回的res参数中查看，对于SPA可以在这里更新签名。
        });


# ios微信端视频自动播放
        调用微信的js-sdk的getNetworkType接口，在回掉函数中执行视频播放事件，只试用与ios，android还没有找到解决办法
        wx.getNetworkType(function(res){
            console.log(res.networkType) //返回网络类型2g，3g，4g，wifi
            document.getElementById("video").play();
        })
        
# ios andorid 微信窗口内播放
  ##<video playsinline webkit-playsinline x5-playsinline></video>  
        
        ios: ios系统微信使用的内核是chrome浏览器的内核，需要兼容之前的版本，所以使用属性 playsinline 和  webkit-playsinline即可窗口内播放，且视频是同层播放，video的上层可以浮动内容
        
        android: android系统微信内使用的内核是腾讯自己的x5内核， x5-playsinline属性可以使video小窗口内播放，但是video被置为最顶层，上面不能浮动任何东西。这个问题目前还没有出来解决方案，看腾讯自己以后怎么做了。还有一种方式就是x5-video-player-type="h5"， 将video同层播放，但是会全屏。 
          
        
        白名单：这个是腾讯的一个版名单，将网页的域名放到wximg.gtimg.co下面，video将不被接管，使用原生的video进行播放，当然也可以小窗口，自动播放应该也不是问题
      
      

