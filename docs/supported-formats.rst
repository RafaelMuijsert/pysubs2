Supported File Formats
======================

.. tip::
   Every supported format has a unique *format identifier* string, eg. ``"srt"``.
   These can be used in the API to force input/output in a certain format, though this
   is usually unnecessary thanks to autodetection from content and file extension.

.. note::
   Some formats support additional parameters to customize input/output.
   These parameters can be provided as extra keyword parameters to relevant methods
   eg. :func:`pysubs2.load()` or :meth:`pysubs2.SSAFile.save()`.

   Please refer to :ref:`subtitle-format-implementations` for details.

Native Format -- SubStation Alpha
---------------------------------

pysubs2 is built around *SubStation Alpha*, the native subtitle format of `Aegisub <http://www.aegisub.org/>`_.
This format is popular in anime fansub community as it allows for rich text formatting and even animations
and vector graphics.

**SubStation Alpha** --- supported in two flavours:

- .ass files (Advanced SubStation Alpha v4.0+), format identifier is ``"ass"``.
- .ssa files (SubStation Alpha v4.0), format identifier is ``"ssa"``.

Implemented in :class:`pysubs2.substation.SubstationFormat`.

.. versionchanged:: 1.2.0
   Added support for embedded fonts. Fonts in loaded files will be preserved on save.
   For simplicity and speed, the data is stored in opaque form (not decoded).
   If you'd like to extract/embed fonts via pysubs2, feel free to open an issue on GitHub.

.. versionchanged:: 1.6.0
   Added support for embedded graphics.

Time-based Formats
------------------

SubRip (SRT)
~~~~~~~~~~~~

This format uses .srt files, format identifier is ``"srt"``. Widely used subtitle format, it uses HTML
tags for formatting, though it is usually not heavily formatted (as opposed to *SubStation*).
Implemented in :class:`pysubs2.subrip.SubripFormat`.

.. versionchanged:: 1.4.0
   Added option to keep SubStation override tags in SRT output. This is useful if your SRT file uses
   tags like ``{\i1}`` instead of ``<i>``.

.. versionchanged:: 1.4.1
   Added option to keep all HTML tags in SRT input. This is useful if you want to output SRT and
   don't want the library to strip any tags it doesn't understand.

MPL2
~~~~

Time-based format similar to MicroDVD, format identifier is ``"mpl2"``. To save subtitles in MPL2 format,
use ``subs.save("subtitles.txt", format_="mpl2")``.
Implemented in :class:`pysubs2.mpl2.MPL2Format`.

.. versionchanged:: 0.2.2
   Added support for MPL2 subtitle format.

TMP
~~~

Time-based format, format identifier is ``"tmp"``. A very simple format which only specifies starting time
for each subtitle, eg. ``0:00:13:This is a subtitle``. Subtitle length is calculated automatically based on character
count. This older subtitle format is also referred to as "TMP Player" format.
Implemented in :class:`pysubs2.tmp.TmpFormat`.

.. versionchanged:: 0.2.4
   Added support for TMP subtitle format.

WebVTT
~~~~~~

Time-based format similar to SubRip, format identifier is ``"vtt"``. Currently implemented
as a flavour of SubRip, with no extra support for WebVTT-specific features like styles or subtitle alignment.
`Link to WebVTT specification <https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API>`_, official name is
"Web Video Text Tracks Format".
Implemented in :class:`pysubs2.webvtt.WebVTTFormat`.

.. versionchanged:: 1.0.0
   Added basic support for WebVTT subtitle format.

SAMI
~~~~

Legacy time-based format based on HTML and CSS, format identifier is ``"sami"``. Only a rudimentary parser is implemented
for this format.

`Link to SAMI 1.0 specification <https://learn.microsoft.com/en-us/previous-versions/windows/desktop/dnacc/understanding-sami-1.0>`_,
official name is "Synchronized Accessible Media Interchange".

Implemented in :class:`pysubs2.sami.SAMIFormat`.

OpenAI Whisper
~~~~~~~~~~~~~~

Time-based format for transcriptions generated from `Whisper JAX <https://huggingface.co/spaces/sanchit-gandhi/whisper-jax>`_,
format identifier is ``"whisper_jax"``. Only parser is implemented.

Implemented in :class:`pysubs2.whisper.WhisperJAXFormat`.

Frame-based Formats
-------------------

MicroDVD
~~~~~~~~

This format uses .sub files, format identifier is ``"microdvd"``. This format uses frames to describe start/end times,
instead of hour/minute/second, which means it is dependent on framerate of the video. For proper retiming and conversion,
you need to know the framerate (sometimes it is given in the first subtitle, which ``pysubs2`` will autodetect and use).
Implemented in :class:`pysubs2.microdvd.MicroDVDFormat`.

Other
-----

JSON
~~~~

This is JSON-serialized internal representation, which amounts to ASS. Format identifier is ``"json"``.
Implemented in :class:`pysubs2.jsonformat.JSONFormat`.

OpenAI Whisper (API)
~~~~~~~~~~~~~~~~~~~~

Transcripts from the `OpenAI Whisper library <https://github.com/openai/whisper>`_
can be directly imported using :func:`pysubs2.load_from_whisper`.
