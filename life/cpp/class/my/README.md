#ifndef SAFE_DELETE
#define SAFE_DELETE(x)                      \
        if ((x) != NULL) {                  \
            delete (x);                     \
            (x) = NULL;                     \
        }
#endif