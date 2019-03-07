---
layout: post
comments: true
title:  "Rate Limiting Using Redis in Python"
date:   2017-05-15 03:24:36
categories: Development
---

Redis is an awesome piece of software, and I am hugely biased towards it. It's very versatile and powerful, could fulfill a wide range of use cases, such as implementing a rate limiter.

One basic pattern for implementing rate limiter is using a counter, and it's explained in Redis official documentation in the [INCR](https://redis.io/commands/incr#pattern-rate-limiter) page.

However, the sample code is implemented in Lua, and I need to use it in Python. And I also  need to drop it into around one hundred concurrent workers, so based on the sample code, I came up with the following implementation:

{% highlight python %}
import time
import redis

class RateLimiter():
    def __init__(self, namespace, uid, limit):
        self.namespace = namespace
        self.uid = uid
        self.limit = limit
        self.redis_conn = redis.StrictRedis(host='localhost', port=6379, db=0)

    def lock(self, namespace, uid, limit):
        # rate limiter never needs to unlock itself after locking
        # due to usage count shouldn't decrease (keep history)
        try:
            key = self.get_key(namespace, uid)
            pipe = self.redis_conn.pipeline()
            pipe.incr(key)
            pipe.expire(key, 1)
            result = pipe.execute()
            if result[0] <= limit:
                return False
            else:
                return True
        except redis.RedisError as e:
            print "Redis Error!" 
            raise

    def get_key(self):
        timestamp = int(time.time())
        return "rate:{0}:{1}:{2}".format(self.namespace, self.uid, timestamp)
{% endhighlight %}

This implementation use pattern `rate:namespace:uid:time_in_second` for the key to differ rate limit counters from each other; use a `lock` method to secure a slot, and expire the key in one second.

This will work for concurrent workers because:

1.  The operation writes ahead with INCR, and since this operation is atomic, thus regardless the number of concurrent workers/processes trying to access the redis concurrently, it will always add up linearly one by one. A check-first-then-add will not work, because after checking other add operations may have already happended.
2. It uses `pipeline` to wrap the INCR and EXPIRE operations in one transaction, thus atomically setting up the key. So in case of errors, there won't be any key lingering around forever.
3. It gets the key by time in the very beginning, and put it into the `key` variable, thus won't change after.

Now in order to use it, just put the actual business logic inside something like a while loop, and keep trying it with sleep interval between each `lock` call until the method returns `True`, such as by doing `time.sleep(random.random() + 0.5)`. Or by telling requester to back off and try again later.

PS: Thanks to my colleagues at Nugit who keep pointing out my mistakes and make this work in the end.

Further Reading: [An alternative approach to rate limiting](https://blog.figma.com/an-alternative-approach-to-rate-limiting-f8a06cf7c94c)

PPS: Thanks David Mnatsakanyan for pointing out the usage of pipeline.
