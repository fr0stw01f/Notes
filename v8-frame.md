 slot      JS frame
      +-----------------+--------------------------------
 -n-1 |   parameter 0   |                            ^
      |- - - - - - - - -|                            |
 -n   |                 |                          Caller
 ...  |       ...       |                       frame slots
 -2   |  parameter n-1  |                       (slot < 0)
      |- - - - - - - - -|                            |
 -1   |   parameter n   |                            v
 -----+-----------------+--------------------------------
  0   |   return addr   |   ^                        ^
      |- - - - - - - - -|   |                        |
  1   | saved frame ptr | Fixed                      |
      |- - - - - - - - -| Header <-- frame ptr       |
  2   | [Constant Pool] |   |                        |
      |- - - - - - - - -|   |                        |
2+cp  |Context/Frm. Type|   v   if a constant pool   |
      |-----------------+----    is used, cp = 1,    |
3+cp  |                 |   ^   otherwise, cp = 0    |
      |- - - - - - - - -|   |                        |
4+cp  |                 |   |                      Callee
      |- - - - - - - - -|   |                   frame slots
 ...  |                 | Frame slots           (slot >= 0)
      |- - - - - - - - -|   |                        |
      |                 |   v                        |
 -----+-----------------+----- <-- stack ptr -------------
