.. _rle-label:

RLE
===

Some ressource file in Realm Of The Haunting use a Run-length encoding (RLE) on some data.

A byte with the top 4 bits set indicates a RLE byte.

The low 4 bits indicate how many times to repeat the next byte.

.. code-block:: C
    :caption: RLE decode C pseudo-code
    
    DestinationIndex = 0x00;
    SourceIndex = 0x00;
    while (DestinationIndex < DecodedSize) {
        if (EncodedData[SourceIndex] > 0xF0) {
            memset(DecodedData + DestinationIndex, EncodedData[SourceIndex + 1], EncodedData[SourceIndex] & 0x0F);
            DestinationIndex += EncodedData[SourceIndex] & 0x0F;
            SourceIndex += 2;
        }
        else {
            DecodedData[DestinationIndex++] = EncodedData[SourceIndex++];
        }
    }