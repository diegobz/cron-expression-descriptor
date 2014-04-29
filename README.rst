# After creating a project in Transifex.com, i.e.:
# https://www.transifex.com/projects/p/cron-expression-descriptor/
# The following steps will describe how to setup the transifex-client using a
# .NET project


# First let's install the transifex-client

[diegobz@localhost transifex]$ pip install git+https://github.com/transifex/transifex-client.git
Downloading/unpacking git+https://github.com/transifex/transifex-client.git
  Cloning https://github.com/transifex/transifex-client.git to /var/folders/0j/r84d5pb51y965q2dpx3s831h0000gn/T/pip-yy6pps-build
  Running setup.py (path:/var/folders/0j/r84d5pb51y965q2dpx3s831h0000gn/T/pip-yy6pps-build/setup.py) egg_info for package from git+https://github.com/transifex/transifex-client.git

    warning: no files found matching '*' under directory 'docs'
Installing collected packages: transifex-client
  Running setup.py install for transifex-client

    warning: no files found matching '*' under directory 'docs'
    warning: build_py: byte-compiling is disabled, skipping.

    changing mode of build/scripts-2.7/tx from 644 to 755
    warning: install_lib: byte-compiling is disabled, skipping.

    changing mode of /Users/diegobz/bin/tx to 755
Successfully installed transifex-client
Cleaning up...


# Now clone the reporitory we want to integrate with Transifex.

[diegobz@localhost transifex]$ git clone https://github.com/diegobz/cron-expression-descriptor.git
Cloning into 'cron-expression-descriptor'...
remote: Reusing existing pack: 751, done.
remote: Total 751 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (751/751), 313.31 KiB | 192.00 KiB/s, done.
Resolving deltas: 100% (427/427), done.
Checking connectivity... done.

[diegobz@localhost transifex]$ cd cron-expression-descriptor


# First transifex-client command

[diegobz@localhost cron-expression-descriptor]$ tx init
Creating .tx folder...
Transifex instance [https://www.transifex.com]:
Creating skeleton...
Creating config file...
/Users/diegobz/.transifexrc not found.
No entry found for host https://www.transifex.com. Creating...
Please enter your transifex username: diegobz
Password:
Updating /Users/diegobz/.transifexrc file...
Done.


# This repository already has some translations. We want to keep them and also
# be able to add more translations using Transifex for the content available in
# the 'Resources.resx' file. This file will map to a file in Transifex also
# called 'resources'.

[diegobz@localhost cron-expression-descriptor]$ ls CronExpressionDescriptor/Resources.*
CronExpressionDescriptor/Resources.Designer.cs    CronExpressionDescriptor/Resources.resx
CronExpressionDescriptor/Resources.es.resx        CronExpressionDescriptor/Resources.tr.Designer.cs
CronExpressionDescriptor/Resources.no.resx        CronExpressionDescriptor/Resources.tr.resx
CronExpressionDescriptor/Resources.pt-br.resx


# We we are setting up a resource (source file) in Transifex. For more info on
# all options used refer to http://docs.transifex.com/developer/client/set

[diegobz@localhost cron-expression-descriptor]$ tx set --auto-local -t RESX -r cron-expression-descriptor.resources --source-lang en --source-file CronExpressionDescriptor/Resources.resx 'CronExpressionDescriptor/Resources.<lang>.resx'
Only printing the commands which will be run if the --execute switch is specified.

tx set --source -r cron-expression-descriptor.resources -l en CronExpressionDescriptor/Resources.resx

tx set -r cron-expression-descriptor.resources -l es CronExpressionDescriptor/Resources.es.resx
tx set -r cron-expression-descriptor.resources -l no CronExpressionDescriptor/Resources.no.resx
tx set -r cron-expression-descriptor.resources -l pt-br CronExpressionDescriptor/Resources.pt-br.resx
tx set -r cron-expression-descriptor.resources -l tr CronExpressionDescriptor/Resources.tr.resx

[diegobz@localhost cron-expression-descriptor]$ tx set --auto-local -t RESX -r cron-expression-descriptor.resources --source-lang en --source-file CronExpressionDescriptor/Resources.resx 'CronExpressionDescriptor/Resources.<lang>.resx' --execute
Updating source for resource cron-expression-descriptor.resources ( en -> CronExpressionDescriptor/Resources.resx ).
Setting source file for resource cron-expression-descriptor.resources ( en -> CronExpressionDescriptor/Resources.resx ).
Updating file expression for resource cron-expression-descriptor.resources ( CronExpressionDescriptor/Resources.<lang>.resx ).


# Language codes in Transifex are handled in the ll_CC format. Bellow we are
# adding a map so 'pt-br' gets pushed to Transifex as 'pt_BR'

[diegobz@localhost cron-expression-descriptor]$ echo "lang_map = pt_BR: pt-br" >> .tx/config


# Taking a look the generated config file for the repository.

[diegobz@localhost cron-expression-descriptor]$ cat .tx/config
[main]
host = https://www.transifex.com

[cron-expression-descriptor.resources]
file_filter = CronExpressionDescriptor/Resources.<lang>.resx
source_file = CronExpressionDescriptor/Resources.resx
source_lang = en
type = RESX

lang_map = pt_BR: pt-br


# Pushing the existing translation files plus the source file to Transifex.

[diegobz@localhost cron-expression-descriptor]$ tx push -s -t
Pushing translations for resource cron-expression-descriptor.resources:
Pushing source file (CronExpressionDescriptor/Resources.resx)
Resource does not exist.  Creating...
Pushing 'tr' translations (file: CronExpressionDescriptor/Resources.tr.resx)
Pushing 'pt_BR' translations (file: CronExpressionDescriptor/Resources.pt-br.resx)
Pushing 'es' translations (file: CronExpressionDescriptor/Resources.es.resx)
Pushing 'no' translations (file: CronExpressionDescriptor/Resources.no.resx)
Done.


# Now that the files are pushed to Transifex, it's possible to see 4 languages
# already translated in the system, because the translations were already
# available in the reporitory. You can check it out at
# https://www.transifex.com/projects/p/cron-expression-descriptor/
# Once you add new languages in Transifex and translate stuff, then the
# `tx pull` command can be issued as follows to get new translations in the
# repository. In the following example we have added the German language in
# Transifex for the project.

[diegobz@localhost cron-expression-descriptor]$ tx pull -a
Pulling translations for resource cron-expression-descriptor.resources (source: CronExpressionDescriptor/Resources.resx)
 -> tr: CronExpressionDescriptor/Resources.tr.resx
 -> es: CronExpressionDescriptor/Resources.es.resx
 -> pt_BR: CronExpressionDescriptor/Resources.pt-br.resx
 -> no: CronExpressionDescriptor/Resources.no.resx
 Pulling new translations for resource cron-expression-descriptor.resources (source: CronExpressionDescriptor/Resources.resx)
 -> de: CronExpressionDescriptor/Resources.de.resx
Done.


# Once everything is setup the only commands that need to be issued are:
 - 'tx push -s' to push new content to be translated.
 - 'tx pull -a' to pull translations.
