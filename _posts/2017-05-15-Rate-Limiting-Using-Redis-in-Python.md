---
layout: post
comments: true
title:  "Rate Limiting Using Redis in Python"
date:   2017-05-15 03:24:36
categories: Development
---

Redis is an awesome piece of software, and I am hugely biased towards it. It's very versatile and powerful, could fulfill a wide range of use cases, such as implementing a rate limiter.

One basic pattern for implementing rate limiter is using a counter, and it's explained in Redis official documentation in its [INCR](https://redis.io/commands/incr#pattern-rate-limiter) page.

However, the sample code is implemented in Lua, and I need to use it in Python. And I need to drop it into around one hundred concurrent workers, so based on the sample code, I came up with the following implementation:

``` python
import time
import redis

class RateLimiter():
    def __init__(self, namespace, uid, limit):
        self.namespace = namespace
        self.uid = uid
        self.limit = limit
        self.redis_conn = redis.StrictRedis(host='localhost', port=6379, db=0)

    def lock(self):
        # rate limiter never needs to unlock itself after locking
        # due to usage count shouldn't decrease (keep history)
        try:
            key = self.get_key()
            pipe = self.redis_conn.pipeline()
            count = self.redis_conn.incr(key)
            self.redis_conn.expire(key, 1)
            pipe.execute()

            if count <= self.limit:
                return True
            else:
                return False
        except redis.RedisError:
            # handle error, for example inform a human
            print 'Redis error!'
            raise

    def get_key(self):
        timestamp = int(time.time())
        return "rate:{0}:{1}:{2}".format(self.namespace, self.uid, timestamp)
```

This implementation use pattern `rate:namespace:uid:time_in_second` for the key to differ rate limit counters from each other; use a `lock` method to secure a slot for executing the request, and expire the key in one second.

This will work for concurrent works because:

1.  The operation writes ahead with INCR, and since this operation is atomic, thus regardless the number of concurrent workers/processes trying to access the redis concurrently, it will always add up linearly one by one. A check-first-then-add will not work, because after checking other add operations may have already happended.
2. It uses `pipeline` to wrap the INCR and EXPIRE operations in one transaction, thus atomically setting up the key. So in case of errors, there won't be any key lingering around forever.
3. It gets the key by time in the very beginning, and put it into the `key` variable, thus won't change after.

Now in order to use it, just put the actual business logic inside something like a while loop, and keep trying it with sleep interval between each `lock` call, such as by doing `time.sleep(random.random() + 0.5)`. Or by telling requester to back off and try again later.

PS: Thanks to my colleagues at Nugit who keep pointing out my mistakes and make this work in the end.

