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

Python Package Manager
----------------------

.. warning::
    We recommend using this method **ONLY FOR** :ref:`ci` systems or where your have
    full permissions to install PlatformIO Core into the global scope of your OS.

    For personal using, and avoiding maintenance and upgrade issues, we
    **HIGHLY RECOMMEND** using :ref:`installation_installer_script` which installs
    PlatformIO Core into an isolated virtual environment and does not affect your OS.

The latest stable version of PlatformIO Core may be installed or upgraded via
Python Package Manager (`pip <https://pip.pypa.io>`_) as follows:

.. code-block:: bash

    pip install -U platformio
