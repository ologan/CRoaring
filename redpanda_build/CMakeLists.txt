cmake_minimum_required(VERSION 3.22)
project(roaringbitmap LANGUAGES C CXX)

option(Roaring_DISABLE_X64 "Forcefully disable x64 optimizations even if hardware supports it (this disables AVX)" OFF)
option(Roaring_DISABLE_AVX "Forcefully disable AVX even if hardware supports it " OFF)
option(Roaring_DISABLE_NEON "Forcefully disable NEON even if hardware supports it" OFF)
option(Roaring_DISABLE_AVX512 "Forcefully disable AVX512 even if compiler supports it" OFF)

set(src
  ../src/isadetection.c
  ../src/array_util.c
  ../src/bitset_util.c
  ../src/bitset.c
  ../src/containers/array.c
  ../src/containers/bitset.c
  ../src/containers/containers.c
  ../src/containers/convert.c
  ../src/containers/mixed_intersection.c
  ../src/containers/mixed_union.c
  ../src/containers/mixed_equal.c
  ../src/containers/mixed_subset.c
  ../src/containers/mixed_negation.c
  ../src/containers/mixed_xor.c
  ../src/containers/mixed_andnot.c
  ../src/containers/run.c
  ../src/memory.c
  ../src/roaring.c
  ../src/roaring_priority_queue.c
  ../src/roaring_array.c)

add_library(roaring ${src})

if(Roaring_DISABLE_AVX512)
  target_compile_definitions(roaring PUBLIC CROARING_COMPILER_SUPPORTS_AVX512=0)
endif()

if(Roaring_DISABLE_AVX)
  target_compile_definitions(roaring PUBLIC ROARING_DISABLE_AVX=1)
endif()

if(Roaring_DISABLE_X64)
  target_compile_definitions(roaring PUBLIC ROARING_DISABLE_X64=1)
endif()

if(Roaring_DISABLE_NEON)
  target_compile_definitions(roaring PUBLIC DISABLENEON=1)
endif()

target_include_directories(roaring PUBLIC ../include)

add_library(Roaring::roaring ALIAS roaring)
