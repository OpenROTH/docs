DBASE500
========

Sound dialog.

Kind of modified WAVE.

Each entry is normally accessed by an offset that can be found in the file :file:`DBASE100.DAT`.

.. note::

    All entry offset must be a mutiple of 8.

.. code-block:: python
    :caption: Align entry offset python pseudo-code

    def align_8(x):
        return (((x) + 8 - 1) & ~(8 - 1))

    next_entry_offset = align_8(len(data) + sizeof(entry_header))


Header
------

.. code-block:: text

    +0x00       SIGNATURE       [BYTE] * 0x08   // "DBASE500"


Entry
-----

Each entry have the header "RIFF" reversed to "FFIR".

They set the audio format to 0x2A (42), but in fact it's **P**\ ulse **C**\ ode **M**\ odulation (0x01 : PCM).

Header
^^^^^^

.. code-block:: text

    +0x00       CHUNK_ID            [BYTE][4]           // "FFIR"
    +0x04       CHUNK_LENGHT        [DWORD]
    +0x08       FORMAT              [BYTE][4]           // "WAVE"
    +0x0C       SUB_CHUNK_1_ID      [BYTE][4]           // "fmt "
    +0x10       SUB_CHUNK_1_SIZE    [DWORD]
    +0x14       AUDIO_FORMAT        [WORD]
    +0x16       NUM_CHANNEL         [WORD]
    +0x18       SAMPLE_RATE         [DWORD]
    +0x1C       BYTE_RATE           [DWORD]
    +0x20       BLOCK_ALIGN         [WORD]
    +0x22       BIT_PER_SAMPLE      [WORD]
    +0x24       SUB_CHUNK_2_ID      [BYTE][4]           // "data"
    +0x28       SUB_CHUNK_2_SIZE    [DWORD]

Data
^^^^

Data are stored in the :class:`sub_chunk_2`.

.. code-block:: text

    HEADER
    ...
    +0x2C       DATA                [BYTE] * SUB_CHUNK_2_SIZE

Example
-------

.. code-block:: text

    > md5sum.exe DBASE500.DAT
    bcee03ec2b751bc0015e8bec55d6ce5a DBASE500.DAT
    > hexdump -C -s 7720056 -n 100 DBASE500.DAT
    0075cc78  46 46 49 52 fd ab 00 00  57 41 56 45 66 6d 74 20  |FFIR....WAVEfmt |
    0075cc88  10 00 00 00 2a 00 01 00  22 56 00 00 22 56 00 00  |....*..."V.."V..|
    0075cc98  01 00 08 00 64 61 74 61  d9 ab 00 00 0b 02 01 03  |....data........|
    0075cca8  04 00 05 00 06 00 03 04  00 03 00 04 00 03 02 04  |................|
    0075ccb8  00 02 03 04 00 03 04 04  03 04 02 02 01 00 00 04  |................|
    0075ccc8  04 01 04 00 01 04 00 04  02 00 02 01 00 04 01 00  |................|
    0075ccd8  04 00 02 02                                       |....|
    0075ccdc

WAV output:
    
.. raw:: html

    <audio controls="controls"><source src="../_static/entry_65_dbase500_demo.wav" type="audio/x-wav" /></audio>
