## Intent

Demonstrate how Python closures bind names, not values, and why loop variables combined with closures often produce unexpected results.

### Explanation

- This example exposes the **late binding rule** directly.

- It shows how:
    - scope
    - lifetime
    - name resolution  
        interact in a way that surprises even experienced programmers.

- It reinforces that **closures are not snapshots**; they are deferred name lookups.

