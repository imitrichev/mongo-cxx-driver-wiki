## How to do a release (legacy driver)

### BUMP commit (to remove -pre)

This commit essentially removes the -pre (1.0.0-rc79-pre -> 1.0.0-rc79) and creates what is considered the final commit in the release.

 - Update the repository README.md (changing version numbers where appropriate)
 - Update top level SConstruct's mongoclientVersion string
 - Update etc/doxygen/config's PROJECT_NUMBER string
 - Commit with message "BUMP legacy-x.y.z[-rcq]"

### Apply tags

 - `git tag legacy-x.y.z[-pre]`

### POST commit (increment version and add -pre)

This commit prepares the branch for new commits towards a future release (1.0.0-rc79 -> 1.0.0-rc80-pre)

 - Update top level SConstruct's mongoclientVersion string
 - Update etc/doxygen/config's PROJECT_NUMBER string
 - Commit with message "POST legacy-x.y.z[-rcq]-pre" (remember to increment either x, y, z, or q to new version)

### Push it to github

 - `git push origin master`
 - `git push origin master --tags`

### Release on GitHub

 - Hit up the [github releases page](https://github.com/mongodb/mongo-cxx-driver/releases)
 - Draft and publish a new release against the tag you just pushed

### Release on Jira

 - Go [here](https://jira.mongodb.org/plugins/servlet/project-config/CXX/versions)
 - Click the cog next to the version you are about to release and select "Release"
 - Follow the dialogs/wizards and whatnot

### Generate and publish documentation

 - Clone the [apidocs repo](https://github.com/mongodb/apidocs)
 - run python2 build.py cxx
 - `git add . && git commit -m 'CXX legacy-x.y.z[-rcq]' && git push origin master`

### Write an e-mail

 - Send it to mongodb-announce@googlegroups.com AND mongodb-user@googlegroups.com
 - Template

   > The C++ Driver Team is excited to announce the availability of the legacy-1.0.0 release of the Legacy C++ Driver.

   > This release is the new stable release of the legacy driver and deprecates the 26compat release stream.

   > The legacy release stream diverges from the 26compat release stream in many ways, some of which will require source level changes to existing programs, so it is not a drop in replacement for 26compat. We are in the process of producing a comprehensive breaking changes document, but it is known to be incomplete. Breaking changes not listed on that page should be reported in the JIRA CXX project.

   > You can obtain the driver source from GitHub, either under the legacy-1.0.0 tag or from the releases page. If you are upgrading from an earlier release changes to your build process and source code may be necessary.

   > Please feel free to post any questions to the mongodb-user mailing list.

   > Thanks,
The C++ Driver Team (Adam, Andrew, Jason, Samantha, Tyler)