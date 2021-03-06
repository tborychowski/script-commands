# Contributing

Thank you for your interest in contributing to Raycast Script Commands! Here you will find simple guidelines that can help you with getting started.

## Guidelines

### Folder structure

Try to bundle scripts that are related in a directory / sub-directory. Avoid having generic folders with lots of different commands. For example instead of having one `media` directory that contains integrations with different services, it's better to create sub-directories for each service:

```markdown
. commands
└─ media
   ├─ spotify
   ├─ apple-music
   └─ youtube
```

Reasoning behind it: To avoid automatically including scripts that people may not be interested in. E.g. if you're using Spotify scripts, there is a lower chance you will need to access Apple Music.

Images should go into dedicated `images` folder:

```markdown
. commands
└─ media
   └─ spotify
      └─ images
         └─ spotify-logo.png
      └─ spotify-next-track.applescript
      └─ spotify-prev-track.applescript
```

### File naming convention

Use dash-case format for script files and directories, and use proper file extensions: Applescript should be `.applescript`, Swift should be `.swift`, Bash should be `.sh`, etc.
Example: `spotify-next-track.applescript`

### Metadata convention

- **Title:** Raycast's UI adopts title-cased strings for all command titles as per Apple's Human Interface Guidelines. Please make sure your command title follows this pattern to look good between other commands.
- **Mode:** Use the `silent` mode for commands that are instant, e.g. `Toggle Hidden Files`. Use the `compact` mode for long-running tasks, e.g. some networking requests. Use the `fullOutput` mode for commands that print more information, e.g. output some file content. And use the `inline` mode for dashboard items, e.g. `Current Weather`.
- **Package Name:** While `packageName` is an optional parameter and if it's missing Raycast will derive it from the directory name, it is required in this repository to improve portability. Make sure to always provide it in your script commands.

### Scripts that require additional modification

1. Ensure that comments include instructions on how to start using the script. E.g. you might need to provide API token, username or tweak parameters.
2. Add `.template.` to the file name for scripts that need modifications. Then scripts won't be automatically parsed by Raycast and people who want to use it will need to copy the file and remove `.template.` part.

Example: `github-notifications.template.sh`

*NOTE:* This might change as soon as we introduce a better way to provide parameters / environmental variables.

### Scripts that require installation of dependencies

First, ask yourself if you can build the Script Command without any dependencies. Less or no dependencies make it easier for others to adopt your command. If the script would become too complex without using a dependency, follow these guidelines:

1. At the top of the file add a comment section explicitly stating the dependency and how to install it. Example:
   ```
   #!/bin/bash

   # Dependency: This script requires `jq` cli installed: https://stedolan.github.io/jq/
   # Install via homebrew: `brew install jq`

   # @raycast.schemaVersion 1
   # @raycast.title Prettify JSON from Clipboard
   ...
   ```


2. Make sure you have code that handles missing dependency. Example:
   ```bash
   if ! command -v download &> /dev/null; then
	     echo "download command is required (https://github.com/kevva/download-cli).";
	     exit 1;
   fi
   ```

### Bash profiles and environmental variables

All Script Commands are executed in a non-login shell to avoid additional information loaded from profiles that aren't relevant to Raycast. With an argument after a shebang, you can run a script in a login shell, e.g. `#!/bin/bash -l`. We don't allow Script Commands that make use of this feature in this repository. Mainly to guarantee easy portability, explicit injection of information and best performance. 

*NOTE:* We will add support for environmental variables in Raycast and keep track of it in [this issue](https://github.com/raycast/script-commands/issues/77).