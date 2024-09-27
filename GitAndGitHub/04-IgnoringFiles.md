# Ignoring Files

Git can be told to ignore (not track) files using the **.gitignore** file.

Some files that should probably be ignored:

* Credentials, API keys, other information that should be kept secret
* Operating system files
* Logs
* Dependencies

The .gitignore file is conventionally created in the root of the repo directory. It contains patterns describing which files to ignore.

Example patterns:

* .DS_Store - will ignore anything named '.DS_Store'
* FolderName/ - will ignore the directory named 'FolderName'
* \*.log - will ignore any files with the .log extension (where the wildcard characer '*' matches anything before the extenstion)


A useful site with .gitignore templates for different types of projects is [**gitignore.io**](https://www.toptal.com/developers/gitignore/)




