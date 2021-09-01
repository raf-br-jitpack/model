1. Every library used in a K/N project must be K/N itself. For this reason the first attempt (a
   multimodule server with a dedicated module for models, which is supposed to be shared with K/N)
   works with JVM projects but fails for K/N: https://github.com/raf-br-jitpack/server_old_ignore
1. 2nd attempt creates a dedicated project which is K/N itself:
   https://github.com/raf-br-jitpack/model
1. This module builds multiple targets and uses the `maven-publish` plugin which integrates nicely
   with K/N. This allows JitPack to build ad-hoc 'releases'.
1. It may be possible to have a single multimodule project, in which only one module is K/N enabled.
1. Releases can be seen here: https://jitpack.io/#raf-br-jitpack/model
1. JitPack builds take ~40 seconds, but seem to be somewhat unreliable, e.g.
   https://jitpack.io/com/github/raf-br-jitpack/model/3681783866/build.log
1. When a build for a specific commit fails, there seems to be no way of re-triggering it, the only
   way I found was to push again, for instance a completely empty commit, which is a nuisance. I
   will ask JitPack about redoing failed builds, maybe there is a better way.
1. Once a commit is built by JitPack the first time, it is remembered and served very fast for later
   builds of dependants.
1. Using such a JitPack based build is easy for JVM or Android, e.g.
   https://github.com/raf-br-jitpack/server/commit/c5ff569a97636dc67224acf7d2ab26d330c80ce0 or
   https://github.com/raf-br-jitpack/shared/commit/29f9adb06be6aa17b0acc219718b305cf2aac403
1. iOS is more difficult. Shocker. K/N iOS targets can't be built on anything other than a Mac
   (https://jitpack.io/com/github/raf-br-jitpack/model/25e11e6a1f/build.log), which the public
   JitPack doesn't do, they use Linux nodes. I.e. it is impossible to do it like this.
1. Just to test, build https://github.com/raf-br-jitpack/model locally (on a Mac) and install to the
   local Maven repository:
   ```shell
   ./gradlew publishToMavenLocal
   ```
1. Now this branch https://github.com/raf-br-jitpack/shared/tree/local can be used and issuing
   ```shell
   ./gradlew createXCFramework
   ```
   will build the framework for iOS and the sample app will work fine.
1. Allegedly it is possible to use JitPack with a self-hosted Gitlab (https://jitpack.io/docs/FAQ/,
   search for 'self-hosted GitLab') but there is little documentation about it, some here:
   https://jitpack.io/docs/PRIVATE/#self-hosted-git
1. However, it seems like even self-hosted GitLabs use the external JitPack by default, which means
   it still won't be able build for iOS.
1. It seems to be possible to have a self-hosted JitPack: https://jitpack.io/enterprise
1. Once this is up and running, it should be possible to build for iOS if the private GitLab has a
   Mac node which the private JitPack can use.
