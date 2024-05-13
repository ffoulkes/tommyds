# Copyright 2021-2024 Intel Corporation
# SPDX-License-Identifier: Apache-2.0


add_library(tommyds_o OBJECT
# tommy.c
# tommy.h
  tommyalloc.c
  tommyalloc.h
  tommyarray.c
  tommyarray.h
  tommyarrayblk.c
  tommyarrayblk.h
  tommyarrayblkof.c
  tommyarrayblkof.h
  tommyarrayof.c
  tommyarrayof.h
  tommychain.h
  tommyhash.c
  tommyhash.h
  tommyhashdyn.c
  tommyhashdyn.h
  tommyhashlin.c
  tommyhashlin.h
  tommyhashtbl.c
  tommyhashtbl.h
  tommylist.c
  tommylist.h
# tommytree.c
# tommytree.h
  tommytrie.c
  tommytrie.h
  tommytrieinp.c
  tommytrieinp.h
  tommytypes.h
)

target_compile_options(tommyds_o PRIVATE
  -Wno-pointer-arith
  -Wno-implicit-fallthrough
  -Wno-free-nonheap-object
)
