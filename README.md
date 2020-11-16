# gitlab-ci-tests

Repository for simulating workflow for PR testing on gitlab:

                         Github (gitlab-ci-tests)
                                    |
                                    |                      <- gh-gl-sync pushes branches
                                    |
                         Gitlab (gitlab-ci-tests)
                         |                    |
                         |                    |            <- multi-project triggers
                         |                    |
     Gitlab (test-downstream)               Gitlab (test-downstream)
           pr_pipeline                          master_pipeline



     Gitlab (test-downstream)               Gitlab (test-downstream)   <- triggered by pushing
       direct_push pipeline                    direct_mr_pipeline         directly to downstream
                                                                          and/or creating MR

All secrets are stored in Gitlab (gitlab-ci-tests) and only exposed on protected
branches, then they're propagate from the trigger jobs via variables.

The reason we cannot store secrets on the downtream repo and then protect those branches,
(e.g. "github/master") is that nothing prevents a malicious user from changing this
project `pr_pipeline` from:

```
  trigger:
    project: scott/test-downstream
    strategy: depend
```

to:

```
  trigger:
    project: scott/test-downstream
    strategy: depend
    branch: github/master
```

According to gitlab, there's no way to know about the protected status of branches
on the downstream project.

Github (gitlab-ci-tests) has a custom package repository which is cloned and used
to allow us to force full hashes to change in a controlled and predictable way,
which lets us test whether the use of the per-PR mirrors is working as expected.
