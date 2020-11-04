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

Github (gitlab-ci-tests) has a custom package repository which is cloned and used
to allow us to force full hashes to change in a controlled and predictable way,
which lets us test whether the use of the per-PR mirrors is working as expected,
which seems to be the case.
