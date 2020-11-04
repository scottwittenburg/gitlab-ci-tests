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
