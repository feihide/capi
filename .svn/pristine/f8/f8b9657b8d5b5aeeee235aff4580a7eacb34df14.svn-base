<?php

//error_reporting(E_ALL);
ini_set('date.timezone','Asia/Shanghai');

// Use Loader() to autoload our model
$loader = new \Phalcon\Loader();
define('HOST','http://www.zendao.com');
$loader->registerDirs(array(
    __DIR__ . '/app/models/',
    __DIR__ . '/app/lib/'
))->register();
ini_set('date.timezone','Asia/Shanghai');
$di = new \Phalcon\DI\FactoryDefault();

//Set up the database service
$di->set('db', function(){
    return new \Phalcon\Db\Adapter\Pdo\Mysql(array(
        "host" => "192.168.1.200",
        "username" => "cailangdb",
        "password" => "redhat",
        "dbname" => "zentao",
          'charset' => 'UTF8',
    ));
});

$app = new Phalcon\Mvc\Micro($di);
$app['db'] =  function(){
    return new \Phalcon\Db\Adapter\Pdo\Mysql(array(
        "host" => "192.168.1.200",
        "username" => "cailangdb",
        "password" => "redhat",
        'charset' => 'UTF8',
        "dbname" => "zentao"
    ));
};

$app['view'] = function() {
    $view = new \Phalcon\Mvc\View\Simple();
    $view->setViewsDir( __DIR__ . '/app/views/');
    return $view;
};



//开始 require 不同的api

function getLink($id,$title){
    return '<a target="_blank" href="'.getTaskUrl($id).'">'.$title.'</a>';
}

function getBugLink($id,$title){
    return '<a target="_blank" href="'.getBugUrl($id).'">'.$title.'</a>';
}


$app->get('/userlist', function () use ($app) {
    $news = $app['db']->fetchAll('SELECT distinct (u.id) ,u.realname,u.account FROM user u left join userGroup g  on u.account = g.account where g.group in (1,2,4) and u.deleted="0"');

});


//提醒API

$app->get('/user_attention_list',function() use ($app) {

});


function decodeUnicode($str)
{
    return preg_replace_callback('/\\\\u([0-9a-f]{4})/i',
        create_function(
            '$matches',
            'return mb_convert_encoding(pack("H*", $matches[1]), "UTF-8", "UCS-2BE");'
        ),
        $str);
}

$app->get('/json',function (){
    $data = $_GET['data'];
    print_r(decodeUnicode($data));
    if(isset($_GET['date'])){
        echo date('Y-m-d H:i:s',$_GET['date']);
    }
});

$app->notFound(function () use ($app) {
    $app->response->setStatusCode(404, "Not Found")->sendHeaders();
    echo 'This is crazy, but this page was not found!';
});

$app->handle();