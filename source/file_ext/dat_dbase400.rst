DBASE400
========

Each entry is normally accessed by an offset that can be found in the file :doc:`DBASE100.DAT <dat_dbase100>`.

.. note::

    All entry offset must be a mutiple of 4.

.. code-block:: python
    :caption: Align entry offset python pseudo-code

    def align_4(x):
        return (((x) + 4 - 1) & ~(4 - 1))

    next_entry_offset = align_4(len(data) + sizeof(entry_header))


Header
------

.. code-block:: text

    +0x00       SIGNATURE       [BYTE] * 0x08   // "DBASE400"

.. _dat_dbase400_standard_entry:

Standard Entry
--------------

.. code-block:: text

    +0x00       OFFSET_DBASE500             [DWORD]             // This offset must be multiplied by 8
    +0x04       LENGTH_STR                  [WORD]
    +0x06       FONT_COLOR                  [WORD]
    +0x08       STRING                      [BYTE] * LENGTH_STR // This field should be aligned to 4

OFFSET_DBASE500 may be zero if there no associated :doc:`DBASE500 resource <dat_dbase500>`
(interface string, for example).

Usually FONT_COLOR represents unique color for each character. For example,
Adam's subtitles have 0xd7 color. Here some examples:

.. code-block:: text

    0x54        Rebecca (yellow)
    0x67        Interface (white)
    0xd7        Adam Randall (cyan)

Example
^^^^^^^

.. code-block:: text

    > md5sum DBASE400.DAT
    465eee4100d3cafb2662c966029b34f5  DBASE400.DAT
    > hexdump -n 100 -C DBASE400.DAT
    00000000  44 42 41 53 45 34 30 30  00 00 00 00 0f 00 67 00  |DBASE400......g.|
    00000010  53 75 62 74 69 74 6c 65  27 73 20 4f 4e 2e 00 00  |Subtitle's ON...|
    00000020  00 00 00 00 10 00 67 00  53 75 62 74 69 74 6c 65  |......g.Subtitle|
    00000030  27 73 20 4f 46 46 2e 00  00 00 00 00 0d 00 67 00  |'s OFF........g.|
    00000040  52 75 6e 20 6d 6f 64 65  20 4f 4e 2e 00 00 00 00  |Run mode ON.....|
    00000050  00 00 00 00 0e 00 67 00  52 75 6e 20 6d 6f 64 65  |......g.Run mode|
    00000060  20 4f 46 46                                       | OFF|
    00000064

First entry:

.. code-block:: text

    > hexdump -s 8 -n 24 -C DBASE400.DAT
    00000008  00 00 00 00 0f 00 67 00  53 75 62 74 69 74 6c 65  |......g.Subtitle|
    00000018  27 73 20 4f 4e 2e 00 00                           |'s ON..|
    00000021

* OFFSET_DBASE500 = 0x00000000 (no sound entry)
* LENGTH_STR = 0x000F (15, which align 4 it will be 16)
* FONT_COLOR = 0x0067 (white color)
* STRING = "Subtitle's ON\\x00\\x00"

Subtitle sequence
^^^^^^^^^^^^^^^^^

Subtitle entries is special case. They have own structure and appears only in
:ref:`cutscene entry <dat_dbase400_cutscene_entry>` references. Each cutscene
may have (or not have at all) one subtitle sequence. All sequences have one or
more subtitle entries followed by "\\x00\\x00\\xFF\\xFF":

.. code-block:: text

    +0x00       SUBTITLE_ENTRIES        Variable
    +0x??       SUBTITLE_END            [DWORD]                 // \\x00\\x00\\xFF\\xFF

Subtitle entry
^^^^^^^^^^^^^^

.. code-block:: text

    +0x00       LENGTH_STR              [WORD]                  // Length of record
    +0x02       UNK_WORD_00             [WORD]                  // Duration?
    +0x02       FONT_COLOR              [BYTE]
    +0x01       STRING                  [BYTE] * LENGTH_STR - 5 // This field should be aligned to 4

