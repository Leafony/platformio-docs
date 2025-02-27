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

.. _board_espressif32_esp-wrover-kit:

Espressif ESP-WROVER-KIT
========================

.. contents::

Hardware
--------

Platform :ref:`platform_espressif32`: Espressif Systems is a privately held fabless semiconductor company. They provide wireless communications and Wi-Fi chips which are widely used in mobile devices and the Internet of Things applications.

.. list-table::

  * - **Microcontroller**
    - ESP32
  * - **Frequency**
    - 240MHz
  * - **Flash**
    - 4MB
  * - **RAM**
    - 320KB
  * - **Vendor**
    - `Espressif <https://espressif.com/en/products/hardware/esp-wrover-kit/overview?utm_source=platformio.org&utm_medium=docs>`__


Configuration
-------------

Please use ``esp-wrover-kit`` ID for :ref:`projectconf_env_board` option in :ref:`projectconf`:

.. code-block:: ini

  [env:esp-wrover-kit]
  platform = espressif32
  board = esp-wrover-kit

You can override default Espressif ESP-WROVER-KIT settings per build environment using
``board_***`` option, where ``***`` is a JSON object path from
board manifest `esp-wrover-kit.json <https://github.com/platformio/platform-espressif32/blob/master/boards/esp-wrover-kit.json>`_. For example,
``board_build.mcu``, ``board_build.f_cpu``, etc.

.. code-block:: ini

  [env:esp-wrover-kit]
  platform = espressif32
  board = esp-wrover-kit

  ; change microcontroller
  board_build.mcu = esp32

  ; change MCU frequency
  board_build.f_cpu = 240000000L


Uploading
---------
Espressif ESP-WROVER-KIT supports the following uploading protocols:

* ``cmsis-dap``
* ``esp-bridge``
* ``esp-prog``
* ``espota``
* ``esptool``
* ``ftdi``
* ``iot-bus-jtag``
* ``jlink``
* ``minimodule``
* ``olimex-arm-usb-ocd``
* ``olimex-arm-usb-ocd-h``
* ``olimex-arm-usb-tiny-h``
* ``olimex-jtag-tiny``
* ``tumpa``

Default protocol is ``esptool``

You can change upload protocol using :ref:`projectconf_upload_protocol` option:

.. code-block:: ini

  [env:esp-wrover-kit]
  platform = espressif32
  board = esp-wrover-kit

  upload_protocol = esptool

Debugging
---------

:ref:`piodebug` - "1-click" solution for debugging with a zero configuration.

.. warning::
    You will need to install debug tool drivers depending on your system.
    Please click on compatible debug tool below for the further
    instructions and configuration information.

You can switch between debugging :ref:`debugging_tools` using
:ref:`projectconf_debug_tool` option in :ref:`projectconf`.

Espressif ESP-WROVER-KIT has on-board debug probe and **IS READY** for debugging. You don't need to use/buy external debug probe.

.. list-table::
  :header-rows:  1

  * - Compatible Tools
    - On-board
    - Default
  * - :ref:`debugging_tool_cmsis-dap`
    - 
    - 
  * - ``esp-bridge``
    - 
    - 
  * - :ref:`debugging_tool_esp-prog`
    - 
    - 
  * - :ref:`debugging_tool_ftdi`
    - Yes
    - Yes
  * - :ref:`debugging_tool_iot-bus-jtag`
    - 
    - 
  * - :ref:`debugging_tool_jlink`
    - 
    - 
  * - :ref:`debugging_tool_minimodule`
    - 
    - 
  * - :ref:`debugging_tool_olimex-arm-usb-ocd`
    - 
    - 
  * - :ref:`debugging_tool_olimex-arm-usb-ocd-h`
    - 
    - 
  * - :ref:`debugging_tool_olimex-arm-usb-tiny-h`
    - 
    - 
  * - :ref:`debugging_tool_olimex-jtag-tiny`
    - 
    - 
  * - :ref:`debugging_tool_tumpa`
    - 
    - 

Frameworks
----------
.. list-table::
    :header-rows:  1

    * - Name
      - Description

    * - :ref:`framework_arduino`
      - Arduino Wiring-based Framework allows writing cross-platform software to control devices attached to a wide range of Arduino boards to create all kinds of creative coding, interactive objects, spaces or physical experiences

    * - :ref:`framework_espidf`
      - ESP-IDF is the official development framework for the ESP32 and ESP32-S Series SoCs.