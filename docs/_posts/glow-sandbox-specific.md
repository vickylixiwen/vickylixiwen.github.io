---
layout: post
title:  "Sandbox Function"
---


## NEED TO ADD THIS FILE TO .gitignore

#### Digest Email
{% highlight python %}
	import emma.disttasks.data_summary as ds
	uid = 14
	ds.daily_digest_email_job(uid, 0) //
{% endhighlight %}


ctx_dict = {
   'platform': 'ios'
}
from suso.utils import context
with context.LocalContext(ctx_dict):


#### disable seasonal sale 
{% highlight python %}
In [2]: import bryo.app.ps_discount.seasonal_op as sop

In [3]: sop.save_global_discounts([])
{% endhighlight %}


#### Base Android 
更新后，需要publish 一下
./gradlew :prime:publishToMavenLocal



#### get baby week and day 
{% highlight python %}
	from datetime import datetime, timedelta
	today = datetime.now().strftime("%Y/%m/%d")
	print(today)
	birth = '2022/02/01'
	d1 = datetime.strptime(today, "%Y/%m/%d")
	d2 = datetime.strptime(birth, "%Y/%m/%d")
	bd = (d1-d2).days
	print(type(bd))

	print("birth days is: %s" % bd)
	week = bd % 7
	print("week is %d, day is %d" %(bd/7, bd%7))
{% endhighlight %}

#### 打包ad hoc
需要distribution 证书： ios_dist_20221009_Certificates.p12
双击添加到keychain，密码Passw0rd
添加到APP的ad hoc target下面
这样就可以本地打包ad hoc了


####  KV
{% highlight python %}
	import kayleee.disttasks.etaqueue as ke
	ke.eta_client.get_all_tasks(uid)
	返回response：

[{'args': [72057594037928097],
  'eta': 1599562962,
  'kwargs': {},
  'task_name': 51,
  'user_id': 72057594037928097},
 {'args': [72057594037928097],
  'eta': 1599562982,
  'kwargs': {},
  'task_name': 52,
  'user_id': 72057594037928097},
 {'args': [72057594037928097],
  'eta': 1599563022,
  'kwargs': {},
  'task_name': 53,
  'user_id': 72057594037928097}]
{% endhighlight %}

  #check to make sure ntf is not sent within one week
    kv_key = '%s_%s' % (user_id, days)
    if kv_table.read(kv_key, kv_cfg.kv_ctx_kick_counter_not_log_notification):
        return
send_user_not_log_kick_counter_notification  这个方法里面，如果kv_table.read(kv_key, kv_cfg.kv_ctx_kick_counter_not_log_notification)读出来是1
就表示七天内发过，这次不发



用这个方法把ts改成一个一周前的时间
served = served_cards_info(user_id)
    info = served.get(card_id) or {}
    info['ts'] = ts
    kv_table.hset(user_id, card_id, info,
                  kv_cfg.kv_ctx_homepromo_card_serve)

#### Android 
local_dev.properties里可以配置本地属性文件来控制debug是否打开，revoke premium的方法需要debug==true才能trigger
这个link放到debug页面 https://glowing.com/debug/premium_status 可以revoke premium
RNDebug=false
debugBeta=true
minifyEnabled=false
注意minify必须是false才能是debug起作用



##### ETAD (NOTIFICATION TRIGGER)
{% highlight python %}
	from bryo.disttasks.etaqueue import eta_client
	import time
	eta_client.upsert_task(uid, task_name=52, eta=time.time() + 60 * 3)
{% endhighlight %}

bryo.data.dbval 查看具体的task：
	class EtaTask(object):
	    IOS_FREE_TRIAL_ENDING_IN_3_DAYS = 11
	    IOS_FREE_TRIAL_ENDING_IN_1_DAY  = 12
	    ANDROID_FREE_TRIAL_ENDING_IN_3_DAYS = 21
	    ANDROID_FREE_TRIAL_ENDING_IN_1_DAY  = 22
	    ANDROID_TRANSACTION_RENEW = 23
	    ONGOING_DISCOUNT = 24
	    HAPPY_BIRTHDAY = 25
	    # 25 is for birthday eta task
	    IOS_RENEW_SINGLE_TRANSACTION = 26
	    OFFER_OF_THE_DAY_CYCLE = 27
	    ONGOING_DISCOUNT_V2 = 28
	    ANDROID_TRANSACTION_ACK = 29

	    NOTIFICATION_BLAST  = 30
	    NOTIFICATION_BLAST_WITH_FILTER = 31
	    FLUSH_USER_TIMEZONE = 40

	    PS_NEWCOMER_DISCOUNT_NTF = 50
	    PS_STAGED_DISCOUNT_NTF = 51

	    PS_FREETRIAL_REMINDER = 52

	    PS_REFUND_WINBACK_NTF = 10001


##### stagedDiscountOperation获取WL
{% highlight python %}
	from bryo.app.ps_discount import staged_discount_op as op
	from suso.utils import context
	ctx = {"code_name": "emma", "platform":"ios"}
	uid = 72057594053783289
	with context.LocalContext(ctx):
		stagedDO = op._get_op(uid)
		


{% endhighlight %}
