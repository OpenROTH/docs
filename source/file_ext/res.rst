ROTH.RES
========

The file :file:`ROTH.RES` is given as first argument of the executable :file:`ROTH.exe`:

.. code-block:: text

    > roth.exe @roth.res

This file is necessary for the game in order to configure several parameter:

+-----------------------------+--------------------------------------+------------------------------------------------+
|                             |                                      |                                                |
| Parameter name              | Type                                 | Description                                    |
|                             |                                      |                                                |
+=============================+======================================+================================================+
| VERSION                     | ARRAY                                | Game version                                   |
+-----------------------------+--------------------------------------+------------------------------------------------+
| HIRES                       | BOOLEAN (FALSE by default)           |                                                |
+-----------------------------+--------------------------------------+------------------------------------------------+
| BLUR                        | BOOLEAN (FALSE by default)           |                                                |
+-----------------------------+--------------------------------------+------------------------------------------------+
| VESA                        | BOOLEAN (FALSE by default)           |                                                |
+-----------------------------+--------------------------------------+------------------------------------------------+
| SND                         | ARRAY                                | Path to sound effect file                      |
+-----------------------------+--------------------------------------+------------------------------------------------+
| DAS2                        | ARRAY                                | Initiate DAS file                              |
+-----------------------------+--------------------------------------+------------------------------------------------+
| NOMAKING                    | BOOLEAN (FALSE by default)           |                                                |
+-----------------------------+--------------------------------------+------------------------------------------------+
| MAPS                        | ARRAY                                | Table of association with RAW and DAS files    |
+-----------------------------+--------------------------------------+------------------------------------------------+
| GDVS                        | ARRAY                                |                                                |
+-----------------------------+--------------------------------------+------------------------------------------------+

Example
--------

.. code-block:: text
    :caption: ROTH.RES (from DEMO version)

    version="Roth USA demo"
    snd=data\fxscript.sfx
    das2=m\ademod
    nomaking
    maps {
    m\dstudy1 m\demod
    m\dauso1ea m\demo4d
    m\dmas7 m\demo4d
    m\dmas3 m\demo4d
    }

.. code-block:: text
    :caption: ROTH.RES (from GOG version)

    version="Roth Version F1.4"
    snd=data\fxscript.sfx
    das2=m\ademo
    
    maps {
    m\study1 m\demo
    m\study2 m\demo
    m\study3 m\demo
    m\study4 m\demo
    m\optemp1 m\demo
    m\abagate2 m\demo3
    m\anubis m\demo2
    m\gnarl1 m\demo2
    m\mauso1ea m\demo4
    m\mauso1eb m\demo4
    m\mas3 m\demo4
    m\mas4 m\demo4
    m\mas6 m\demo4
    m\mas7 m\demo4
    m\aelf m\demo4
    m\caverns2 m\demo4
    m\caverns3 m\demo4
    m\grave m\demo4
    m\maze m\demo4
    m\dominion m\demo3
    m\salvat m\demo3
    m\dopple m\demo3
    m\soulst2 m\demo1
    m\soulst3 m\demo1
    m\tower1 m\demo2
    m\tgate1f m\demo2
    m\tgate1g m\demo2
    m\tgate1h m\demo2
    m\tgate1i m\demo2
    m\caverns m\demo
    m\temple1 m\demo
    m\raquia1 m\demo1
    m\raquia2 m\demo1
    m\raquia3 m\demo1
    m\raquia4 m\demo1
    m\raquia5 m\demo1
    m\church1 m\demo1
    m\vicar m\demo1
    m\aqua1 m\demo1
    m\aqua2 m\demo1
    m\lrinth m\demo1
    m\lrinth1 m\demo1
    m\elohim1 m\demo1
    m\vicar1 m\demo1
    }