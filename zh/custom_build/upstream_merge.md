# Keeping your custom build up to date

## Repository Setup

* Create a new repository from main QGC repo. Do not clone, create a new repo, initializing it from the main QGC repo.
* You can now clone the above repo to do your work in and create pull requests from.
* In your clone create a remote called 'mavlink' which points back to the main QGC repo. 
  *     git remote add mavlink https://github.com/mavlink/qgroundcontrol.git

## Upstream Merge

We call the process of updating your custom build to the latest QGC bits and 'Upstream Merge'. Here is an example of how to do it:

* First make sure your local master is up to date with your own repos master.
* Create a branch to make all the changes to: 
  *     git checkout -b UpstreamMerge

* Pull in the latest bits from QGC: 
  *     git pull mavlink master
  
  * You'll get an editor to update merge comments. They are fine, just ```:q``` to exit.
* Now you need to update the resources in your custom build: 
  *     cd custom
  
  *     python updateqrc.py

* Build it all to make sure there are no problems.
* You are now done. You can submit that as a Pull against your repo or however you want to get the changes into your main repo.

Note: This assume your custom build is based off of QGC master. If it is based off of a Stable branch just replace master with the stable branch name.