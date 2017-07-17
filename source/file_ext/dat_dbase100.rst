DBASE100
========

DBASE100 is index file that reference to other resources in other DAT-files. It 
holds information about actions, cutscenes, subtitles, interface and items from
inventory.

Unlike other game engines, there no unique resource_id identificator. Instead of that
ROTH engine refers resource by offset in particular file. For convention,
OFFSET_DBASE*00 is means offset to entry in that file.

Header
------

.. code-block:: text

    +0x00       SIGNATURE                   [BYTE] * 0x08   // "DBASE100"
    +0x08       FILESIZE                    [DWORD]
    +0x0C       UNK_DWORD_02                [DWORD]
    +0x10       NB_DBASE100_INVENTORY       [DWORD]         // Numbers of inventory entries
    +0x14       DBASE100_TABLE_INVENTORY    [DWORD]         // Offset to table of inventory entries
    +0x18       NB_DBASE100_ACTION          [DWORD]
    +x01C       DBASE100_TABLE_ACTION       [DWORD]
    +x020       NB_DBASE400_CUTSCENE        [DWORD]         // Numbers of cutscene entries
    +x024       DBASE400_TABLE_CUTSCENE     [DWORD]         // Offset to table of cutscene entries
    +x028       NB_DBASE400_INTERFACE       [DWORD]         // Numbers of interface entries
    +x02C       DBASE400_TABLE_INTERFACE    [DWORD]         // Offset to table of interface entries
    +x030       UNK_DWORD_11                [DWORD]

Table of inventory entries
--------------------------

This table begins in DBASE100_TABLE_INVENTORY offset. This is array of offsets
to DBASE100 inventory entries.


.. code-block:: text

    +0x00       OFFSET_DBASE100             [DWORD] * NB_DBASE100_INVENTORY

Inventory entry
^^^^^^^^^^^^^^^

These entries accessible via `Table of inventory entries`_

.. todo:: There is not completely parsed structure in UNK_BYTES_01 which may have additional OFFSET_DBASE400 entries.

.. code-block:: text

    +0x00       LENGTH                      [WORD]
    +0x02       UNK_WORD_01                 [WORD]
    +0x04       CLOSEUP_TYPE                [BYTE]
    +0x05       ITEM_TYPE                   [BYTE]
    +0x06       UNK_BYTE_02                 [BYTE]                  // Always 0. Padding?
    +0x07       UNK_BYTE_03                 [BYTE]                  // Always 0. Padding?
    +0x08       CLOSEUP_IMAGE               [DWORD]                 // Animated image in inventory (OFFSET_DBASE300?)
    +0x0c       INVENTORY_IMAGE             [DWORD]                 // Image in inventory (OFFSET_DBASE200?)
    +0x10       OFFSET_DBASE400             [DWORD]                 // Name of item
    +0x14       ADD_LENGTH                  [WORD]
    +0x16       UNK_BYTE_04                 [BYTE]                  // Always 0. Padding?
    +0x17       UNK_BYTE_05                 [BYTE]
    +0x18       UNK_DWORD_04                [DWORD]
    +0x1c       UNK_BYTES_01                [BYTE] * ADD_LENGTH

ITEM_TYPE may have follow values:

.. code-block:: text

    0x00 - generic
    0x01 - weapon
    0x02 - characters and important info
    0x03 - magic items (?)
    0x04 - books, notes, letters etc.
    0x12 - Adam Randall, protagonist
    0x20 - coin
    0x21 - ammo
    0x40 - interactive items
    0x43 - consumable or wearable items

Table of action entries
-----------------------

This table begins in DBASE100_TABLE_ACTION offset. This is array of offsets to
DBASE100 actions entries.

.. code-block:: text

    +0x00       OFFSET_DBASE100             [DWORD] * NB_DBASE100_ACTION

Action entry
^^^^^^^^^^^^

These entries accessible via `Table of action entries`_. Not clear whats actions
actually do. These entries is some sort of scripting engine. Each entry has zero
or more opcodes, each can accept own value. For example, opcode 0x07 plays 
cutscene based on number of that cutscene from `Table of cutscene entries`_.

Each entry has alignment to 4.

.. code-block:: text

    +0x00       LENGTH                      [WORD]
    +0x02       UNK_WORD_01                 [WORD]              // Always 0x0300 if length != 0, otherwise 0x0000 for padding
    +0x04       OPCODES                     [DWORD] * (LENGTH / 4 - 1)

Opcodes
^^^^^^^

.. todo:: Meaning of opcodes not parsed yet

Each opcode is divided into two parts: higher byte - command, three lower bytes 
- value of command.

.. code-block:: text

    +0x00       VALUE                       [BYTE] * 3
    +0x03       COMMAND                     [BYTE]
    
There are table of commands and accepted values.

+-------------------+-----------------------------+-------------------------------------------------------------+
| Command           | Value                       | Description                                                 |
+===================+=============================+=============================================================+
| 0x01              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x02              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x04              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x05              | OFFSET_DBASE400             | "Say" something                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x07              | Number of cutscene          | Play cutscene (number from :doc:`here <../annex/cutscenes>`)|
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x08              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x0b              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x0d              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x11              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x19              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x1a              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x26              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x2d              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x36              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x81              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x82              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x84              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x8d              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+
| 0x91              |                             |                                                             |
+-------------------+-----------------------------+-------------------------------------------------------------+

Table of interface entries
--------------------------

This table begins in DBASE100_TABLE_INTERFACE offset. This is array of offsets
to DBASE100 interface entries. Each entry in that array is OFFSET_DBASE400,
which is :ref:`standard entry <dat_dbase400_standard_entry>` of DBASE400.DAT.

.. code-block:: text

    +0x00       OFFSET_DBASE400             [DWORD] * NB_DBASE100_INTERFACE

Table of cutscene entries
-------------------------

This table begins in DBASE100_TABLE_CUTSCENE offset. This is array of offsets
to DBASE100 cutscene entries.

.. code-block:: text

    +0x00       OFFSET_DBASE100             [DWORD] * NB_DBASE100_CUTSCENE

.. _dat_dbase400_cutscene_entry:  

Cutscene entry
^^^^^^^^^^^^^^

These entries accessible via `Table of cutscene entries`_. 

.. code-block:: text

    +0x00       NAME                        [BYTE] * 8          // ASCII name of file from GDV folder
    +0x08       UNK_WORD_01                 [WORD]              // Always 0. Padding?
    +0x10       LENGTH_SUBTITLES            [WORD]              // Length of subtitle entry. 0 if no subtitles
    +0x12       OFFSET_DBASE400             [DWORD]             // In-game name of cutscene (standard DBASE400 entry)
    +0x16       OFFSET_DBASE400_SUBTITLE    [DWORD]             // Offset to beginning DBASE400 subtitle sequence

