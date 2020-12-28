typedef struct exp_node {
  union {
    struct {
       char *sp;//stptr 本次efwrite要写的内容
       size_t slen;//stlen
    } val;
  } sub;