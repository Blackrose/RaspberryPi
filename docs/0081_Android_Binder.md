# Android Binder

## 参考文档

* [Binder系列1—Binder Driver初探](http://gityuan.com/2015/11/01/binder-driver/)
* [Android Binder机制(三) ServiceManager守护进程](https://wangkuiwu.github.io/2014/09/03/Binder-ServiceManager-Daemon/)
* [binder-for-linux](https://github.com/hungys/binder-for-linux)
* [Android Binder 修炼之道（二）Client Server 实例](https://blog.csdn.net/lizuobin2/article/details/76321770)
* [Binder子系统之调试分析(二)](http://gityuan.com/2016/08/28/binder-debug-2/)

## Code

* [binder4linux](https://github.com/ZengjfOS/RaspberryPi/tree/binder4linux)

## menuconfig

* cat .config

  ```config
  [...省略]
  #
  # Android
  #
  CONFIG_ANDROID=y
  CONFIG_ANDROID_BINDER_IPC=y
  CONFIG_ANDROID_BINDER_DEVICES="binder,hwbinder,vndbinder"
  CONFIG_ANDROID_BINDER_IPC_SELFTEST=y
  [...省略]
  ```

## dev

```bash
pi@raspberrypi:~/zengjf/linux/linux-rpi-4.19.y $ ls -al /dev/binder
crw------- 1 root root 10, 61 Feb 23 03:21 /dev/binder
crw------- 1 root root 10, 60 Feb 23 03:21 /dev/hwbinder
crw------- 1 root root 10, 59 Feb 23 03:21 /dev/vndbinder
```

## servicemanager test steps

* sudo ./bctest publish zengjf

  ```bash
  pi@raspberrypi:~/zengjf/servicemanager $ sudo ./bctest publish zengjf
  0000: 00 . 00 . 00 . 00 . 1a . 00 . 00 . 00 . 61 a 00 . 6e n 00 . 64 d 00 . 72 r 00 .
  0016: 6f o 00 . 69 i 00 . 64 d 00 . 2e . 00 . 6f o 00 . 73 s 00 . 2e . 00 . 49 I 00 .
  0032: 53 S 00 . 65 e 00 . 72 r 00 . 76 v 00 . 69 i 00 . 63 c 00 . 65 e 00 . 4d M 00 .
  0048: 61 a 00 . 6e n 00 . 61 a 00 . 67 g 00 . 65 e 00 . 72 r 00 . 00 . 00 . 00 . 00 .
  0064: 06 . 00 . 00 . 00 . 7a z 00 . 65 e 00 . 6e n 00 . 67 g 00 . 6a j 00 . 66 f 00 .
  0080: 00 . 00 . fd . b6 . 85 . 2a * 62 b 73 s 7f . 01 . 00 . 00 . 70 p 30 0 02 . 00 .
  0096: 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 .
  BR_NOOP:
  BR_INCREFS:
    0xbea5c2fc, 0xbea5c300
  BR_ACQUIRE:
    0xbea5c310, 0xbea5c314
  BR_TRANSACTION_COMPLETE:
  BR_REPLY:
    target 0000000000000000  cookie 0000000000000000  code 00000000  flags 00000000
    pid        0  uid        0  data 4  offs 0
  0000: 00 . 00 . 00 . 00 .
  ```

* sudo ./bctest lookup zengjf

  ```bash
  pi@raspberrypi:~/zengjf/servicemanager $ sudo ./bctest lookup zengjf
  0000: 00 . 00 . 00 . 00 . 1a . 00 . 00 . 00 . 61 a 00 . 6e n 00 . 64 d 00 . 72 r 00 .
  0016: 6f o 00 . 69 i 00 . 64 d 00 . 2e . 00 . 6f o 00 . 73 s 00 . 2e . 00 . 49 I 00 .
  0032: 53 S 00 . 65 e 00 . 72 r 00 . 76 v 00 . 69 i 00 . 63 c 00 . 65 e 00 . 4d M 00 .
  0048: 61 a 00 . 6e n 00 . 61 a 00 . 67 g 00 . 65 e 00 . 72 r 00 . 00 . 00 . 00 . 00 .
  0064: 06 . 00 . 00 . 00 . 7a z 00 . 65 e 00 . 6e n 00 . 67 g 00 . 6a j 00 . 66 f 00 .
  0080: 00 . 00 . f9 . b6 .
  BR_NOOP:
  BR_TRANSACTION_COMPLETE:
  BR_REPLY:
    target 0000000000000000  cookie 0000000000000000  code 00000000  flags 00000000
    pid        0  uid        0  data 24  offs 8
  0000: 85 . 2a * 68 h 73 s 7f . 01 . 00 . 00 . 01 . 00 . 00 . 00 . 00 . 00 . 00 . 00 .
  0016: 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 .
    - type 73682a85  flags 0000017f  ptr 0000000000000001  cookie 0000000000000000
  lookup(zengjf) = 1
  ```

* sudo ./servicemanager

  ```bash
  pi@raspberrypi:~/zengjf/servicemanager $ sudo ./servicemanager
  BR_NOOP:
  BR_TRANSACTION:
    target 0000000000000000  cookie 0000000000000000  code 00000003  flags 00000000
    pid     4469  uid        0  data 108  offs 8
  0000: 00 . 00 . 00 . 00 . 1a . 00 . 00 . 00 . 61 a 00 . 6e n 00 . 64 d 00 . 72 r 00 .
  0016: 6f o 00 . 69 i 00 . 64 d 00 . 2e . 00 . 6f o 00 . 73 s 00 . 2e . 00 . 49 I 00 .
  0032: 53 S 00 . 65 e 00 . 72 r 00 . 76 v 00 . 69 i 00 . 63 c 00 . 65 e 00 . 4d M 00 .
  0048: 61 a 00 . 6e n 00 . 61 a 00 . 67 g 00 . 65 e 00 . 72 r 00 . 00 . 00 . 00 . 00 .
  0064: 06 . 00 . 00 . 00 . 7a z 00 . 65 e 00 . 6e n 00 . 67 g 00 . 6a j 00 . 66 f 00 .
  0080: 00 . 00 . fd . b6 . 85 . 2a * 68 h 73 s 7f . 01 . 00 . 00 . 01 . 00 . 00 . 00 .
  0096: 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 .
    - type 73682a85  flags 0000017f  ptr 0000000000000001  cookie 0000000000000000
  BR_NOOP:
  BR_TRANSACTION_COMPLETE:
  BR_NOOP:
  BR_TRANSACTION:
    target 0000000000000000  cookie 0000000000000000  code 00000002  flags 00000000
    pid     4477  uid        0  data 84  offs 0
  0000: 00 . 00 . 00 . 00 . 1a . 00 . 00 . 00 . 61 a 00 . 6e n 00 . 64 d 00 . 72 r 00 .
  0016: 6f o 00 . 69 i 00 . 64 d 00 . 2e . 00 . 6f o 00 . 73 s 00 . 2e . 00 . 49 I 00 .
  0032: 53 S 00 . 65 e 00 . 72 r 00 . 76 v 00 . 69 i 00 . 63 c 00 . 65 e 00 . 4d M 00 .
  0048: 61 a 00 . 6e n 00 . 61 a 00 . 67 g 00 . 65 e 00 . 72 r 00 . 00 . 00 . 00 . 00 .
  0064: 06 . 00 . 00 . 00 . 7a z 00 . 65 e 00 . 6e n 00 . 67 g 00 . 6a j 00 . 66 f 00 .
  0080: 00 . 00 . f9 . b6 .
  BR_NOOP:
  BR_TRANSACTION_COMPLETE:
  ```

* sudo ./bctest lookup alt

  ```bash
  pi@raspberrypi:~/zengjf/servicemanager $ sudo ./bctest alt
  0000: 00 . 00 . 00 . 00 . 1a . 00 . 00 . 00 . 61 a 00 . 6e n 00 . 64 d 00 . 72 r 00 .
  0016: 6f o 00 . 69 i 00 . 64 d 00 . 2e . 00 . 6f o 00 . 73 s 00 . 2e . 00 . 49 I 00 .
  0032: 53 S 00 . 65 e 00 . 72 r 00 . 76 v 00 . 69 i 00 . 63 c 00 . 65 e 00 . 4d M 00 .
  0048: 61 a 00 . 6e n 00 . 61 a 00 . 67 g 00 . 65 e 00 . 72 r 00 . 00 . 00 . 00 . 00 .
  0064: 0b . 00 . 00 . 00 . 61 a 00 . 6c l 00 . 74 t 00 . 5f _ 00 . 73 s 00 . 76 v 00 .
  0080: 63 c 00 . 5f _ 00 . 6d m 00 . 67 g 00 . 72 r 00 . 00 . 00 .
  BR_NOOP:
  BR_TRANSACTION_COMPLETE:
  BR_REPLY:
    target 0000000000000000  cookie 0000000000000000  code 00000000  flags 00000000
    pid        0  uid        0  data 4  offs 0
  0000: 00 . 00 . 00 . 00 .
  cannot find alt_svc_mgr
  ```

  * 这里失败的原因主要是因为没注册这个service，所以需要先publish alt_svc_mgr这个service还行；
  * sudo ./bctest publish alt_svc_mgr

## service client test steps

* sudo ./service

  ```bash
  pi@raspberrypi:~/zengjf/servicemanager $ sudo ./service
  0000: 00 . 00 . 00 . 00 . 1a . 00 . 00 . 00 . 61 a 00 . 6e n 00 . 64 d 00 . 72 r 00 .
  0016: 6f o 00 . 69 i 00 . 64 d 00 . 2e . 00 . 6f o 00 . 73 s 00 . 2e . 00 . 49 I 00 .
  0032: 53 S 00 . 65 e 00 . 72 r 00 . 76 v 00 . 69 i 00 . 63 c 00 . 65 e 00 . 4d M 00 .
  0048: 61 a 00 . 6e n 00 . 61 a 00 . 67 g 00 . 65 e 00 . 72 r 00 . 00 . 00 . 00 . 00 .
  0064: 09 . 00 . 00 . 00 . 63 c 00 . 61 a 00 . 6c l 00 . 63 c 00 . 75 u 00 . 6c l 00 .
  0080: 61 a 00 . 74 t 00 . 65 e 00 . 00 . 00 . 85 . 2a * 62 b 73 s 7f . 01 . 00 . 00 .
  0096: 7b { 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 .
  BR_NOOP:
  BR_INCREFS:
    0xbea1b31c, 0xbea1b320
  BR_ACQUIRE:
    0xbea1b330, 0xbea1b334
  BR_TRANSACTION_COMPLETE:
  BR_REPLY:
    target 0000000000000000  cookie 0000000000000000  code 00000000  flags 00000000
    pid        0  uid        0  data 4  offs 0
  0000: 00 . 00 . 00 . 00 .
  BR_NOOP:
  BR_TRANSACTION:
    target 000000000000007b  cookie 0000000000000000  code 00000000  flags 00000000
    pid     4884  uid        0  data 8  offs 0
  0000: 00 . 00 . 00 . 00 . 79 y 00 . 00 . 00 .
  i am server,add one 121
  BR_NOOP:
  BR_TRANSACTION_COMPLETE:
  ```

* sudo ./client a 121

  ```bash
  pi@raspberrypi:~/zengjf/servicemanager $ sudo ./client a 121
  0000: 00 . 00 . 00 . 00 . 1a . 00 . 00 . 00 . 61 a 00 . 6e n 00 . 64 d 00 . 72 r 00 .
  0016: 6f o 00 . 69 i 00 . 64 d 00 . 2e . 00 . 6f o 00 . 73 s 00 . 2e . 00 . 49 I 00 .
  0032: 53 S 00 . 65 e 00 . 72 r 00 . 76 v 00 . 69 i 00 . 63 c 00 . 65 e 00 . 4d M 00 .
  0048: 61 a 00 . 6e n 00 . 61 a 00 . 67 g 00 . 65 e 00 . 72 r 00 . 00 . 00 . 00 . 00 .
  0064: 09 . 00 . 00 . 00 . 63 c 00 . 61 a 00 . 6c l 00 . 63 c 00 . 75 u 00 . 6c l 00 .
  0080: 61 a 00 . 74 t 00 . 65 e 00 . 00 . 00 .
  BR_NOOP:
  BR_TRANSACTION_COMPLETE:
  BR_REPLY:
    target 0000000000000000  cookie 0000000000000000  code 00000000  flags 00000000
    pid        0  uid        0  data 24  offs 8
  0000: 85 . 2a * 68 h 73 s 7f . 01 . 00 . 00 . 01 . 00 . 00 . 00 . 00 . 00 . 00 . 00 .
  0016: 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 .
    - type 73682a85  flags 0000017f  ptr 0000000000000001  cookie 0000000000000000
  0000: 00 . 00 . 00 . 00 . 79 y 00 . 00 . 00 .
  BR_NOOP:
  BR_TRANSACTION_COMPLETE:
  BR_REPLY:
    target 0000000000000000  cookie 0000000000000000  code 00000000  flags 00000000
    pid        0  uid        0  data 4  offs 0
  0000: 7a z 00 . 00 . 00 .
  get ret of addone= 122
  ```

* sudo ./servicemanager

  ```bash
  pi@raspberrypi:~/zengjf/servicemanager $ sudo ./servicemanager
  BR_NOOP:
  BR_TRANSACTION:
    target 0000000000000000  cookie 0000000000000000  code 00000002  flags 00000000
    pid     4743  uid        0  data 88  offs 0
  0000: 00 . 00 . 00 . 00 . 1a . 00 . 00 . 00 . 61 a 00 . 6e n 00 . 64 d 00 . 72 r 00 .
  0016: 6f o 00 . 69 i 00 . 64 d 00 . 2e . 00 . 6f o 00 . 73 s 00 . 2e . 00 . 49 I 00 .
  0032: 53 S 00 . 65 e 00 . 72 r 00 . 76 v 00 . 69 i 00 . 63 c 00 . 65 e 00 . 4d M 00 .
  0048: 61 a 00 . 6e n 00 . 61 a 00 . 67 g 00 . 65 e 00 . 72 r 00 . 00 . 00 . 00 . 00 .
  0064: 09 . 00 . 00 . 00 . 63 c 00 . 61 a 00 . 6c l 00 . 63 c 00 . 75 u 00 . 6c l 00 .
  0080: 61 a 00 . 74 t 00 . 65 e 00 . 00 . 00 .
  BR_NOOP:
  BR_TRANSACTION_COMPLETE:
  BR_NOOP:
  BR_TRANSACTION:
    target 0000000000000000  cookie 0000000000000000  code 00000002  flags 00000000
    pid     4884  uid        0  data 88  offs 0
  0000: 00 . 00 . 00 . 00 . 1a . 00 . 00 . 00 . 61 a 00 . 6e n 00 . 64 d 00 . 72 r 00 .
  0016: 6f o 00 . 69 i 00 . 64 d 00 . 2e . 00 . 6f o 00 . 73 s 00 . 2e . 00 . 49 I 00 .
  0032: 53 S 00 . 65 e 00 . 72 r 00 . 76 v 00 . 69 i 00 . 63 c 00 . 65 e 00 . 4d M 00 .
  0048: 61 a 00 . 6e n 00 . 61 a 00 . 67 g 00 . 65 e 00 . 72 r 00 . 00 . 00 . 00 . 00 .
  0064: 09 . 00 . 00 . 00 . 63 c 00 . 61 a 00 . 6c l 00 . 63 c 00 . 75 u 00 . 6c l 00 .
  0080: 61 a 00 . 74 t 00 . 65 e 00 . 00 . 00 .
  BR_NOOP:
  BR_TRANSACTION_COMPLETE:
  ```

## Publish数据格式

* sudo ./bctest publish zengjf

  ```bash
  pi@raspberrypi:~/zengjf/servicemanager $ sudo ./bctest publish zengjf
  0000: 00 . 00 . 00 . 00 . 1a . 00 . 00 . 00 . 61 a 00 . 6e n 00 . 64 d 00 . 72 r 00 .
  0016: 6f o 00 . 69 i 00 . 64 d 00 . 2e . 00 . 6f o 00 . 73 s 00 . 2e . 00 . 49 I 00 .
  0032: 53 S 00 . 65 e 00 . 72 r 00 . 76 v 00 . 69 i 00 . 63 c 00 . 65 e 00 . 4d M 00 .
  0048: 61 a 00 . 6e n 00 . 61 a 00 . 67 g 00 . 65 e 00 . 72 r 00 . 00 . 00 . 00 . 00 .
  0064: 06 . 00 . 00 . 00 . 7a z 00 . 65 e 00 . 6e n 00 . 67 g 00 . 6a j 00 . 66 f 00 .
  0080: 00 . 00 . fd . b6 . 85 . 2a * 62 b 73 s 7f . 01 . 00 . 00 . 70 p 30 0 02 . 00 .
  0096: 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 . 00 .
  BR_NOOP:
  BR_INCREFS:
    0xbea5c2fc, 0xbea5c300
  BR_ACQUIRE:
    0xbea5c310, 0xbea5c314
  BR_TRANSACTION_COMPLETE:
  BR_REPLY:
    target 0000000000000000  cookie 0000000000000000  code 00000000  flags 00000000
    pid        0  uid        0  data 4  offs 0
  0000: 00 . 00 . 00 . 00 .
  ```

* publish code

  ```C
  int svcmgr_publish(struct binder_state *bs, uint32_t target, const char *name, void *ptr)
  {
      int status;
      // 128 bytes
      unsigned iodata[512/4];
      struct binder_io msg, reply;
  
      bio_init(&msg, iodata, sizeof(iodata), 4);
      // 4 bytes with 0
      bio_put_uint32(&msg, 0);  // strict mode header
      /**
       * * start with 4 bytes with len and following with string and '\0' for end string：
       *   bio_put_uint32(bio, len);
       *   ptr = bio_alloc(bio, (len + 1) * sizeof(uint16_t));
       * * bio_alloc采用了4字节对齐，所以你会看到这一段多出了2个byte，(54 + 2) % 4 = 0；
       *   size = (size + 3) & (~3);
       */
      bio_put_string16_x(&msg, SVC_MGR_NAME);
      // start with 4 bytes with len and following with string and '\0' for end string：
      bio_put_string16_x(&msg, name);
      bio_put_obj(&msg, ptr);
  
      if (binder_call(bs, &msg, &reply, target, SVC_MGR_ADD_SERVICE))
          return -1;
  
      status = bio_get_uint32(&reply);
  
      binder_done(bs, &msg, &reply);
  
      return status;
  }
  ```

## binder log

* /sys/kernel/debug/binder

  ```bash
  total 0
  drwxr-xr-x  3 root root 0 Jan  1  1970 .
  drwx------ 34 root root 0 Jan  1  1970 ..
  -r--r--r--  1 root root 0 Jan  1  1970 failed_transaction_log
  drwxr-xr-x  2 root root 0 Feb 23 09:37 proc
  -r--r--r--  1 root root 0 Jan  1  1970 state
  -r--r--r--  1 root root 0 Jan  1  1970 stats
  -r--r--r--  1 root root 0 Jan  1  1970 transaction_log
  -r--r--r--  1 root root 0 Jan  1  1970 transactions
  ```

* cat transaction_log

  ```bash
  root@raspberrypi:/sys/kernel/debug/binder# cat transaction_log
  [...省略]
  88: reply from 5283:5283 to 5426:5426 context binder node 0 handle 0 size 24:8 ret 0/0 l=0
  90: call  from 5455:5455 to 5283:0 context binder node 69 handle 0 size 92:0 ret 0/0 l=0
  91: reply from 5283:5283 to 5455:5455 context binder node 0 handle 0 size 24:8 ret 0/0 l=0
  93: call  from 5878:5878 to 5283:0 context binder node 69 handle 0 size 92:0 ret 0/0 l=0
  94: reply from 5283:5283 to 5878:5878 context binder node 0 handle 0 size 24:8 ret 0/0 l=0
  ```

* debug_id: call_type from from_proc:from_thread to to_proc:to_thread node to_node handle target_handle size data_size:offsets_size
  * call_type：有3种，分别为async, call, reply；
  * data_size单位是字节数；
* transaction_log以及还有binder_transaction_log_failed会只会记录最近的32次的transaction过程；
* proc目录下的进程文件中有线程信息可以；
