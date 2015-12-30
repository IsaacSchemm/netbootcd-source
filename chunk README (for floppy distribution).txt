(This documentation is formatted for an 80-column fixed font display device.)

Chunk -- Simple, fast file splitter, by trixter@oldskool.org
The most current version of this program should always be available at:
http://www.oldskool.org/pc/

Chunk was written to serve one purpose, and do it well:  Split files into
differently-sized chunks, primarily so that large files can be transported via
media that is smaller than the size of the file.  In fact, the specific
application for developing this particular program is copying large files onto
floppy disks for transport on/off an old computer, but of course you can use it
for any purpose that requires splitting a file into chunks (or combining chunks
into a file).

Features:
  - Fast! (uses up to 128K RAM to buffer I/O)
  - Robust floppy disk support.  Diskette Mode automatically prompts for new
    disks without requiring the user to press any keys; Automatic mode
    automatically determines the amount of free space to use on the target.
  - Simple and flexible command-line usage (switches can be - or / and appear
    anywhere on the command line)
  - Non-proprietary; each chunk is numbered sequentially, and no additional
    junk is added to each file.
  - Lots of error checking and verbose error messages.
  - Follows the oldskool.org credo:  Works On Any Machine(tm)
    In fact, it was coded on an original IBM Model 5150 (4.77MHz 8088).

The basic principle behind splitting files with chunk is to supply an input
filename with extension, and Chunk produces a set of output files with a base
name and increasing sequential extensions.  This means, if you tell it to split
with a basename of "output", it creates files called "output.000",
"output.001", "output.002", etc.  Unlike some splitting utilities, no
additional information is written to each chunk, nor is any index file created
or maintained.  This is intentional, as I hate that crap (it locks you into
using just that utility).

Re-combining chunks into the original file requires access to the sequentially
numbered chunked files.  Chunk looks for them in sequential order, and then
adds them to the target filename.ext until it can't find any more.  (In
Diskette mode, there is no way for Chunk to know which file is the last one
unless one of two conditions occur:  a chunk is found that is smaller than the
previous chunks, or the user presses a key to signify the last diskette has
been read.)

Usage:

Chunk <options> [source] [target] <options>
where
  [source] and [target] are either a full filename.ext or a basename (a
  filename without an extension) depending on usage (see below).
  <options> can appear anywhere on the command-line, and can be delimited
  by either "/" or "-".

Splitting options are:
  /s<num> Size of each chunk in bytes.  If size ends in K or M, size will
          be in kilobytes (1024 bytes) or megabytes (1048576 bytes).
          Example:  "chunk filename.ext basename /s256k"

  /f<num> Size of chunks appropriate for a blank/formatted floppy of size <num>
          Available sizes are 160, 180, 320, 360, 720, 1200, and 1440.
          Example:  "chunk filename.ext basename /f360"

  /n<num> Number of chunks to make.  File will be split into N chunks.
          Example:  "chunk filename.ext basename /n5"

  /a      Autosize mode enabled.  Size of each chunk will be determined
          by the amount of free space on the target.  Diskette mode required.
          Example:  ~chunk /d /a filename.ext a:basename"

Combining options are:
  /c      Combine split parts into file.  Chunk extension must be zero-padded.
          Example: "chunk /c basename filename.ext~

Diskette options are:
  /d      Diskette mode enabled.  User will be prompted to switch disks.
          Example (splitting): "chunk /d /f360 filename.ext a:basename"
          Example (combining): "chunk /c /d a:basename filename.ext"

  /e      Erase contents of diskette before writing target.
          There is no user prompting with this command -- use with caution!
          Example:  "chunk /d /a /e filename.ext a:basename"

General options are:
  /h[w]   Print command-line usage.  If called as /hw, help text will
          be written to the file "chunk.txt"

  /v[c]   Verify readability of target file(s).  This re-reads the target
          to ensure it is readable.  If called as /vc, data buffers are
          compared byte-for-byte to ensure file was written correctly.
          Example: "chunk filename.ext basename /n5 /v"

  /o      Overwrite.  If set, target(s) will be overwritten without prompting.
          Example: "chunk filename.ext basename /n5 /o"

Examples:

Splitting a file into disk-sized chunks:
  chunk filename.ext basename /f720

Splitting a file into four parts:
  chunk filename.ext basename /n4

Splitting a file onto floppy disks, using all available disk space:
  chunk filename.ext a:basename /d /a /e

Combining chunks into a file:
  chunk /c c:\import\basename c:\output\filename.ext

Combining chunks located on multiple disks into a file:
  chunk a:basename filename.ext /d

To-Do List:

  - /v[c] isn't implemented yet.  Sorry, but I want to get this thing out the
    door so I can put this 8088 away and resume production on Mindcandy Vol. 2.
    If you simply MUST have verification, do this:
      1. "set VERIFY=ON" at DOS prompt or in autoexec.bat
      2. Make chunks with /f parameter ("chunk /f360 filename.ext basename")
      3. Copy each chunk ("basename.000", "basename.001", etc.) manually.

  - DOS errorlevel codes are not returned.  If you want this implemented so you
    can use Chunk in batch files, let me know, as I would be flattered :-)

Known Bugs/Issues:

  - Source files being split with the readonly flag set are not recognized due
    to lack of proper error handling by Turbo Pascal (ioresult returns 103,
    File Not Open, which makes no damn sense).  If you are trying to split
    files directly off of a CDROM, you will probably get bitten by this. This
    can be fixed with additional error checking, but see above point about why
    /v[c] isn't implemented yet.

  - I have only tested this utility on an 8088 running MS-DOS 6.22.  If it
    doesn't work for you, let me know what you're running it on/under and I'll
    try to fix it.

  - Because integers are used for the chunk extensions (*.000, *.001, etc.),
    you are limited to making no more than 1000 chunks.  I'm sure this won't be
    a problem for most people, but thought I should mention it.

License and source code:

This program is free for use by any individual for any purpose, under the
understanding that the author of this program is not liable for any damages
incurred from the use or mis-use of this program.  I wrote it and I use it
myself, but that doesn't mean you have to, and I cannot be held responsible if
something bad happens.

The source code is included in the source\ directory.  You should be able to
recompile and re-make the executable using everything there, although some
editing of the build.bat file will be necessary for your system.  Turbo Pascal
7.0 is required, although you should be able to use anything that properly
implements the objects unit (specifically TStream and its descendants).

If you make any additions or corrections to the source code, let me know so
that I can incorporate them into the official distributable file on
www.oldskool.org/pc.

Greets:

Greets to the owb crew, and anyone who still uses computers more than two
decades old.

PS:  Visit www.mobygames.com!
