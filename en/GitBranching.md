# Git Branching

## Semantic Versioning
QGC uses semantic version for the version numbers associated with its releases. Semantic Versioning is a 3-component number in the format of ```vX.Y.Z```, where:

* X stands for a major version
* Y stands for a minor version
* Z stands for a patch

## Stable Builds
The current stable build represents the highest quality build available for QGC. Stable builds are built from a Stable branch. The latest stable branch will be in the format: ```Stable_VX.Y```. There will also be a git tag on that branch in the format ```vX.Y.Z```. This tag indicates the point in the repo which represents the source code the released stable was built from.

### Patch releases
Patch releases increment the patch version number only. A patch release represents fixes made to stable which were important enough to require an update to Stable. And also they must be safe enough to be made against stable such that stable continues to maintain high quality.

### Patch - Development stage
As issues are reported against stable if the fix meets the criteria discussed above the development of this fix is done on the current stable branch. These fixes continue to queue up in the stable branch until a patch release is made.

While patch development is being done in stable, those changes must also be brought over to master either through cherry pick or separate pulls.

### Patch - Release stage
At the point where the decision is made to do a patch release only a new tag for release is created. New branches are not created for patch releases.

## Daily Builds

### Development stage
Daily builds are built from master and this is where all new major feature development happens. The state of master represents the either next minor point release or possibly a new major version release depending on the level of change.

There are no git tags associated with a released daily builds. The released daily build will always match repo HEAD.

### Release stage
When the decision is made to release a new major/minor version the master branch tends to go through an intial lockdown mode. This is where only important fixes for the release are accepted as pull requests. During this time new features are not allowed in master.

At some point hopefully the level of fixes associated with the release slows down to a low level. At that point a new stable branch is created and now master can move forward again as the latest and greatest. Then for some small (hopefully) amount of time fixes continue in the stable branch until it is deemed ready to release. At that point the branch is tagged with the new version tag.

## Custom Builds
A proposed strategy for branching on custom builds can be found [here](custom_build/GitBranching.md).
