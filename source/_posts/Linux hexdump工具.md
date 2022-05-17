---
title: Linux Hexdump
date: 2022-05
tags: [linux, hexdump]
---

# HEXDUMP 工具
hexdump 
display file contents in hexadecimal, decimal, octal, or ascii
hexdump options file ...

       hd options file ...
DESCRIPTION         top
       The hexdump utility is a filter which displays the specified
       files, or standard input if no files are specified, in a
       user-specified format.
OPTIONS    
       Below, the length and offset arguments may be followed by the
       multiplicative suffixes KiB (=1024), MiB (=1024*1024), and so on
       for GiB, TiB, PiB, EiB, ZiB and YiB (the "iB" is optional, e.g.,
       "K" has the same meaning as "KiB"), or the suffixes KB (=1000),
       MB (=1000*1000), and so on for GB, TB, PB, EB, ZB and YB.


-b 单字节八进制显示
-b, --one-byte-octal
           One-byte octal display. Display the input offset in
           hexadecimal, followed by sixteen space-separated,
           three-column, zero-filled bytes of input data, in octal, per
           line.

-c 单字节字符显示
-c, --one-byte-char
           One-byte character display. Display the input offset in
           hexadecimal, followed by sixteen space-separated,
           three-column, space-filled characters of input data per line.

-C 输出规范的十六进制和ASCII码
-C, --canonical
           Canonical hex+ASCII display. Display the input offset in
           hexadecimal, followed by sixteen space-separated, two-column,
           hexadecimal bytes, followed by the same sixteen bytes in %_p
           format enclosed in '|' characters. Invoking the program as hd
           implies this option.

-d 双字节十进制显示
-d, --two-bytes-decimal
           Two-byte decimal display. Display the input offset in
           hexadecimal, followed by eight space-separated, five-column,
           zero-filled, two-byte units of input data, in unsigned
           decimal, per line.

-e 指定格式字符串，格式字符串包含在一对单引号中，格式字符串形如：'a/b "format1" "format2"'
-e, --format format_string
           Specify a format string to be used for displaying data.

       -f, --format-file file
           Specify a file that contains one or more newline-separated
           format strings. Empty lines and lines whose first non-blank
           character is a hash mark (#) are ignored.

       -L, --color[=when]
           Accept color units for the output. The optional argument when
           can be auto, never or always. If the when argument is
           omitted, it defaults to auto. The colors can be disabled; for
           the current built-in default see the --help output. See also
           the Colors subsection and the COLORS section below.

-n length 只格式化输入文件的前length个字节。
-n, --length length
Interpret only length bytes of input.

-o 双字节八进制显示
-o, --two-bytes-octal
           Two-byte octal display. Display the input offset in
           hexadecimal, followed by eight space-separated, six-column,
           zero-filled, two-byte quantities of input data, in octal, per
           line.

-s 从偏移量开始输出
-s, --skip offset
           Skip offset bytes from the beginning of the input.

       -v, --no-squeezing
           The -v option causes hexdump to display all input data.
           Without the -v option, any number of groups of output lines
           which would be identical to the immediately preceding group
           of output lines (except for the input offsets), are replaced
           with a line comprised of a single asterisk.

-x 双字节十六进制显示
-x, --two-bytes-hex
           Two-byte hexadecimal display. Display the input offset in
           hexadecimal, followed by eight space-separated, four-column,
           zero-filled, two-byte quantities of input data, in
           hexadecimal, per line.

       -V, --version
           Display version information and exit.

       -h, --help
           Display help text and exit.

       For each input file, hexdump sequentially copies the input to
       standard output, transforming the data according to the format
       strings specified by the -e and -f options, in the order that
       they were specified.