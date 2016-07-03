# 8. WebSocket

swoole_websocket_server 继承自 swoole_http_server,所以websocket_server 不仅拥有http_server的回调函数[onRequest](c7.md)。还有 onHandShake[可选] onOpen onMessage 等回调函数，下面一一来介绍。。当然，websocket_server也可以当Http服务器来用。
```php
$server = new swoole_websocket_server("0.0.0.0", 9501);

$server->on('open', function (swoole_websocket_server $server, $request) {
    echo "server: handshake success with fd{$request->fd}\n";
});

$server->on('message', function (swoole_websocket_server $server, $frame) {
    echo "receive from {$frame->fd}:{$frame->data},opcode:{$frame->opcode},fin:{$frame->finish}\n";
    $server->push($frame->fd, "this is server");
});

$server->on('close', function ($ser, $fd) {
    echo "client {$fd} closed\n";
});

$server->start();
```
短短十来行代码就可以实现简单的websocket server。

## onOpen
当有新的WebSocket客户端与本服务建立连接并完成握手后会回调此函数。如果在onConnect里有代码，会先执行onConnect会滴啊函数。

## onMessage







[点击查看](example/websocket)聊天室完整代码样例：