The algorithm is just a simple xor with a fixed key that starts from a dynamic offset.
The mechanism for calculating the starting offset is changed, the good news is that it can be brute forced.

idstring "BGENCD>>"
savepos OFFSET
get SIZE asize
math SIZE - OFFSET

set secret string ""
strlen secret_size secret

# wrong, it's no longer valid!
xmath sb "(~size & 0x1039498) % secret_size"

for sb = 0 < secret_size
    filexor secret sb
    log MEMORY_FILE OFFSET 4
    filexor ""
    get TMP long MEMORY_FILE
    if TMP == 0x3cbfbbef
        break
    endif
next sb

get NAME basename
get EXT extension
string NAME p "%s_dec.%s" NAME EXT
filexor secret sb
log NAME OFFSET SIZE
