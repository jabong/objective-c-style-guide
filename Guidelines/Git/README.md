# Rocket Internet Guideline: Source Code Management with Git

## Keeping files out

Many development tools, like IDEs, produce temporary or local meta data files during their co-existence with the development and project. 
Most of these files should not be shared among various development environments and therefor kept  out of the central source code management. 
In Git a dedicated file [`.gitignore`](http://git-scm.com/docs/gitignore) maintains syntactical patterns matching on the names of those files that should be excluded of the code versioning.

Github provides a [public repository](https://github.com/github/gitignore) of templates for `.gitignore` files. Among others there is a [dedicated template for Objective-C](https://github.com/github/gitignore/blob/master/Objective-C.gitignore) combines an extensive set of file name patterns already in the preferred format and 
suitable for most Objective-C projects.

Also do we have another custom template called [`gitignore`](gitignore) in this directory of the repository that this guideline recommends recommends to use as a basis to produce a `.gitignore` file for either code project managed with Git.