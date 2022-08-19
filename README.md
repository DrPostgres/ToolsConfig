ToolsConfig
===========

The ToolsConfig Specification provides a very simple, lightweight, and elegant
way to remove the clutter caused by various tools' configuration files and
directories.

The Problem
-----------

There is a prolifiration of developer tools, and each tool requires its
configuration be specified in a very specific file, in a software project's
directory. This leads to projects' top-level, and sometimes lower level,
directories filled with specially-named files and directories required by the
tools. These specially-named files and directories don't have anything to do
with the source-code of the project, and hence cause a lot of distraction, at
best, and wasted effort and resources, at worst. See the examples section,
below, to witness this problem, and the solution, in some major projects.

The Solution
------------

ToolsConfig specification provides a very simple, lightweight, and elegant way
to solve this problem, and declutter projects' directories. This specification
is supposed to be adopted by the _creators_ of tools, and has very little impact
on the _users_ of the tools; the little impact on the users leads to cleanup and
simplification of their project's directory structure, and is very good for
their project's health.

ToolsConfig Algorithm
---------------------

The algorithm is targeted at the tool makers, is designed to be backwards
compatible, and easy to implement. Here it is described as pseudocode, and
sample implementations in various programming languages are available in the
samples directory.

Let's assume following is how your tool opens its config file.

```
    config_dir = "example/path/"
    config_file = fopen(config_dir + "file.conf")
```

The ToolsConfig specification can be adopted by changing the code as follows.

```
    file_name = 'file.conf'
    config_dir = "example/path/"
    toolsconfig_dir = ".toolsconfig/"
    tc_dir = ".tc/"

    if (file_exists(config_dir + toolsconfig_dir + file_name))
        config_file = fopen(config_dir + toolsconfig_dir + file_name)
    else
    if (file_exists(config_dir + tc_dir + file_name))
        config_file = fopen(config_dir + tc_dir + file_name)
    else
        config_file = fopen(config_dir + file_name)
```

The ToolsConfig Specification
-----------------------------

The ToolsConfig Specification is defined in the following paragraph.

If the tool currently honors a file, or directory, at filesystem location
"$LOCATION", then it must _first_ look for that file, or directory, at the
location "$LOCATION/.toolsconfig". If the desired file, or directory, is not
found there, then it must look for it at the location "$LOCATION/.tc". If the
desired file, or directory, is not found there either, then it must honor the
file, or directory, at location "$LOCATION", if present.

If the desired file, or directory, is found in more than one locations during
this lookup, then only the first one encountered is considered valid, and the
others are treated as if they were absent.

How to Adopt
------------

In the following paragraphs, we use the terms `project` and `tool` to mean two
different things.

A project can adopt the ToolsConfig Specification by creating the `.toolsconfig`
(or the `.tc`) directory, and then slowly, at their own convenience, they can
move the config files to the `.toolsconfig` directory, as and when the tools,
that the project uses, start supporting the ToolsConfig Specification.

A tool can adopt the ToolsConfig Specification by implementing the
specification. The tools can use the peudocode shown in `ToolsConfig Algorithm`
section, above, as a guide.

The tool's documentation does _not_ need to change all occurrences of config
paths with details of the additional location `.toolsconfig`, or `.tc`. The
tool's documentation may simply include one section where it says that the tool
supports the ToolsConfig Specification, and hence the config files placed in the
`.toolsconfig`, or the `.tc`, directory will be honored in compliance the the
ToolsConfig Specification.

Choices
-------

The directory name `.toolsconfig` was chosen after considering a few other
names. This name was chosen since it appears to be unique, not much in use (if
at all), and because it clearly conveys the intent and purpose of its existence.

The alternative name `.tc` was chosen for people, and projects, for whom the
full name, `.toolsconfig`, is too onerous; either because it's a small project,
or becuase the people prefer short names.

Alternatives
------------

Examples
--------

Here are a few examples of real-world projects that may benefit tremendously if
their tools adopted the ToolsConfig Specification. The screenshots are arranged
in pairs, where in each pair we first see the _before_ (or current) state, and
then we wee the _after_ state, presuming all their tools have adopted the
ToolsConfig Specification.

