..  Copyright (c) 2014-present PlatformIO <contact@platformio.org>
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at
       http://www.apache.org/licenses/LICENSE-2.0
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

.. _projectconf_section_env_build:

Build options
-------------

.. contents::
    :local:

.. _projectconf_build_type:

``build_type``
^^^^^^^^^^^^^^

Type: ``String`` | Multiple: ``No`` | Default: ``release``

See extended documentation for :ref:`build_configurations`.

.. _projectconf_build_flags:

``build_flags``
^^^^^^^^^^^^^^^

Type: ``String`` | Multiple: ``Yes``

These flags/options affect the preprocessing, compilation, assembly,
and linking processes for C and C++ code. All compiler and linker
flags can be used. Despite the name, ``CPPDEFINES`` (C PreProcesor)
rows also apply to the C compiler.

For more detailed information about available compiler flags/options,
please visit `GCC Command Options <https://gcc.gnu.org/onlinedocs/gcc/Invoking-GCC.html>`__
official documentation.

This option can also be set by the global environment variable
:envvar:`PLATFORMIO_BUILD_FLAGS`.

.. contents:: Contents
    :local:

Scopes (SCons Variables)
''''''''''''''''''''''''

.. list-table::
    :header-rows:  1

    * - Format
      - Affects build variable
      - Description
    * - ``-D name``
      - CPPDEFINES
      - Predefine *name* as a macro, with definition 1.
    * - ``-D name=definition``
      - CPPDEFINES
      - The contents of *definition* are tokenized and processed as if they
        appeared during translation phase three in a ``#define`` directive.
    * - ``-U name``
      - CPPDEFINES
      - Cancel any previous definition of *name*, either built in or provided
        with a ``-D`` option.
    * - ``-Wp,option``
      - CPPFLAGS
      - Bypass the compiler driver and pass *option* directly  through to the
        preprocessor
    * - ``-Wall``
      - CCFLAGS
      - Turn on all optional warnings which are desirable for normal code.
    * - ``-Werror``
      - CCFLAGS
      - Make all warnings into hard errors. With this option, if any source code triggers warnings, the compilation will be aborted.
    * - ``-w``
      - CCFLAGS
      - Suppress all warnings, including those which GNU CPP issues by default.
    * - ``-include file``
      - CCFLAGS
      - Process *file* as if ``#include "file"`` appeared as the first line of
        the primary source file.
    * - ``-Idir``
      - CPPPATH
      - Add the directory *dir* to the list of directories to be searched
        for header files.
    * - ``-Wa,option``
      - ASFLAGS, CCFLAGS
      - Pass *option* as an option to the assembler. If *option* contains
        commas, it is split into multiple options at the commas.
    * - ``-Wl,option``
      - LINKFLAGS
      - Pass *option* as an option to the linker. If *option* contains
        commas, it is split into multiple options at the commas.
    * - ``-llibrary``
      - LIBS
      - Search the *library* named library when linking
    * - ``-Ldir``
      - LIBPATH
      - Add directory *dir* to the list of directories to be searched for
        ``-l``.

Built-in Variables
''''''''''''''''''

You can inject the built-in variables into your build flags, such as:

* ``$PYTHONEXE``, full path to current Python interpreter
* ``$UNIX_TIME``, current time in Unix format
* ``$PIOENV``, name of build environment from :ref:`projectconf`
* ``$PIOPLATFORM``, name of development platform
* ``$PIOFRAMEWORK``, a list of frameworks
* ``$PROJECT_DIR``, project directory
* ``$PROJECT_CORE_DIR``, PlatformIO Core directory, see :ref:`projectconf_pio_core_dir`
* ``$PROJECT_BUILD_DIR``, project build directory per all environments
* ``$BUILD_DIR``, build directory per current environment

See the `full list of PlatformIO variables <https://github.com/platformio/platformio-core/blob/develop/platformio/builder/main.py>`_.

Please use target ``envdump`` for the :option:`pio run --target`
command to see ALL variable values for a build environment.

Stringification
'''''''''''''''

Sometimes you may want to convert a macro argument into a valid C string constant.
In this case, you need to wrap a value with double quotes (``"``) and escape
double quotes (``"`` -> ``\\"``) in the constant value.

Here is an example of a macro definition that uses stringification
and :ref:`projectconf_build_flags`. Please note that we
enclosed the flag in the single quotes to prevent the shell evaluation:

**platformio.ini**

.. code:: ini

  [env:myenv]
  build_flags =
      '-D MYSTRING="Text is \\"Quoted\\""'
      '-D MYFONT=&roboto14'

**src/main.cpp**

.. code:: cpp

  #include <stdio.h>

  #define CONFIG_LV_FONT_DEFAULT MYFONT

  int main(void) {
      printf("MYSTRING=<%s>\n", MYSTRING);
      return(0);
  }

If you use :ref:`scripting`, we recommend benefiting from the
``env.StringifyMacro(value)`` helper function. In this case,
you don't need to apply any escaping, PlatformIO will do this
for you:

**platformio.ini**

.. code:: ini

  [env:myenv]
  extra_scripts = myscript.py

**myscript.py**

.. code:: python

  Import("env")

  env.Append(CPPDEFINES=[
      ("MYSTRING", env.StringifyMacro('Text is "Quoted"')),
  ])

.. _projectconf_dynamic_build_flags:

Dynamic build flags
'''''''''''''''''''

PlatformIO allows users to run an external command/script which
outputs build flags into STDOUT by prepending the shell command with a
``!`` character. PlatformIO will automatically replace commands with
their output when appending flags to build environments.

**You can use any shell or programming language.**

This external command will be called on each :ref:`cmd_run` command before
building/uploading process.

Use cases:

 * Macro with the latest VCS revision/tag "on-the-fly"
 * Generate dynamic headers (``*.h``)
 * Process media content before generating SPIFFS image
 * Make some changes to source code or related libraries

.. note::
  If you need more advanced control and would like to apply changes to
  a PlatformIO Build System environment, please refer to :ref:`scripting`.

Example:

.. code-block:: ini

    [env:generate_flags_with_external_command]
    build_flags = !cmd_or_path_to_script

    ; Unix only, get output from internal command
    build_flags = !echo '-D COMMIT_HASH=\\"'$(git log -1 --format=%%h)'\\"'


**Use Case: Create a "PIO_SRC_REV" macro with the latest Git revision**

This example includes a separate file named ``git_rev_macro.py``, to be placed
in the same directory as ``platformio.ini``.

``platformio.ini``:

.. code-block:: ini

    [env:git_revision_macro]
    build_flags = !python git_rev_macro.py

``git_rev_macro.py``:

.. code-block:: py

  import subprocess

  revision = (
      subprocess.check_output(["git", "rev-parse", "HEAD"])
      .strip()
      .decode("utf-8")
  )
  print("'-DGIT_REV=\"%s\"'" % revision)

Examples
''''''''

.. code-block:: ini

    [env:specific_defines]
    build_flags =
      -DFOO -DBAR=1
      -D BUILD_ENV_NAME=$PIOENV
      -D CURRENT_TIME=$UNIX_TIME
      -DFLOAT_VALUE=1.23457e+07

    [env:specific_inclibs]
    build_flags =
      -I/opt/include
      -L/opt/lib
      -lfoo

    [env:ignore_incremental_builds]
    ; We dynamically change the value of "LAST_BUILD_TIME" macro,
    ; PlatformIO will not cache objects
    build_flags = -DLAST_BUILD_TIME=$UNIX_TIME

.. note::
  If you need to control build flags that are specific for debug configuration please
  refer to :ref:`projectconf_debug_build_flags`.

.. _projectconf_build_src_flags:

``build_src_flags``
^^^^^^^^^^^^^^^^^^^

Type: ``String`` | Multiple: ``Yes``

An option ``build_src_flags`` has the same behavior as ``build_flags``
but will be applied only for the project source files in the
:ref:`projectconf_pio_src_dir` directory.

This option can also be set by the global environment variable
:envvar:`PLATFORMIO_BUILD_SRC_FLAGS`.

.. _projectconf_build_unflags:

``build_unflags``
^^^^^^^^^^^^^^^^^

Type: ``String`` | Multiple: ``Yes``

Selectively remove base/initial flags that were set by the development platform.

.. code-block:: ini

   [env:unflags]
   build_unflags = -Os -std=gnu++11
   build_flags = -O2

.. _projectconf_build_src_filter:

``build_src_filter``
^^^^^^^^^^^^^^^^^^^^

Type: ``String (Templates)`` | Multiple: ``Yes``

This option allows one to specify which source files should be
included or excluded from :ref:`projectconf_pio_src_dir` for a build process.
Filter supports two templates:

* ``+<PATH>`` include template
* ``-<PATH>`` exclude template

``PATH`` is relative to :ref:`projectconf_pio_src_dir`. All patterns will
be applied in their order of definition.
`GLOB Patterns <http://en.wikipedia.org/wiki/Glob_(programming)>`_ are allowed.

By default, ``build_src_filter`` is predefined to ``+<*> -<.git/> -<.svn/>``,
meaning "include ALL files, then exclude the ``.git`` and ``svn`` repository folders.

This option can also be set by the global environment variable
:envvar:`PLATFORMIO_BUILD_SRC_FILTER`.

.. _projectconf_targets:

``targets``
^^^^^^^^^^^

Type: ``String`` | Multiple: ``Yes``

A list of targets which will be processed by the :ref:`cmd_run` command by
default. You can enter more than one target, if separated by comma+space **", "**.

Please follow to :option:`pio run --list-targets` documentation for the other
targets.

**Examples**

1. Build a project using :ref:`Release Configuration <build_configurations>`,
   upload the firmware, and start :ref:`Serial Monitor <cmd_device_monitor>`
   automatically:

    .. code-block:: ini

       [env:upload_and_monitor]
       targets = upload, monitor

2. Build a project using :ref:`Debug Configuration <build_configurations>`.


**Tip!** You can use these targets like an option to
:option:`pio run --target` command. For example:

.. code-block:: bash

    # clean project
    pio run -t clean

    # dump current build environment
    pio run --target envdump

When no targets are defined, *PlatformIO* will build only sources by default.
