# Namespace Lock & Unlock
Lock the API for a part of your organization. If a parent is locked, the descendants also. If both
locked, just first unlock parent and then the descedant.

You must use:
```sh
vault namespace lock
```

A unlock key will be returned. It must be keep for future unlocking.

To unlock:
```sh
vault namespace unlock
```