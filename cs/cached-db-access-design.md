# Cached Entity DB Access

The following approach is a good balance between complexity and consistency levels. We will get read committed consistency with relatively low complexity.

## Write Operation

> The lock TTL must be strictly greater than max(DB write latency) + max(cache write latency)

> U0 sets a small TTL on the key so it expires if the writer crashes. But:
> If the TTL is too small, it expires before U2 completes (even without a crash), causing a cache miss window that forces lock-failed readers to hit the DB.
> If it is too large, crash recovery takes longer than necessary.

Take `UpdateKeyLock` TTL

1. If Lock is not available, wait for it for `lockWaitTimeout`
    - If Timeout: Return Error
    - Else you got the lock. Go to step 2.
2. If Lock is available:
    - U0: Set TTL of key to small value. [In case of crash key will be expired and read will do refresh.] `TTL = min(locktimeout, keytimeout)`
    - U1: Update DB
    - U2:
        - Update the key in cache. [Better for hit rate.]
            - Mutators return type and data must be consistent as loader's return type.
                - If returns nothing, delete the key in cache.
    - Release the Lock.

### Notes:
- No fundamental difference between inter pod and intra pod concurrency.
- High Lock Contention when there are multiple writes to the same key.
    - Not new: DB Lock contention being pushed up to the application layer.

## Read Operation

Assume 10000 concurrent reads for a key, and 100 pods. So per pod 100 request.

1. SingleFlight Coalesces: [This is to avoid cache stampede within a pod. 100 become 1]
2. GetKey
    - Cache Hit: Return Value
    - Cache Miss: Go to step 3
3. Grab `UpdateKeyLock` TTL [Max concurrent refresh = #pods]
    - If Lock is not available:
        - R0: Check cache again. If found return value.
        - If miss, Get value from DB and return.
    - If Lock is available:
        - R0: Check cache again. If found return value.
        - R1: Get Value from DB
        - R2: Set Key in Cache
        - R3: Release the Lock
        - Return value

---

## Notes

**Writer Starvation**

- Readers can hold the lock in a tight chain during a cache miss storm, causing writers waiting on `lockWaitTimeout` to fail.
- Use a write-preferring lock, or treat write-lock failures as retriable with backoff rather than hard errors.

---

**Read path: N-1 pods hit DB on lock failure**

- When a popular key misses, every pod that fails to acquire the lock falls back directly to DB — up to `(pods - 1)` concurrent DB reads per miss event.
- This is correct for read-committed but effectively bypasses the cache for those requests during that window; document it as a known trade-off.

### Future Improvements:
- Add negative caching to avoid DB calls when key is not found. This will prevent DDOS attacks.


