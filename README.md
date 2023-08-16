# drone-testing
Dummy repo to test drone configuration

## Promoting a build

Once a build is ready to be promoted to CI (aka, basic linting checks have passed and it has been reviewed) the 
following command can be used to promote it to the drone ci target:

```sh
drone build promote jakefhyde/drone-testing -p TESTCASES=<TESTCASES> <BUILD_NUMBER> ci
```

- `TESTCASES` will be rendered as an environment variable which is set under `DAPPER_ENV`, which is then 
utilized in the `scripts/e2e` to determine which tests to run.
- `BUILD_NUMBER` is the drone build number.

The current expected workflow will be to comment `/test --unit=abc --e2e=xyz` on the pull request, when an issue is 
ready to test. Promotion is required EACH time due to drone limitations, and it is my personal recommendation that if 
this behavior is determined to be unsuitable, an alternative solution such as k8s' prow/tide which is known to work with
this format, or alternative solutions such as github actions or dagger should be explored further. Additionally, it 
would be possibly to bot out some of this behavior, to automatically promote to ci if it has been previously promoted 
and pre-checks (linting, etc.) have passed.