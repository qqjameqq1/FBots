# FBots

[![stable version](https://img.shields.io/badge/stable-0.2.4-brightgreen.svg?style=flat-square)](https://packagist.org/packages/thekdesign/fbots)
[![unstable version](https://img.shields.io/badge/unstable-dev-orange.svg?style=flat-square)](https://packagist.org/packages/thekdesign/fbots)
[![php version](https://img.shields.io/badge/php->=7.0.0-blue.svg?style=flat-square)](https://packagist.org/packages/thekdesign/fbots)
[![GitHub license](https://img.shields.io/github/license/ThekDesign/FBots.svg?style=flat-square)](https://github.com/ThekDesign/FBots/blob/master/LICENSE)<br>

reference - [BotMan](https://botman.io/)<br>
reference - [Messenger](https://developers.facebook.com/docs/messenger-platform)<br>
reference - [Pages](https://developers.facebook.com/docs/pages)

## 開始使用 Getting Started

+ 產生一個空的 laravel 專案
+ new a laravel project

```
laravel new fbots
```

or

```
composer create-project --prefer-dist laravel/laravel fbots
```

+ 有一個 facebook developer 的帳號
+ have a facebook developer account

+ 擁有一個 https 的網址 (推薦使用 heroku 部署)
+ have a https web (recommend heroku)

### 必要條件 Prerequisites

+ 確定你有一個 facebook developer 帳號，並新建一個應用程式
+ make sure that you have a facebook developer account and create an application

### 安裝及設定 Install and Settings

```
composer require thekdesign/fbots
```

到 .env 檔內新增三個設定值
add three values in .env

```
//粉絲專頁的權杖
FACEBOOK_TOKEN = "your-facebook-page-token"

//自己設定，主要是拿來驗證webhook
FACEBOOK_VERIFICATION = "your-facebook-verification-token"

//應用程式的密鑰
FACEBOOK_APP_SECRET = "your-facebook-app-secret"
```
-----------------------------------------------------------

## 快速使用 Quick Start

<strong>安裝所有元件 install all </strong>

```
composer require thekdesign/fbots
```

新增一個 Controller 再新增一個 function
<br>new a Controller and new a function

<strong>Controller</strong>

```
class ChatController extends Controller
{
    public function talk(Request $request)
    {
    
        
        /*************************************************************/
        /*                            config                         */
        /* --------------------------------------------------------- */
        /* token         : facebook page's access_token              */
        /* app_secret    : application's secret                      */
        /* verification  : to connect webhook, so it depends on you  */
        /* --------------------------------------------------------- */
        /*************************************************************/

        // 記得去獲取永久不過期的 facebook page's access_token
        // get the facebook page's access_token that never expires

        $config = [
            'facebook' => [
                'token' => env('FACEBOOK_TOKEN'),
                'app_secret' => env('FACEBOOK_APP_SECRET'),
                'verification' => env('FACEBOOK_VERIFICATION'),
            ]
        ];

        $bots = new FBots($request, $config);

        $bots->chat('Hey', 'Hello');

        $bots->listen();

    }
}
```

<strong>Route</strong>

```
Route::match(['get', 'post'], '/bots', 'ChatController@talk');
```

<strong>VerifyCsrfToken</strong>

reference - https://laravel.com/docs/5.5/csrf#csrf-excluding-uris

將你的route加入白名單 <br>
Excluding URIs From CSRF Protection


### Messenger 設定 Messenger Settings

參考
https://developers.facebook.com/docs/messenger-platform

reference
https://developers.facebook.com/docs/messenger-platform


## 功能說明 functions

詳細功能請查閱
https://botman.io/2.0/driver-facebook-messenger

reference
https://botman.io/2.0/driver-facebook-messenger

----------------------------------

<strong>聊天機器人</strong><br>
<strong>ChatBots</strong>

```

    /************************/
    /* -------------------- */
    /*         Chat         */
    /* -------------------- */
    /************************/
    
    // new a fbot
    
        $bots = new FBots($request);
    
    // chat function
    
        $bots->chat('Hey', 'Hello!');
        
    // other chat function ($method is array or object)
    
        $bots->chat('Hey', $method);

    // conversation function (YourConversation is class)
    
        $bots->chat('Hey', startConvesation(new YourConversation));
        
    // listen function
    
        $bots->listen();

```

```
        $bots = new FBots($request);
        
        // 按鈕形式 button template
        $button = ButtonTemplate::create('123')
            ->addButtons([
                    ElementButton::create('Tell me more')->type("postback")->payload("tellmemore"),
                    ElementButton::create('visit')->url("http://botman.io/")
                ]
            );

        // 滑動瀏覽 generic template
        $generic = GenericTemplate::create()
            ->addImageAspectRatio(GenericTemplate::RATIO_SQUARE)
            ->addElements([
                Element::create('BotMan Documentation')
                    ->subtitle('All about BotMan')
                    ->image('http://botman.io/img/botman-body.png')
                    ->addButton(ElementButton::create('visit')->url('http://botman.io'))
                    ->addButton(ElementButton::create('tell me more')
                        ->payload('tellmemore')->type('postback')),
                Element::create('BotMan Laravel Starter')
                    ->subtitle('This is the best way to start with Laravel and BotMan')
                    ->image('http://botman.io/img/botman-body.png')
                    ->addButton(ElementButton::create('visit')
                        ->url('https://github.com/mpociot/botman-laravel-starter')
                    )
            ]);

        //列表形式 list template
        $list = ListTemplate::create()
            ->useCompactView()
            ->addGlobalButton(ElementButton::create('view more')->url('http://test.at'))
            ->addElements([
                Element::create('BotMan Documentation')
                    ->subtitle('All about BotMan')
                    ->image('http://botman.io/img/botman-body.png')
                    ->addButton(ElementButton::create('tell me more')
                        ->payload('tellmemore')->type('postback')),

                Element::create('BotMan Laravel Starter')
                    ->subtitle('This is the best way to start with Laravel and BotMan')
                    ->image('http://botman.io/img/botman-body.png')
                    ->addButton(ElementButton::create('visit')
                        ->url('https://github.com/mpociot/botman-laravel-starter')
                    )
            ]);

        // 點餐形式 receipt template
        $receipt = ReceiptTemplate::create()
            ->recipientName('Christoph Rumpel')
            ->merchantName('BotMan GmbH')
            ->orderNumber('342343434343')
            ->timestamp('1428444852')
            ->orderUrl('http://test.at')
            ->currency('USD')
            ->paymentMethod('VISA')
            ->addElement(ReceiptElement::create('T-Shirt Small')->price(15.99)->image('http://botman.io/img/botman-body.png'))
            ->addElement(ReceiptElement::create('Sticker')->price(2.99)->image('http://botman.io/img/botman-body.png'))
            ->addAddress(ReceiptAddress::create()
                ->street1('Watsonstreet 12')
                ->city('Bot City')
                ->postalCode(100000)
                ->state('Washington AI')
                ->country('Botmanland')
            )
            ->addSummary(ReceiptSummary::create()
                ->subtotal(18.98)
                ->shippingCost(10)
                ->totalTax(15)
                ->totalCost(23.98)
            )
            ->addAdjustment(ReceiptAdjustment::create('Laravel Bonus')->amount(5));

        // 圖片形式 image template
        $image = MediaTemplate::create()
            ->element(MediaAttachmentElement::create('image')
                ->attachmentId('1543527005693234')
                ->addButton(ElementButton::create('Tell me more')
                    ->type('postback')
                    ->payload('Tell me more'))
                ->addButton(ElementButton::create('Documentation')
                    ->url('https://botman.io/')));

        //影片形式 video template
       $video = MediaTemplate::create()
            ->element(MediaUrlElement::create('video')
                ->url('https://www.facebook.com/liechteneckers/videos/10155225087428922/')
                ->addButtons([
                    ElementButton::create('Web URL')
                        ->url('http://liechtenecker.at'),
                    ElementButton::create('payload')
                        ->type('postback')
                        ->payload('test'),
                ]));

        $bots->chat('Hey', 'Hello!');
        
        // 加入你想要的 add whatever you want
        // $bots->chat('give me button', $button);
        // $bots->chat('give me generic', $generic);
        // $bots->chat('give me list', $list);
        // $bots->chat('give me receipt', $receipt);
        // $bots->chat('give me image', $image);
        // $bots->chat('give me video', $video);

        $bots->listen();
```
--------------------------------------

<strong>粉絲團</strong><br>
<strong>Pages</strong>

```
    
        /***************************/
        /* ----------------------- */
        /*         Publish         */
        /* ----------------------- */
        /***************************/
        
    // new a publish
    
        $pages = new Publish();    

    /***********************************************************************/
    /*                          post method                                */
    /* ------------------------------------------------------------------- */
    /* $pages_id     : id from your facebook pages        | string         */
    /* $message      : write what you want to post        | string         */
    /* $link         : if you want to post include links  | string         */
    /* $photo        : if you want to post include photos | string(url)    */
    /* $published    : decide that publish now or later   | string(url)    */
    /* $publish_time : decide that publish time           | Unix timestamp */
    /* ------------------------------------------------------------------- */
    /***********************************************************************/
        
    // post function
    
    // 圖片請使用url
    // use url for photo 
    
        $pages->post($pages_id, $message);
    
    /*************************************************************/
    /*                          get method                       */
    /* --------------------------------------------------------- */
    /* $page_id  : the post's id                        | string */
    /* $type     : get information from pages or posts  | string */
    /* --------------------------------------------------------- */
    /*************************************************************/
        
    // get function
        
        $pages->get($page_id);

    /************************************************************************/
    /*                               reply method                           */
    /* -------------------------------------------------------------------- */
    /* $type     : comments, private_replies(Permissions problems) | string */
    /* $page_id  : the post's id                                   | string */
    /* $message  : write what you want to reply                    | string */
    /* -------------------------------------------------------------------- */
    /************************************************************************/
    
    // reply function
    
        $pages->reply($type, $post_id, $message);

    /**********************************************************/
    /*                    autoReply method                    */
    /* ------------------------------------------------------ */
    /* $type_reply : comments, private_replies    | string    */
    /* $post_id    : the post's id                | string    */
    /* $hears      : keyword that start to reply  | string    */
    /* $reply      : write what you want to reply | string    */
    /* $repeat     : remove the repeat user       | boolean   */
    /* ------------------------------------------------------ */
    /**********************************************************/
        
    // autoReply function
    
        $pages->autoReply($type_reply, $post_id, $hears, $reply);
        
    /****************************************/
    /*          finishPost method           */
    /* ------------------------------------ */
    /* $post_id  : the post's id   | string */
    /* ------------------------------------ */
    /****************************************/
    
    // finishPost function
    
        $pages->finishPost($post_id);
```

```
Get Method

reference - https://developers.facebook.com/docs/graph-api/reference/page/

$pages->get('page_id', $type);
```

```
    public function publish()
    {
        $pages = new Publish();

        $pages_id = '***';
        $message = 'Hello World';
        $link = 'https://github.com/ThekDesign';
        // $link = 'https://www.youtube.com/';
        $photo = 'https://c1.staticflickr.com/9/8755/16962787241_0f4839004a_b.jpg';
        
        /* 排定發佈，選擇false會得到response後的id，根據個人需要發佈的時機發佈 */
        /* If you set "$publish = false", you will get the id of response. */
        /* You can decide the publish time. */ 
        // reference - https://developers.facebook.com/docs/pages/publishing
        $publish = true;

        $result = $pages->post(
            $pages_id,
            $message,
            $link,
            $photo,
            $publish);

        return $result;
    }

    public function reply()
    {
        $pages = new Publish();


        // type: comments(公開回覆), private_replies(私人訊息)
        $pages->reply('comments', 'page_id', 'message');
    }
    
    public function obtain()
    {
        $pages = new Publish();

        // get the post 拿到那則貼文
        $result = $pages->get('page_id');
        
        // type: comments(公開回覆)
        // get all comments 拿到所有留言
        // $result = $pages->get('page_id', 'comments');

        return $result;
    }
    
    public function send()
    {
        $pages = new Publish();
        
        $pages->finishPost('page_id');
    }

```


## Deployment

## Built With

## Contributing

## Versioning

## Authors

* **Kevin Xu** - *Initial work* - [ThekDesign](https://github.com/ThekDesign)

## License

FBots is licensed under the MIT License - see the [LICENSE](LICENSE) file for details
