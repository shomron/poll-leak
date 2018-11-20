## Overview

The purpose of this repository is to reproduce the goroutine leak found in [`wait.PollUntil`](https://github.com/kubernetes/apimachinery/blob/1b0702fe2927f82126da9c2ecf567f14676f6072/pkg/util/wait/wait.go#L289-L291), as exercised through typical use of a `SharedInformer`.

## Build

```bash
dep ensure
go run main.go
```

You should see the number of goroutines monotonically increase as we continue to call `WaitForCacheSync()`.
This is because there is an implicit requirement for the caller to close the stopCh passed to `WaitForCacheSync`, even upon
successful completion.
