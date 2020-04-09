# Git Branching Strategy For Custom Builds
You are certainly welcome to create whatever setup you want for your custom builds. That said the further you stray from layering on top of the standard QGC git strategy the more problems you are likely to have.

## Simplest strategy
The simplest thing to do is to follow along with QGC branching mechanism as well as timing your releases to fall after QGC releases depending on type. This means you release your new builds only after upstream QGC releases it's own builds. I'll discuss how to do out of band release later in the document.

## Daily Builds

### Development stage
Your custom daily builds are built from master and this is where all new major feature development happens. You keep your custom master up to date with QGC master on a frequent basis. It is important to not lag behind on this. If you lag behind then you may end up being surprised by new upstream features which may cause you problems and suddenly a lot of work to react to. Or maybe you need some changes in these new QGC features to make them work for you. If you don't let QGC devs know soon enough, it may end up being too late to get things changed.

## Stable builds
### Release stage
When upstream QGC does a point release you can now also do a point release. You then follow the process of stablization, branching and tagging as described in the upstream docs. Your release branch should be created from the upstream stable branch and only be synced with the that upstream stable branch to bring across patches. You can apply your own patches to your code int that branch as well as needed.

### Patch releases
Your custom patch releases should be build from your own stable branches. Make sure again to keep your stable branch in sync with upstream stable. If you don't do that then you will miss fixes. Since upstream stable is always at high quality you can release your own patches based on upstream stable HEAD at any time you want. You don't need to wait until QGC releases it's next patch release.

### Out Of Band Release
An "Out of band" release is one where you want to release some of your next features prior to upstream QGC doing a point release. In this case you create the branch for your own point release off of your latest stable branch prior to doing any development. You then do your new feature development in that branch. You should continue to keep this branch in sync with the associated upstream stable branch. Then when you are ready to release you just go through the normal tagging release process.

## Never do this!
You should never release any of your custom stable builds based on upstream QGC master. If you do you will have no idea of the quality of that release since although upstream daily build quality is fairly high it is not as good as the upstream stable quality.