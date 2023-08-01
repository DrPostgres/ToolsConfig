ToolsConfig
===========

The ToolsConfig Specification provides a very simple, lightweight, and elegant
method to remove the clutter in software projects caused by various tools'
configuration files and directories.

The Problem
-----------

There is a prolifiration of developer tools, and every tool requires its
configuration file be stored in the project. This leads to projects' top-level
directories, and sometimes even lower level directories, to be filled with
configuration files required by these tools. These configuration files don't
have anything to do with the source-code of the project, and hence they cause a
lot of distraction at best, and wasted effort and resources at worst.

The Solution
------------

ToolsConfig specification provides a very simple, lightweight, and elegant
method to solve this problem, and declutter projects' directories gradually over
time. The ToolsConfig Specification is supposed to be adopted by the _creators_
of tools, and has very little impact on the projects that use the tools; the
little impact on the users leads to cleanup and simplification of their
project's directory structure, and is good for their project's long-term health.

ToolsConfig Algorithm
---------------------

The ToolsConfig algorithm is targeted at the tool makers. It is designed to
provide backwards compatibility, and is easy to implement. Here it is described
as pseudocode.

Let's assume following is how your tool opens its config file.

```
    file_name = 'file.conf'
    config_dir = 'example/path/'
    config_file = fopen(config_dir + file_name)
```

The ToolsConfig Specification can be adopted by changing the tool's code as
follows.

```
    file_name = 'file.conf'
    config_dir = 'example/path/'
    toolsconfig_dir = '.toolsconfig/'

    if (file_exists(config_dir + toolsconfig_dir + file_name))
        config_file = fopen(config_dir + toolsconfig_dir + file_name)
    else
        config_file = fopen(config_dir + file_name)
```

The ToolsConfig Specification
-----------------------------

The ToolsConfig Specification is defined in the following paragraph.

If the tool currently honors a file, or directory, at filesystem location
"$LOCATION", then it must _first_ look for that file, or directory, at the
location "$LOCATION/.toolsconfig". If the desired file, or directory, is not
found there, then it must honor the file, or directory, at location "$LOCATION",
if found.

If the desired file, or directory, is found in more than one locations during
this lookup, then only the first one encountered is considered valid, and the
others are treated as if they were absent.

How to Adopt
------------

In the following paragraphs, we use the terms `project` and `tool` to mean two
different things. A `project` is source code being written and developed by a
Software Developer.  A `tool` is a program that the Software Developer uses to
transform the project's code for some purpose. For example, a web application
may be considered a 'project', and Docker may be considered a `tool` it uses for
deployment. Hence, a `Dockerfile` preesnt in the project's directory is a tool's
configuration file that this specification applies to.

### For Projects

A project can adopt the ToolsConfig Specification by creating the `.toolsconfig`
directory, and then slowly, at their own convenience, the users can move the
config files to the `.toolsconfig` directory, as and when the tools used by the
project start supporting the ToolsConfig Specification.

### For Tools Developers

A tool can adopt the ToolsConfig Specification by implementing the specification
described above. The tools can use the peudocode shown in `ToolsConfig
Algorithm` section above, as a guide.

The tool's documentation does _not_ need to change all occurrences of config
paths with details of the additional location `.toolsconfig`. The tool's
documentation may simply include one section where it says that the tool
supports the ToolsConfig Specification, and hence the config files placed in the
`.toolsconfig` directory will be honored in compliance with the ToolsConfig
Specification.

Choices
-------

The directory name `.toolsconfig` was chosen after considering a few other
names. This name was chosen since it appears to be unique, not much in use (if
at all), and because it clearly conveys the intent and purpose of its existence.
That is, `.toolsconfig` directory is for storage of configuration files of the
tools.

The alternative name `.tc` was considered for people, and projects, for whom the
full name, `.toolsconfig`, is too onerous; either because it's a small project,
or becuase the people prefer short names. But the alternative name `.tc` was
removed from the specification, considering that it may result in debates among
people, for their preference between the choices. The removal of the alternative
name `.tc` also makes it easy for the tools developers to adopt the ToolsConfig
Specification.

Alternatives
------------

The following proposals also try to solve the same problem.

- [.meta][]

[.meta]: https://dotmeta.org/

Discuss
-------

Visit ToosConfig Specification's [Git repository][] to discuss, propose changes,
etc.

[Git repository]: https://github.com/DrPostgres/ToolsConfig/

