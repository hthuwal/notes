### Most Complex Solution

## Write

1. GetKey
    a. Cace Miss: NoOp
    b. Cace Hit: Mark Key as stale

2. Take `UpdateKeyLock` TTL
    a. If Lock is not available, wait for it for lockWaitTimeout
        - If Timeout: Return Error
        - Else you got the lock. Go to b.
    b. If Lock is available:
        - Update DB
        - Delete the Key
        - Release the Lock

### Notes:
- No fundamental difference between inter pod and intra pod concurrency.
- High Lock Contention when there are multiple writes to the same key.
    - Not new: DB Lock contention being pushed up to the application layer.

## Read

Assume 10000 concurrent reads for a key, and 100 pods. So per pod 100 request.

0. SingleFlight Coalesces: [ This is to avoid cache stampede within a pod. 100 become 1] 
1. GetKey
    a. Cace Hit
        - NotStale: Return Value
        - Stale:
            - EC: Return Value
            - SC: Go to step 2
    b. Cace Miss: Go to step 2

2. SetNX ReadingKey TTL [ This is to avoid cache stampede across pods. ] 100 calls will reach here (1 per pod)
    a. SetNX Failure
        - Make a DB call and return value.
    b. SetNX Success:
        - Grab `UpdateKeyLock` TTL
            a. If Lock is not available, get value from DB and return.
            b. If Lock is available:
                - Get Value from DB
                - Set Key in Cache
                - Release the Lock
                - Return value
        - DEL ReadingKey




