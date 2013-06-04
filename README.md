haml_test
=========

Haml textarea post-processing performance

The problematic line is in `app/views/haml_test/_textareas.html.haml`

line 3 if uncommented causes drastic performance regression (from ~200ms to 3.5m on i5 laptop)

The problem seems to reside in haml library function `fix_textareas!`

The pattern used in method: `/([ ]*)<(textarea)([^>]*)>(\n|&#x000A;)(.*?)(<\/\2>)/im`
fails to parse closing <\/textarea> tag escaped by `escape_javascript`
(see `app/views/haml_test/textareas.js.haml`)
and each "<textarea..." match then proceeds to analyze input until end of string.
