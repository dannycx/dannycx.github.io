# service

## 启动方式
```java
    startService(new Intent(this, MusicService.class));
    stopService(new Intent(this, MusicService.class));

    private ServiceConnection mConn = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {}

        @Override
        public void onServiceDisconnected(ComponentName name) {}
    };
    
    bindService(new Intent(this,MusicService.class),mConn, Context.BIND_AUTO_CREATE);
    unbindService(mConn);
```

## 启动过程
- 从ContextWrapper的startService()开始
