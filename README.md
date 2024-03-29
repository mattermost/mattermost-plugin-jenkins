# Disclaimer

**This repository is community supported and not maintained by Mattermost. Mattermost disclaims liability for integrations, including Third Party Integrations and Mattermost Integrations. Integrations may be modified or discontinued at any time.**

# Mattermost Jenkins Plugin

[![Build Status](https://img.shields.io/circleci/project/github/mattermost/mattermost-plugin-jenkins/master.svg)](https://circleci.com/gh/mattermost/mattermost-plugin-jenkins)
[![Code Coverage](https://img.shields.io/codecov/c/github/mattermost/mattermost-plugin-jenkins/master.svg)](https://codecov.io/gh/mattermost/mattermost-plugin-jenkins)

A Jenkins plugin to interact with jobs and builds, with slash commands in Mattermost. 

Originally developed by [Wasim Thabraze](https://github.com/waseem18).

<img src="https://github.com/mattermost/mattermost-plugin-jenkins/blob/master/screenshots/2019-08-07_04-14-58%20(3).gif" alt="drawing" width="1000"/>

By extending Mattermost with the Jenkins integration, your team will enjoy:

* Improved collaboration. Keep your team in sync with your latest build by automatically routing notifications to specific Mattermost Channels to display information about when builds start, whether they’re successful, or if exceptions occurred.
* Increased productivity. Start Jenkins job builds and get logs and artifacts—all without leaving the Mattermost platform.
* Centralized workflows. Consolidate builds into one channel to keep a timeline of key Jenkins builds in one place and avoid hunting across builds. 

Integrating Mattermost with Jenkins lets you keep your whole DevOps team on top of recent builds and any failures that may occur. With the Mattermost Jenkins plugin, control over Jenkins functions is always at the fingertips of your entire DevOps team.

By using the Mattermost Jenkins integration, your team will spend less time switching over to Jenkins, logging in, and finishing builds. 

## Plugin for Jenkins Server

For a Jenkins integration that sends webhook notifications from Jenkins to Mattermost as post-build actions, see this repository: https://github.com/jenkinsci/mattermost-plugin This is useful for sending Jenkins notifications to specific channels.  For example - whenever a new Release of your software is cut, it should notify the #Release channel in Mattermost.  Using the Jenkins post-build action plugin lets you notify the right channels about success/failures. 

## Features

This plugin enables you to interact with jobs via slash commands in Mattermost. The supported slash commands are listed below:

#### Connect and disconnect with Jenkins server
* __Connect to Jenkins server__ - `/jenkins connect username APIToken` - Connect your Mattermost account to Jenkins.
* __Disconnect from Jenkins server__ - `/jenkins disconnect` - Disconnect your Mattermost account from Jenkins.

#### Interact with Jenkins jobs
* __Create a Jenkins job__  - `/jenkins createjob` - Create a Jenkins job using contents of `config.xml`. The slash command opens an interactive dialog for the user to input the job name and paste the contents of `config.xml`.
* __Trigger a Jenkins job__ -  `/jenkins build jobname` - Trigger a build for the given job. If the job accepts parameters, an interactive dialog pops up for the user to input the required parameters.
  
  * If the job resides in a folder, specify the job as `folder1/jobname`. Note the slash character.
  * If the folder name or job name has spaces in it, wrap the jobname in double quotes as `"job name with space"` or `"folder with space/jobname"`.
  * Follow similar pattern for all commands which takes jobname as input.

* __Abort a build__ - `/jenkins abort jobname <build number>` - Abort the given build of the specified job. If `build number` is not specified, the command aborts the last build of the job.
* __Enable a job__ -  `/jenkins enable jobname` - Enable a given Jenkins job.
* __Disable a job__ -  `/jenkins disable jobname` - Disable a given Jenkins job.
* __Delete a job__ - `/jenkins delete jobname` - Delete a given job.
* __Get artifacts__ -  `/jenkins get-artifacts jobname` - Get artifacts of the last build of the given job.
* __Get test results__ -  `/jenkins test-results jobname` - Get test results of the last build of the given job.
* __Get build log__ - `/jenkins get-log jobname <build number>` - Get log of a given build of the specified job as a file attachment to the channel. If `build number` is not specified, the command fetches the log of the last build of the job.

#### Interact with Plugins
* __List of installed plugins__ - `/jenkins plugins` - Get a list of installed plugins on Jenkins server along with the version of the plugin.

#### Adhoc commands
* __Safe restart Jenkins server__ - `/jenkins safe-restart` - Safe restart the Jenkins server.
* __Find connected Jenkins account__ -  `/jenkins me` - Display the connected Jenkins account.
* __Get help__ - `/jenkins help` - Find help related to the syntax of the slash commands.

### Installation
1. Install the plugin
    1. Download the latest version of the plugin from the GitHub releases page
    2. In Mattermost, go to **System Console -> Plugins -> Management**
    3. Upload the plugin
1. Enter Jenkins server URL
    1. Go to the **System Console -> Plugins -> Jenkins**
    2. Set the Jenkins server URL along with the protocol. Example: http://jenkins.example.com, https://jenkins.example.com
    3. Save the settings
1. Generate an at rest encryption key
    1. Go to the **System Console -> Plugins -> Jenkins** and click "Regenerate" under "At Rest Encryption Key"
    2. Save the settings
1. Enable the plugin
    1. Go to System Console -> Plugins -> Management and click "Enable" underneath the Jenkins plugin
1. Test it out
    1. In Mattermost, run the slash command `/jenkins connect <Jenkins Username> <Jenkins API Token>`

### Development

This plugin contains both a server and web app portion. Read our documentation about the [Developer Workflow](https://developers.mattermost.com/integrate/plugins/developer-workflow/) and [Developer Setup](https://developers.mattermost.com/integrate/plugins/developer-setup/) for more information about developing and extending plugins.

### Releasing new versions

The version of a plugin is determined at compile time, automatically populating a `version` field in the [plugin manifest](plugin.json):
* If the current commit matches a tag, the version will match after stripping any leading `v`, e.g. `1.3.1`.
* Otherwise, the version will combine the nearest tag with `git rev-parse --short HEAD`, e.g. `1.3.1+d06e53e1`.
* If there is no version tag, an empty version will be combined with the short hash, e.g. `0.0.0+76081421`.

To disable this behaviour, manually populate and maintain the `version` field.

### FAQ
**How do I generate API Token for a given Jenkins user?**

Since Jenkins 2.129 the API token configuration has changed:

You can now have multiple tokens and name them. They can be revoked individually.

 1. Log in to Jenkins.
 2. Click you name (upper-right corner).
 3. Click **Configure** (left-side menu).
 4. Use "Add new Token" button to generate a new one then name it.
 5. You must copy the token when you generate it as you cannot view the token afterwards.
 6. Revoke old tokens when no longer needed.

Before Jenkins 2.129: Show the API token as follows:

1. Log in to Jenkins.
2. Click your name (upper-right corner).
3. Click **Configure** (left-side menu).
4. Click **Show API Token**.

_Source : https://stackoverflow.com/a/45466184/6852930_

### License
MIT
